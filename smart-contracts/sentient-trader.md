# SentientTrader.sol

## Full Contract Specification

### Contract Header

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/Pausable.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

/**
 * @title SentientTrader
 * @notice Non-custodial proxy for executing options trades on Thetanuts Finance
 * @dev Only authorized BethNa agents can execute trades
 */
contract SentientTrader is Ownable, Pausable, ReentrancyGuard {
    // Contract implementation...
}
```

## State Variables

```solidity
/// @notice Thetanuts V4 Router address
address public thetanutsRouter;

/// @notice USDC token address on Base
address public immutable USDC;

/// @notice Maximum allowed slippage in basis points (10% = 1000 bps)
uint256 public constant MAX_SLIPPAGE_BPS = 1000;

/// @notice Authorized trading agents
mapping(address => bool) public authorizedTraders;

/// @notice Trade counter for analytics
uint256 public totalTradesExecuted;
```

## Events

```solidity
/// @notice Emitted when an option is purchased
event OptionPurchased(
    address indexed user,
    address indexed optionToken,
    uint256 amountIn,
    uint256 amountOut,
    uint256 timestamp
);

/// @notice Emitted when an option is sold
event OptionSold(
    address indexed user,
    address indexed optionToken,
    uint256 amountIn,
    uint256 amountOut,
    uint256 timestamp
);

/// @notice Emitted when trader authorization changes
event TraderAuthorizationChanged(
    address indexed trader,
    bool authorized
);

/// @notice Emitted when router address is updated
event RouterUpdated(
    address indexed oldRouter,
    address indexed newRouter
);
```

## Modifiers

```solidity
/// @notice Restricts function access to authorized traders
modifier onlyAuthorized() {
    require(
        authorizedTraders[msg.sender],
        "SentientTrader: Not authorized"
    );
    _;
}
```

## Constructor

```solidity
/**
 * @notice Initializes the SentientTrader contract
 * @param _thetanutsRouter Address of Thetanuts V4 Router
 * @param _usdc Address of USDC token on Base
 * @param _initialAuthorizedTrader Address of first authorized trader (BethNa backend)
 */
constructor(
    address _thetanutsRouter,
    address _usdc,
    address _initialAuthorizedTrader
) {
    require(_thetanutsRouter != address(0), "Invalid router");
    require(_usdc != address(0), "Invalid USDC");
    require(_initialAuthorizedTrader != address(0), "Invalid trader");

    thetanutsRouter = _thetanutsRouter;
    USDC = _usdc;
    authorizedTraders[_initialAuthorizedTrader] = true;

    emit TraderAuthorizationChanged(_initialAuthorizedTrader, true);
}
```

## Core Functions

### buyOption()

```solidity
/**
 * @notice Purchases options from Thetanuts using USDC
 * @param optionToken Address of the option token to purchase
 * @param amountIn Amount of USDC to spend
 * @param minAmountOut Minimum option tokens to receive (slippage protection)
 * @return amountOut Actual amount of option tokens received
 */
function buyOption(
    address optionToken,
    uint256 amountIn,
    uint256 minAmountOut
) 
    external 
    onlyAuthorized 
    whenNotPaused 
    nonReentrant 
    returns (uint256 amountOut) 
{
    require(optionToken != address(0), "Invalid option token");
    require(amountIn > 0, "Amount must be > 0");

    // Transfer USDC from user (requires prior approval)
    IERC20(USDC).transferFrom(msg.sender, address(this), amountIn);

    // Approve Thetanuts router to spend USDC
    IERC20(USDC).approve(thetanutsRouter, amountIn);

    // Execute swap via Thetanuts
    amountOut = IThetanutsRouter(thetanutsRouter).swapExactTokensForTokens(
        amountIn,
        minAmountOut,
        getPath(USDC, optionToken),
        address(this),
        block.timestamp + 300 // 5-minute deadline
    )[1]; // Return amount of option tokens

    // Enforce slippage protection
    uint256 maxSlippage = (amountIn * MAX_SLIPPAGE_BPS) / 10000;
    require(
        amountOut >= minAmountOut,
        "Slippage exceeded"
    );

    // Transfer option tokens to user
    IERC20(optionToken).transfer(msg.sender, amountOut);

    totalTradesExecuted++;

    emit OptionPurchased(
        msg.sender,
        optionToken,
        amountIn,
        amountOut,
        block.timestamp
    );

    return amountOut;
}
```

### sellOption()

```solidity
/**
 * @notice Sells options back to Thetanuts for USDC
 * @param optionToken Address of the option token to sell
 * @param amountIn Amount of option tokens to sell
 * @param minAmountOut Minimum USDC to receive (slippage protection)
 * @return amountOut Actual amount of USDC received
 */
