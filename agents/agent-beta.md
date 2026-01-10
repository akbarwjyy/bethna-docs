# Agent Beta - The Executor

## Role Overview

**Agent Beta** is the execution engine of BethNa. It takes validated signals from Agent Alpha and interacts directly with the blockchain to execute trades with minimal slippage.

## Core Function

**Trade execution and slippage verification.**

## Responsibilities

### 1. Signal Reception
- Listens for validated signals from Agent Alpha
- Verifies signal integrity (checksums, signatures)
- Confirms user approval before execution

### 2. Pre-Execution Checks
Before executing any trade, Agent Beta validates:

| Check | Purpose |
|-------|---------|
| **Liquidity Depth** | Ensures sufficient liquidity for the trade size |
| **Gas Price** | Confirms gas is below threshold (avoids expensive trades) |
| **Slippage Estimate** | Predicts expected slippage based on order book |
| **Smart Contract State** | Verifies `SentientTrader.sol` is not paused |

### 3. Execution Strategy

Agent Beta uses **smart order routing**:

```python
if trade_size < liquidity_threshold:
    # Small trade: Direct execution
    execute_atomic_swap()
else:
    # Large trade: Split into smaller chunks
    execute_twap_strategy()  # Time-Weighted Average Price
```

### 4. Slippage Protection

Agent Beta enforces strict slippage limits:

**Maximum Allowed Slippage**: 10% (hard-coded in `SentientTrader.sol`)

**Execution Logic**:
```solidity
uint256 expectedOutput = quote(inputAmount);
uint256 minOutput = expectedOutput * (10000 - MAX_SLIPPAGE_BPS) / 10000;

require(actualOutput >= minOutput, "Slippage too high");
```

If slippage exceeds limits, Agent Beta:
1. Cancels the transaction
2. Alerts Agent Alpha to recalculate
3. Notifies user with explanation

## Trade Execution Flow

### Step-by-Step Process

**1. Receive Signal from Agent Alpha**
```json
{
  "signal_type": "LONG_CALL",
  "asset": "ETH",
  "strike": 3200,
  "expiry": "2026-01-17",
  "amount": "0.05 ETH"
}
```

**2. Fetch Current Liquidity**
```python
liquidity = thetanuts_router.get_liquidity(
    asset="ETH",
    strike=3200,
    expiry="2026-01-17"
)

if liquidity < required_amount:
    return ERROR_INSUFFICIENT_LIQUIDITY
```

**3. Estimate Gas**
```python
gas_price = base_network.get_gas_price()
execution_cost = gas_price * estimated_gas_units

if execution_cost > MAX_GAS_THRESHOLD:
    return ERROR_GAS_TOO_HIGH
```

**4. Execute Atomic Swap**
```solidity
// In SentientTrader.sol
function buyOption(
    address optionToken,
    uint256 amountIn,
    uint256 minAmountOut
) external onlyAuthorized whenNotPaused {
    // Transfer USDC from user (via approval)
    IERC20(USDC).transferFrom(msg.sender, address(this), amountIn);
    
    // Approve Thetanuts router
    IERC20(USDC).approve(thetanutsRouter, amountIn);
    
    // Execute swap
    uint256 optionsReceived = IThetanutsRouter(thetanutsRouter).swap(
        USDC,
        optionToken,
        amountIn,
        minAmountOut
    );
    
    // Return option tokens to user
    IERC20(optionToken).transfer(msg.sender, optionsReceived);
    
    emit OptionPurchased(msg.sender, optionToken, optionsReceived);
}
```

**5. Verify Execution**
```python
tx_receipt = wait_for_confirmation(tx_hash)

if tx_receipt.status == "SUCCESS":
    actual_slippage = calculate_slippage(expected, actual)
    log_execution(signal_id, actual_slippage, tx_hash)
    notify_agent_gamma(position_id, entry_price, greeks)
else:
    log_failure(signal_id, tx_receipt.error)
    notify_user(ERROR_EXECUTION_FAILED)
```

## Advanced Execution Features

### TWAP (Time-Weighted Average Price)

For large trades, Agent Beta splits execution:

```python
def execute_twap(total_amount, num_chunks, time_interval):
    chunk_size = total_amount / num_chunks
    
    for i in range(num_chunks):
        execute_atomic_swap(chunk_size)
        wait(time_interval)
        
        # Recalculate if market moves significantly
        if price_deviation > 2%:
            recalculate_remaining_chunks()
```

**Use Case**: User wants to buy 10 ETH worth of options, but order book only has 3 ETH liquidity. Agent Beta splits into 4 chunks of 2.5 ETH each, executed over 20 minutes.

### Gas Optimization

Agent Beta monitors Base network gas prices:

| Gas Price | Action |
|-----------|--------|
| < 0.01 gwei | Execute immediately |
| 0.01 - 0.05 gwei | Execute (standard) |
| 0.05 - 0.1 gwei | Warn user, ask for approval |
| > 0.1 gwei | Delay execution, wait for lower gas |

### Retry Logic

If execution fails, Agent Beta retries with adjusted parameters:

```python
max_retries = 3
retry_count = 0

while retry_count < max_retries:
    try:
        execute_swap()
        break
    except InsufficientLiquidity:
        reduce_trade_size(10%)
        retry_count += 1
    except GasTooHigh:
        wait(60)  # Wait 1 minute
        retry_count += 1
```

## Safety Features

### Access Control
Only whitelisted addresses (BethNa backend) can call `SentientTrader.sol` functions:

```solidity
modifier onlyAuthorized() {
    require(authorizedTraders[msg.sender], "Not authorized");
    _;
}
```

### Emergency Pause
If a vulnerability is detected, the DAO can pause all trading:

```solidity
function pause() external onlyOwner {
    _pause();
}

function unpause() external onlyOwner {
    _unpause();
}
```

### Event Logging
Every trade is logged on-chain for transparency:

```solidity
event OptionPurchased(
    address indexed user,
    address indexed optionToken,
    uint256 amount,
    uint256 timestamp
);
```

## Performance Metrics

| Metric | Target | Actual (Q1 2026) |
|--------|--------|------------------|
| **Success Rate** | >95% | 97% |
| **Average Slippage** | <3% | 2.1% |
| **Execution Time** | <30 seconds | 18 seconds |
| **Gas Efficiency** | <$2 per trade | $1.20 per trade |

---

{% hint style="success" %}
**Next**: Discover [Agent Gamma - The Risk Manager](agent-gamma.md)
{% endhint %}
