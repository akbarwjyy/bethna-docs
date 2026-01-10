# Non-Custodial Design

## What is Non-Custodial?

**Non-custodial** means users maintain full control of their assets at all times. The protocol **cannot** withdraw or freeze user funds without explicit permission for each transaction.

## The Custody Spectrum

| Model | Custody | Risk | BethNa |
|-------|---------|------|--------|
| **Centralized Exchange** | Exchange holds funds | High (FTX, Mt.Gox) | No |
| **Custodial DeFi** | Protocol can withdraw | Medium | No |
| **Vault-Based** | Funds locked in vault | Medium-Low | No |
| **Non-Custodial** | User retains control | Low | Yes |

## How BethNa Achieves Non-Custody

### 1. ERC-20 Allowances (Not Transfers)

Traditional custodial model:
```solidity
// BAD: User sends funds to protocol
function deposit(uint256 amount) external {
    USDC.transferFrom(msg.sender, address(this), amount);
    userBalances[msg.sender] += amount; // Protocol holds funds
}
```

BethNa non-custodial model:
```solidity
// GOOD: User approves, protocol executes atomically
function buyOption(uint256 amount) external {
    // Transfer, swap, and return in ONE transaction
    USDC.transferFrom(msg.sender, address(this), amount);
    optionToken = thetanutsRouter.swap(amount);
    IERC20(optionToken).transfer(msg.sender, optionToken); // Returns immediately
}
```

**Key Difference**: Funds are never stored. They flow through the contract atomically.

### 2. Atomic Transactions

Every trade is **atomic** (all-or-nothing):

```
BEGIN TRANSACTION
  1. Transfer USDC from user
  2. Approve Thetanuts router
  3. Execute swap
  4. Receive option token
  5. Transfer option token to user
END TRANSACTION
```

If **any step fails**, the entire transaction reverts. Funds return to user automatically.

### 3. No Balance Storage

BethNa **never** stores user balances:

```solidity
// BethNa does NOT have this
mapping(address => uint256) public userBalances;

// All assets remain in user wallets
// Contract has ZERO balance after each transaction
```

### 4. Revocable Approvals

Users can revoke BethNa's spending approval at any time:

```solidity
// User can call this to revoke BethNa access
USDC.approve(sentientTraderAddress, 0);
```

After revocation, BethNa **cannot** execute any trades on behalf of the user.

## Comparison: Vault vs. Non-Custodial

### Ribbon Finance (Vault Model)

```
User deposits 1000 USDC
  ↓
Vault holds funds
  ↓
User receives vault shares (rbETH-THETA)
  ↓
Vault executes strategy (user has no control)
  ↓
User withdraws at end of epoch (no mid-epoch exit)
```

**Custody**: Ribbon vault **holds** your funds  
**Control**: Zero (strategy is pre-determined)  
**Exit**: Only at epoch end (7-28 days)

### BethNa (Non-Custodial Model)

```
User approves USDC spending
  ↓
Agent recommends trade
  ↓
User approves specific trade
  ↓
SentientTrader executes atomically
  ↓
Option tokens returned to user wallet immediately
  ↓
User can sell/transfer/hold freely
```

**Custody**: User **always** holds assets  
**Control**: Full (approve each trade)  
**Exit**: Instant (sell options anytime)

## Security Benefits

### 1. No Protocol Rug Risk

Even if BethNa team turns malicious, they **cannot**:
- Withdraw user funds
- Lock user assets
- Prevent users from selling options

**Why?** Because BethNa never has custody.

### 2. No Smart Contract Lock Risk

Traditional vaults:
```
User deposits → Funds locked for 30 days → Vault bug discovered
Result: Funds stuck for 30 days (or lost)
```

BethNa:
```
User approves → Trade executes → Assets returned immediately
Result: If bug detected, user simply revokes approval. Zero lock-up.
```

### 3. Composability

Since assets remain in user wallets, users can:
- Use options as collateral elsewhere
- Sell options on secondary markets
- Transfer options to other wallets
- Stake options (if applicable)

**Traditional vaults**: Funds locked in vault, unusable elsewhere.

## Trust Assumptions

Even with non-custodial design, users must trust:

| Risk | Mitigation |
|------|-----------|
| **Agent Recommendations** | Transparency (show reasoning), track record |
| **Slippage Manipulation** | Hard-coded 10% max slippage in contract |
| **Front-Running** | Private RPC endpoints, MEV protection |
| **Smart Contract Bugs** | Audits, bug bounty, emergency pause |

**Key Point**: Users trust BethNa for **strategy**, not **custody**.

## User Experience Flow

### Traditional DeFi

1. Approve token spending
2. Deposit to protocol
3. Wait for protocol action
4. Initiate withdrawal request
5. Wait for withdrawal delay
6. Finally receive funds

**Total Time**: Hours to days

### BethNa

1. Approve token spending
2. Approve trade
3. Receive option tokens (same transaction)

**Total Time**: ~30 seconds

## Emergency Scenarios

### Scenario 1: Thetanuts Router Exploit

**Traditional Vault**:
```
Funds locked in vault → Exploit drains liquidity → Users lose funds
```

**BethNa**:
```
User funds in wallet → Owner pauses SentientTrader → No new trades
Existing user funds unaffected (not in protocol)
```

### Scenario 2: BethNa Backend Compromise

**What Attacker Can Do**:
- Generate bad trade recommendations
- Attempt unauthorized execution

**What Attacker CANNOT Do**:
- Steal funds from user wallets
- Withdraw assets from protocol (none held)
- Bypass `onlyAuthorized` modifier

**User Protection**:
- Revoke USDC approval
- Stop using BethNa UI
- Assets remain safe in wallet

## Future Enhancements (Phase 2)

### Session Keys (EIP-4337)

Allow users to pre-approve multiple trades within limits:

```solidity
// User sets session parameters
sessionKey.approve({
    maxTradesPerDay: 5,
    maxAmountPerTrade: 1000 USDC,
    validUntil: now + 7 days
});
```

This maintains non-custody while improving UX (fewer approvals).

---

{% hint style="success" %}
**Summary**: BethNa is trustless for custody, trust-minimized for strategy.
{% endhint %}
