# Trust Assumptions

## What Users Must Trust

Despite non-custodial design, users still trust certain components:

## 1. Agent Recommendations

**Trust Required**: AI agents provide accurate analysis.

**Risks**:
- Inaccurate signals leading to losses
- Model drift (performance degradation over time)
- Black swan events (AI trained on normal markets)

**Mitigations**:
- Track record transparency
- Confidence scores shown
- Users can reject recommendations
- Continuous model retraining

## 2. Smart Contract Security

**Trust Required**: No exploitable bugs in SentientTrader.sol.

**Risks**:
- Contract vulnerability
- Reentrancy attacks
- Access control bypass

**Mitigations**:
- OpenZeppelin contracts (battle-tested)
- Internal + external audits
- Bug bounty program
- Emergency pause capability

## 3. Oracle Integrity

**Trust Required**: Price feeds are accurate and not manipulated.

**Risks**:
- Oracle manipulation
- Stale prices
- Flash loan attacks on oracle sources

**Mitigations**:
- Decentralized oracles (Pyth)
- Multiple data sources
- Outlier detection
- Slippage protection (10% max)

## 4. Backend Availability

**Trust Required**: Backend services remain operational.

**Risks**:
- Server downtime
- API unavailability
- Database failures

**Mitigations**:
- 99.9% uptime SLA
- Redundant infrastructure
- Auto-scaling
- Real-time monitoring

## 5. Authorized Trader Integrity

**Trust Required**: Whitelisted agent addresses won't act maliciously.

**Risks**:
- Compromised backend service
- Malicious insider
- Key theft

**Mitigations**:
- Multi-signature for auth changes
- Rate limiting on agent actions
- Anomaly detection
- Regular key rotation
- Phase 2: Session keys (user-defined limits)

---

## Trust Minimization Roadmap

### Phase 1 (Current)
- Centralized backend, whitelisted agents
- **Trust Level**: Medium

### Phase 2 (Q3 2026)
- Session keys (user-defined limits)
- DAO governance over auth changes
- **Trust Level**: Low-Medium

### Phase 3 (2027)
- Fully on-chain agents (smart contract-based)
- Zero backend dependency for trading
- **Trust Level**: Minimal

---

## User Safety Checklist

**Do**:
- Start with small positions
- Review agent reasoning before approving
- Set stop-losses
- Diversify across strategies
- Revoke approvals if inactive

**Don't**:
- Invest more than you can afford to lose
- Blindly trust AI recommendations
- Ignore risk warnings
- Share wallet private keys
- Use untested strategies with full portfolio

---

{% hint style="danger" %}
**Remember**: BethNa is non-custodial for **funds**, but you still trust agents for **strategy**. Always do your own research.
{% endhint %}
