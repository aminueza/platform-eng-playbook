---
description: Strategy for safely releasing the high-risk 6-month feature
---

# De-risking the 6-Month Feature

A major feature that's been in progress for 6 months and "impacts the whole app" is the kind of thing that causes outages. Especially in a monolith with no feature flags and a deployment process that already has reliability problems.

---

## The Risk

| Factor | Severity | Why It Matters |
|--------|----------|---------------|
| Long-lived branch | High | Diverging from main every day; merge conflicts compound |
| Big-bang merge likely | High | Massive PR that nobody can meaningfully review |
| No feature flag | High | Once it's live, there's no off switch |
| Impacts the whole app | High | Blast radius is maximum, every user affected |
| No dedicated monitoring | Medium | We won't know it's broken until users tell us |
| No rollback plan | High | Recovery time is unknown |

This has a real chance of causing a significant incident if we don't manage it carefully.

## What I'd Recommend

### 1. Understand the current state (Week 1)

Is this a long-lived feature branch? How often is it rebased from main? How many files have diverged? Who owns the merge decision? The answers determine how urgent the flag-wrapping is.

### 2. Wrap it behind a feature flag (Week 1-2)

Even if this takes a week of dedicated effort, it's worth it. The feature flag gives us:

- The ability to merge to main immediately (flag off = old behavior)
- Testing in production with limited users before full rollout
- An instant kill switch if something goes wrong
- Gradual rollout rather than all-or-nothing

### 3. Merge incrementally (Weeks 3-4)

Instead of one massive PR:

```
Week 1: Merge database schema changes (flag off)
Week 2: Merge new models and services (flag off)
Week 3: Merge UI changes (flag off)
Week 4: Merge integration points (flag off)
Week 5: Enable for internal users
Week 6: Enable for 5% of users
...
```

Each merge is small enough to review, small enough to debug, and small enough that if something breaks, we know exactly where to look.

### 4. Add dedicated monitoring

Build a dashboard specific to this feature:

- Feature flag evaluation count (are users hitting the new code path?)
- Error rate comparison: new code vs. legacy
- Latency comparison: new code vs. legacy
- Business metrics (are the core workflows still completing?)

Alert if error rate in the new path exceeds 1% or latency exceeds 2x the legacy path.

### 5. Phased rollout with clear rollback criteria

| Phase | Audience | Duration | Rollback Trigger |
|-------|----------|----------|-----------------|
| 0 | Internal users | 3 days | Any bug |
| 1 | 5% of users | 3 days | Error rate > 1% |
| 2 | 25% of users | 3 days | Error rate > 0.5% |
| 3 | 50% of users | 3 days | Error rate > 0.3% |
| 4 | 100% | — | Error rate > 0.1% |

Rollback at any phase = disable the feature flag. No deployment needed. Users fall back to the legacy code path in seconds.

## Timeline

| Week | Action | Owner |
|------|--------|-------|
| 1 | Meet with feature team, understand current state | Me |
| 1-2 | Design and implement feature flag wrapper | Me + Feature Lead |
| 2 | Create monitoring dashboard | Engineer B |
| 3-4 | Incremental merges to main (flag off) | Feature Team |
| 5 | Enable for internal users | Feature Lead |
| 6+ | Phased rollout per the table above | Feature Lead + Platform |

## The Goal

Turn the feature launch from a high-risk moment ("merge it, deploy it, hope it works") into a controlled, observable, reversible event. The feature flag is what makes this possible. It decouples "code is in main" from "feature is live."
