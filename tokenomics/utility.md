# Token Utility

## Comprehensive Overview of $BETH Use Cases

$BETH is a **utility and governance token** with multiple value-accruing mechanisms. It's not just a speculative asset—it powers the entire BethNa ecosystem.

## 1. Governance Rights

### What Can You Vote On?

#### New Agent Strategies
```
Proposal: "Add BTC Iron Condor Strategy"
Description: Implement iron condor for BTC to capitalize on low-volatility periods.
Required $BETH: 100,000 (proposal threshold)
Voting Period: 5 days
Outcome: 68% approval → Implemented in Phase 2
```

#### Risk Parameters
```
Proposal: "Increase Maximum Position Size to 25%"
Current: 20% of portfolio per position
Proposed: 25% of portfolio per position
Rationale: Allow users with smaller portfolios more flexibility
Voting Result: 52% approval → Implemented
```

#### Fee Structure
```
Proposal: "Reduce Free Tier Performance Fee to 15%"
Current: 20% performance fee
Proposed: 15% performance fee
Rationale: Attract more users to the platform
Voting Result: 45% approval → Rejected
```

### Governance Process

**Step 1: Temperature Check** (Forum Discussion)
- Post idea on BethNa Forum
- Community feedback (7 days)
- Gauge initial interest

**Step 2: Formal Proposal** (On-Chain)
- Submit proposal contract
- Requires 100,000 $BETH deposit (refunded if passed)
- 7-day discussion period

**Step 3: Voting** (On-Chain)
- 5-day voting window
- Snapshot voting (gas-free)
- Quorum: 10% of circulating supply
- Pass threshold: 51% approval

**Step 4: Execution** (Timelock)
- 2-day timelock (safety buffer)
- Automatic execution via governance contracts

---

## 2. Fee Discount Mechanics

### Staking for Discounts

```solidity
// Staking contract (simplified)
function stake(uint256 amount) external {
    BETH.transferFrom(msg.sender, address(this), amount);
    userStake[msg.sender] += amount;
    updateTier(msg.sender);
}

function updateTier(address user) internal {
    uint256 staked = userStake[user];
    
    if (staked >= 500_000) userTier[user] = Tier.PLATINUM;
    else if (staked >= 100_000) userTier[user] = Tier.GOLD;
    else if (staked >= 50_000) userTier[user] = Tier.SILVER;
    else if (staked >= 10_000) userTier[user] = Tier.BRONZE;
    else userTier[user] = Tier.FREE;
}
```

### Fee Calculation Example

**Scenario**: User makes $10,000 profit over a month

| Tier | Staked | Fee % | Fee Paid | Net Profit | Savings |
|------|--------|-------|----------|------------|---------|
| Free | 0 | 20% | $2,000 | $8,000 | - |
| Bronze | 10K | 15% | $1,500 | $8,500 | $500 |
| Silver | 50K | 12% | $1,200 | $8,800 | $800 |
| Gold | 100K | 10% | $1,000 | $9,000 | $1,000 |
| Platinum | 500K | 5% | $500 | $9,500 | $1,500 |

**ROI Analysis**: If you trade frequently and are profitable, staking to Gold tier ($100K $BETH) can pay for itself in 3-6 months through fee savings.

---

## 3. Pro Access Features

### Agent Delta (Hedging) - 25,000 $BETH

**What You Get**:
- Delta-neutral strategy recommendations
- Perp hedging automation
- Cross-asset hedging (ETH with BTC)
- Dynamic rebalancing

**Value Prop**: If you're managing a $100K portfolio, delta hedging can reduce drawdowns by 30-50%, worth far more than the cost of $BETH.

### Agent Gamma (Advanced Risk) - 10,000 $BETH

**What You Get**:
- Advanced portfolio analytics
- Greek exposure tracking
- Custom risk alerts
- Backtesting tools

### Custom Strategy Builder - 100,000 $BETH

**What You Get**:
- Build your own trading strategies
- Define custom entry/exit rules
- Backtest against 3 years of data
- Deploy to Agent Alpha

**Example Custom Strategy**:
```python
def my_strategy(market_data):
    if (market_data.rsi < 30 and 
        market_data.funding < -0.01 and
        market_data.iv_percentile < 40):
        return "LONG_CALL"
    return "WAIT"
```

### API Access - 50,000 $BETH

**What You Get**:
- REST API for programmatic trading
- WebSocket for real-time data
- Rate limit: 1000 req/min (vs 100 for free)
- Historical data export

**Use Case**: Institutional traders, quant funds, trading bots.

---

## 4. Revenue Share & Deflationary Mechanics

### How Buybacks Work

**Weekly Buyback Cycle**:
```
1. Calculate Protocol Revenue (in USDC)
   Example: $500,000 in fees collected this week

2. Allocate 30% to Buyback
   Buyback Amount: $150,000

3. Execute Market Buy on Uniswap V3
   USDC → $BETH swap
   Amount Bought: 1,500,000 $BETH (at $0.10)

4. Burn Tokens
   Send to 0x000...dead
   Permanently remove from circulation

5. Update Circulating Supply
   New Supply: 998,500,000 $BETH
```

**Impact on Token Price**:
- Reduced supply → Increased scarcity
- Constant buying pressure → Price support
- Transparent & predictable → Confidence

### Projected Buyback Volume

**Assumptions**:
- Protocol TVL: $10M (average)
- Average fee per user: 15% on profits
- User profitability: 60% win rate
- Monthly volume: $50M

**Monthly Calculation**:
```
Trading Volume: $50M
Profitable Trades: $50M * 60% = $30M
Average Fee: 15%
Protocol Revenue: $30M * 15% = $4.5M

Buyback Allocation: $4.5M * 30% = $1.35M/month
Annual Buyback: $16.2M
```

At $0.10/token: **162M $BETH burned per year** (16.2% of supply).

---

## 5. Additional Utility (Phase 2 & 3)

### Liquidity Mining Rewards (Phase 2)

Provide liquidity to $BETH/USDC pool on Uniswap:
- Earn trading fees (0.3%)
- Earn $BETH rewards (20M tokens/year)
- Boost: Stake LP tokens for 2x rewards

### Agent Marketplace (Phase 3)

Developers can sell custom agents:
- List your agent for sale
- Users pay in $BETH
- Platform takes 10% fee (in $BETH)
- Sellers receive 90% (in $BETH)

**Example**:
- Agent: "Momentum Scalper Bot"
- Price: 5,000 $BETH
- Sales: 200 users
- Developer Revenue: 900,000 $BETH ($90K at $0.10)

### Insurance Fund (Phase 3)

Stake $BETH to backstop losses:
- Insurance pool protects against smart contract bugs
- Stakers earn 5% APY
- First-loss capital in case of exploit

---

## Value Accrual Summary

| Mechanism | Type | Impact |
|-----------|------|--------|
| **Governance** | Utility | Increases participation → protocol improvement |
| **Fee Discount** | Utility | Demand from active traders → price floor |
| **Pro Access** | Utility | Demand from power users → sustained buying |
| **Buyback & Burn** | Deflationary | Reduces supply → price appreciation |
| **Staking Rewards** | Yield | Reduces circulating supply → scarcity |

---

{% hint style="success" %}
**Deep dive**: [Tiered Access System](tiered-access.md) | [Value Accrual Mechanics](value-accrual.md)
{% endhint %}
