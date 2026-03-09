# Architecture Unknown Unknowns Checklist

## Scaling Blind Spots
- Single database as bottleneck — no read replicas or connection pooling
- N+1 query problems hiding behind small datasets
- No caching strategy — hitting DB for every request
- Synchronous processing for tasks that should be async (emails, image processing, webhooks)
- No queue system — everything processed in the request cycle
- WebSocket connections not scalable beyond single server
- File storage on local disk instead of object storage (can't scale horizontally)
- Session state stored in memory (breaks with multiple instances)

## Database
- No migration strategy — manual schema changes
- Missing indexes on columns used in WHERE/JOIN/ORDER BY
- No connection pooling — exhausting database connections
- Transactions missing where data consistency matters
- No soft delete — losing data permanently
- No database backups or untested backup restoration
- Schema coupling — changing one table breaks everything
- Missing created_at/updated_at timestamps
- UUID vs auto-increment tradeoffs not considered
- No pagination — loading entire tables into memory

## Error Handling & Resilience
- No circuit breaker for external service calls
- Missing retry logic with exponential backoff
- No timeout on HTTP calls to external services
- Cascading failures — one service down takes everything down
- No graceful degradation — all or nothing
- Missing health check endpoints
- No dead letter queue for failed async jobs
- Error swallowing — catch blocks that do nothing

## Deployment & DevOps
- No zero-downtime deployment strategy
- Missing rollback plan
- No staging environment that mirrors production
- Database migrations that lock tables in production
- No feature flags for gradual rollouts
- Missing monitoring and alerting
- No centralized logging
- No infrastructure as code — manual server setup
- Missing CI/CD pipeline
- No load testing before launch

## API Design
- No versioning strategy — breaking changes affect all clients
- Missing pagination on list endpoints
- No rate limiting per user/API key
- Inconsistent error response format
- No idempotency keys for critical operations (payments, orders)
- Missing request/response validation
- No API documentation (OpenAPI/Swagger)
- Tight coupling between frontend and backend data shapes

## State Management
- Distributed state without consensus mechanism
- Race conditions in concurrent writes
- Optimistic locking not implemented where needed
- Cache invalidation strategy missing
- Event ordering not guaranteed in async systems
- No idempotent event handlers — duplicate events cause duplicate actions

## Common by Architecture Type

### Microservices
- No service discovery
- Distributed transactions without saga pattern
- Missing correlation IDs for request tracing
- Network latency not accounted for
- Data duplication without sync strategy
- No API gateway

### Monolith
- No modular boundaries — spaghetti coupling
- Single point of failure
- Deploy-everything-for-one-change
- Memory leaks from long-running process

### Serverless
- Cold start latency not accounted for
- Execution time limits hitting complex operations
- No local development environment that mirrors production
- Vendor lock-in without abstraction layer
- Connection pooling issues (new DB connection per invocation)
