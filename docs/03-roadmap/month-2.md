---
description: Acceleration - Speed and Safety at Scale
---

# Month 2: Acceleration

Phase 3 of 4. Builds on the foundations from [Week 3-4](week-3-4.md). See the [timeline overview](README.md) for the full plan.

## Goals

- Hit the under-30-minute deployment target
- Implement feature flags to decouple merge from deploy
- Add automated rollback
- Improve staging/production parity

## Activities by Owner

### Me

| Activity | Ships |
|----------|-------|
| Implement feature flags (LaunchDarkly or Unleash, depending on budget) | Flag system integrated with Django app |
| Push deploy time under 30 minutes, enable multiple deploys/day | Main-to-prod pipeline fully streamlined |
| Work with teams on the 6-month feature: understand blast radius, gate behind flag | Feature safely merged to main with flag off |

### Engineer A

| Activity | Ships |
|----------|-------|
| Automated deployment health checks | Error rate spike triggers auto-rollback |
| Canary/progressive rollout | Percentage-based traffic shifting |
| Continue alert refinement | 80%+ actionable ratio (stretch: 90%) |

### Engineer B

| Activity | Ships |
|----------|-------|
| Staging/prod parity improvements | Anonymized data snapshots, matching topology |
| Second Datadog training session | Hands-on debugging workshop |
| "Debugging in production" guide | Written reference for common investigation patterns |

### Dev Team

| What Changes for Them |
|----------------------|
| PRs can be merged at any time, no more waiting for deployments |
| First blameless postmortem with the broader team |

## Feature Flag Implementation

### Evaluation

| Criteria | LaunchDarkly | Unleash (open source) |
|----------|--------------|----------------------|
| Setup time | Fast | Medium (need to host it) |
| Cost | Significant | Free (self-hosted) |
| Features | Full suite | Covers our needs |
| Maintenance | Managed | We maintain it |

If spend is truly "negligible" as a concern, LaunchDarkly gets us moving faster. If there's any budget sensitivity, Unleash does the job.

### Rollout Strategy for Flagged Features

| Phase | Audience | Duration | Proceed When |
|-------|----------|----------|-------------|
| 0 | Internal users | 3 days | No errors |
| 1 | 5% of users | 3 days | Error rate < 0.1% |
| 2 | 25% of users | 3 days | Error rate < 0.1% |
| 3 | 100% of users | — | Stable |

## Automated Rollback

Triggers:

- 5xx error rate > 1% for 2 minutes → automatic rollback
- p99 latency > 2000ms for 5 minutes → alert, then rollback
- Health check failure > 20% for 1 minute → immediate rollback

Flow: deploy v2 → health check → shift 10% traffic → monitor 5 min → error rate OK? → shift to 50% → 100%. At any point, error threshold exceeded → rollback to v1 in under 60 seconds.

## Staging/Production Parity

We need realistic data in staging without exposing PHI:

```bash
# Weekly job: dump prod, scrub PII, restore to staging
pg_dump --no-owner production_db | \
  python scripts/scrub_phi.py --config phi_fields.yaml | \
  psql staging_db
```

The `scrub_phi.py` script needs to be thorough. We're dealing with patient data. Hash or fake names, SSNs, contact info, medical notes. This is a prerequisite, not a nice-to-have.

Infrastructure matching (proportional, not identical):

| Component | Production | Staging (target) |
|-----------|------------|-----------------|
| App instances | 6 | 2 |
| DB instance | r5.xlarge | r5.large |
| Redis | Cluster mode | Cluster mode |
| External services | Live | Sandbox |

## De-risking the 6-Month Feature

This is running in parallel with the rest of the work. The feature has been in progress for 6 months, impacts the whole app, and likely lives on a diverging branch.

| Week | Action | Owner |
|------|--------|-------|
| 5 | Meet with feature team, understand current merge strategy | Me |
| 6 | Wrap behind feature flag | Me + Feature Lead |
| 7 | Begin incremental merges to main (flag off) | Feature Team |
| 8 | Add dedicated monitoring dashboard | Engineer B |

## Deliverables by End of Month 2

| Deliverable | Target |
|-------------|--------|
| Deployment time | Under 30 minutes |
| Automated rollback | Operational and tested |
| PR merge blocking | Eliminated |
| Feature flags | Operational |
| 6-month feature | Merged to main behind flag |
| Staging parity | Reproduces prod behavior with anonymized data |
