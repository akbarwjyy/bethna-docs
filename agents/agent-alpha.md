# Agent Alpha - The Analyst

## Role Overview

**Agent Alpha** is the brain of the BethNa system. It ingests market data, identifies patterns, and generates actionable trading signals.

## Core Function

**Market perception and signal generation.**

## Inputs

Agent Alpha aggregates data from multiple sources:

### 1. Real-Time Price Feeds
- **Pyth Network**: Sub-second ETH, BTC, and altcoin prices
- **Thetanuts AMM**: Option prices, implied volatility
- **DEX Aggregators**: Spot prices across Uniswap, Curve, etc.

### 2. On-Chain Data
- **Volume Analysis**: Trading volume trends on Thetanuts
- **Open Interest**: How many options are outstanding
- **Liquidity Depth**: Available liquidity at different strikes

### 3. Funding Rates
- **Perp Markets**: dYdX, GMX, Perpetual Protocol
- **Signal**: Negative funding = potential long squeeze
- **Signal**: Positive funding = potential short squeeze

### 4. Social Sentiment
- **Twitter**: Crypto influencer sentiment analysis
- **Discord/Telegram**: Community mood indicators
- **Reddit**: r/CryptoCurrency, r/ethfinance sentiment

### 5. Technical Indicators
- **RSI**: Relative Strength Index (overbought/oversold)
- **MACD**: Moving Average Convergence Divergence
- **Bollinger Bands**: Volatility compression detection
- **Volume Profile**: Support/resistance levels

## Signal Generation Process

### Step 1: Data Normalization
```python
# Pseudocode
price_data = fetch_pyth_prices()
funding_data = fetch_perp_funding()
sentiment_data = analyze_social_sentiment()
volatility_data = fetch_thetanuts_iv()

normalized_data = {
    "price_trend": calculate_trend(price_data),
    "funding_bias": normalize_funding(funding_data),
    "sentiment_score": sentiment_data.aggregate(),
    "volatility_percentile": calculate_iv_percentile(volatility_data)
}
```

### Step 2: Pattern Recognition
```python
if (normalized_data["price_trend"] == "BULLISH" and
    normalized_data["funding_bias"] < -0.01 and
    normalized_data["volatility_percentile"] < 50):
    
    pattern = "BULLISH_DIVERGENCE_LOW_IV"
    confidence = 0.85
```

### Step 3: Strategy Matching
```python
strategy_db = {
    "BULLISH_DIVERGENCE_LOW_IV": {
        "recommended": "LONG_CALL",
        "strike_selection": "OTM_30_DELTA",
        "expiry": "7_DAYS",
        "historical_win_rate": 0.72
    }
}

recommended_strategy = strategy_db[pattern]
```

### Step 4: Output Generation
```json
{
  "signal_type": "LONG_CALL",
  "asset": "ETH",
  "confidence": 0.85,
  "reasoning": "Bullish divergence detected on 4H timeframe. Funding rates negative (long squeeze potential). IV percentile: 35th (underpriced volatility).",
  "recommended_strike": 3200,
  "expiry": "7_DAYS",
  "expected_return": "2.5x if ETH reaches $3500",
  "max_loss": "100% of premium ($240 for 0.05 ETH position)",
  "historical_performance": {
    "win_rate": 0.72,
    "avg_return": 1.8,
    "max_drawdown": 0.45
  }
}
```

## Output

Agent Alpha produces **semantic signals** like:

> _"Bullish Divergence on ETH. Volatility compressing. Recommend Long Call (Strike: $3200, 7-day expiry). Confidence: 85%. Historical win rate: 72%."_

## Advanced Features

### Multi-Timeframe Analysis

Agent Alpha analyzes price action across multiple timeframes:

| Timeframe | Use Case |
|-----------|----------|
| **1-minute** | Micro-scalping signals |
| **15-minute** | Intraday momentum |
| **1-hour** | Short-term trends |
| **4-hour** | Primary signal generation |
| **Daily** | Macro trend confirmation |

**Signal Validation**: A bullish 4H signal is only acted upon if the daily trend is also bullish (or neutral).

### Volatility Regime Detection

Agent Alpha classifies market conditions:

| Regime | Characteristics | Recommended Strategies |
|--------|----------------|------------------------|
| **Low Vol** | IV < 30th percentile | Long options (straddles/calls) |
| **Normal Vol** | IV 30th-70th percentile | Directional plays |
| **High Vol** | IV > 70th percentile | Short premium (covered calls) |
| **Volatility Spike** | IV > 90th percentile | Wait for mean reversion |

### Machine Learning Integration

Agent Alpha uses a **Random Forest Classifier** trained on 3 years of data:

**Features**:
- Price momentum (7, 14, 30 days)
- Funding rate trends
- IV percentile
- Social sentiment score
- On-chain volume

**Prediction**: Probability of +10% move in 7 days

**Confidence Calibration**: Model outputs are calibrated to match historical accuracy (prevents overconfidence).

## Performance Metrics

Agent Alpha's performance is tracked:

| Metric | Target | Actual (Q1 2026) |
|--------|--------|------------------|
| **Signal Accuracy** | >70% | 73% |
| **False Positive Rate** | <20% | 18% |
| **Average Confidence** | 0.75-0.85 | 0.79 |
| **Sharpe Ratio** | >1.5 | 1.7 |

---

{% hint style="success" %}
**Next**: Learn about [Agent Beta - The Executor](agent-beta.md)
{% endhint %}
