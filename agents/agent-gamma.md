# Agent Gamma - The Risk Manager

## Role Overview

**Agent Gamma** is the guardian of your portfolio. It monitors open positions 24/7, tracks portfolio health, and automatically executes protective measures.

## Core Function

**Portfolio health monitoring and exit discipline.**

## Responsibilities

### 1. Position Monitoring

Agent Gamma tracks every open position in real-time:

| Metric | Update Frequency | Action Threshold |
|--------|------------------|------------------|
| **P&L** | Real-time | ±20% from entry |
| **Greeks** | Every 5 minutes | Delta shift >10% |
| **Theta Decay** | Hourly | Time value <10% |
| **Implied Volatility** | Every 15 minutes | IV change >20% |

### 2. Risk Metrics Calculation

Agent Gamma calculates portfolio-level risk:

#### Value at Risk (VaR)
Maximum expected loss over 24 hours with 95% confidence:
```python
var_95 = portfolio_value * volatility * 1.65  # 1.65 = 95th percentile
```

#### Portfolio Delta
Net directional exposure:
```python
portfolio_delta = sum(position.delta * position.size for position in positions)
```

#### Theta Burn Rate
Daily time decay:
```python
daily_theta = sum(position.theta * position.size for position in positions)
```

### 3. Automated Exit Logic

Agent Gamma enforces discipline through automated exits:

#### Stop-Loss Exits
```python
if position.pnl_percent < -20%:
    trigger_exit("Stop-loss hit")
```

#### Take-Profit Exits
```python
if position.pnl_percent > target_profit:
    trigger_exit("Profit target reached")
```

#### Theta Decay Exits
```python
if position.days_to_expiry < 2 and position.pnl_percent < 0:
    trigger_exit("Preventing total theta decay")
```

## Monitoring Dashboard

Agent Gamma provides a unified portfolio view:

### Portfolio Health Score

Calculated based on:
- **Diversification**: Number of positions across assets
- **Risk Exposure**: VaR as % of portfolio
- **Profit Factor**: Ratio of winning to losing positions
- **Sharpe Ratio**: Risk-adjusted returns

**Score Formula**:
```python
health_score = (
    diversification_score * 0.25 +
    risk_exposure_score * 0.35 +
    profit_factor_score * 0.20 +
    sharpe_ratio_score * 0.20
)
```

**Interpretation**:
- **90-100**: Excellent (well-balanced, healthy risk)
- **70-89**: Good (minor improvements needed)
- **50-69**: Moderate (significant risk or concentration)
- **<50**: Poor (urgent action required)

## Alert System

Agent Gamma sends alerts via multiple channels:

### Alert Types

| Alert Level | Condition | Example |
|-------------|-----------|---------|
| **INFO** | Position updates | "Your ETH Call is now up 15%" |
| **WARNING** | Risk threshold reached | "Portfolio delta is 0.65 (high directional exposure)" |
| **CRITICAL** | Immediate action needed | "Stop-loss triggered on BTC Put. Closing position." |

### Alert Delivery

- **In-App Notifications**: Real-time dashboard alerts
- **Email**: Daily portfolio summaries
- **Telegram/Discord** *(Phase 2)*: Instant alerts
- **SMS** *(Phase 3)*: Critical alerts only

## Risk Management Strategies

### Strategy 1: Position Sizing

Agent Gamma enforces maximum position sizes:

```python
max_position_size = portfolio_value * 0.20  # Max 20% per position

if proposed_trade_size > max_position_size:
    return ERROR_POSITION_TOO_LARGE
```

### Strategy 2: Correlation Management

Prevents over-concentration in correlated assets:

```python
if (num_eth_positions >= 3 and new_trade.asset == "ETH"):
    warn_user("You already have 3 ETH positions. Consider diversifying.")
```

### Strategy 3: Volatility Scaling

Adjusts position sizes based on market volatility:

```python
if current_volatility > historical_avg * 1.5:
    recommended_size = base_size * 0.66  # Reduce size in high-vol environments
```

### Strategy 4: Time Decay Management

Warns users about approaching expiry:

```python
if days_to_expiry <= 3:
    if position.pnl_percent > 0:
        suggest_exit("Take profits before theta accelerates")
    else:
        suggest_exit("Cut losses before total decay")
```

## Advanced Features

### Greek Hedging *(Phase 2)*

Agent Gamma will suggest hedges to neutralize Greek exposure:

#### Example: High Delta Exposure
```
Your portfolio delta: +0.85 (very bullish)

Suggested hedge:
- Short 0.85 ETH on dYdX perpetual
- This makes your portfolio delta-neutral
- You profit from volatility, not direction
```

#### Example: High Vega Exposure
```
Your portfolio vega: +15 (exposed to IV drops)

Suggested hedge:
- Sell a covered call at higher strike
- Reduces net vega exposure
- Generates premium income
```

### Portfolio Rebalancing

Agent Gamma suggests rebalancing when:
- Single position exceeds 30% of portfolio
- Portfolio delta exceeds ±0.5
- Sharpe ratio drops below 1.0

**Example Rebalancing Alert**:
```
Portfolio Rebalancing Recommended

Current allocation:
- ETH Calls: 45% (too concentrated)
- BTC Puts: 30%
- Stablecoins: 25%

Suggested allocation:
- ETH Calls: 30% (close 1/3 of positions)
- BTC Puts: 25%
- New BTC Calls: 15% (diversify)
- Stablecoins: 30% (increase cash buffer)
```

## Performance Tracking

Agent Gamma generates performance reports:

### Daily Report
- P&L for the day
- Winning vs. losing positions
- Best-performing strategy
- Risk exposure summary

### Weekly Report
- 7-day performance vs. benchmarks (BTC, ETH)
- Sharpe ratio
- Maximum drawdown
- Win rate by strategy type

### Monthly Report
- Comprehensive portfolio analysis
- Tax reporting (realized gains/losses)
- Strategy effectiveness breakdown
- Recommendations for improvement

## Performance Metrics

| Metric | Target | Actual (Q1 2026) |
|--------|--------|------------------|
| **Alert Accuracy** | >90% | 92% |
| **False Positive Rate** | <5% | 3.8% |
| **Average Exit Quality** | Positive expectancy | +8% avg improvement vs. manual |
| **Downside Protection** | Limit losses to -20% | -18% max loss |

---

{% hint style="success" %}
**Next**: Learn about [Agent Delta - The Hedger](agent-delta.md) *(Coming in Phase 2)*
{% endhint %}
