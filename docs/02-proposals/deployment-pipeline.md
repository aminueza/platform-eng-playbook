---
description: From 4-hour deploys to under 1 hour (stretch target 30 minutes) with automated rollback
---

# Proposal 1: Safe & Fast Deployment Pipeline

## The Problem

Deployments take 4 hours, block 50 engineers from merging PRs, and caused both Sev-1 incidents this month. This is the single largest source of both instability and wasted developer time.

## What We Build

### Phase 1: DB Migration Safety Rails (Week 1-2)

The two Sev-1s were both database-related. Before we speed up deployments, we need to make them safe.

**Add timeouts to prevent runaway locks:**

```python
# settings.py (enforce at the connection level)
DATABASES = {
    'default': {
        ...
        'OPTIONS': {
            'options': '-c statement_timeout=30000 -c lock_timeout=4000'
        }
    }
}
```

This would have prevented Sev-1 #1 (the dev script that locked half the DB for 6 hours). With a 4-second lock timeout, it fails fast instead of blocking everything.

**What else goes in:**

- `django-migration-linter` in CI to catch full table rewrites, NOT NULL without defaults, missing FK indexes
- Separate migration PRs that require platform team review
- Large data migrations (>100k rows) run as batched Celery tasks, not in the deploy pipeline

### Phase 2: Blue/Green Deployments on Fargate (Month 1)

![Blue-Green Deployment](../images/blue-green-deployment.svg)

Deploy the new version alongside the current one. Health checks pass? Shift the ALB. They don't? The old version never stopped serving traffic. Rollback goes from "hours of scrambling" to "point the target group back." Seconds.

### Phase 3: Feature Flags + Merge Unblocking (Month 2)

The key principle: **merged code is not released code.**

Right now, devs can't merge to main during deployments because there's no way to control what's live. Feature flags fix that. Engineers merge continuously, and we control what's active in production separately from what's deployed.

Evaluate [LaunchDarkly](https://launchdarkly.com/pa/feature-flags/) (fast setup, $$) vs. [Unleash](https://docs.getunleash.io/concepts/feature-flags) (self-hosted, free) based on budget. Start with simple on/off Release flags; add targeting and variants later as needed.

## Expected Impact

| Metric | Before | After (confident) | Stretch |
|--------|--------|-------------------|---------|
| Deploy time | ~4 hours | < 1 hour | < 30 minutes (depends on test suite health) |
| Deploy frequency | 1x/day | 2-3x/day | 5+/day (requires team trust in the pipeline) |
| Engineers blocked by deploy | ~50 | 0 | 0 |
| Sev-1 from deploys (last month) | 2 | Target: 0 (same failure modes eliminated) | New failure modes still possible |
| Rollback time (app-only) | Hours (manual) | < 60 seconds (ALB swap) | < 60 seconds (ALB swap) |
| Rollback time (with migration) | Hours (manual) | Minutes (if migration is reversible) | Still complex for irreversible schema changes |

## Investment

~4 weeks of focused effort (me + 1 engineer).

| Week | Focus | Ships |
|------|-------|-------|
| 1-2 | DB migration safety | Lock timeouts + linter in CI |
| 3-4 | Blue/green deploy | ECS rolling update with health gates |
| 5-6 | Feature flags | Flag system integrated |
| 7-8 | Hardening | Automated rollback on error rate spikes, runbooks |

## Risks

| Risk | How We Handle It |
|------|------------------|
| Feature flag complexity grows | Start with on/off flags only, add targeting when there's a real need |
| Blue/green doubles infra during deploys | Green environment only lives during the deploy window; tear down after cutover |
| Migration linter blocks legitimate changes | Platform team can override with documented justification. The goal is awareness, not a wall |
| Team pushback on new process | Show the quick wins first. Nobody argues with "your PRs aren't blocked anymore" |

## How We Know It Worked

- Deploy time is consistently under 1 hour, trending toward 30 minutes
- Zero Sev-1 incidents from deployments for 30 days
- Engineers can merge PRs at any time without coordinating with on-call
- App-only rollback executes in under 60 seconds
