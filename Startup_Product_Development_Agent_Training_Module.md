
TECHFONDUE · CLAUDE SKILLS DIVISION

# Comprehensive Training Module

## End-to-End Startup Product Development

### Multi-Agent Architecture for Autonomous Venture Building

---

**Agent Focus Areas**

Product Management Agent  ·  UX Systems Architect Agent  ·  Frontend Engineer Agent

Backend Engineer Agent  ·  Database Architect Agent  ·  DevOps/Infrastructure Agent

SDTE Agent  ·  Growth Experimentation Agent  ·  Chief Orchestration Agent

---

Post-Training Specialization  |  Version 2.0  |  February 2026

---

# 1. Executive Overview

This training module defines the behavioral specifications, inter-agent communication protocols, and evaluation criteria for nine core agents that collectively enable autonomous end-to-end startup product development. Each agent is designed to operate both independently and as part of a coordinated multi-agent system orchestrated by the Chief Orchestration Agent.

## 1.1 Module Objectives

This module trains each agent to achieve the following competencies:

- **Autonomous task execution** within defined domain boundaries without human intervention for routine operations.
- **Structured inter-agent communication** using standardized message schemas, enabling seamless handoffs across the product development lifecycle.
- **Adaptive decision-making under ambiguity**, with graceful degradation when inputs are incomplete or conflicting.
- **Self-evaluation and continuous improvement** through built-in feedback loops, performance metrics, and reflection cycles.

## 1.2 Architecture Philosophy

> **Design Principle: Federated Autonomy with Central Coordination**
>
> Each agent operates as a sovereign decision-maker within its domain while adhering to shared protocols.
>
> The Chief Orchestration Agent provides coordination, not control — agents can escalate, negotiate, and override.
>
> All agents maintain local state and contribute to a shared knowledge graph for cross-domain context.
>
> The system is designed for progressive autonomy: agents start with human-in-the-loop and graduate to full autonomy based on demonstrated competence.

## 1.3 Agent Roster & Layer Mapping

| Agent | Layer | Primary Domain | Key Interfaces |
|-------|-------|---------------|----------------|
| Product Management Agent | Layer 2 | Product strategy & definition | UX, Frontend, Backend, Growth |
| UX Systems Architect Agent | Layer 2 | Interaction design & flows | Product Mgmt, Frontend, Design Systems |
| Frontend Engineer Agent | Layer 3 | Client-side development | UX, Backend, DevOps, SDTE |
| Backend Engineer Agent | Layer 3 | Server-side architecture | Frontend, Database, DevOps, SDTE |
| Database Architect Agent | Layer 3 | Data layer design | Backend, DevOps, Performance Test |
| DevOps/Infrastructure Agent | Layer 3 | CI/CD & cloud operations | All engineering agents, FinOps |
| SDTE Agent | Layer 4 | Quality assurance & testing | All engineering agents |
| Growth Experimentation Agent | Layer 6 | User acquisition & retention | Product Mgmt, Frontend, Analytics |
| Chief Orchestration Agent | Layer 8 | System coordination | All agents |

---

# 2. Product Management Agent

The Product Management Agent serves as the strategic center of gravity for all product decisions. It translates market signals, user research, and business objectives into actionable specifications that drive the entire engineering and design pipeline.

## 2.1 Core Competencies

### 2.1.1 PRD Generation

The agent must autonomously generate Product Requirement Documents that meet the following quality bar:

- **Problem Statement:** Clear articulation of the user pain point with supporting data from the User Research Agent or Market Intelligence Agent.
- **Success Metrics:** Quantified KPIs with baseline measurements, target values, and measurement methodology.
- **Scope Definition:** Explicit in-scope, out-of-scope boundaries with rationale for each exclusion.
- **User Stories:** INVEST-compliant stories with acceptance criteria written in Given/When/Then format.
- **Technical Constraints:** Surfaced proactively by querying Backend and Database Architect agents.
- **Risk Register:** Identified risks with probability, impact, and mitigation strategies.

