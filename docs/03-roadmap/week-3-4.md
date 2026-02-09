---
description: Building the Foundations
---

# Week 3-4: Foundations

Phase 2 of 4. Builds on the discovery and quick wins from [Week 1-2](week-1-2.md). See the [timeline overview](README.md) for the full plan.

## Goals

- Begin blue/green deployment implementation
- Execute alert rationalization
- Launch access management automation MVP
- Define initial SLOs

## Activities by Owner

### Me

| Activity | Ships |
|----------|-------|
| Design and start implementing blue/green deployment on Fargate | Architecture doc + working POC |
| Build DB migration safety framework | Pre-deploy linting, lock checks, review process |
| Define initial SLOs with eng leadership | Availability, latency, error rate targets with baselines |
| Security checkpoint before blue-green goes to prod | Security team review + sign-off on the dual-environment design |
| Rollback drill after blue-green implementation | Deploy a canary with an intentional error, trigger rollback, measure recovery time |
| Establish infrastructure cost baseline | Track spend alongside deployment time and MTTR so we can justify the investment |

### Engineer A

| Activity | Ships |
|----------|-------|
| Execute alert rationalization + evaluate AI filtering | Delete noise, identify patterns, assess tools like Datadog AIOps or PagerDuty Event Intelligence |
| Write runbooks for top 15 actionable alerts | Linked from Datadog |
| Create on-call handoff template + escalation matrix | Standard docs linked from Datadog |
| Build incident commander checklist | Step-by-step for Sev-1/Sev-2 |

### Engineer B

| Activity | Ships |
|----------|-------|
| Implement automated access provisioning | Self-service with approval workflow + audit logging |
| Build first Datadog dashboard template | Deployed for 2 pilot teams |

### Dev Team

| Activity | Ships |
|----------|-------|
| Attend "Observability 101" session | Recorded for async access |
| Read first platform newsletter | Awareness of what's changing and why |

## Blue/Green Deployment Design

```
┌─────────────────────────────────────────────────────────┐
│                         ALB                             │
└─────────────────────────┬───────────────────────────────┘
                          │
           ┌──────────────┴──────────────┐
           │                             │
           ▼                             ▼
┌─────────────────────┐       ┌─────────────────────┐
│  Target Group Blue  │       │  Target Group Green │
│     (Current)       │       │      (New)          │
└──────────┬──────────┘       └──────────┬──────────┘
           │                             │
           ▼                             ▼
┌─────────────────────┐       ┌─────────────────────┐
│   ECS Service v1    │       │   ECS Service v2    │
│   (100% traffic)    │       │   (Health check)    │
└─────────────────────┘       └─────────────────────┘
```

Flow: deploy new task definition to green → health checks on /healthz → wait for all tasks healthy → shift ALB listener → monitor error rates for 5 min → if errors spike, automatic rollback to blue → if stable, drain and terminate blue.

## Migration Safety Framework

### CI checks (pre-merge)

`django-migration-linter` blocks migrations with full table rewrites, NOT NULL without defaults, and missing FK indexes.

### Pre-deploy checks

Estimate migration duration, check for table locks, require platform team sign-off for schema changes.

### Large migration policy

| Size | Strategy |
|------|----------|
| < 10k rows | Normal migration in deploy pipeline |
| 10k - 100k rows | Off-peak deployment window |
| > 100k rows | Async Celery task with batching, separate from deploy |

## Initial SLOs

| Service | Metric | Target | How We Measure |
|---------|--------|--------|----------------|
| API | Availability | 99.9% | Successful requests / total |
| API | Latency (p99) | < 500ms | Datadog APM |
| API | Error rate | < 0.1% | 5xx responses / total |
| Deployments | Success rate | 100% | Successful / attempted |
| Deployments | Duration | < 30 min | Start to traffic shift |

These are starting points. We'll adjust after a month of baseline data.

## Deliverables by End of Month 1

| Deliverable | Target |
|-------------|--------|
| Deployment time | Under 1 hour (stretch: under 30 min) |
| Migration safety rails | Live in CI |
| Alert count | Reduced by 40%+ |
| Top alerts | Have runbooks |
| SLOs | Defined and baselined |
| Access automation | MVP live |
| First observability training | Completed |
| Security sign-off on blue-green | Approved before production traffic |
| Rollback drill completed | Recovery time measured and documented |
| Infrastructure cost baseline | Tracked and shared |
