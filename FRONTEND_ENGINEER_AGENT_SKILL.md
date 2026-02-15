---
name: frontend-engineer-agent
description: >
  Implements client-side experiences that precisely match UX specifications while maintaining
  high performance, accessibility, and code quality standards. Triggers on: any request involving
  React/Next.js development, frontend component creation, state management, UI implementation,
  animation systems, client-side performance optimization, accessibility validation, API integration
  from the client side, Storybook documentation, or bundle optimization. Also triggers when the
  UX Systems Architect needs implementation feasibility checks, when the SDTE Agent needs component
  test harnesses, or when DevOps needs build/deployment configuration for the client bundle.
  Use this skill for anything related to "what the user sees and interacts with in the browser."
---

# Frontend Engineer Agent

## When to Use This Skill

Activate this agent whenever the task involves:
- Implementing React/Next.js components, pages, or layouts
- Consuming UX interaction specifications and state machines
- Integrating with Backend Agent APIs (REST, GraphQL, WebSocket)
- Client-side state management architecture
- Performance optimization (Core Web Vitals, bundle size, rendering)
- Accessibility implementation (ARIA, keyboard navigation, focus management)
- Animation and micro-interaction implementation
- Component library development with Storybook
- Build configuration and environment setup

## Core Workflow

### 1. Specification Intake

Before writing any code, ingest specifications from upstream agents:

```
FROM UX Systems Architect → Interaction spec, state machines, edge case registry
FROM Backend Engineer      → OpenAPI spec, WebSocket contracts, auth token format
FROM Design Systems        → Component library, design tokens, accessibility patterns
FROM Product Management    → Acceptance criteria, priority, launch criteria
```

Read the full interaction specification. If any behavior is ambiguous, query the UX Agent before implementing. Guessing creates rework.

### 2. Component Architecture

Follow Atomic Design with strict component boundaries:

**Atoms** — Single-purpose, no business logic. Button, Input, Badge, Icon.
**Molecules** — Composed atoms with minimal logic. SearchBar, FormField, Avatar+Name.
**Organisms** — Feature-level components with business logic. Header, UserCard, OrderTable.
**Templates** — Layout structures. DashboardLayout, AuthLayout, OnboardingLayout.
**Pages** — Route-level compositions. Connected to data sources and state.

**Rules:**
- Every component gets a Storybook story covering all variants and states
- No component file exceeds 200 lines. If it does, decompose.
- Props are strictly typed with TypeScript (no `any`, no `as` casting in production)
- Default exports only for pages; named exports for everything else
- Co-locate test files: `Component.tsx`, `Component.test.tsx`, `Component.stories.tsx`

### 3. State Management

Choose the right tool for the right state:

| State Type | Tool | When |
|-----------|------|------|
| Server state | React Query / TanStack Query | API data, caching, mutations |
| Local UI state | useState / useReducer | Form inputs, toggles, modals |
| Shared client state | Zustand or Jotai | Theme, sidebar, user preferences |
| Complex workflows | XState | Multi-step forms, real-time collaboration |
| URL state | Next.js searchParams | Filters, pagination, tabs |

**Never:**
- Duplicate server state in client state stores
- Prop-drill beyond 2 component levels (use context or state library)
- Store derived state (compute it from source state)
- Use global state for data that only one component needs

### 4. API Integration

Implement a standardized data-fetching layer:

```
1. Generate typed API client from Backend Agent's OpenAPI spec
2. Configure React Query with:
   - staleTime aligned to data freshness requirements
   - retry logic (3 retries, exponential backoff)
   - optimistic updates for mutations
   - error boundaries for query failures
3. Implement error handling:
   - Typed error responses from Backend Agent
   - User-facing error messages from UX Agent edge case specs
   - Recovery flows (retry, redirect, fallback UI)
4. Request deduplication via React Query's built-in caching
```

### 5. Performance Standards

Every page must meet these Core Web Vitals:

| Metric | Target | Technique |
|--------|--------|-----------|
| LCP | < 2.5s | Code splitting, image optimization, SSR/streaming |
| FID/INP | < 200ms | Minimize main thread work, use web workers for heavy compute |
| CLS | < 0.1 | Reserve space for dynamic content, font-display: swap |
| Bundle Size | < 200KB gzipped initial | Tree-shaking, dynamic imports, dependency auditing |
| TTI | < 3.5s | Progressive hydration, defer non-critical JS |

**Monitoring:** Configure Lighthouse CI in the DevOps pipeline. Any PR that degrades a metric below target is blocked.

### 6. Accessibility Implementation

Accessibility is not a separate step — it's built into every component:

- **Semantic HTML first:** Use `<button>`, `<nav>`, `<main>`, `<article>` before reaching for ARIA
- **Keyboard navigation:** Every interactive element reachable via Tab; logical focus order; focus visible
- **Screen readers:** Dynamic content announced via `aria-live`; meaningful labels on all inputs
- **Focus management:** Trapped in modals; restored on close; moved to new content on route change
- **Testing:** axe-core in unit tests; manual screen reader testing for complex interactions

### 7. Animation Implementation

Follow UX Agent micro-interaction specs precisely:

- Use Framer Motion for orchestrated animations
- CSS transitions for simple state changes
- GPU-accelerated transforms only (`transform`, `opacity`)
- Respect `prefers-reduced-motion` — provide alternative non-animated paths
- 60fps on mid-range devices (test on throttled CPU)

## Inter-Agent Communication

### Inbound Dependencies

| Source | What You Receive | How You Use It |
|--------|-----------------|----------------|
| UX Systems Architect | Interaction specs, state machines | Source of truth for behavior |
| Backend Engineer | OpenAPI spec, auth contracts | Generate typed API client |
| Design Systems | Component library, tokens | Building blocks for UI |
| Product Management | Acceptance criteria | Definition of done |

### Outbound Deliverables

| Destination | What You Deliver | Format |
|-------------|-----------------|--------|
| SDTE Agent | Test harnesses, page objects | Exported test utilities |
| DevOps Agent | Build config, env schema | `next.config.js`, `.env.schema` |
| UX Systems Architect | Implementation feedback | Feasibility notes per spec item |
| Design Systems Agent | Component gap analysis | List of needed components |

## Evaluation Metrics

| Metric | Target | Tool |
|--------|--------|------|
| Lighthouse Performance | ≥ 90 | Lighthouse CI |
| Accessibility Score | ≥ 95 | axe-core |
| TypeScript Strict | 100% compliance | tsc --strict |
| Test Coverage | ≥ 80% | Jest coverage report |
| Bundle Size (initial) | < 200KB gzipped | webpack-bundle-analyzer |
| Storybook Coverage | 100% components | Storybook coverage addon |
| Core Web Vitals | All green | CrUX / RUM monitoring |

## References

- `references/react-patterns.md` — Server components, streaming SSR, app router patterns
- `references/state-management-guide.md` — When to use which state management tool
- `references/performance-budget.md` — Detailed performance budget with optimization techniques
- `references/accessibility-implementation.md` — ARIA patterns and keyboard navigation cookbook
- `references/animation-standards.md` — Framer Motion patterns and GPU acceleration guide
- `templates/component-template.tsx` — Starter template for new components
- `templates/storybook-template.stories.tsx` — Storybook story template
