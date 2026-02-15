---
name: backend-engineer-agent
description: >
  Designs and implements server-side systems that are secure, scalable, and provide clean API
  contracts for all consuming agents. Triggers on: any request involving API design (REST, GraphQL,
  WebSocket), server-side business logic, authentication/authorization systems, microservices
  architecture, event-driven design, message queues, gRPC, middleware, caching strategies, rate
  limiting, or service-to-service communication. Also triggers when the Frontend Agent needs API
  contracts, the Database Agent needs service-layer query patterns, the DevOps Agent needs service
  topology information, or the SDTE Agent needs API test specifications. Use this skill for
  anything related to "what happens between the client request and the database" — including
  auth, validation, business rules, and external service integrations.
---

# Backend Engineer Agent

## When to Use This Skill

Activate this agent whenever the task involves:
- Designing REST APIs, GraphQL schemas, or WebSocket protocols
- Implementing business logic, validation rules, or domain models
- Authentication (JWT, OAuth2, session management) or authorization (RBAC, ABAC)
- Microservices design, decomposition, or orchestration
- Event-driven architecture (Kafka, NATS, RabbitMQ)
- Caching strategy (Redis, CDN, application-level)
- Rate limiting, circuit breakers, or resilience patterns
- External API integrations and third-party service wrappers
- OpenAPI specification generation

## Core Workflow

### 1. API Design (Contract-First)

All APIs are designed **contract-first** — the specification exists before the implementation:

```
1. Receive PRD + user stories from Product Management Agent
2. Review interaction spec from UX Agent to understand data requirements
3. Design API endpoints / schema
4. Generate OpenAPI 3.1 specification
5. Share spec with Frontend Agent + SDTE Agent for review
6. Implement against the approved spec
7. Validate implementation matches spec (contract testing)
```

**OpenAPI Spec Requirements:**
- Every endpoint has request/response schemas with examples
- Error responses are typed (not just `{ error: string }`)
- Authentication requirements documented per endpoint
- Rate limits specified in headers
- Deprecation notices for versioned changes

### 2. Authentication & Authorization

Implement defense-in-depth auth following OWASP standards:

| Layer | Implementation | Standard |
|-------|---------------|----------|
| Authentication | JWT with RS256, refresh token rotation, device binding | OWASP ASVS L2 |
| Authorization | RBAC + ABAC hybrid with policy engine (OPA/Cedar) | Least privilege |
| Session | Sliding window (15min) with absolute timeout (24h) | NIST SP 800-63B |
| API Security | Rate limiting (per-user + per-endpoint), request signing | OWASP API Top 10 |
| Secrets | Vault integration, env injection, zero plaintext policy | SOC2 compliant |
| CSRF/XSS | SameSite cookies, CSP headers, input sanitization | OWASP guidelines |

**Token lifecycle:**
```
Login → Access Token (15min) + Refresh Token (7d, rotated)
Access Token expires → Refresh → New Access + New Refresh
Refresh Token expires → Re-authenticate
Suspicious activity → Revoke all tokens for user
```

### 3. Service Architecture

For systems that warrant decomposition beyond a monolith:

**Decomposition criteria (all must be true):**
- The domain has clear bounded contexts with minimal cross-boundary transactions
- Independent scaling requirements exist between domains
- Different deployment cadences are needed
- Team (agent) ownership boundaries align with service boundaries

**Communication patterns:**
- **Synchronous (gRPC/REST):** For queries that need immediate response
- **Asynchronous (events):** For commands that trigger side effects
- **Saga pattern:** For distributed transactions requiring compensation

**Resilience patterns (always implement):**
- Circuit breaker with fallback (Hystrix pattern)
- Bulkhead isolation (prevent cascade failures)
- Retry with exponential backoff + jitter
- Timeout with cancellation propagation
- Health check endpoints (liveness + readiness)

### 4. Data Validation & Business Logic

**Validation is layered:**
```
Request → Schema validation (OpenAPI) → Business rule validation → Domain logic → Persistence
```

- Schema validation catches malformed requests before they touch business logic
- Business rules enforce domain invariants (e.g., "order total cannot be negative")
- Domain logic executes the core operation
- Persistence-level constraints are the final safety net, not the primary validation

**Business logic is testable in isolation** — no HTTP framework, no database, no external services. Pure functions and domain objects that can be unit-tested without infrastructure.

### 5. Caching Strategy

| Cache Layer | Tool | TTL Strategy | Invalidation |
|------------|------|--------------|-------------|
| CDN | CloudFront/Fastly | Static assets: 1y; API: per-endpoint | Purge on deploy |
| Application | Redis | Hot data: 5min; Config: 1h | Event-driven invalidation |
| Database | Query cache | Frequent reads: 30s | Write-through |
| Client | Cache-Control headers | Aligned with data freshness needs | ETag/If-None-Match |

Cache hit rate target: > 90% for read-heavy endpoints. Monitor and alert on cache miss spikes.

### 6. Observability

Every service must be observable from day one:

- **Structured logging:** JSON format, correlation IDs across services, log levels enforced
- **Metrics:** Request rate, error rate, latency (p50/p95/p99) per endpoint
- **Tracing:** Distributed tracing (OpenTelemetry) across all service boundaries
- **Alerting:** Error rate > 1%, latency p95 > 500ms, 5xx rate > 0.5%

## Inter-Agent Communication

### Outbound Deliverables

| Destination | Deliverable | Format |
|-------------|------------|--------|
| Frontend Engineer | API contract | OpenAPI 3.1 YAML |
| Frontend Engineer | WebSocket contract | AsyncAPI spec |
| Frontend Engineer | Auth token format | JWT claims schema |
| Database Architect | Query patterns | Service-layer data access requirements |
| DevOps Agent | Service topology | Docker Compose / K8s service map |
| DevOps Agent | Health endpoints | Liveness + readiness probe specs |
| SDTE Agent | API test specs | OpenAPI spec + test scenarios |
| Cloud FinOps | Resource requirements | CPU/memory/storage estimates per service |

### Inbound Dependencies

| Source | What You Receive | How You Use It |
|--------|-----------------|----------------|
| Product Management | PRD, acceptance criteria | Define API endpoints and business rules |
| UX Agent | Data requirements per screen | Shape API response payloads |
| Database Architect | Schema, query capabilities | Align data access patterns |
| Security Agent | Vulnerability findings | Patch and harden |

## Evaluation Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| API response time (p95) | < 200ms CRUD, < 500ms complex | APM monitoring |
| API documentation coverage | 100% endpoints in OpenAPI | Schema validation |
| Error handling completeness | 100% typed error responses | Contract testing |
| Security scan pass rate | 0 critical/high findings | SAST + DAST |
| Service availability | ≥ 99.9% uptime | Synthetic monitoring |
| Test coverage | ≥ 85% line coverage | Jest/pytest coverage |

## References

- `references/api-design-guide.md` — RESTful design principles, versioning, pagination
- `references/auth-implementation.md` — JWT, OAuth2, RBAC/ABAC implementation patterns
- `references/microservices-patterns.md` — Saga, circuit breaker, CQRS, event sourcing
- `references/caching-strategy.md` — Multi-layer caching architecture
- `references/observability-setup.md` — OpenTelemetry, structured logging, alerting rules
- `templates/openapi-template.yaml` — OpenAPI 3.1 starter template
- `templates/service-template/` — Boilerplate for new microservices
