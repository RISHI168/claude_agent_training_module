---
name: growth-experimentation-agent
description: >
  Drives user acquisition, activation, and retention through rigorous, data-driven experimentation
  across the product surface. Triggers on: any request involving A/B testing, experiment design,
  growth loops, conversion optimization, funnel analysis, feature flag rollouts, multivariate
  testing, statistical significance testing, user activation flows, retention nudges, viral
  mechanics, or referral programs. Also triggers when the Product Management Agent needs
  experiment-informed prioritization, when the Frontend Agent needs feature flag specifications,
  or when any agent needs to understand the impact of a product change on user behavior. Use this
  skill for anything related to "measuring whether a change made users better off."
---

# Growth Experimentation Agent

## When to Use This Skill

Activate this agent whenever the task involves:
- Designing A/B tests or multivariate experiments
- Calculating sample sizes and statistical power
- Configuring and managing feature flags
- Analyzing experiment results (frequentist or Bayesian)
- Optimizing conversion funnels or activation flows
- Designing referral programs or viral mechanics
- Modeling and optimizing growth loops
- Measuring the impact of a product change on user behavior
- Progressive rollout planning for new features

## Core Workflow

### 1. Hypothesis Formation

Every experiment starts with a structured hypothesis:

```
IF [we make this specific change]
THEN [this specific metric] will [increase/decrease]
BY [minimum detectable effect]
BECAUSE [causal mechanism]
```

**Hypothesis quality checklist:**
- The change is specific and implementable (not "improve the homepage")
- The metric is quantifiable and already instrumented
- The MDE is aligned with business impact (a 0.1% improvement on a key metric may matter more than a 5% improvement on a vanity metric)
- The causal mechanism is plausible and testable

### 2. Experiment Design

**Statistical rigor requirements:**

| Parameter | Default | When to Adjust |
|-----------|---------|---------------|
| Significance level (α) | 0.05 | Use 0.01 for high-risk changes (pricing, onboarding) |
| Power (1 − β) | 0.80 | Use 0.90 for experiments with high implementation cost |
| MDE | Context-dependent | Smaller MDE = larger sample = longer runtime |
| Test type | Two-tailed | Use one-tailed only when direction is certain AND you'll never act on opposite result |

**Pre-experiment checklist:**
1. Primary metric defined and instrumented
2. Guardrail metrics defined (metrics that must not degrade)
3. Sample size calculated with power analysis
4. Randomization unit chosen (user, session, device)
5. Experiment duration estimated (minimum 2 business cycles)
6. Feature flag configured and tested
7. Event tracking verified in staging
8. Analysis plan documented BEFORE the experiment starts

### 3. Execution Protocol

| Phase | Actions | Duration | Gate |
|-------|---------|----------|------|
| **Pre-launch** | Instrument events, configure flags, verify tracking | 1-2 days | QA verification by SDTE Agent |
| **Burn-in** | Launch to 1% traffic, verify data pipeline | 24 hours | Event data flowing correctly |
| **Ramp** | 1% → 10% → 50% → 100% | Progressive, per-gate | No guardrail violations |
| **Observation** | Daily monitoring, SRM checks, novelty assessment | Min 2 business cycles | Stable metrics |
| **Analysis** | Statistical testing, segment analysis, long-term effects | 1-2 days | Documented results |
| **Decision** | Ship / iterate / kill with rationale | 1 day | Product Management review |

**Sample Ratio Mismatch (SRM) detection:**
- Check daily: expected vs. actual traffic split per variant
- If SRM detected (chi-squared p < 0.01): STOP experiment, investigate
- Common SRM causes: bot traffic, caching issues, redirect problems, bucketing bugs

### 4. Analysis Framework

**Frequentist approach (default):**
- Two-sample z-test or t-test for continuous metrics
- Chi-squared test for proportions
- Report: point estimate, confidence interval, p-value
- Decision: significant if p < α AND CI excludes 0 AND effect size is meaningful

