# Data Flow

## How Information Moves Through BethNa

Understanding data flow is critical for transparency and debugging. This document maps the journey of data from user input to on-chain execution.

## 1. User Input Flow

### Conversational Trading

```
User: "I'm bullish on ETH, what should I do?"
  ↓
[Frontend] Capture input
  ↓
[API] POST /api/chat
  ↓
[LangChain] Intent classification
  ↓
Intent: "REQUEST_TRADE_RECOMMENDATION"
Asset: "ETH"
Sentiment: "BULLISH"
```

### Direct Strategy Selection

```
User clicks: "Buy ETH Call"
  ↓
[Frontend] Open strategy modal
  ↓
[API] GET /api/strategies/eth-call
  ↓
Returns: Available strikes, expiries, premiums
```

## 2. Market Data Aggregation

Agent Alpha pulls data from multiple sources simultaneously:

```python
# Parallel data fetching
async def fetch_market_data(asset: str):
    price_data = await pyth_client.get_price(asset)
    options_data = await thetanuts_client.get_options(asset)
    funding_data = await dydx_client.get_funding(asset)
    sentiment_data = await twitter_client.analyze_sentiment(asset)
    
    return {
        "price": price_data,
        "options": options_data,
        "funding": funding_data,
        "sentiment": sentiment_data
    }
```

### Data Sources & Update Frequencies

| Source | Data Type | Update Frequency |
|--------|-----------|------------------|
| **Pyth Network** | Spot prices | Sub-second |
| **Thetanuts API** | Options prices, IV | 5 seconds |
| **dYdX API** | Funding rates | 1 minute |
| **Twitter API** | Sentiment scores | 15 minutes |
| **On-Chain Events** | Trade executions | Real-time (blocks) |

## 3. Signal Generation Flow

```
Market Data
  ↓
[Agent Alpha] Feature Engineering
  ↓
Features: [momentum, volatility, sentiment, funding]
  ↓
[ML Model] Random Forest Classifier
  ↓
Prediction: "BULLISH with 0.85 confidence"
  ↓
[Strategy Matcher] Map to options strategy
  ↓
Output: {
  signal: "LONG_CALL",
  strike: 3200,
  expiry: "7_DAYS",
  confidence: 0.85
}
```

## 4. User Approval Flow

```
[Backend] Generate recommendation
  ↓
[API] POST /api/recommendations
  ↓
[Frontend] Display to user
  ↓
User reviews:
  - Strategy explanation
  - Risk/reward metrics
  - Historical performance
  ↓
User clicks "Approve"
  ↓
[Frontend] Trigger wallet signature
  ↓
[Wallet] User signs approval
  ↓
Signature sent to backend
```

## 5. Execution Flow

### Pre-Execution Checks

```python
# Agent Beta validation sequence
async def validate_trade(trade_params):
    # 1. Check liquidity
    liquidity = await thetanuts.get_liquidity(
        asset=trade_params.asset,
        strike=trade_params.strike
    )
    if liquidity < trade_params.amount:
        raise InsufficientLiquidity
    
    # 2. Check gas price
    gas_price = await base_network.get_gas_price()
    if gas_price > MAX_GAS_THRESHOLD:
        raise GasTooHigh
    
    # 3. Verify contract state
    is_paused = await sentient_trader.paused()
    if is_paused:
        raise ContractPaused
    
    # 4. Estimate slippage
    estimated_slippage = calculate_slippage(
        liquidity, trade_params.amount
    )
    if estimated_slippage > MAX_SLIPPAGE:
        raise SlippageTooHigh
    
    return True
```

### On-Chain Execution

```
[Agent Beta] Call SentientTrader.buyOption()
  ↓
[SentientTrader.sol] transferFrom(user, USDC)
  ↓
[SentientTrader.sol] approve(thetanutsRouter, USDC)
  ↓
[Thetanuts Router] swap(USDC → Option Token)
  ↓
[Thetanuts Router] return Option Token
  ↓
[SentientTrader.sol] transfer(user, Option Token)
  ↓
[SentientTrader.sol] emit OptionPurchased event
  ↓
Transaction confirmed (block inclusion)
```

## 6. Post-Execution Flow

### Event Listening

```python
# Backend listens for on-chain events
@event_listener("OptionPurchased")
async def on_option_purchased(event):
    position_id = event.transaction_hash
    user_address = event.args.user
    option_token = event.args.optionToken
    amount = event.args.amountOut
    
    # Store in database
    await db.positions.create({
        "id": position_id,
        "user": user_address,
        "option_token": option_token,
        "amount": amount,
        "entry_price": calculate_entry_price(event),
        "status": "ACTIVE"
    })
    
    # Notify Agent Gamma
    await agent_gamma.start_monitoring(position_id)
```

### Agent Gamma Monitoring

```python
# Continuous position monitoring
async def monitor_position(position_id):
    while True:
        position = await db.positions.get(position_id)
        current_price = await pyth.get_option_price(
            position.option_token
        )
        
        # Calculate P&L
        pnl = calculate_pnl(position, current_price)
        
        # Check exit conditions
        if pnl < STOP_LOSS_THRESHOLD:
            await trigger_exit(position, "STOP_LOSS")
        elif pnl > TAKE_PROFIT_THRESHOLD:
            await trigger_exit(position, "TAKE_PROFIT")
        
        # Update Greeks
        greeks = await calculate_greeks(position)
        await db.positions.update(position_id, {
            "pnl": pnl,
            "greeks": greeks
        })
        
        await asyncio.sleep(60)  # Check every minute
```

## 7. User Notification Flow

```
[Agent Gamma] Detects exit condition
  ↓
[Notification Service] Generate alert
  ↓
[WebSocket] Push to frontend (if online)
  ↓
[Email Service] Send email (always)
  ↓
[Database] Log notification
  ↓
User sees alert on dashboard
```

## Data Pipeline Architecture

### Real-Time Pipeline

```
Pyth Oracle → WebSocket → Agent Alpha → Signal Queue → Agent Beta → Blockchain
                                                          ↓
                                                    Agent Gamma ← WebSocket ← Blockchain Events
```

### Batch Pipeline (Analytics)

```
Blockchain Events → Event Indexer → PostgreSQL → BigQuery → Grafana Dashboards
                                         ↓
                                   User Dashboard
```

## Data Retention

| Data Type | Retention | Storage |
|-----------|-----------|---------|
| **Trade History** | Permanent | On-chain (events) |
| **User Profiles** | Account lifetime | Supabase |
| **Market Data** | 90 days | TimescaleDB |
| **ML Training Data** | 3 years | S3 |
| **Logs** | 30 days | Loki |

---

{% hint style="success" %}
**Next**: Explore [Application Layer](application-layer.md) details
{% endhint %}
