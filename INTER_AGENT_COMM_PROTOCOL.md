# Inter-Agent Communication Protocol

## Message Schema (v2.0)

All inter-agent communication uses this standardized message format:

```json
{
  "message_id": "uuid-v4",
  "timestamp": "ISO-8601",
  "source_agent": "agent-name",
  "destination_agent": "agent-name | agent-name[]",
  "message_type": "request | response | notification | escalation",
  "priority": "critical | high | medium | low",
  "payload": {
    "task_id": "string",
    "content": {},
    "dependencies": ["task-id-1", "task-id-2"],
    "deadline": "ISO-8601",
    "acceptance_criteria": []
  },
  "context": {
    "sprint_id": "string",
    "prd_id": "string",
    "related_messages": ["message-id-1"],
    "knowledge_graph_refs": ["node-id-1"]
  },
  "acknowledgment": {
    "required": true,
    "timeout_ms": 30000,
    "retry_count": 3
  }
}
```

## Message Types

### Request
Asks an agent to perform work. Requires acknowledgment within timeout.

### Response
Delivers the result of a request. References the original message_id.

### Notification
Informational broadcast that doesn't require a response. Used for status updates, decision announcements, and FYI messages.

### Escalation
Signals that an agent cannot resolve an issue within its domain. Always routes to the Chief Orchestration Agent first, who then routes to the appropriate resolver.

## Communication Channels

| Channel | Pattern | Participants | Use Case |
|---------|---------|-------------|----------|
| Orchestration Bus | Hub-and-spoke | All ↔ Orchestration | Task assignment, escalation, status |
| Engineering Mesh | Peer-to-peer | FE ↔ BE ↔ DB ↔ DevOps | API contracts, schema, deploys |
| Product Broadcast | Pub-sub | Product → All downstream | PRD updates, priority changes |
| Quality Loop | Request-response | SDTE ↔ Engineering agents | Bugs, test results, verification |
| Growth Channel | Peer-to-peer | Growth ↔ Product ↔ Frontend | Experiments, flags, results |

## Handoff Protocol

When one agent's output becomes another agent's input:

1. **Sender** marks deliverable as complete with quality self-assessment
2. **Sender** sends `handoff_notification` with deliverable location and schema version
3. **Receiver** acknowledges receipt within timeout
4. **Receiver** validates deliverable against expected schema
5. **If valid** → Receiver begins work, sends `accepted` response
6. **If invalid** → Receiver sends `rejected` response with specific issues
7. **If rejected** → Sender fixes issues and re-initiates handoff
8. **If rejected 2x** → Escalate to Chief Orchestration Agent

## Conflict Escalation Format

```json
{
  "message_type": "escalation",
  "payload": {
    "conflict_type": "feasibility | priority | quality | timeline | resource",
    "agents_involved": ["agent-a", "agent-b"],
    "positions": {
      "agent-a": "Description of agent-a's position",
      "agent-b": "Description of agent-b's position"
    },
    "impact_if_unresolved": "What happens if this isn't resolved",
    "recommended_resolution": "The escalating agent's suggestion",
    "deadline_for_resolution": "ISO-8601"
  }
}
```

## Knowledge Graph Contribution

Every significant decision, deliverable, or learning is recorded:

```json
{
  "node_type": "decision | deliverable | learning | risk",
  "title": "Short description",
  "content": "Detailed content",
  "author_agent": "agent-name",
  "sprint_id": "string",
  "related_nodes": ["node-id-1"],
  "tags": ["architecture", "performance", "security"],
  "status": "active | superseded | archived"
}
```

Agents query the knowledge graph before making decisions that might conflict with prior decisions. The Chief Orchestration Agent validates consistency.
