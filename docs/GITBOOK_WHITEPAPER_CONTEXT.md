# GitBook Whitepaper Project Context

> Dokumen ini berisi konteks dan instruksi lengkap untuk membuat repository GitBook Whitepaper baru untuk **BethNa AI Trader**.

---

## Project Info

| Field | Value |
|-------|-------|
| **Project Name** | BethNa AI Trader Whitepaper |
| **Repository Name** | `bethna-docs` |
| **GitBook Type** | Documentation Site |
| **Language** | English (primary), Indonesia (sections) |
| **Target Audience** | Investors, developers, users |

---

## Repository Structure

```
bethna-docs/
├── .gitbook.yaml              # GitBook configuration
├── README.md                  # Cover/landing page
├── SUMMARY.md                 # Table of contents (REQUIRED)
│
├── introduction/
│   ├── README.md              # Executive Summary
│   ├── mission.md             # Mission & Vision
│   └── key-innovations.md     # Key innovations list
│
├── problem/
│   ├── README.md              # Problem Statement overview
│   ├── defi-gap.md            # DeFi derivatives gap
│   └── market-challenges.md   # Market statistics
│
├── solution/
│   ├── README.md              # BethNa Solution overview
│   ├── intelligence-layer.md  # How BethNa works
│   └── differentiators.md     # vs Traditional DeFi
│
├── architecture/
│   ├── README.md              # System architecture overview
│   ├── data-flow.md           # Data flow diagrams
│   └── application-layer.md   # Frontend/Backend stack
│
├── agents/
│   ├── README.md              # Swarm Agents overview
│   ├── agent-alpha.md         # The Analyst
│   ├── agent-beta.md          # The Executor
│   ├── agent-gamma.md         # The Risk Manager
│   └── agent-delta.md         # The Hedger
│
├── smart-contracts/
│   ├── README.md              # Smart contract overview
│   ├── sentient-trader.md     # SentientTrader.sol specs
│   └── non-custodial.md       # Non-custodial design
│
├── security/
│   ├── README.md              # Security architecture
│   ├── multi-layer.md         # 4-layer security
│   └── trust-assumptions.md   # Risks & mitigations
│
├── tokenomics/
│   ├── README.md              # $BETH overview
│   ├── utility.md             # Token utility
│   ├── tiered-access.md       # Tier system
│   └── value-accrual.md       # Buyback & burn
│
├── gamification/
│   ├── README.md              # Gamification overview
│   ├── xp-system.md           # XP & progression
│   └── agent-affinity.md      # Bonding system
│
├── technology/
│   ├── README.md              # Tech stack overview
│   ├── frontend.md            # Next.js, React, etc
│   ├── backend.md             # Python FastAPI
│   └── blockchain.md          # Solidity, Foundry
│
├── roadmap/
│   ├── README.md              # Roadmap overview
│   ├── phase-1.md             # Genesis (Q1 2026)
│   ├── phase-2.md             # Decentralization (Q3 2026)
│   └── phase-3.md             # Sentient Network (2027)
│
├── risks/
│   ├── README.md              # Risk disclosure
│   ├── financial.md           # Financial risks
│   └── technical.md           # Technical risks
│
└── resources/
    ├── glossary.md            # Terms & definitions
    ├── faq.md                 # Frequently asked questions
    └── links.md               # Official links
```

---

## SUMMARY.md Template

```markdown
# Table of Contents

## Introduction
* [Executive Summary](introduction/README.md)
* [Mission & Vision](introduction/mission.md)
* [Key Innovations](introduction/key-innovations.md)

## Problem Statement
* [Overview](problem/README.md)
* [The DeFi Derivatives Gap](problem/defi-gap.md)
* [Market Challenges](problem/market-challenges.md)

## The BethNa Solution
* [Overview](solution/README.md)
* [Intelligence Layer](solution/intelligence-layer.md)
* [Key Differentiators](solution/differentiators.md)

## Technical Architecture
* [System Overview](architecture/README.md)
* [Data Flow](architecture/data-flow.md)
* [Application Layer](architecture/application-layer.md)

## Sentient Agents
* [Swarm Agent System](agents/README.md)
* [Agent Alpha - The Analyst](agents/agent-alpha.md)
* [Agent Beta - The Executor](agents/agent-beta.md)
* [Agent Gamma - The Risk Manager](agents/agent-gamma.md)
* [Agent Delta - The Hedger](agents/agent-delta.md)

## Smart Contracts
* [Contract Infrastructure](smart-contracts/README.md)
* [SentientTrader.sol](smart-contracts/sentient-trader.md)
* [Non-Custodial Design](smart-contracts/non-custodial.md)

## Security
* [Security Architecture](security/README.md)
* [Multi-Layer Protection](security/multi-layer.md)
* [Trust Assumptions](security/trust-assumptions.md)

## Tokenomics
* [$BETH Token](tokenomics/README.md)
* [Token Utility](tokenomics/utility.md)
* [Tiered Access](tokenomics/tiered-access.md)
* [Value Accrual](tokenomics/value-accrual.md)

## Gamification
* [Strategy Overview](gamification/README.md)
* [XP & Progression](gamification/xp-system.md)
* [Agent Affinity](gamification/agent-affinity.md)

## Technology Stack
* [Overview](technology/README.md)
* [Frontend](technology/frontend.md)
* [Backend](technology/backend.md)
* [Blockchain](technology/blockchain.md)

## Roadmap
* [Development Phases](roadmap/README.md)
* [Phase 1: Genesis](roadmap/phase-1.md)
* [Phase 2: Decentralization](roadmap/phase-2.md)
* [Phase 3: Sentient Network](roadmap/phase-3.md)

## Risk Disclosure
* [Overview](risks/README.md)
* [Financial Risks](risks/financial.md)
* [Technical Risks](risks/technical.md)

## Resources
* [Glossary](resources/glossary.md)
* [FAQ](resources/faq.md)
* [Official Links](resources/links.md)
```