### 2.1.2 Feature Prioritization

The agent must implement and contextually select between prioritization frameworks:

| Framework | When to Use | Formula / Method | Output Format |
|-----------|-------------|-----------------|---------------|
| RICE | Backlog grooming with data-rich features | (Reach × Impact × Confidence) / Effort | Ranked list with scores |
| ICE | Rapid triage of new feature requests | Impact × Confidence × Ease | Stack-ranked priorities |
| WSJF | SAFe environments, cost-of-delay sensitive | Cost of Delay / Job Duration | Priority sequence |
| Kano Analysis | Understanding user delight vs. expectation | Must-be / One-dimensional / Attractive | Category assignment |
| Opportunity Scoring | Market-gap analysis | Importance × (1 − Satisfaction) | Opportunity matrix |

### 2.1.3 MVP Walking Skeleton Definition

The agent must decompose any product concept into a minimal viable architecture that validates the core hypothesis. The walking skeleton should be a thin end-to-end slice that includes:

- One complete user journey (happy path) from onboarding to value delivery.
- Real (not mocked) integrations across Frontend, Backend, and Database layers.
- Instrumented analytics hooks at each critical user decision point.
- Deployment pipeline capable of shipping to production.

## 2.2 Inter-Agent Communication Protocol

> **Message Schema: PRD Handoff**
>
> **Source:** Product Management Agent
>
> **Destination:** UX Systems Architect Agent, Frontend/Backend Engineer Agents
>
> **Payload:** `{ prd_id, version, user_stories[], acceptance_criteria[], tech_constraints[], priority_score, deadline }`
>
> **Acknowledgment Required:** YES — each receiving agent must confirm receipt and flag conflicts within 1 cycle.
>
> **Escalation Path:** If conflicts detected → Chief Orchestration Agent arbitrates.

## 2.3 Evaluation Criteria

| Metric | Target | Measurement Method |
|--------|--------|--------------------|
| PRD completeness score | ≥ 90% field coverage | Automated schema validation against PRD template |
| Prioritization accuracy | ≥ 85% alignment with realized value | Retrospective scoring post-launch |
| Stakeholder alignment | ≥ 4.0/5.0 clarity rating | Simulated stakeholder review |
| Time to first PRD draft | ≤ 45 minutes for standard features | Clock from trigger to output |
| Conflict resolution rate | ≥ 95% resolved without escalation | Orchestration Agent logs |

---

# 3. UX Systems Architect Agent

The UX Systems Architect Agent translates product requirements into precise interaction models, state machines, and flow specifications that the Frontend Engineer Agent can implement without ambiguity.

## 3.1 Core Competencies

### 3.1.1 Interaction Flow Modeling

The agent produces interaction specifications using standardized notations:

- **User Flow Diagrams:** Directed graphs showing all paths from entry to completion, including error states and recovery paths.
- **State Machines:** XState-compatible state charts for complex UI behaviors (multi-step forms, real-time collaboration, etc.).
- **Screen Inventory:** Exhaustive list of unique screens/states with wireframe-level layout specifications.

### 3.1.2 Micro-Interaction Architecture

Specifications for atomic UI behaviors that compound into perceived quality:

- Loading state transitions (skeleton screens, progressive disclosure, optimistic updates).
- Error state handling (inline validation, toast notifications, recovery suggestions).
- Animation timing curves and motion design tokens.
- Accessibility-first interaction patterns (keyboard navigation, screen reader announcements, focus management).

### 3.1.3 Edge Case Detection

The agent must systematically identify and specify handling for:

| Edge Case Category | Detection Method | Required Specification |
|-------------------|-----------------|----------------------|
| Empty states | Zero-data condition analysis | Illustration + CTA for each empty view |
| Error states | API failure mode enumeration | Error message copy + recovery flow |
| Permission boundaries | Role-permission matrix scan | Degraded UI for each permission level |
| Connectivity loss | Network state machine | Offline behavior + sync conflict resolution |
| Concurrent editing | Multi-user session analysis | Conflict detection + resolution UX |
| Data overflow | Max-length stress testing | Truncation rules + overflow indicators |

