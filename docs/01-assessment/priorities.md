---
description: Ordered by impact and dependency
---

# Top 5 Priorities

Based on the [situation analysis](README.md), here's the priority order. Each item unlocks the next.

### 1. Deployment Pipeline & DB Migration Safety

Both Sev-1 incidents originated here. Four-hour deployments block engineers from merging PRs. If 10-15 engineers have PRs ready during each deploy window and each loses about an hour to waiting and context switching, that's 50-75 engineer-hours per week across 5 deploys, and that's before counting the incident cost.

This is simultaneously the biggest stability risk and the biggest productivity bottleneck. Everything else we want to do (observability, decoupling, feature flags) depends on a deployment pipeline that works.

**Target:** Deployments under 1 hour (stretch: under 30 minutes), zero Sev-1s from migrations, engineers merge PRs at any time.

### 2. Alert Rationalization & Incident Response

On-call is overwhelmed and often doesn't know what to do with alerts. That's the textbook definition of alert fatigue, and it's the fastest way to miss a real incident or extend one that's already happening.

**Target:** Intelligent alert filtering to suppress noise and correlate related signals, only actionable alerts reach on-call, MTTR cut in half.

### 3. Observability Democratization

Right now, only the platform team can effectively use Datadog. That means 3 people debugging for 50. If we get the broader engineering team self-sufficient with observability tools, we multiply our troubleshooting capacity dramatically.

This also addresses the staging/prod parity gap. Devs report they can't reproduce prod issues, so bugs only surface in production where they hurt customers.

**Target:** Standard dashboards per domain, developers debug production independently, staging reproduces prod behavior.

### 4. Access Management Automation

Manual credential management 2-3 times a week is toil. But with PHI in scope, it's also a compliance risk because every unaudited access grant is a potential HIPAA finding.

**Target:** Self-service access with approval workflows, full audit trail, ~2-3 hours/week of toil eliminated.

### 5. Strategic Decoupling Groundwork

The CEO wants microservices in 3 months. I get why: deployment independence and risk isolation are real needs. But attempting service decomposition before we have reliable deployments and observability would be building on sand.

This priority lays the groundwork responsibly: feature flags, identified domain boundaries, and one extracted service as a proof-of-concept.

**Target:** Feature flags live, service boundaries identified, one bounded context extracted.
