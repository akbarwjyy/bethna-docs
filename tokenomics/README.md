# $BETH Token

## The Bethna Governance Token

The **Bethna Governance Token ($BETH)** is designed to align the incentives of users, the DAO, and the AI protocol.

## Token Overview

| Attribute | Value |
|-----------|-------|
| **Token Name** | Bethna Token |
| **Token Symbol** | $BETH |
| **Token Standard** | ERC-20 |
| **Network** | Base (Ethereum L2) |
| **Total Supply** | 1,000,000,000 $BETH |
| **Initial Circulating** | 150,000,000 $BETH (15%) |

## Token Distribution

```
Total Supply: 1,000,000,000 $BETH

├─ Community & Users: 40% (400,000,000)
│  ├─ Airdrop: 10% (100,000,000)
│  ├─ Trading Rewards: 20% (200,000,000)
│  └─ Liquidity Mining: 10% (100,000,000)
│
├─ Team & Advisors: 20% (200,000,000)
│  ├─ Vesting: 4-year linear, 1-year cliff
│  └─ Allocation: Core team, advisors, early contributors
│
├─ Protocol Treasury: 25% (250,000,000)
│  ├─ Development Fund: 15% (150,000,000)
│  └─ Marketing & Partnerships: 10% (100,000,000)
│
├─ Investors: 10% (100,000,000)
│  └─ Vesting: 2-year linear, 6-month cliff
│
└─ Liquidity Provision: 5% (50,000,000)
   └─ Initial DEX liquidity (Uniswap V3)
```

## Vesting Schedule

### Team & Advisors
- **Total**: 200,000,000 $BETH
- **Cliff**: 1 year (0 tokens released)
- **Vesting**: Linear over 4 years after cliff
- **Monthly Release**: ~4,166,666 $BETH (after cliff)

### Investors
- **Total**: 100,000,000 $BETH
- **Cliff**: 6 months
- **Vesting**: Linear over 2 years after cliff
- **Monthly Release**: ~4,166,666 $BETH (after cliff)

### Community Trading Rewards
- **Total**: 200,000,000 $BETH
- **Distribution**: 4 years (50M/year)
- **Allocation**: Based on trading volume and profitability

## Token Utility

$BETH serves four primary functions:

### 1. Governance

Token holders vote on:
- New Agent strategies (e.g., "Add BTC Straddle Strategy")
- Risk parameters (max slippage, position limits)
- Fee structure adjustments
- Protocol upgrades
- Treasury allocation

**Voting Power**: 1 $BETH = 1 vote

**Governance Process**:
1. Proposal submitted (requires 100,000 $BETH)
2. 7-day discussion period
3. 5-day voting period
4. Execution if >51% approval + 10% quorum

### 2. Fee Discount

Staking $BETH reduces performance fees:

| Tier | $BETH Staked | Performance Fee |
|------|--------------|-----------------|
| **Free** | 0 | 20% |
| **Bronze** | 10,000 | 15% |
| **Silver** | 50,000 | 12% |
| **Gold** | 100,000 | 10% |
| **Platinum** | 500,000 | 5% |

**Performance Fee**: Charged only on profitable trades.

**Example**:
- Profit: $1,000
- Free tier (20% fee): $200 to protocol, $800 to user
- Gold tier (10% fee): $100 to protocol, $900 to user

### 3. Pro Access

Unlock advanced features by holding $BETH:

| Feature | Required $BETH |
|---------|----------------|
| **Agent Delta (Hedging)** | 25,000 |
| **Agent Gamma (Advanced Risk)** | 10,000 |
| **Custom Strategy Builder** | 100,000 |
| **API Access** | 50,000 |
| **Priority Support** | 5,000 |

### 4. Revenue Share & Buyback

A portion of protocol revenue is used to buy back and burn $BETH:

**Revenue Allocation**:
- **30%**: Buyback & Burn
- **40%**: Protocol Treasury (development, security)
- **30%**: Team & Operations

**Buyback Mechanism**:
```
Weekly Protocol Revenue (in USDC)
  ↓
30% allocated to buyback
  ↓
Market buy $BETH on Uniswap
  ↓
Burn tokens (send to 0x000...dead)
  ↓
Reduce circulating supply (deflationary)
```

**Transparency**: All buybacks are on-chain and publicly auditable.

---

{% hint style="success" %}
**Explore**: [Token Utility Details](utility.md) | [Tiered Access](tiered-access.md)
{% endhint %}