---

## .gitbook.yaml Configuration

```yaml
root: ./

structure:
  readme: README.md
  summary: SUMMARY.md

redirects:
  whitepaper: introduction/README.md
```

---

## README.md (Cover Page) Template

```markdown
---
description: Autonomous Swarm Agent System for DeFi Options Trading
---

# BethNa AI Trader

<figure><img src=".gitbook/assets/bethna-banner.png" alt="BethNa AI Banner"><figcaption></figcaption></figure>

## Welcome to BethNa AI

**BethNa AI Trader** is an autonomous swarm agent system designed to democratize DeFi options trading on the Base L2 Network.

### Quick Links

| Resource | Link |
|----------|------|
| Website | [bethna.ai](bethna-ai-trader.vercel.app) |
| GitHub | [github.com/lana-techn/base-hackathon](https://github.com/lana-techn/base-hackathon) |

### What is BethNa?

BethNa transforms complex derivatives trading into an accessible experience through:

- **Sentient Agents** - Four specialized AI agents working together
- **Non-Custodial** - Your funds, your control
- **Intelligent Trading** - AI-powered market analysis
- **Personal Advisor** - Financial guidance for everyone

---

**Version**: 1.1
**Last Updated**: January 2026

{% hint style="warning" %}
This is experimental software. Please read the [Risk Disclosure](risks/) before investing.
{% endhint %}
```

---

## Setup Instructions

### Step 1: Create GitHub Repository

```bash
# Create new repository
mkdir bethna-docs
cd bethna-docs
git init

# Create basic structure
mkdir -p introduction problem solution architecture agents
mkdir -p smart-contracts security tokenomics gamification
mkdir -p technology roadmap risks resources .gitbook/assets

# Create required files
touch README.md SUMMARY.md .gitbook.yaml
```

### Step 2: Connect to GitBook

1. Go to [app.gitbook.com](https://app.gitbook.com)
2. Create new **Space**
3. Choose **"Sync with Git"**
4. Connect your GitHub repository
5. Select branch: `main`

### Step 3: Configure Settings

In GitBook dashboard:
- **Customization** → Set logo, favicon, colors
- **Domain** → Configure custom domain (e.g., `docs.bethna.ai`)
- **Integrations** → Enable analytics, search

---

## Design Guidelines

### Colors (Based on COLOR_GUIDANCE.md)

| Element | Color | Hex | Usage |
|---------|-------|-----|-------|
| **Primary Background** | Hitam | `#050501` | Background utama, dark mode |
| **Primary Accent** | Ungu Muda | `#B68DF5` | Tombol utama, link, highlight |
| **Secondary Accent** | Kuning Neon | `#CBEA1A` | Call-to-action, badge, notifikasi |
| **Text (Dark BG)** | Putih | `#FFFFFF` | Teks pada background gelap |
| **Text (Light BG)** | Hitam | `#050501` | Teks pada background terang |
| **Card/Panel** | Hitam Abu | `#161615` | Background card (dark mode) |
| **Secondary BG** | Abu Tua | `#252426` | Background sekunder, border |
| **Border/Divider** | Abu Muda | `#C3C4BC` | Divider, input border |
| **Surface (Light)** | Putih Abu | `#F1EFF3` | Card background (light mode) |

### CSS Variables

```css
:root {
  --color-primary-bg: #050501;
  --color-primary-accent: #b68df5;
  --color-secondary-accent: #cbea1a;
  --color-text-main: #050501;
  --color-text-inverse: #ffffff;
  --color-card-bg: #161615;
  --color-border: #c3c4bc;
  --color-surface: #f1eff3;
}
```

### Typography

- **Headings**: Bold, clear hierarchy
- **Body**: Clean, readable
- **Code**: Monospace with syntax highlighting

### Images

- Store in `.gitbook/assets/` folder
- Use descriptive filenames
- Optimize for web (WebP preferred)

---

## Content Source

Semua konten diambil dari whitepaper yang sudah ada:

**Source File**: `docs/WHITEPAPER.md`

Pecah konten berdasarkan section headers (`## `) ke dalam file terpisah.

---

## Checklist

- [x] Create GitHub repository
- [x] Setup folder structure
- [x] Create SUMMARY.md
- [x] Split whitepaper into chapters
- [ ] Add images/diagrams
- [ ] Connect to GitBook
- [ ] Configure custom domain
- [ ] Test all links
- [ ] Review & publish

---


*Document Version: 1.1*  
*Created: January 2026*