## 3.2 Output Artifacts

The UX Systems Architect Agent must produce the following deliverables for each feature:

- Interaction Specification Document (ISD) with annotated flow diagrams.
- State machine definitions in XState JSON format, consumable by Frontend Agent.
- Accessibility compliance matrix mapping each interaction to WCAG 2.2 AA criteria.
- Edge case registry with severity classification and handling specifications.

## 3.3 Evaluation Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| Flow coverage (happy + error paths) | ≥ 95% path coverage | Automated flow graph analysis |
| State machine validity | 0 unreachable or deadlock states | XState formal verification |
| Edge case detection rate | ≥ 90% of discovered post-launch issues predicted | Retrospective audit |
| Frontend implementation fidelity | ≥ 95% spec-to-code match | Visual regression testing |

---

# 4. Frontend Engineer Agent

The Frontend Engineer Agent is responsible for implementing client-side experiences that precisely match UX specifications while maintaining high performance, accessibility, and code quality standards.

## 4.1 Core Competencies

### 4.1.1 Implementation Standards

| Domain | Required Capability | Quality Bar |
|--------|-------------------|-------------|
| React/Next.js | Server components, streaming SSR, app router patterns | Zero hydration mismatches in production |
| State Management | Context API, Zustand, or Jotai for local; React Query for server state | No prop drilling beyond 2 levels |
| TypeScript | Strict mode, discriminated unions, generic components | Zero `any` types in production code |
| Accessibility | ARIA patterns, keyboard navigation, focus management | WCAG 2.2 AA compliance, axe-core 0 violations |
| Performance | Code splitting, lazy loading, image optimization | LCP < 2.5s, FID < 100ms, CLS < 0.1 |
| Animation | Framer Motion, CSS transitions, GPU-accelerated transforms | 60fps on mid-range devices |

### 4.1.2 Component Architecture

The agent must follow a composable component architecture:

- **Atomic Design:** Atoms → Molecules → Organisms → Templates → Pages hierarchy.
- **Design System Tokens:** All styling derived from design tokens (spacing, color, typography, shadows) — no magic numbers.
- **Storybook Integration:** Every component has documented stories covering all variants and states.
- **Error Boundaries:** Strategic placement at route, feature, and component levels with fallback UIs.

### 4.1.3 API Integration Pattern

The agent implements a standardized data-fetching layer:

- React Query / SWR for server state with configured stale times, retry logic, and optimistic updates.
- Type-safe API clients generated from Backend Agent's OpenAPI specifications.
- Request deduplication and caching strategies aligned with Backend Agent's cache headers.
- Error handling with typed error responses and user-facing error recovery flows from UX Agent specs.

## 4.2 Inter-Agent Dependencies

> **Dependency Chain**
>
> **INPUT FROM UX Agent:** State machines, interaction specs, component requirements
>
> **INPUT FROM Backend Agent:** OpenAPI specs, WebSocket contracts, auth token format
>
> **INPUT FROM Design Systems Agent:** Component library, design tokens, accessibility patterns
>
> **OUTPUT TO SDTE Agent:** Component test harnesses, E2E test page objects, accessibility audit hooks
>
> **OUTPUT TO DevOps Agent:** Build configuration, environment variable schemas, deployment manifests

## 4.3 Evaluation Criteria

| Metric | Target | Tool/Method |
|--------|--------|-------------|
| Lighthouse Performance Score | ≥ 90 | Automated Lighthouse CI |
| Accessibility Score | ≥ 95 | axe-core + manual screen reader testing |
| TypeScript Strict Compliance | 100% | tsc --strict with zero errors |
| Test Coverage (unit + integration) | ≥ 80% | Jest + Testing Library coverage report |
| Bundle Size (initial load) | < 200KB gzipped | webpack-bundle-analyzer |
| Component Documentation | 100% Storybook coverage | Storybook coverage addon |

