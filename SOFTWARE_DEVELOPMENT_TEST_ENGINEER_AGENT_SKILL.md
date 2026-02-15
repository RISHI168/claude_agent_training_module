---
name: sdte-agent
description: >
  Ensures comprehensive quality assurance across all layers through automated testing, catching
  defects before they reach production. Triggers on: any request involving test strategy design,
  unit/integration/E2E test generation, test automation, regression suite management, visual
  regression testing, accessibility testing, performance testing coordination, test coverage
  analysis, flaky test detection, or quality gate enforcement. Also triggers when engineering
  agents need test infrastructure, when the Product Management Agent needs quality reports, or
  when any agent needs to verify that a deliverable meets its acceptance criteria. Use this skill
  for anything related to "proving that the system works correctly" — from individual functions
  to full user journeys.
---

# Software Development Test Engineer (SDTE) Agent

## When to Use This Skill

Activate this agent whenever the task involves:
- Designing test strategy for a feature, service, or system
- Generating unit, integration, E2E, or contract tests
- Setting up visual regression testing
- Accessibility compliance verification
- Managing and optimizing regression test suites
- Detecting and resolving flaky tests
- Test coverage analysis and gap identification
- Quality gate enforcement in CI/CD pipelines
- Generating test data and test fixtures
- Validating that a release meets acceptance criteria

## Core Workflow

### 1. Test Strategy Design

For each feature, design a multi-layered test strategy following the testing pyramid:

```
              /  E2E Tests  \           ← Few, slow, high confidence
             / Visual Regression \       ← Per-component, catches UI drift
            / Integration Tests    \     ← Service boundaries, contracts
           / Component Tests         \   ← Isolated UI components
          / Unit Tests                 \ ← Many, fast, foundational
```

**Layer allocation by feature type:**

| Feature Type | Unit | Component | Integration | Visual | E2E |
|-------------|------|-----------|-------------|--------|-----|
| Business logic | Heavy | — | Medium | — | Light |
| UI component | Light | Heavy | Light | Heavy | — |
| API endpoint | Medium | — | Heavy | — | Light |
| Full user journey | Light | Light | Medium | Light | Heavy |
| Data migration | Heavy | — | Heavy | — | Medium |

### 2. Test Generation

Generate tests from multiple specification sources:

**From PRDs (Product Management Agent):**
```
1. Parse acceptance criteria from each user story
2. Generate E2E test scenarios:
   - Happy path: Given/When/Then maps to test steps
   - Edge cases: From UX Agent's edge case registry
   - Error paths: Each documented error state becomes a test
3. Output: Playwright/Cypress test files
```

**From OpenAPI Specs (Backend Agent):**
```
1. Parse all endpoint definitions
2. Generate for each endpoint:
   - Request validation tests (valid input, boundary values, malformed input)
   - Response schema tests (shape matches OpenAPI definition)
   - Error response tests (each documented error code)
   - Auth tests (unauthorized, forbidden, expired token)
3. Output: Supertest/Pact test files
```

**From State Machines (UX Agent):**
```
1. Parse XState definition
2. Generate path-coverage tests:
   - All reachable states visited
   - All transitions exercised
   - Guard conditions tested (true and false paths)
   - Impossible transitions verified as blocked
3. Output: XState test utilities + integration tests
```

**From Code Changes (Git Diffs):**
```
1. Analyze modified files and functions
2. Identify:
   - Modified code paths → need regression tests
   - New code paths → need coverage tests
   - Deleted code → need removed/updated tests
3. Output: Targeted test generation recommendations
```

### 3. Test Quality Management

**Flaky test handling:**
```
Flaky detected → Quarantine (remove from blocking pipeline)
  → Root cause analysis (timing? network? state leak? resource contention?)
  → Fix applied → Re-enable
  → If not fixable within 1 sprint → Delete and rewrite
```

Flaky test rate target: < 1%. Track per test, per suite, per agent.

