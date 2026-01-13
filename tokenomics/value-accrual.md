# Value Accrual

## How $BETH Captures Protocol Value

$BETH is designed with **multiple value capture mechanisms** that align token holders with protocol success.

## Primary Value Drivers

### 1. Buyback & Burn (Deflationary)

**Mechanism**: 30% of protocol revenue is used to buy $BETH from the market and permanently burn it.

#### Revenue Calculation

```python
# Weekly Protocol Revenue
trading_volume = $50_000_000  # Example weekly volume
avg_profit_margin = 0.60  # 60% of trades are profitable
avg_fee_rate = 0.15  # 15% average fee (across all tiers)

profitable_volume = trading_volume * avg_profit_margin
protocol_revenue = profitable_volume * avg_fee_rate

# Example
profitable_volume = $50M * 0.60 = $30M
protocol_revenue = $30M * 0.15 = $4.5M per week
```

#### Buyback Execution

```python
# Buyback Calculation
weekly_revenue = $4_500_000
buyback_allocation = 0.30  # 30%
buyback_amount = weekly_revenue * buyback_allocation

# Example
buyback_amount = $4.5M * 0.30 = $1.35M per week

# Convert to $BETH
beth_price = $0.10  # Current market price
beth_to_burn = buyback_amount / beth_price

# Example
beth_to_burn = $1.35M / $0.10 = 13.5M $BETH burned per week
```

**Annual Impact**:
- Weekly burn: 13.5M $BETH
- Annual burn: **702M $BETH** (70.2% of initial supply)
- **Note**: Burn rate decreases as price increases

---

### 2. Staking Demand (Supply Reduction)

**Mechanism**: Users stake $BETH to access tier benefits, removing tokens from circulation.

#### Expected Staking Participation

| Tier | Users | Avg Stake | Total Staked |
|------|-------|-----------|--------------|
| Bronze | 5,000 | 10,000 | 50,000,000 |
| Silver | 2,000 | 50,000 | 100,000,000 |
| Gold | 500 | 100,000 | 50,000,000 |
| Platinum | 50 | 500,000 | 25,000,000 |
| **Total** | **7,550** | - | **225,000,000** |

**Circulating Supply Impact**:
- Total Supply: 1,000,000,000
- Staked: 225,000,000 (22.5%)
- Team/Investor Locked: 300,000,000 (30%)
- **Liquid Supply**: 475,000,000 (47.5%)

---

### 3. Utility Demand (Ongoing Buying Pressure)

Users must acquire $BETH to:
- Access premium features
- Participate in governance
- Pay for Agent Marketplace services *(Phase 3)*
- Insurance pool deposits *(Phase 3)*

**Monthly Demand Projection**:

```python
# New users joining
new_users_per_month = 1_000
avg_beth_per_new_user = 25_000  # Mix of tiers

monthly_demand_new_users = 1_000 * 25_000 = 25M $BETH

# Existing users upgrading tiers
upgrades_per_month = 200
avg_beth_per_upgrade = 50_000  # Silver → Gold

monthly_demand_upgrades = 200 * 50_000 = 10M $BETH

# Total
total_monthly_demand = 35M $BETH
```

At $0.10: **$3.5M buying pressure per month**.

---

### 4. Governance Premium

**Mechanism**: $BETH holders control protocol direction, creating governance premium.

**Historical Precedents**:
- **Curve (CRV)**: Governance controls emissions → 0.5-1x premium
- **Uniswap (UNI)**: Fee switch vote → 0.3x premium
- **Compound (COMP)**: Interest rate governance → 0.4x premium

**BethNa Governance Value**:
- Control agent strategies (high impact on user returns)
- Fee structure (direct revenue impact)
- Treasury allocation ($250M under management)

**Expected Premium**: 0.3-0.5x protocol value.

---

## Value Accrual Model

### Token Price Projection

```python
# Assumptions
protocol_tvl = $50_000_000  # Conservative target
annual_revenue = $100_000_000  # 2x TVL (realistic for trading protocols)
revenue_multiple = 5  # Crypto protocol average: 3-10x

# Fully Diluted Valuation (FDV)
fdv = annual_revenue * revenue_multiple
fdv = $100M * 5 = $500M

# Circulating Market Cap (assuming 50% circulating)
circulating_supply = 500_000_000 $BETH
market_cap = fdv * 0.50 = $250M

# Token Price
token_price = market_cap / circulating_supply
token_price = $250M / 500M = $0.50 per $BETH
```

**Scenarios**:

| Scenario | TVL | Revenue | FDV | Price |
|----------|-----|---------|-----|-------|
| **Bear** | $10M | $20M | $60M | $0.12 |
| **Base** | $50M | $100M | $500M | $0.50 |
| **Bull** | $200M | $400M | $2B | $2.00 |

---

## Comparison: $BETH vs. Competitors

| Protocol | Token | Utility | Buyback | Staking | Governance |
|----------|-------|---------|---------|---------|------------|
| **Ribbon** | RBN | None | No | Yes | Yes |
| **Lyra** | LYRA | Fee discount | No | Yes | Yes |
| **Opyn** | OPYN | None | No | No | Yes |
| **BethNa** | BETH | Multi-utility | Yes | Yes | Yes |

**BethNa Advantage**: Only protocol with **buyback + burn + multi-utility**.

---

## Deflationary Impact

### Long-Term Supply Projection

Assuming consistent buyback at $1M/week:

| Year | Burned | Remaining Supply | % Reduction |
|------|--------|------------------|-------------|
| **Year 0** | 0 | 1,000,000,000 | 0% |
| **Year 1** | 100,000,000 | 900,000,000 | 10% |
| **Year 2** | 180,000,000 | 820,000,000 | 18% |
| **Year 3** | 250,000,000 | 750,000,000 | 25% |
| **Year 5** | 380,000,000 | 620,000,000 | 38% |

**Note**: Burn rate in $ terms, not tokens. As price rises, fewer tokens are burned per dollar.

---

## Risk Factors

### 1. Revenue Dependency

If protocol revenue is lower than projected:
- Reduced buyback pressure
- Lower token price
- Less staking demand

**Mitigation**: Conservative projections, diversified revenue streams (fees, subscriptions, marketplace).

### 2. Sell Pressure from Unlocks

When team/investor tokens vest:
- Increased circulating supply
- Potential sell pressure

**Mitigation**: 
- Long vesting periods (2-4 years)
- Alignment: Team has incentive to grow protocol

### 3. Competitive Risk

If competitors offer better services:
- Users leave platform
- Revenue declines
- Token value drops

**Mitigation**: Continuous innovation, community feedback, open governance.

---

## Value Accrual Summary

$BETH accrues value through:

1. **Buyback & Burn**: Reduces supply (deflationary)
2. **Staking**: Removes tokens from circulation
3. **Utility**: Ongoing demand for features
4. **Governance**: Premium for protocol control
5. **Network Effects**: More users → more revenue → more buybacks

**Expected Result**: Token price correlated with protocol success.

---

{% hint style="success" %}
**Back to**: [Tokenomics Overview](README.md)
{% endhint %}
