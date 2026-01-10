# Application Layer

## Frontend Architecture

### Technology Stack

| Technology | Version | Purpose |
|-----------|---------|---------|
| **Next.js** | 15.0 (Turbo) | React framework with SSR |
| **React** | 18.2 | UI component library |
| **TypeScript** | 5.3 | Type-safe development |
| **Tailwind CSS** | 3.4 | Utility-first styling |
| **Framer Motion** | 11.0 | Animations |
| **Wagmi** | 2.0 | Wallet connection |
| **Viem** | 2.0 | Ethereum interactions |

### Performance Optimizations

#### 1. Web3SmartProvider

Lazy-loads wallet connectors to avoid blocking initial render:

```typescript
// Traditional approach (blocks page load)
import { WagmiConfig } from 'wagmi'
import { chains, wagmiClient } from './wagmi'

// BethNa approach (lazy load)
const Web3SmartProvider = dynamic(
  () => import('./providers/Web3Provider'),
  { ssr: false } // Skip server-side rendering
)
```

**Benefit**: Page interactive in <1 second, even before wallet loads.

#### 2. Server Components

Next.js 15 Server Components reduce client bundle size:

```tsx
// Server Component (runs on server, zero JS sent to client)
async function PositionsList() {
  const positions = await db.positions.findMany()
  
  return (
    <div>
      {positions.map(p => (
        <PositionCard key={p.id} position={p} />
      ))}
    </div>
  )
}

// Client Component (interactive, only when needed)
'use client'
function TradeButton() {
  const handleClick = () => { /* ... */ }
  return <button onClick={handleClick}>Trade</button>
}
```

#### 3. Image Optimization

```tsx
import Image from 'next/image'

<Image
  src="/assets/eth-logo.png"
  alt="ETH"
  width={32}
  height={32}
  priority // Preload critical images
  placeholder="blur" // Show blur while loading
/>
```

**Benefit**: 40% faster image loads, automatic WebP conversion.

### Design System: "Bespoke Radiant"

#### Color Palette

```css
:root {
  /* Primary */
  --color-primary-bg: #050501;
  --color-primary-accent: #b68df5;
  --color-secondary-accent: #cbea1a;
  
  /* Text */
  --color-text-main: #050501;
  --color-text-inverse: #ffffff;
  
  /* Surfaces */
  --color-card-bg: #161615;
  --color-surface: #f1eff3;
  
  /* Borders */
  --color-border: #c3c4bc;
}
```

#### Typography

```css
/* Headings */
h1 { font-size: 3rem; font-weight: 700; }
h2 { font-size: 2.25rem; font-weight: 700; }
h3 { font-size: 1.875rem; font-weight: 600; }

/* Body */
body { font-size: 1rem; font-weight: 400; line-height: 1.6; }

/* Monospace (code, numbers) */
.mono { font-family: 'JetBrains Mono', monospace; }
```

#### Glassmorphism

```css
.glass-card {
  background: rgba(22, 22, 21, 0.7);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
}
```

### Key UI Components

#### 1. AI Chat Interface

```tsx
<ChatInterface>
  <MessageList>
    {messages.map(msg => (
      <Message key={msg.id} role={msg.role}>
        {msg.content}
      </Message>
    ))}
  </MessageList>
  <InputBar onSend={handleSend} />
</ChatInterface>
```

**Features**:
- Streaming responses (word-by-word)
- Code syntax highlighting
- Copyable trade details
- Voice input *(Phase 2)*

#### 2. Position Card

```tsx
<PositionCard position={position}>
  <PositionHeader asset={position.asset} />
  <PnLDisplay 
    current={position.currentValue} 
    entry={position.entryValue} 
  />
  <GreeksDisplay greeks={position.greeks} />
  <ActionButtons>
    <CloseButton />
    <AdjustButton />
  </ActionButtons>
</PositionCard>
```

#### 3. Strategy Selector