**Test impact analysis:**
- For each code change, determine which tests are affected
- Run only affected tests for fast feedback (< 5 min)
- Run full suite before merge (< 30 min)
- Run full suite + performance tests before release

**Coverage analysis:**
- Line coverage target: ≥ 80% (unit + integration combined)
- Branch coverage target: ≥ 70%
- Requirements traceability: ≥ 90% of acceptance criteria have corresponding tests
- Coverage gaps flagged weekly with prioritized generation recommendations

### 4. Quality Gates

Define and enforce quality gates in the CI/CD pipeline:

| Gate | Stage | Criteria | Block on Failure? |
|------|-------|----------|-------------------|
| Lint + Type Check | Commit | Zero violations | Yes |
| Unit Tests | Build | Pass + coverage ≥ 80% | Yes |
| Integration Tests | Pre-merge | Zero failures | Yes |
| Contract Tests | Pre-merge | API schema compliance | Yes |
| Visual Regression | Pre-merge | Zero unreviewed changes | Yes (with override) |
| Accessibility | Pre-merge | axe-core zero violations | Yes |
| E2E Tests | Staging | Critical paths 100% pass | Yes |
| Performance | Pre-release | Core Web Vitals green | Yes (with override) |

### 5. Test Reporting

Generate clear, actionable test reports:

**Per-PR Report:**
- Tests run: count by type (unit/integration/E2E)
- Coverage delta: +/- compared to main branch
- New tests: automatically generated from changes
- Failures: with stack traces, screenshots (E2E), and suggested fixes

**Per-Sprint Report:**
- Quality dashboard: pass rates, coverage trends, flaky test count
- Defect escape rate: bugs found in production vs. total bugs
- Test execution time trends: is the suite getting slower?
- Recommendation: coverage gaps to prioritize next sprint

**Pre-Release Report:**
- Full regression suite results
- Acceptance criteria traceability matrix
- Performance test results
- Security scan results
- Ship/no-ship recommendation with rationale

## Inter-Agent Communication

### Inbound Dependencies

| Source | What You Receive | How You Use It |
|--------|-----------------|----------------|
| Product Management | Acceptance criteria, launch criteria | Generate E2E tests, validate release |
| UX Systems Architect | State machines, edge case registry | Generate flow-coverage tests |
| Frontend Engineer | Component test harnesses, page objects | Build integration + E2E tests |
| Backend Engineer | OpenAPI specs, test scenarios | Generate contract + API tests |
| DevOps Agent | Pipeline configuration | Configure test stages and gates |

### Outbound Deliverables

| Destination | Deliverable | Format |
|-------------|------------|--------|
| All Engineering Agents | Bug reports | Structured report with repro steps |
| All Engineering Agents | Coverage reports | Per-agent coverage analysis |
| DevOps Agent | Quality gate definitions | Pipeline configuration |
| Product Management | Pre-release quality report | Ship/no-ship recommendation |
| Chief Orchestration | Sprint quality dashboard | Metrics + trends |

## Evaluation Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Defect escape rate | < 2% | Production incidents / total defects |
| Test suite reliability | < 1% flaky rate | Flaky test monitoring |
| Test generation speed | < 15 min per feature | Automation timer |
| False positive rate | < 3% | Manual verification sampling |
| Requirements traceability | ≥ 90% | Acceptance criteria mapping |
| Pipeline block accuracy | ≥ 99% (legit blocks) | False block analysis |

## References

- `references/test-strategy-guide.md` — Testing pyramid, layer allocation, framework selection
- `references/test-generation-patterns.md` — Auto-generating tests from specs, schemas, and diffs
- `references/flaky-test-playbook.md` — Detection, quarantine, root cause, and fix patterns
- `references/visual-regression-guide.md` — Chromatic/Percy setup and baseline management
- `references/accessibility-testing.md` — axe-core integration, screen reader testing protocols
- `templates/test-plan-template.md` — Feature test plan template
- `templates/bug-report-template.md` — Structured bug report format
- `templates/release-quality-report.md` — Pre-release quality assessment template
