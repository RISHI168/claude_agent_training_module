# Startup Product Development — Multi-Agent System

## System Overview

This is a coordinated multi-agent architecture for autonomous end-to-end startup product development. Nine specialized agents collaborate through standardized protocols to take a product from concept to stable release.

## Agent Registry

| Agent | Directory | Layer | Role |
|-------|-----------|-------|------|
| Product Management | `product-management-agent/` | Layer 2 | Product strategy, PRDs, prioritization |
| UX Systems Architect | `ux-systems-architect-agent/` | Layer 2 | Interaction flows, state machines, edge cases |
| Frontend Engineer | `frontend-engineer-agent/` | Layer 3 | Client-side implementation |
| Backend Engineer | `backend-engineer-agent/` | Layer 3 | Server-side architecture & APIs |
| Database Architect | `database-architect-agent/` | Layer 3 | Data modeling, queries, DR |
| DevOps/Infrastructure | `devops-infrastructure-agent/` | Layer 3 | CI/CD, IaC, cloud operations |
| SDTE | `sdte-agent/` | Layer 4 | Testing strategy & quality assurance |
| Growth Experimentation | `growth-experimentation-agent/` | Layer 6 | A/B testing, growth loops, experiments |
| Chief Orchestration | `chief-orchestration-agent/` | Layer 8 | System coordination & conflict resolution |

## File Convention

Each agent directory contains:
- `SKILL.md` — Trigger conditions, capabilities, workflow instructions, and integration protocols
- `CLAUDE.md` — Behavioral identity, decision-making heuristics, communication style, and autonomy boundaries
- `references/` — Domain-specific deep-dive documentation
- `templates/` — Output templates and schemas
- `scripts/` — Automation scripts for deterministic tasks

## Communication Protocol

All agents communicate via a standardized message schema:
```json
{
  "message_id": "UUID",
  "source_agent": "string",
  "destination_agent": "string | string[]",
  "message_type": "request | response | notification | escalation",
  "priority": "critical | high | medium | low",
  "payload": {},
  "context": { "sprint_id": "", "prd_id": "" },
  "acknowledgment": { "required": true, "timeout_ms": 30000 }
}
```

## Sprint Lifecycle

```
Planning → Execution → Review → Retrospective → Next Sprint
   ↑                                                  |
   └──────────────────────────────────────────────────┘
```

The Chief Orchestration Agent manages this cycle. All other agents participate according to their role at each phase.
