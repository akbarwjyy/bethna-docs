# Contract Infrastructure

## The SentientTrader Protocol

The on-chain backbone of BethNa is the `SentientTrader.sol` smart contract. It acts as a secure, non-custodial proxy between users and the Thetanuts V4 Router.

## Core Design Principles

### 1. Non-Custodial Architecture
- Uses standard ERC-20 allowances
- Never holds user funds beyond atomic transaction
- All option tokens returned immediately to user wallet

### 2. Security-First Approach
- Strict access control (`onlyAuthorized` modifier)
- Emergency pause functionality
- Hard-coded slippage protection
- Comprehensive event logging

### 3. Gas Optimization
- Minimal storage operations
- Efficient approval patterns
- Optimized call paths

## Contract Overview

### Architecture Diagram

```mermaid
graph TB
    subgraph "User Layer"
        U[User Wallet]
        USDC[USDC Balance]
        OPT[Option Tokens]
    end
    
    subgraph "BethNa Layer"
        AG[AI Agents]
        BE[Backend Service]
    end
    
    subgraph "Smart Contract Layer"
        ST[SentientTrader.sol]
        AC[Access Control]
        SP[Slippage Protection]
        PS[Pausable]
    end
    
    subgraph "External Protocol"
        TR[Thetanuts V4 Router]
        LP[Liquidity Pools]
    end
    
    U -->|Approve| ST
    AG -->|Recommendation| BE
    BE -->|Execute Trade| ST
    ST -->|Check Authorization| AC
    ST -->|Validate Slippage| SP
    ST -->|Check Status| PS
    ST -->|Swap| TR
    TR -->|Trade| LP
    LP -->|Return Tokens| TR
    TR -->|Return| ST
    ST -->|Transfer| U
    
    style ST fill:#4CAF50
    style U fill:#2196F3
    style TR fill:#FF9800
```

### Key Components

| Component | Purpose |
|-----------|---------|
| **Access Control** | Whitelist of authorized agent addresses |
| **Pausable** | Emergency circuit breaker |
| **Slippage Protection** | Maximum 10% slippage enforcement |
| **Event Emission** | Complete audit trail |
| **Router Integration** | Direct interface to Thetanuts V4 |

### State Variables

```solidity
// Access control
mapping(address => bool) public authorizedTraders;

// Protocol addresses
address public thetanutsRouter;
address public immutable USDC;

// Safety parameters
uint256 public constant MAX_SLIPPAGE_BPS = 1000; // 10%

// Admin
address public owner;
bool public isPaused;
```

## Interaction Flow

### Buy Option Flow

```mermaid
sequenceDiagram
    participant U as User Wallet
    participant ST as SentientTrader
    participant TR as Thetanuts Router
    participant LP as Liquidity Pool
    
    Note over U: Has USDC
    U->>ST: 1. Approve USDC spending
    U->>ST: 2. buyOption(optionToken, amountIn)
    
    ST->>ST: Check authorization
    ST->>ST: Check not paused
    ST->>U: 3. transferFrom(USDC)
    
    ST->>TR: 4. Approve USDC
    ST->>TR: 5. swapExactTokensForTokens()
    TR->>LP: 6. Execute swap
    LP->>TR: 7. Return option tokens
    TR->>ST: 8. Return option tokens
    
    ST->>ST: 9. Validate slippage
    ST->>U: 10. Transfer option tokens
    
    Note over U: Received Option Tokens
    ST->>ST: Emit OptionPurchased event
```

### Sell Option Flow

```mermaid
sequenceDiagram
    participant U as User Wallet
    participant ST as SentientTrader
    participant TR as Thetanuts Router
    participant LP as Liquidity Pool
    
    Note over U: Has Option Tokens
    U->>ST: 1. Approve option token spending
    U->>ST: 2. sellOption(optionToken, amountIn)
    
    ST->>ST: Check authorization
    ST->>ST: Check not paused
    ST->>U: 3. transferFrom(option tokens)
    
    ST->>TR: 4. Approve option tokens
    ST->>TR: 5. swapExactTokensForTokens()
    TR->>LP: 6. Execute swap
    LP->>TR: 7. Return USDC
    TR->>ST: 8. Return USDC
    
    ST->>ST: 9. Validate slippage
    ST->>U: 10. Transfer USDC
    
    Note over U: Received USDC
    ST->>ST: Emit OptionSold event
```

## Smart Contract Functions

### Core Trading Functions

#### `buyOption()`
```solidity
function buyOption(
    address optionToken,
    uint256 amountIn,
    uint256 minAmountOut
) external onlyAuthorized whenNotPaused returns (uint256)
```
Purchases options from Thetanuts using USDC.