function sellOption(
    address optionToken,
    uint256 amountIn,
    uint256 minAmountOut
) 
    external 
    onlyAuthorized 
    whenNotPaused 
    nonReentrant 
    returns (uint256 amountOut) 
{
    require(optionToken != address(0), "Invalid option token");
    require(amountIn > 0, "Amount must be > 0");

    // Transfer option tokens from user (requires prior approval)
    IERC20(optionToken).transferFrom(msg.sender, address(this), amountIn);

    // Approve Thetanuts router to spend option tokens
    IERC20(optionToken).approve(thetanutsRouter, amountIn);

    // Execute swap via Thetanuts
    amountOut = IThetanutsRouter(thetanutsRouter).swapExactTokensForTokens(
        amountIn,
        minAmountOut,
        getPath(optionToken, USDC),
        address(this),
        block.timestamp + 300 // 5-minute deadline
    )[1]; // Return amount of USDC

    // Enforce slippage protection
    require(
        amountOut >= minAmountOut,
        "Slippage exceeded"
    );

    // Transfer USDC to user
    IERC20(USDC).transfer(msg.sender, amountOut);

    totalTradesExecuted++;

    emit OptionSold(
        msg.sender,
        optionToken,
        amountIn,
        amountOut,
        block.timestamp
    );

    return amountOut;
}
```

## Administrative Functions

### setAuthorizedTrader()

```solidity
/**
 * @notice Adds or removes an authorized trader
 * @param trader Address to authorize/deauthorize
 * @param authorized True to authorize, false to revoke
 */
function setAuthorizedTrader(address trader, bool authorized) 
    external 
    onlyOwner 
{
    require(trader != address(0), "Invalid trader");
    authorizedTraders[trader] = authorized;
    
    emit TraderAuthorizationChanged(trader, authorized);
}
```

### updateRouter()

```solidity
/**
 * @notice Updates the Thetanuts router address
 * @param newRouter Address of new router
 */
function updateRouter(address newRouter) external onlyOwner {
    require(newRouter != address(0), "Invalid router");
    
    address oldRouter = thetanutsRouter;
    thetanutsRouter = newRouter;
    
    emit RouterUpdated(oldRouter, newRouter);
}
```

### pause() / unpause()

```solidity
/**
 * @notice Pauses all trading (emergency use only)
 */
function pause() external onlyOwner {
    _pause();
}

/**
 * @notice Resumes trading after pause
 */
function unpause() external onlyOwner {
    _unpause();
}
```

## Utility Functions

### getPath()

```solidity
/**
 * @notice Generates swap path for Thetanuts router
 * @param tokenIn Input token address
 * @param tokenOut Output token address
 * @return path Array of token addresses for swap route
 */
function getPath(address tokenIn, address tokenOut) 
    internal 
    pure 
    returns (address[] memory path) 
{
    path = new address[](2);
    path[0] = tokenIn;
    path[1] = tokenOut;
    return path;
}
```

## Security Analysis

### Attack Vectors & Mitigations

| Attack | Mitigation |
|--------|-----------|
| **Unauthorized Execution** | `onlyAuthorized` modifier + whitelist |
| **Reentrancy** | `ReentrancyGuard` from OpenZeppelin |
| **Front-Running** | Slippage protection via `minAmountOut` |
| **Flash Loan Attack** | No price oracles used (atomic swaps only) |
| **Router Vulnerability** | Emergency pause + router upgradability |

### Audit Status

| Auditor | Status | Report |
|---------|--------|--------|
| **Internal Review** | Complete | [View](https://github.com/bethna/audits) |
| **External Audit** | Q2 2026 | TBD |
| **Bug Bounty** | Q2 2026 | Up to $50,000 |

---

{% hint style="info" %}
**Learn more**: [Non-Custodial Design](non-custodial.md)
{% endhint %}