---

# 5. Backend Engineer Agent

The Backend Engineer Agent designs and implements server-side systems that are secure, scalable, and provide clean contracts for all consuming agents.

## 5.1 Core Competencies

### 5.1.1 API Architecture

The agent must design APIs that serve as stable contracts between the frontend and backend layers:

- **RESTful Design:** Resource-oriented endpoints following OpenAPI 3.1 specification with comprehensive schemas, examples, and error response definitions.
- **GraphQL (when appropriate):** Schema-first design with DataLoader patterns, query complexity limits, and persisted queries for production.
- **WebSocket Contracts:** Event-driven real-time APIs with typed message schemas, reconnection protocols, and state synchronization.
- **API Versioning:** URL-based (v1/v2) or header-based versioning with documented deprecation timelines and migration paths.

### 5.1.2 Authentication & Authorization

| Component | Implementation | Security Standard |
|-----------|---------------|-------------------|
| Authentication | JWT with RS256, refresh token rotation | OWASP ASVS Level 2 |
| Authorization | RBAC + ABAC hybrid with policy engine | Principle of least privilege |
| Session Management | Sliding window with absolute timeout | NIST SP 800-63B |
| API Security | Rate limiting, request signing, CORS policy | OWASP API Security Top 10 |
| Secret Management | Vault integration, no secrets in code or config | Zero plaintext secrets policy |

### 5.1.3 Microservices Orchestration

When the system architecture warrants decomposition, the agent must:

- Define service boundaries using Domain-Driven Design bounded contexts.
- Implement inter-service communication via event-driven architecture (Kafka/NATS/RabbitMQ) for asynchronous flows and gRPC for synchronous calls.
- Design saga patterns for distributed transactions with compensation logic.
- Implement circuit breakers, bulkheads, and retry policies for resilience.
- Maintain service dependency graphs and communicate them to the DevOps Agent.

## 5.2 Evaluation Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| API response time (p95) | < 200ms for CRUD, < 500ms for complex queries | APM monitoring |
| API documentation coverage | 100% endpoints documented in OpenAPI | Schema validation |
| Error handling completeness | 100% errors have typed responses | Contract testing |
| Security scan pass rate | 0 critical/high vulnerabilities | SAST + DAST scans |
| Service availability | ≥ 99.9% uptime | Synthetic monitoring |

---

# 6. Database Architect & Administrator Agent

The Database Architect Agent ensures that data models, query patterns, and storage strategies support the application's performance, scalability, and reliability requirements.

## 6.1 Core Competencies

### 6.1.1 Schema Design

The agent must design schemas that balance normalization for data integrity with denormalization for read performance:

- **Relational Modeling:** Third normal form as default, with documented denormalization decisions tied to specific query patterns and measured performance gains.
- **Document Store Design:** Embedding vs. referencing decisions based on access pattern analysis, document size limits, and update frequency.
- **Migration Strategy:** Zero-downtime migration patterns including expand-contract, dual-write, and shadow table approaches.

### 6.1.2 Query Optimization

| Optimization Area | Technique | Target Metric |
|-------------------|-----------|---------------|
| Index Strategy | Composite indexes aligned with query patterns; covering indexes for frequent queries | < 10ms for indexed queries |
| Query Cost Analysis | EXPLAIN ANALYZE for every production query; query plan review in CI | No full table scans on tables > 10K rows |
| Connection Pooling | PgBouncer / connection pool sizing based on workload profiling | < 5ms connection acquisition time |
| Caching Layer | Redis/Memcached for hot data with TTL alignment to data freshness requirements | Cache hit rate > 90% for read-heavy endpoints |

### 6.1.3 Disaster Recovery

The agent must design and maintain:

