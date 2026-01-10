# Agent Delta - The Hedger

{% hint style="info" %}
**Status**: Coming in Phase 2 (Q3 2026)
{% endhint %}

## Role Overview

**Agent Delta** is the portfolio hedging specialist. It executes delta-neutral and volatility-harvesting strategies to generate returns independent of market direction.

## Core Function

**Neutralizing market direction risk through sophisticated hedging strategies.**

## Why Hedging Matters

### The Problem with Directional Bets

Most traders (retail and institutional) are **directionally exposed**:
- Long calls → Profit only if price goes up
- Long puts → Profit only if price goes down

**Result**: 50% of the time, you're wrong (or break even).

### The Delta-Neutral Alternative

Delta-neutral strategies profit from **volatility**, not direction:
- Long straddle (call + put) → Profit if price moves significantly in either direction
- Long call + short perp → Profit from volatility while hedging delta

**Result**: Positive expectancy regardless of market direction.

## Agent Delta Strategies

### Strategy 1: Long Straddle

**Setup**:
- Buy ATM call
- Buy ATM put (same strike, same expiry)

**Payoff**:
- Profits if price moves >20% in either direction
- Max loss: Total premium paid

**When Agent Delta Recommends**:
- Implied volatility < 40th percentile (volatility is "cheap")
- Upcoming catalyst (Fed meeting, protocol upgrade)
- Market consolidating (squeeze pending)

**Example**:
```
ETH at $3,000

Agent Delta recommends:
- Buy ETH $3,000 Call (7-day) for 0.05 ETH
- Buy ETH $3,000 Put (7-day) for 0.05 ETH
- Total cost: 0.10 ETH ($300)

Profit scenarios:
- ETH > $3,300: Call profits exceed total cost
- ETH < $2,700: Put profits exceed total cost
- ETH $2,700-$3,300: Loss capped at $300
```

---

### Strategy 2: Delta-Neutral Hedging

**Setup**:
- Buy call option (delta = +0.30)
- Short 0.30 ETH on perpetual (delta = -0.30)
- **Net delta**: 0 (market neutral)

**Payoff**:
- Profits from volatility expansion
- Unaffected by price direction

**When Agent Delta Recommends**:
- High implied volatility (>60th percentile)
- Expected volatility crush after event
- User wants exposure without directional risk

**Example**:
```
ETH at $3,000

Agent Delta strategy:
1. Buy ETH $3,200 Call (delta = 0.30)
2. Short 0.30 ETH on dYdX at $3,000

Outcome 1: ETH pumps to $3,500
- Call profit: +$150
- Perp loss: -$150
- Net: $0 (delta-neutral)

Outcome 2: IV expands from 50% to 70%
- Call value increases due to vega
- Perp position unaffected
- Net: +$80 (volatility profit)
```

---

### Strategy 3: Iron Condor

**Setup** *(Advanced)*:
- Sell OTM call
- Buy further OTM call
- Sell OTM put
- Buy further OTM put

**Payoff**:
- Profits if price stays within a range
- Limited risk, limited reward

**When Agent Delta Recommends**:
- Low expected volatility
- Market in consolidation
- User wants income generation

**Example**:
```
ETH at $3,000

Iron Condor:
- Sell $3,200 Call
- Buy $3,400 Call
- Sell $2,800 Put
- Buy $2,600 Put

Max profit: $100 (if ETH stays between $2,800-$3,200)
Max loss: $100 (if ETH moves beyond $3,400 or $2,600)
```

---

## How Agent Delta Works

### Step 1: Portfolio Analysis

Agent Delta assesses your current exposure:

```python
portfolio_delta = sum(position.delta * position.size)
portfolio_vega = sum(position.vega * position.size)
portfolio_theta = sum(position.theta * position.size)
```

### Step 2: Risk Identification

```python
if abs(portfolio_delta) > 0.5:
    risk = "HIGH_DIRECTIONAL_EXPOSURE"
    recommendation = "HEDGE_DELTA"
elif portfolio_vega > 20:
    risk = "HIGH_VOLATILITY_EXPOSURE"
    recommendation = "REDUCE_VEGA"
```

### Step 3: Hedge Recommendation

```json
{
  "hedge_type": "DELTA_NEUTRAL_PERP",
  "reasoning": "Your portfolio delta is +0.72 (very bullish). To neutralize, short 0.72 ETH on dYdX.",
  "execution": {
    "action": "SHORT_PERP",
    "asset": "ETH",
    "size": 0.72,
    "venue": "dYdX"
  },
  "expected_outcome": "Portfolio becomes market-neutral. You profit from volatility, not direction."
}
```

### Step 4: Execution via Agent Beta

Once user approves, Agent Delta coordinates with Agent Beta:
1. Agent Beta shorts 0.72 ETH on dYdX
2. Position is linked to existing options portfolio
3. Agent Gamma monitors combined portfolio

---

## Advanced Features

### Dynamic Hedging

Agent Delta adjusts hedges as markets move:

```python
if portfolio_delta > 0.3:
    increase_short_perp_position()
elif portfolio_delta < -0.3:
    increase_long_perp_position()
```

**Example**:
- Initial: Long 1 ETH call (delta = 0.30) + short 0.30 ETH perp
- ETH pumps → Call delta increases to 0.50
- Agent Delta: "Your delta increased to +0.20. Hedge with additional 0.20 ETH short."

### Cross-Asset Hedging

Agent Delta can hedge using correlated assets:

```python
if low_liquidity_on_eth_perps:
    hedge_with_btc_perps(correlation_factor=0.85)
```

**Example**:
- User has 1 ETH call (delta = 0.30)
- ETH perp liquidity is low
- Agent Delta: "Hedge with 0.35 BTC short (ETH/BTC correlation = 0.85)"

---

## Risk Management

Agent Delta includes safety features:

### Maximum Hedge Ratio
```python
if hedge_ratio > 1.5:
    return ERROR_OVER_HEDGING
```
Prevents over-hedging (which creates the opposite directional risk).

### Correlation Monitoring
```python
if eth_btc_correlation < 0.7:
    warn_user("ETH-BTC correlation is low. Cross-asset hedging may be ineffective.")
```

### Funding Rate Monitoring
```python
if perp_funding_rate < -0.05%:
    warn_user("Negative funding on perps. You'll earn funding while hedging.")
```

---

## Performance Projections

| Metric | Target (Phase 2) |
|--------|------------------|
| **Sharpe Ratio Improvement** | +0.5 vs. unhedged |
| **Downside Protection** | -30% in 30%+ market dumps |
| **Volatility Capture** | 60% of IV expansion profits |
| **Hedge Accuracy** | >85% correlation with intended exposure |

---

## Roadmap

### Phase 2 (Q3 2026)
- Basic delta hedging via dYdX
- Long straddle recommendations
- Manual hedge execution (user approval required)

### Phase 3 (2027)
- Fully automated dynamic hedging
- Iron condor and butterfly strategies
- Cross-chain hedging (Arbitrum, Optimism)
- Custom hedge strategy builder

---

{% hint style="success" %}
**Back to overview**: [Swarm Agent System](README.md)
{% endhint %}
