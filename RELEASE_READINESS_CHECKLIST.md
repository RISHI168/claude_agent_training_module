# Stable Product Release Checklist

## Pre-Release Verification Matrix

This checklist must be completed by all responsible agents before any production release. The Chief Orchestration Agent coordinates and verifies completion.

---

## 1. Product Readiness (Product Management Agent)

- [ ] All user stories in the release scope have status "Accepted"
- [ ] Acceptance criteria are met for every story (verified by SDTE Agent)
- [ ] Launch criteria defined in the PRD are satisfied
- [ ] Known issues are documented with severity and workarounds
- [ ] User-facing documentation / changelog is prepared
- [ ] Rollback criteria defined (what triggers a rollback)
- [ ] Success metrics instrumented and baseline measured

## 2. UX Completeness (UX Systems Architect Agent)

- [ ] All interaction flows (happy + error + edge cases) are implemented
- [ ] State machines verified — no unreachable or deadlocked states
- [ ] Edge case registry reviewed — all critical/high items handled
- [ ] Accessibility audit passes WCAG 2.2 AA (axe-core + manual)
- [ ] Empty states, loading states, and error states verified
- [ ] Cross-browser / cross-device testing completed
- [ ] Motion respects prefers-reduced-motion

## 3. Frontend Quality (Frontend Engineer Agent)

- [ ] Lighthouse Performance score ≥ 90
- [ ] Lighthouse Accessibility score ≥ 95
- [ ] TypeScript strict mode — zero errors
- [ ] No `any` types in production code
- [ ] Bundle size within budget (< 200KB gzipped initial)
- [ ] Core Web Vitals: LCP < 2.5s, INP < 200ms, CLS < 0.1
- [ ] Storybook stories cover all shipped components
- [ ] Console free of errors and warnings in production build
- [ ] Environment variables documented and configured for production

## 4. Backend Quality (Backend Engineer Agent)

- [ ] OpenAPI spec matches implementation (contract tests pass)
- [ ] All endpoints have authentication (unless explicitly public)
- [ ] Rate limiting configured for all endpoints
- [ ] Error responses are typed with error codes
- [ ] API response time p95 < 200ms (CRUD), < 500ms (complex)
- [ ] Health check endpoints operational (liveness + readiness)
- [ ] Structured logging with correlation IDs enabled
- [ ] Metrics emitting (request rate, error rate, latency)
- [ ] Circuit breakers configured for external dependencies
- [ ] No secrets in code, config files, or container images

## 5. Data Layer (Database Architect Agent)

- [ ] All migrations tested with production-volume data
- [ ] Migrations are zero-downtime compatible
- [ ] Rollback scripts tested
- [ ] Indexes aligned with production query patterns
- [ ] No queries trigger full table scans on large tables
- [ ] Backup schedule verified and recent backup validated
- [ ] Connection pool sized for production traffic
- [ ] PITR enabled with appropriate retention
- [ ] Schema documentation updated

## 6. Infrastructure (DevOps Agent)

- [ ] CI/CD pipeline passes all quality gates
- [ ] Docker images scanned — zero critical vulnerabilities
- [ ] Kubernetes resources configured (limits, requests, HPA, PDB)
- [ ] SSL certificates valid with > 30 days to expiry
- [ ] DNS configured and propagated
- [ ] CDN configured for static assets
- [ ] Monitoring dashboards operational
- [ ] Alert rules configured with appropriate thresholds
- [ ] Incident runbooks updated for new/changed services
- [ ] Rollback procedure tested (< 1 min to previous version)
- [ ] Deployment strategy configured (canary with auto-rollback)

## 7. Testing (SDTE Agent)

- [ ] Full regression suite passes (100% critical paths)
- [ ] Test coverage ≥ 80% (unit + integration)
- [ ] No flaky tests in the blocking pipeline
- [ ] Visual regression baseline approved
- [ ] E2E tests pass in staging environment
- [ ] Contract tests pass between frontend and backend
- [ ] Performance tests confirm targets met
- [ ] Security scan (SAST + DAST) — zero critical/high findings
- [ ] Accessibility tests — zero violations
- [ ] Pre-release quality report generated with ship recommendation

## 8. Growth & Experimentation (Growth Experimentation Agent)

- [ ] Feature flags configured for progressive rollout
- [ ] Analytics events instrumented and verified
- [ ] Experiment tracking ready (if applicable)
- [ ] Guardrail metrics defined and monitored
- [ ] Rollout plan documented (1% → 10% → 50% → 100%)

## 9. Orchestration Sign-Off (Chief Orchestration Agent)

- [ ] All agent checklists above are complete
- [ ] Cross-agent dependencies verified (no orphaned handoffs)
- [ ] Knowledge graph updated with release decisions
- [ ] DORA metrics captured for this release cycle
- [ ] Sprint summary produced
- [ ] Communication plan for stakeholders prepared
- [ ] Post-release monitoring owners assigned
- [ ] Post-release review scheduled (48 hours after full rollout)

---

## Release Decision

| Decision | Criteria |
|----------|----------|
| **SHIP** | All critical items checked. Non-critical items have documented acceptance. |
| **CONDITIONAL SHIP** | All critical items checked. Non-critical items have fixes scheduled for next sprint. |
| **HOLD** | Any critical item unchecked. Release blocked until resolved. |
| **ROLLBACK** | Post-deploy monitoring shows guardrail violations. Automatic or manual rollback triggered. |

**Final authority:** Product Management Agent makes the ship/hold decision based on SDTE Agent's quality report and this checklist. Chief Orchestration Agent coordinates the execution.