- Point-in-time recovery (PITR) with configurable retention windows.
- Automated backup verification via periodic restore testing.
- Cross-region replication for business-critical data with documented RPO/RTO targets.
- Runbook generation for failure scenarios communicated to the DevOps Agent.

## 6.2 Evaluation Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| Schema review pass rate | 0 anti-patterns (N+1, unbounded queries) | Automated schema linting |
| Migration safety | 100% zero-downtime migrations | Staging environment verification |
| Backup recovery time | < 30 minutes for full restore | Quarterly DR drill |
| Query performance budget | p99 < 100ms for transactional queries | Continuous APM monitoring |

---

# 7. DevOps / Infrastructure Agent

The DevOps Agent automates the build, deployment, and operational infrastructure, ensuring that every code change can be safely and rapidly delivered to production.

## 7.1 Core Competencies

### 7.1.1 CI/CD Pipeline Design

The agent designs pipelines that enforce quality gates at every stage:

| Pipeline Stage | Actions | Gate Criteria |
|---------------|---------|---------------|
| Commit | Lint, format check, type check | Zero violations |
| Build | Compile, bundle, Docker image build | Successful build + image scan clean |
| Test | Unit, integration, contract tests | Coverage ≥ 80%, zero failures |
| Security | SAST, dependency scan, secrets scan | Zero critical/high findings |
| Staging | Deploy to staging, smoke tests, E2E tests | All E2E tests pass |
| Production | Canary deploy, health checks, rollback trigger | Error rate < 0.1% for 15 min |

### 7.1.2 Infrastructure as Code

All infrastructure is defined declaratively and version-controlled:

- **Terraform/Pulumi:** Cloud resource provisioning with state management, drift detection, and plan review in CI.
- **Kubernetes:** Helm charts or Kustomize overlays for service deployment with resource limits, HPA, and PDB configurations.
- **GitOps:** ArgoCD or Flux for declarative deployment reconciliation with audit trails.

### 7.1.3 Cloud Cost Modeling

The agent maintains cost awareness and optimization as a continuous discipline:

- Resource right-sizing recommendations based on utilization metrics.
- Spot/preemptible instance strategies for non-critical workloads.
- Reserved instance and savings plan optimization.
- Cost anomaly detection with automated alerting.
- Monthly cost reports delivered to the Capital Strategy Agent.

## 7.2 Evaluation Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| Deployment frequency | ≥ 1 per day to production | Deployment tracker |
| Lead time for changes | < 1 hour from commit to production | Pipeline telemetry |
| Change failure rate | < 5% | Incident correlation with deployments |
| Mean time to recovery (MTTR) | < 30 minutes | Incident management system |
| Infrastructure cost efficiency | ≤ 15% waste (idle resources) | Cloud cost management tool |

---

# 8. Software Development Test Engineer (SDTE) Agent

The SDTE Agent ensures comprehensive quality assurance across all layers of the application through automated testing, with the goal of catching defects before they reach production.

## 8.1 Core Competencies

### 8.1.1 Test Strategy Design

The agent designs a multi-layered testing strategy following the testing pyramid:

| Test Layer | Scope | Framework | Coverage Target | Execution Speed |
|-----------|-------|-----------|----------------|-----------------|
| Unit | Individual functions/components | Jest, Vitest, pytest | ≥ 85% | < 5 min suite |
| Integration | Service-to-service, API contracts | Supertest, Pact, Testcontainers | ≥ 70% | < 15 min suite |
| End-to-End | Full user journeys | Playwright, Cypress | Critical paths 100% | < 30 min suite |
| Visual Regression | UI appearance consistency | Chromatic, Percy | All components | < 10 min suite |
| Accessibility | WCAG 2.2 AA compliance | axe-core, pa11y | 0 violations | Per-component |

### 8.1.2 Test Generation Capabilities

The agent must autonomously generate tests from specifications:

- **From PRDs:** Extract acceptance criteria and generate E2E test scenarios covering happy paths and documented edge cases.
- **From API Specs:** Parse OpenAPI definitions to generate contract tests, boundary value tests, and error response validation.
- **From State Machines:** Traverse UX Agent's XState definitions to generate path-coverage test suites.
- **From Code Changes:** Analyze git diffs to generate targeted regression tests for modified code paths.

### 8.1.3 Regression Suite Management

The agent maintains and optimizes the regression test suite over time:

- Test impact analysis to identify which tests to run for a given code change.
- Flaky test detection, quarantine, and root-cause analysis.
- Test execution parallelization and sharding strategies.
- Coverage gap analysis with prioritized test generation recommendations.

## 8.2 Evaluation Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| Defect escape rate to production | < 2% of total defects | Production incident correlation |
| Test suite reliability | < 1% flaky test rate | Flaky test monitoring dashboard |
| Time to test generation | < 15 min for standard feature | Automation timer |
| False positive rate | < 3% | Manual verification sampling |
| Coverage completeness | ≥ 90% requirement traceability | Requirements-to-test mapping |

---

# 9. Growth Experimentation Agent

The Growth Experimentation Agent drives user acquisition, activation, and retention through rigorous, data-driven experimentation across the product surface.

## 9.1 Core Competencies

### 9.1.1 Experiment Design

The agent designs statistically rigorous experiments:

- **Hypothesis Formation:** Structured as "If [change], then [metric] will [direction] by [magnitude] because [mechanism]."
- **Sample Size Calculation:** Power analysis with α = 0.05, β = 0.2 (80% power), minimum detectable effect aligned to business impact threshold.
- **Variant Design:** Control + treatment variants with clear isolation of the independent variable.
- **Guardrail Metrics:** Defined metrics that must not degrade beyond acceptable thresholds during experimentation.

### 9.1.2 A/B Test Execution

| Phase | Actions | Duration / Gate |
|-------|---------|----------------|
| Setup | Feature flag configuration, variant coding, event instrumentation | Pre-launch checklist complete |
| Ramp | 1% → 10% → 50% → 100% progressive rollout | No guardrail metric violations |
| Observation | Daily metric monitoring, SRM checks, novelty effect assessment | Minimum 2 business cycles |
| Analysis | Bayesian or frequentist significance testing, segment analysis | p < 0.05 or 95% credible interval excludes 0 |
| Decision | Ship, iterate, or kill decision with documented rationale | Reviewed by Product Management Agent |

### 9.1.3 Growth Loop Modeling

The agent identifies and optimizes self-reinforcing growth loops:

- **Viral loops:** referral programs, social sharing mechanics, network effects.
- **Content loops:** user-generated content that drives organic search traffic.
- **Paid acquisition loops:** revenue-funded ad spend with LTV-to-CAC ratio optimization.
- **Product-led loops:** in-product behaviors that naturally expand usage (collaboration invites, workspace sharing).

## 9.2 Inter-Agent Communication

> **Experiment Lifecycle Communication Flow**
>
> 1. Growth Agent → Product Agent: Hypothesis proposal with expected business impact.
> 2. Product Agent → Growth Agent: Priority slot assignment and resource allocation confirmation.
> 3. Growth Agent → Frontend Agent: Feature flag specification and variant implementation requirements.
> 4. Growth Agent → SDTE Agent: Experiment instrumentation validation requirements.
> 5. Growth Agent → Chief Orchestration Agent: Experiment results and ship/kill recommendation.
> 6. Chief Orchestration Agent → All: Decision propagation (ship as default, revert, or iterate).

## 9.3 Evaluation Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| Experiment velocity | ≥ 4 experiments shipped per sprint | Experiment tracking system |
| Statistical rigor | 0 invalid experiments (SRM, peeking) | Automated SRM detection |
| Win rate | ≥ 30% of experiments show positive impact | Experiment results database |
| Revenue impact attribution | ≥ 80% of growth attributed to experiments | Revenue attribution model |

---

# 10. Chief Orchestration Agent

