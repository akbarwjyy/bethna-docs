# Multi-Layer Protection

## BethNa's 4-Layer Security Model

## Layer 1: Smart Contract Security

### Non-Custodial Architecture
- Funds never stored in contracts
- Atomic execution (all-or-nothing)
- Immediate token return to user wallet

### Code Auditing
- Internal security review
- External audit planned (Q2 2026)
- Bug bounty program ($50K max)

### Access Control
```solidity
modifier onlyAuthorized() {
    require(authorizedTraders[msg.sender], "Not authorized");
    _;
}
```

### Emergency Pause
```solidity
function pause() external onlyOwner {
    _pause();
    emit EmergencyPause(block.timestamp);
}
```

## Layer 2: Oracle & Data Integrity

### Price Feeds
- **Primary**: Pyth Network (decentralized)
- **Fallback**: Chainlink oracles
- **Verification**: Cross-reference multiple sources

### Slippage Protection
- Maximum 10% enforced on-chain
- Pre-trade price quotes
- Post-trade verification

## Layer 3: Backend Infrastructure

### API Security
- JWT authentication
- Rate limiting by tier
- DDoS protection (Cloudflare)

### Data Encryption
- At rest: AES-256
- In transit: TLS 1.3
- Database: Encrypted columns for sensitive data

### Secret Management
- Private keys in HSM
- API keys in HashiCorp Vault
- Zero secrets in code

## Layer 4: User-Facing Security

### Wallet Integration
- No private key handling
- Transaction simulation
- Clear approval messages

### Frontend Hardening
- Content Security Policy
- XSS protection
- HTTPS only (HSTS)

---

{% hint style="success" %}
**Related**: [Trust Assumptions](trust-assumptions.md)
{% endhint %}