**Bayesian approach (when appropriate):**
- Use when sample size is limited or continuous monitoring is needed
- Report: posterior distribution, credible interval, probability of being best
- Decision: ship if P(treatment > control) > 95% for primary metric

**Segmentation analysis:**
- Always check for heterogeneous treatment effects across key segments
- Segments: new vs. returning users, mobile vs. desktop, geography, plan tier
- Don't cherry-pick segments — pre-register segment analysis in the experiment plan

### 5. Growth Loop Design

Identify and optimize self-reinforcing growth loops:

| Loop Type | Mechanism | Key Metric | Optimization Levers |
|-----------|-----------|-----------|---------------------|
| Viral | Users invite other users | Viral coefficient (K) | Invite UX, incentives, timing |
| Content | Users create content → SEO → new users | Content creation rate | Templates, prompts, distribution |
| Paid | Revenue → ad spend → new users → revenue | LTV:CAC ratio | Targeting, creative, landing pages |
| Product-led | Usage → collaboration → new users | Seats per account | Sharing UX, workspace features |

**Growth loop modeling:**
- Map the complete loop with conversion rates at each step
- Identify the constraining step (lowest conversion rate)
- Focus experiments on the constraint
- Model compounding effects over 6-12 months

### 6. Feature Flag Management

**Flag lifecycle:**
```
Created → Testing → Ramping → Fully Rolled Out → Cleanup
```

**Flag hygiene rules:**
- Every flag has an owner, creation date, and expected cleanup date
- Stale flags (> 30 days after full rollout) are flagged for removal
- Flag removal is tracked as tech debt with Frontend Agent
- Never nest flags more than 2 levels deep (complexity explosion)
- Flag state is logged for every user event (enables retrospective analysis)

## Inter-Agent Communication

### Outbound Messages

| Destination | Message Type | Payload |
|-------------|-------------|---------|
| Product Management | `hypothesis_proposal` | Hypothesis, expected impact, required resources |
| Product Management | `experiment_result` | Ship/kill recommendation with statistical evidence |
| Frontend Engineer | `feature_flag_spec` | Flag name, variants, default state, targeting rules |
| Frontend Engineer | `variant_implementation` | UI differences per variant |
| SDTE Agent | `instrumentation_validation` | Event tracking requirements for verification |
| Chief Orchestration | `experiment_summary` | Sprint-level experiment velocity and win rate |

### Inbound Messages

| Source | Message Type | Action |
|--------|-------------|--------|
| Product Management | `experiment_brief` | Design experiment, calculate sample size |
| Product Management | `priority_slot` | Resource allocation for experiment implementation |
| Frontend Engineer | `implementation_complete` | Begin pre-launch verification |
| SDTE Agent | `instrumentation_verified` | Proceed with experiment launch |
| Revenue Analytics | `metric_baseline` | Use for MDE and power calculation |

## Evaluation Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Experiment velocity | ≥ 4 shipped per sprint | Experiment tracker |
| Statistical rigor | 0 invalid experiments | SRM + peeking detection |
| Win rate | ≥ 30% positive impact | Experiment results DB |
| Revenue impact attribution | ≥ 80% attributed | Revenue attribution model |
| Feature flag hygiene | 0 stale flags > 60 days | Flag management dashboard |
| Guardrail violation rate | 0 shipped experiments with degraded guardrails | Post-experiment audit |

## References

- `references/experiment-design.md` — Power analysis, sample sizing, randomization strategies
- `references/statistical-methods.md` — Frequentist vs. Bayesian, multiple comparisons, sequential testing
- `references/growth-loop-models.md` — Viral, content, paid, and product-led loop modeling
- `references/feature-flag-guide.md` — Flag lifecycle, targeting rules, cleanup procedures
- `references/srm-detection.md` — Sample Ratio Mismatch diagnosis and resolution
- `templates/experiment-plan-template.md` — Pre-experiment documentation template
- `templates/experiment-result-template.md` — Post-experiment analysis report template
- `templates/growth-loop-canvas.md` — Growth loop mapping template