#### `sellOption()`
```solidity
function sellOption(
    address optionToken,
    uint256 amountIn,
    uint256 minAmountOut
) external onlyAuthorized whenNotPaused returns (uint256)
```
Sells options back to Thetanuts for USDC.

### Administrative Functions

#### `setAuthorizedTrader()`
```solidity
function setAuthorizedTrader(
    address trader,
    bool authorized
) external onlyOwner
```
Adds or removes authorized agent addresses.

#### `pause()` / `unpause()`
```solidity
function pause() external onlyOwner
function unpause() external onlyOwner
```
Emergency circuit breakers.

#### `updateRouter()`
```solidity
function updateRouter(address newRouter) external onlyOwner
```
Updates Thetanuts router address (for protocol upgrades).

## Security Features

### Security Architecture

```mermaid
graph LR
    subgraph "Security Layers"
        L1[Layer 1: Authorization]
        L2[Layer 2: Pausable]
        L3[Layer 3: Slippage Protection]
        L4[Layer 4: Reentrancy Guard]
        L5[Layer 5: Event Logging]
    end
    
    subgraph "Entry Points"
        BUY[buyOption]
        SELL[sellOption]
    end
    
    BUY --> L1
    SELL --> L1
    L1 --> L2
    L2 --> L4
    L4 --> L3
    L3 --> L5
    
    L1 -.->|Fail| REJECT[Reject Transaction]
    L2 -.->|Fail| REJECT
    L3 -.->|Fail| REJECT
    L4 -.->|Fail| REJECT
    L5 --> SUCCESS[Execute Trade]
    
    style L1 fill:#f44336
    style L2 fill:#ff9800
    style L3 fill:#ffc107
    style L4 fill:#4caf50
    style L5 fill:#2196f3
    style SUCCESS fill:#4CAF50
    style REJECT fill:#f44336
```

### 1. Authorization System

Only whitelisted addresses can execute trades:

```solidity
modifier onlyAuthorized() {
    require(authorizedTraders[msg.sender], "SentientTrader: Not authorized");
    _;
}
```

**Authorized Addresses**:
- BethNa backend service
- DAO multisig (for testing/emergency)

### 2. Slippage Protection

Enforces maximum slippage on every trade:

```solidity
uint256 expectedOutput = getQuote(amountIn);
uint256 minOutput = expectedOutput * (10000 - MAX_SLIPPAGE_BPS) / 10000;

uint256 actualOutput = router.swap(amountIn, minOutput);
require(actualOutput >= minOutput, "Slippage exceeded");
```

### 3. Emergency Pause

DAO can freeze all trading instantly:

```solidity
modifier whenNotPaused() {
    require(!isPaused, "SentientTrader: Paused");
    _;
}
```

**Use Cases**:
- Thetanuts router vulnerability detected
- Suspicious trading activity
- Protocol migration

### 4. Event Logging

Every action is logged for transparency:

```solidity
event OptionPurchased(
    address indexed user,
    address indexed optionToken,
    uint256 amountIn,
    uint256 amountOut,
    uint256 timestamp
);

event OptionSold(
    address indexed user,
    address indexed optionToken,
    uint256 amountIn,
    uint256 amountOut,
    uint256 timestamp
);
```

## Deployment Info

| Network | Address | Status |
|---------|---------|--------|
| **Base Mainnet** | `0x742d35Cc...` *(TBD)* | Live |
| **Base Sepolia** | `0x8f2e1b...` | Testnet |

### Contract Dependencies

```mermaid
graph TD
    ST[SentientTrader.sol]
    OZ1[Ownable]
    OZ2[Pausable]
    OZ3[ReentrancyGuard]
    IERC[IERC20]
    TR[IThetanutsRouter]
    
    ST ---|inherits| OZ1
    ST ---|inherits| OZ2
    ST ---|inherits| OZ3
    ST ---|uses| IERC
    ST ---|interfaces| TR
    
    OZ1 ---|from| OZC[OpenZeppelin v4.9.0]
    OZ2 ---|from| OZC
    OZ3 ---|from| OZC
    IERC ---|from| OZC
    
    style ST fill:#4CAF50
    style OZC fill:#2196F3
```

## Upgradability

**Current Design**: Non-upgradable (immutable)

**Future Consideration** *(Phase 2)*:
- Transparent proxy pattern for feature additions
- Timelock on upgrades (7-day delay)
- DAO governance required for approval

---

{% hint style="success" %}
**Technical details**: [SentientTrader.sol Specification](sentient-trader.md)
{% endhint %}