```tsx
<StrategyGrid>
  {strategies.map(strategy => (
    <StrategyCard 
      key={strategy.id}
      name={strategy.name}
      description={strategy.description}
      winRate={strategy.historicalWinRate}
      onClick={() => selectStrategy(strategy)}
    />
  ))}
</StrategyGrid>
```

## Backend Architecture

### Technology Stack

| Technology | Version | Purpose |
|-----------|---------|---------|
| **Python** | 3.11 | Backend language |
| **FastAPI** | 0.109 | API framework |
| **Pydantic** | 2.5 | Data validation |
| **LangChain** | 0.1.0 | AI orchestration |
| **Celery** | 5.3 | Task queue |
| **Redis** | 7.2 | Caching, pub/sub |
| **PostgreSQL** | 15 | Relational database |

### API Structure

#### RESTful Endpoints

```python
from fastapi import FastAPI, Depends
from typing import List

app = FastAPI()

# Chat
@app.post("/api/chat")
async def chat(message: str, user_id: str):
    response = await agent_alpha.process(message)
    return {"response": response}

# Strategies
@app.get("/api/strategies/{asset}")
async def get_strategies(asset: str) -> List[Strategy]:
    strategies = await db.strategies.find(asset=asset)
    return strategies

# Positions
@app.get("/api/positions")
async def get_positions(user_id: str) -> List[Position]:
    positions = await db.positions.find(user_id=user_id)
    return positions

# Execute Trade
@app.post("/api/execute")
async def execute_trade(trade: TradeRequest):
    result = await agent_beta.execute(trade)
    return {"status": "success", "tx_hash": result.tx_hash}
```

#### WebSocket Endpoints

```python
from fastapi import WebSocket

@app.websocket("/ws/positions")
async def positions_websocket(websocket: WebSocket):
    await websocket.accept()
    
    while True:
        # Send real-time position updates
        positions = await get_user_positions()
        await websocket.send_json(positions)
        await asyncio.sleep(5)
```

### Background Tasks

#### Celery Workers

```python
from celery import Celery

celery_app = Celery('bethna', broker='redis://localhost:6379')

@celery_app.task
def monitor_position(position_id: str):
    """Background task to monitor position health"""
    position = db.positions.get(position_id)
    current_price = pyth.get_price(position.option_token)
    
    pnl = calculate_pnl(position, current_price)
    
    if pnl < STOP_LOSS:
        trigger_exit(position)
    
    return {"position_id": position_id, "pnl": pnl}
```

### Caching Strategy

```python
from redis import Redis

redis_client = Redis(host='localhost', port=6379)

async def get_market_data(asset: str):
    # Check cache first
    cached = redis_client.get(f"market:{asset}")
    if cached:
        return json.loads(cached)
    
    # Fetch from API
    data = await pyth_client.get_price(asset)
    
    # Cache for 5 seconds
    redis_client.setex(
        f"market:{asset}",
        5,
        json.dumps(data)
    )
    
    return data
```

## Database Schema

### Key Tables

#### Users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY,
  wallet_address VARCHAR(42) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  last_active TIMESTAMP,
  settings JSONB
);
```

#### Positions
```sql
CREATE TABLE positions (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  option_token VARCHAR(42) NOT NULL,
  amount DECIMAL NOT NULL,
  entry_price DECIMAL NOT NULL,
  current_price DECIMAL,
  pnl DECIMAL,
  status VARCHAR(20), -- ACTIVE, CLOSED
  created_at TIMESTAMP DEFAULT NOW(),
  closed_at TIMESTAMP
);
```

#### Trade History
```sql
CREATE TABLE trades (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  position_id UUID REFERENCES positions(id),
  tx_hash VARCHAR(66) UNIQUE,
  trade_type VARCHAR(10), -- BUY, SELL
  amount_in DECIMAL,
  amount_out DECIMAL,
  slippage DECIMAL,
  gas_used BIGINT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

{% hint style="info" %}
**Learn about**: [Tokenomics](../tokenomics/README.md) | [Roadmap](../roadmap/README.md)
{% endhint %}
