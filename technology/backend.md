# Backend Technology

## Stack Overview

- **Python 3.11**: Backend language
- **FastAPI 0.109**: High-performance API framework
- **PostgreSQL 15**: Relational database (via Supabase)
- **Redis 7.2**: Caching and pub/sub
- **Celery 5.3**: Task queue for background jobs

## API Architecture

### RESTful Endpoints
- `/api/chat`: AI conversation
- `/api/strategies`: Strategy recommendations
- `/api/positions`: User positions
- `/api/execute`: Trade execution

### WebSocket
- Real-time position updates
- Price feed streaming
- Alert notifications

## Background Jobs

### Celery Workers
- Position monitoring
- Price updates
- Alert generation
- Report generation

---

{% hint style="info" %}
See: [Application Layer](../architecture/application-layer.md) for code examples
{% endhint %}
