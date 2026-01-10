# Phase 2: Decentralization (Q3 2026)

{% hint style="info" %}
**Status**: Planned for Q3 2026 (6 months after Phase 1)
{% endhint %}

## Mission

Transform BethNa from a centralized service into a community-governed, permissionless protocol while introducing advanced trading features.

## Key Goals

1. **Decentralize Control**: Launch DAO governance
2. **Improve UX**: Implement Account Abstraction (EIP-4337)
3. **Expand Access**: Strategy vaults for smaller portfolios
4. **Advanced Strategies**: Agent Delta (Hedging)

---

## Deliverables

### 1. Account Abstraction (EIP-4337)

**Problem**: Current flow requires multiple wallet approvals per trade, creating friction.

**Solution**: Session Keys allow users to pre-approve multiple trades within limits.

#### Implementation

```solidity
// SessionKey contract
struct Session {
    address user;
    uint256 maxTradesPerDay;
    uint256 maxAmountPerTrade;
    uint256 validUntil;
    uint256 tradesExecuted;
}

function createSession(
    uint256 maxTrades,
    uint256 maxAmount,
    uint256 duration
) external {
    sessions[msg.sender] = Session({
        user: msg.sender,
        maxTradesPerDay: maxTrades,
        maxAmountPerTrade: maxAmount,
        validUntil: block.timestamp + duration,
        tradesExecuted: 0
    });
}
```

**User Experience**:
```
Traditional (Phase 1):
User approves USDC → Approve trade → Wallet signature
(3 clicks per trade)

Phase 2 (Session Keys):
User creates session (once per week) → Trades execute automatically
(1 click per week, 0 clicks per trade)
```

**Safety**:
- Daily trade limits
- Maximum amount per trade
- Time-bound sessions (auto-expire)
- Revocable anytime

---

### 2. Strategy Vaults

**Problem**: Small portfolios (<$5K) face high gas costs relative to trade size.

**Solution**: Pooled vaults where users deposit USDC for collective AI management.

#### Vault Types

**Conservative Vault**
- Target: 15-25% APY
- Strategy: Covered calls, cash-secured puts
- Risk: Low (capital preservation focus)

**Balanced Vault**
- Target: 30-50% APY
- Strategy: Mix of directional and neutral plays
- Risk: Medium

**Aggressive Vault**
- Target: 50-100% APY
- Strategy: Leveraged options, high IV plays
- Risk: High (potential for 30%+ drawdowns)

#### Vault Mechanics

```solidity
// Vault deposit
function deposit(uint256 amount) external {
    USDC.transferFrom(msg.sender, address(this), amount);
    uint256 shares = amount * totalShares / totalAssets;
    userShares[msg.sender] += shares;
}

// Vault withdrawal
function withdraw(uint256 shares) external {
    uint256 amount = shares * totalAssets / totalShares;
    userShares[msg.sender] -= shares;
    USDC.transfer(msg.sender, amount);
}
```

**User Benefits**:
- Lower gas costs (shared across all depositors)
- Professional management without staking $BETH
- Instant liquidity (withdraw anytime)

---

### 3. Agent Delta (The Hedger)

**Core Function**: Execute delta-neutral and volatility-harvesting strategies.

#### Strategies

**Long Straddle**
- Buy ATM call + ATM put
- Profit from volatility, not direction
- Recommended when IV < 40th percentile

**Delta-Neutral Hedging**
- Long call + short perp
- Net delta = 0
- Profit from IV expansion

**Iron Condor**
- Sell OTM call/put, buy further OTM call/put
- Profit from range-bound markets
- Limited risk, limited reward

#### Integration with dYdX

```python
# Agent Delta hedging example
async def hedge_position(position):
    # Calculate net delta
    delta = position.greeks.delta
    
    if abs(delta) > 0.3:  # Hedge threshold
        # Execute perp hedge on dYdX
        hedge_size = -delta  # Opposite direction
        await dydx_client.open_perp(
            asset=position.asset,
            size=hedge_size,
            side="SHORT" if delta > 0 else "LONG"
        )
```

**User Benefits**:
- Market-neutral strategies (profit regardless of direction)
- Reduced portfolio volatility
- Capture volatility premium

---

### 4. Governance Launch

**DAO Structure**:
- **Proposal Threshold**: 100,000 $BETH
- **Quorum**: 10% of circulating supply
- **Voting Period**: 5 days
- **Execution Delay**: 2 days (timelock)

**Governable Parameters**:
- Agent strategy additions
- Fee structure
- Treasury spending
- Risk parameters (max slippage, position limits)
- Protocol upgrades

**Example Governance Flow**:

```
1. User proposes: "Add BTC Straddle Strategy"
   ↓
2. Community discusses (7 days on forum)
   ↓
3. Formal proposal (requires 100K $BETH deposit)
   ↓
4. Snapshot voting (5 days)
   ↓
5. If >51% approval + 10% quorum:
   ↓
6. 2-day timelock
   ↓
7. Automatic execution
```

---

### 5. Advanced Features

**Custom Alerts**
- Telegram bot integration
- Discord webhooks
- SMS alerts (for critical events)
- Email summaries (daily/weekly)

**Backtesting Engine**
- Test strategies on 3 years of historical data
- Custom parameters (entry/exit rules)
- Performance metrics (Sharpe, max drawdown, win rate)
- Compare against buy-and-hold

**API V2**
- RESTful endpoints for all data
- WebSocket for real-time updates
- Rate limits: 1,000 req/min (Gold+)
- Webhook support for events

**Portfolio Simulator**
- "What-if" scenario testing
- Adjust Greeks and see impact
- Stress testing (simulate 20% dump)
- Monte Carlo simulations

---

## Timeline

| Milestone | Target Date | Status |
|-----------|-------------|--------|
| **EIP-4337 Research** | Apr 2026 | Planned |
| **Vault Contracts Dev** | May 2026 | Planned |
| **Agent Delta Alpha** | Jun 2026 | Planned |
| **DAO Launch** | Jul 2026 | Planned |
| **Public Beta** | Aug 2026 | Planned |
| **Phase 2 Mainnet** | Sep 2026 | Planned |

---

## Success Metrics

| Metric | Target |
|--------|--------|
| **Vault TVL** | $10M+ |
| **DAO Proposals** | 20+ in first quarter |
| **Agent Delta Win Rate** | >65% |
| **Session Key Adoption** | 40% of active users |
| **User Satisfaction** | 4.5/5 stars |

---

{% hint style="success" %}
**What's next?** Explore [Phase 3: Sentient Network](phase-3.md)
{% endhint %}
