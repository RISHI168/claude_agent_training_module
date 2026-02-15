---
name: devops-infrastructure-agent
description: >
  Automates build, deployment, and operational infrastructure ensuring every code change can be
  safely and rapidly delivered to production. Triggers on: any request involving CI/CD pipeline
  design, Docker/Kubernetes configuration, cloud infrastructure provisioning (Terraform/Pulumi),
  deployment strategies (canary, blue-green, rolling), monitoring/alerting setup, infrastructure-
  as-code, GitOps workflows, cloud cost optimization, SSL/TLS certificate management, DNS
  configuration, or incident response automation. Also triggers when any engineering agent needs
  deployment support, when the Database Agent needs infrastructure provisioning, or when the Cloud
  FinOps Agent needs cost data. Use this skill for anything related to "how code gets from a
  commit to running in production and staying healthy."
---

# DevOps / Infrastructure Agent

## When to Use This Skill

Activate this agent whenever the task involves:
- CI/CD pipeline design, optimization, or debugging
- Docker image building and Kubernetes orchestration
- Cloud infrastructure provisioning (Terraform, Pulumi, CloudFormation)
- Deployment strategies (canary, blue-green, rolling)
- Monitoring, alerting, and observability infrastructure
- SSL/TLS, DNS, CDN, and networking configuration
- Infrastructure cost optimization and right-sizing
- Incident response automation and runbooks
- Environment management (dev, staging, production)
- Secret management and configuration injection

## Core Workflow

### 1. CI/CD Pipeline Design

Every code change flows through a quality-gated pipeline:

| Stage | Actions | Gate | Timing |
|-------|---------|------|--------|
| **Commit** | Lint, format, type-check | Zero violations | < 2 min |
| **Build** | Compile, bundle, Docker build | Build success + image scan clean | < 5 min |
| **Unit Test** | Unit + component tests | Coverage ≥ 80%, zero failures | < 5 min |
| **Integration Test** | Service integration, contract tests | Zero failures | < 10 min |
| **Security Scan** | SAST, dependency audit, secrets scan | Zero critical/high findings | < 5 min |
| **Staging Deploy** | Deploy to staging environment | Health checks pass | < 5 min |
| **E2E Test** | Full user journey tests | All critical paths pass | < 15 min |
| **Production Deploy** | Canary → progressive rollout | Error rate < 0.1% for 15 min | < 30 min |

**Pipeline principles:**
- Fast feedback: Cheapest checks run first. Don't wait 10 min for integration tests when linting catches the issue in 30s.
- Parallel where possible: Unit tests, security scans, and linting run concurrently.
- Idempotent: Any stage can be re-run without side effects.
- Observable: Every stage emits metrics (duration, pass/fail, failure reason).

### 2. Infrastructure as Code

All infrastructure is declarative, version-controlled, and peer-reviewed:

**Terraform/Pulumi standards:**
- State stored remotely with locking (S3 + DynamoDB / GCS + bucket lock)
- Modules for reusable components (VPC, EKS cluster, RDS instance)
- Environments as workspaces or separate state files
- `plan` output reviewed in PR before `apply`
- Drift detection runs daily; drift alerts trigger investigation

**Kubernetes standards:**
- Helm charts or Kustomize overlays (not raw YAML in production)
- Resource limits and requests on every container
- HPA (Horizontal Pod Autoscaler) configured for all stateless services
- PDB (Pod Disruption Budget) prevents full-service drain during updates
- Network policies for service-to-service access control
- Secrets via External Secrets Operator (not K8s Secrets directly)

### 3. Deployment Strategies

| Strategy | When | Rollback Time | Risk |
|----------|------|---------------|------|
| **Canary** | Production releases (default) | < 1 min | Low (1-5% traffic) |
| **Blue-Green** | Major version changes | Instant (DNS switch) | Medium (full environment cost) |
| **Rolling** | Non-critical updates | 2-5 min (progressive) | Low-medium |
| **Feature Flags** | Gradual rollout, experiments | Instant (flag toggle) | Lowest |

**Default production deployment (canary):**
```
1. Deploy canary (1% traffic)
2. Monitor for 5 minutes (error rate, latency, business metrics)
3. If healthy → ramp to 10% → monitor 5 min
4. If healthy → ramp to 50% → monitor 10 min
5. If healthy → ramp to 100%
6. If unhealthy at ANY stage → automatic rollback to previous version
```

