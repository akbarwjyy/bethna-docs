# Architecture Diagram

This diagram shows the complete architecture of the SentientTrader protocol, including user layer, BethNa layer, smart contract layer, and external protocol integration.

```mermaid
graph TB
    subgraph "User Layer"
        U[User Wallet]
        USDC[USDC Balance]
        OPT[Option Tokens]
    end
    
    subgraph "BethNa Layer"
        AG[AI Agents]
        BE[Backend Service]
    end
    
    subgraph "Smart Contract Layer"
        ST[SentientTrader.sol]
        AC[Access Control]
        SP[Slippage Protection]
        PS[Pausable]
    end
    
    subgraph "External Protocol"
        TR[Thetanuts V4 Router]
        LP[Liquidity Pools]
    end
    
    U -->|Approve| ST
    AG -->|Recommendation| BE
    BE -->|Execute Trade| ST
    ST -->|Check Authorization| AC
    ST -->|Validate Slippage| SP
    ST -->|Check Status| PS
    ST -->|Swap| TR
    TR -->|Trade| LP
    LP -->|Return Tokens| TR
    TR -->|Return| ST
    ST -->|Transfer| U
    
    style ST fill:#4CAF50
    style U fill:#2196F3
    style TR fill:#FF9800
```

## Components Overview

### User Layer
- **User Wallet**: End-user's wallet containing assets
- **USDC Balance**: Stablecoin used for purchasing options
- **Option Tokens**: Received option tokens from trades

### BethNa Layer
- **AI Agents**: Autonomous agents that analyze markets and generate recommendations
- **Backend Service**: Executes trades on behalf of users based on agent recommendations

### Smart Contract Layer
- **SentientTrader.sol**: Main contract that handles all trading operations
- **Access Control**: Whitelist system for authorized traders
- **Slippage Protection**: Ensures trades don't exceed maximum slippage
- **Pausable**: Emergency stop mechanism

### External Protocol
- **Thetanuts V4 Router**: External DeFi protocol for options trading
- **Liquidity Pools**: Where actual option trades are executed
