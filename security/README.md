# Security Architecture

## Multi-Layer Security

BethNa implements **defense in depth** with multiple security layers to protect user funds and data.

## Security Layers

### Layer 1: Smart Contract Security

**Non-Custodial Design**
- Zero fund storage in contracts
- Atomic transactions only
- User approval required for each trade

**Access Control**
- Whitelisted agent addresses
- Owner-only administrative functions
- Time-locked governance changes

**Code Quality**
- OpenZeppelin battle-tested contracts
- Comprehensive unit tests (95% coverage)
- Integration tests with Foundry

**Emergency Measures**
- Pausable functionality
- Upgradeable parameters (not core logic)
- Circuit breakers for abnormal activity

### Layer 2: Backend Security

**API Security**
- JWT authentication
- Rate limiting (100-10,000 req/min by tier)
- DDoS protection (Cloudflare)
- Input validation (Pydantic)

**Database Security**
- Encrypted at rest (AES-256)
- Encrypted in transit (TLS 1.3)
- Regular backups (hourly snapshots)
- Role-based access control

**Secret Management**
- Private keys in HSM (Hardware Security Module)
- API keys in encrypted vault (HashiCorp Vault)
- No secrets in code or logs

### Layer 3: Oracle Security

**Price Feed Integrity**
- Pyth Network (decentralized oracle)
- Multiple data sources
- Outlier detection
- Fallback oracles (Chainlink)

**Slippage Protection**
- Hard-coded maximum: 10%
- Pre-execution price quotes
- Post-execution verification

### Layer 4: Frontend Security

**Wallet Security**
- No private key handling
- Wallet adapter pattern (WalletConnect, MetaMask)
- Transaction simulation before signing
- Clear approval messages

**XSS Protection**
- Content Security Policy headers
- Input sanitization
- React auto-escaping

**HTTPS Only**
- TLS 1.3 enforcement
- HSTS headers
- Certificate pinning

---

## Audit Status

| Component | Status | Details |
|-----------|--------|---------|
| **SentientTrader.sol** | Internal | Reviewed January 2026 |
| **Backend API** | Internal | Penetration tested |
| **External Smart Contract Audit** | Q2 2026 | [Auditor TBD] |
| **Bug Bounty Program** | Q2 2026 | Up to $50,000 |

---

## Incident Response

### Response Team
- On-call engineer (24/7)
- Security lead
- DAO multisig signers (for contract actions)

### Escalation Process
1. **Detection**: Automated monitoring alerts
2. **Assessment**: Severity classification (Critical/High/Medium/Low)
3. **Containment**: Pause contracts if needed
4. **Resolution**: Deploy fix
5. **Communication**: Notify users via Discord, Twitter, email
6. **Post-Mortem**: Public report within 7 days

---

{% hint style="warning" %}
**Report Security Issues**: security@bethna.ai (PGP key available)
{% endhint %}