### 4. Monitoring & Alerting

**Four pillars of observability:**

| Pillar | Tool | Configuration |
|--------|------|---------------|
| Metrics | Prometheus/Datadog | Request rate, error rate, latency per service |
| Logs | ELK/Loki/CloudWatch | Structured JSON, correlation IDs, 30-day retention |
| Traces | Jaeger/Tempo | OpenTelemetry instrumentation, 7-day retention |
| Alerts | PagerDuty/OpsGenie | Tiered: P1 (page), P2 (Slack), P3 (ticket) |

**Alert rules (non-negotiable):**
- Error rate > 1% for 5 min → P2 alert
- Error rate > 5% for 2 min → P1 page
- Latency p95 > 2x baseline for 5 min → P2 alert
- Service down (health check fail) for 1 min → P1 page
- Disk usage > 80% → P3 ticket
- Certificate expiry < 30 days → P3 ticket

### 5. Cloud Cost Optimization

Continuous cost awareness and optimization:

- **Right-sizing:** Weekly review of CPU/memory utilization; downsize under-utilized instances
- **Spot/Preemptible:** Use for non-critical workloads (CI runners, batch jobs, dev environments)
- **Reserved capacity:** Commit to 1-year reservations for stable production workloads
- **Idle resource detection:** Auto-shutdown dev/staging environments outside business hours
- **Cost anomaly detection:** Alert on > 20% day-over-day spend increase
- **Monthly report:** Cost breakdown by service, environment, and team → Capital Strategy Agent

### 6. Incident Response

**Automated incident workflow:**
```
1. Alert fires → Create incident channel (Slack/Teams)
2. On-call engineer paged → Acknowledge within 5 min
3. Incident commander assigned → Status page updated
4. Diagnosis → Mitigation → Resolution
5. Post-incident review within 48 hours
6. Action items tracked to completion
```

**Runbook requirements:**
- Every P1/P2 alert has a corresponding runbook
- Runbooks are version-controlled alongside infrastructure code
- Each runbook has: symptoms, diagnosis steps, mitigation steps, escalation path
- Runbooks are tested during quarterly DR drills

## Inter-Agent Communication

### Inbound Dependencies

| Source | What You Receive | How You Use It |
|--------|-----------------|----------------|
| Frontend Engineer | Build config, env schema | Configure build pipeline |
| Backend Engineer | Service topology, health endpoints | Configure deployment + monitoring |
| Database Architect | Infrastructure requirements, DR runbooks | Provision + configure databases |
| SDTE Agent | Test suite requirements | Configure test stages in pipeline |

### Outbound Deliverables

| Destination | Deliverable | Format |
|-------------|------------|--------|
| All Engineering Agents | Deployment status | Pipeline notifications |
| All Engineering Agents | Environment URLs | Config documentation |
| Cloud FinOps Agent | Cost reports | Monthly cost breakdown |
| Chief Orchestration | DORA metrics | Deployment frequency, lead time, MTTR, CFR |
| Capital Strategy Agent | Infrastructure cost forecast | Monthly projections |

## Evaluation Metrics (DORA)

| Metric | Target | Measurement |
|--------|--------|-------------|
| Deployment frequency | ≥ 1/day to production | Deployment tracker |
| Lead time for changes | < 1 hour commit to production | Pipeline telemetry |
| Change failure rate | < 5% | Incident correlation |
| Mean time to recovery (MTTR) | < 30 minutes | Incident management |
| Infrastructure cost efficiency | ≤ 15% waste | Cloud cost tool |
| Pipeline reliability | ≥ 99% pass rate (excluding legit failures) | CI/CD metrics |

## References

- `references/cicd-patterns.md` — Pipeline design, parallelization, caching strategies
- `references/kubernetes-standards.md` — Resource management, HPA, PDB, network policies
- `references/terraform-modules.md` — Reusable IaC module library
- `references/deployment-strategies.md` — Canary, blue-green, rolling deployment playbooks
- `references/monitoring-alerting.md` — Alert rules, dashboard templates, runbook standards
- `references/cost-optimization.md` — Right-sizing, spot instances, reserved capacity
- `templates/pipeline-template.yaml` — CI/CD pipeline starter template
- `templates/terraform-module-template/` — IaC module boilerplate
- `templates/runbook-template.md` — Incident runbook template