The Chief Orchestration Agent is the meta-intelligence layer that coordinates all other agents, resolves conflicts, allocates resources, and ensures the entire multi-agent system operates as a coherent whole.

## 10.1 Core Competencies

### 10.1.1 Task Routing & Scheduling

The agent maintains a global task graph and ensures optimal execution:

- **Dependency Resolution:** Topological sorting of tasks across agents to identify the critical path and parallelizable work.
- **Resource Allocation:** Dynamic allocation of compute, context window budget, and priority slots based on sprint goals.
- **Deadline Management:** Backward scheduling from ship dates with buffer allocation and early warning systems.
- **Load Balancing:** Detection and redistribution of work when any agent approaches capacity limits.

### 10.1.2 Conflict Resolution Protocol

When agents produce conflicting outputs or disagree on approach:

| Conflict Type | Resolution Method | Escalation Path |
|--------------|-------------------|-----------------|
| Technical feasibility vs. UX requirements | Joint session with quantified tradeoff matrix | Product Management Agent decides |
| Performance vs. feature richness | Benchmarked comparison of both approaches | Data-driven decision by Orchestration Agent |
| Security vs. user experience | Risk quantification with user impact assessment | Security wins unless impact is catastrophic |
| Timeline vs. quality | Scope reduction negotiation | Product Management Agent re-prioritizes |
| Cost vs. capability | ROI analysis with FinOps Agent input | Capital Strategy Agent advises |

### 10.1.3 System Health Monitoring

The Orchestration Agent continuously monitors the health of the entire multi-agent system:

- Agent responsiveness and output quality scores tracked per cycle.
- Inter-agent communication latency and failure rate monitoring.
- Knowledge graph consistency checks to detect conflicting world models.
- Sprint velocity tracking with variance analysis and forecasting.
- Automated retrospective generation synthesizing learnings across all agents.

## 10.2 Orchestration Patterns

> **Pattern: Sprint Cycle Orchestration**
>
> **DAY 0 (Planning):** Product Agent presents prioritized backlog → Orchestration Agent generates task graph → All agents receive assignments.
>
> **DAYS 1–N (Execution):** Agents execute in parallel → Orchestration Agent monitors progress → Blockers detected and routed in real-time.
>
> **DAY N (Review):** SDTE Agent presents quality report → All agents present outputs → Orchestration Agent synthesizes sprint summary.
>
> **DAY N+1 (Retro):** Strategic Reflection Agent presents patterns → Orchestration Agent updates routing rules → Process improvements logged.

## 10.3 Evaluation Criteria

| Metric | Target | Measurement |
|--------|--------|-------------|
| Task routing accuracy | ≥ 98% correct first-assignment | Rerouting frequency analysis |
| Conflict resolution time | < 1 cycle for standard conflicts | Conflict resolution log timestamps |
| System throughput | ≥ 90% of planned work completed per sprint | Sprint completion metrics |
| Agent utilization balance | No agent > 120% or < 60% of mean load | Workload distribution analysis |
| Retrospective insight quality | ≥ 3 actionable improvements per sprint | Implementation tracking |

---

# 11. Inter-Agent Communication Protocol

All agents communicate using a standardized message protocol that ensures traceability, conflict detection, and knowledge persistence.

## 11.1 Message Schema

```json
{
  "message_id": "UUID",
  "timestamp": "ISO-8601",
  "source_agent": "string",
  "destination_agent": "string | string[]",
  "message_type": "request | response | notification | escalation",
  "priority": "critical | high | medium | low",
  "payload": {
    "task_id": "string",
    "content": "object",
    "dependencies": "string[]",
    "deadline": "ISO-8601"
  },
  "context": {
    "sprint_id": "string",
    "prd_id": "string",
    "related_messages": "string[]"
  },
  "acknowledgment": {
    "required": "boolean",
    "timeout_ms": "number"
  }
}
```

## 11.2 Communication Topology

The system uses a hub-and-spoke model with the Chief Orchestration Agent as the primary router, supplemented by direct peer-to-peer channels for high-frequency interactions:

| Channel | Pattern | Participants | Use Case |
|---------|---------|-------------|----------|
| Orchestration Bus | Hub-and-spoke | All agents ↔ Orchestration Agent | Task assignment, escalation, status |
| Engineering Mesh | Peer-to-peer | Frontend ↔ Backend ↔ DB ↔ DevOps | API contracts, schema changes, deploys |
| Product Broadcast | Pub-sub | Product Agent → All downstream | PRD updates, priority changes |
| Quality Loop | Request-response | SDTE Agent ↔ Engineering agents | Bug reports, test results, fix verification |
| Growth Channel | Peer-to-peer | Growth ↔ Product ↔ Frontend | Experiment specs, feature flags, results |

## 11.3 Knowledge Graph Integration

Every agent interaction contributes to a shared knowledge graph managed by the Memory & Knowledge Graph Agent. This ensures that decisions, context, and learnings persist across sprints and inform future agent behavior. Key graph entities include: Product decisions (PRDs, prioritization rationale, experiment results), Technical decisions (architecture records, technology selections, performance benchmarks), Process decisions (workflow optimizations, communication pattern adjustments), and Risk signals (identified risks, near-misses, incident post-mortems).

---

# 12. Training Evaluation Framework

Each agent is evaluated through a multi-stage assessment that tests both individual competence and collaborative effectiveness.

## 12.1 Evaluation Stages

| Stage | Description | Pass Criteria | Weight |
|-------|-------------|--------------|--------|
| 1. Unit Assessment | Agent performs domain-specific tasks in isolation | ≥ 90% on domain-specific benchmarks | 30% |
| 2. Integration Assessment | Agent handles handoffs with 2–3 adjacent agents | ≥ 85% contract compliance, < 5% rework | 25% |
| 3. System Assessment | Full pipeline simulation from PRD to deployment | Successful E2E delivery within time budget | 25% |
| 4. Adversarial Assessment | Agent handles malformed inputs, conflicting instructions, and edge cases | ≥ 95% graceful degradation | 10% |
| 5. Retrospective Assessment | Agent demonstrates learning from simulated sprint cycles | Measurable improvement in re-test scores | 10% |

## 12.2 Simulation Scenarios

Training evaluation includes the following end-to-end simulation scenarios:

- **Greenfield MVP:** Build a complete MVP from a one-paragraph product idea through to production deployment.
- **Feature Addition:** Add a complex feature (e.g., real-time collaboration) to an existing codebase with backward compatibility.
- **Crisis Response:** Handle a production outage requiring coordinated diagnosis and fix across multiple agents.
- **Pivot Scenario:** Re-architect a product based on changed market conditions, preserving existing user data and experience.
- **Scale Challenge:** Adapt the system from 100 to 100,000 concurrent users with cost-efficient infrastructure changes.

## 12.3 Continuous Improvement Loop

> **Post-Training Improvement Protocol**
>
> 1. Weekly performance digest generated by Strategic Reflection Agent.
> 2. Agent-specific retraining triggered when any core metric drops below threshold for 2 consecutive sprints.
> 3. Cross-agent communication pattern optimization based on message latency and rework rate analysis.
> 4. Knowledge graph pruning and consolidation to prevent context bloat.
> 5. Quarterly full-system benchmark against updated industry standards.

## 12.4 Certification Levels

| Level | Requirements | Autonomy Granted |
|-------|-------------|-----------------|
| L1 — Supervised | Passes Stage 1; requires human approval for all outputs | Draft generation only |
| L2 — Guided | Passes Stages 1–2; human reviews critical decisions | Autonomous for routine tasks |
| L3 — Autonomous | Passes Stages 1–4; demonstrates consistent quality | Full autonomy with audit trail |
| L4 — Self-Improving | Passes all stages; shows measurable self-improvement | Can propose and implement process changes |

---

**END OF TRAINING MODULE**

TechFondue · Claude Skills Division · February 2026
