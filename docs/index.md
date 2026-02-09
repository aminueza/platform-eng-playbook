---
description: 90-Day Platform Engineering Plan
---

# Platform Engineering: 90-Day Plan

**Author:** Amanda Souza<br>
**Date:** February 2026<br>
**Role:** Senior Platform Engineer (IC-3)

## The Core Insight

Almost every pain point in this scenario traces back to the deployment pipeline. Two Sev-1s last month and both deployment-related. Fifty engineers blocked from merging PRs daily. On-call overwhelmed because they can't debug what they can't observe.

Fix the pipeline first, and you unlock everything else: developer productivity, incident response, and the foundation the CEO needs for service decomposition.

## Executive Summary

### Current State

| Area | Impact |
|------|--------|
| 4-hour deployments | ~50-75 lost engineer-hours/week (10-15 engineers blocked per deploy, ~1 hr context-switch cost each, 5 deploys/week) |
| 2 Sev-1 incidents | Both DB-related, both during deployments |
| Alert fatigue | On-call overwhelmed, doesn't know what to action |
| Observability gap | Only the platform team can effectively debug production |

### Where We'll Be in 90 Days

| Metric | Today | Day 90 |
|--------|-------|--------|
| Deploy time | ~4 hours | < 1 hour (stretch: < 30 min) |
| Sev-1 from deploys | 2/month | 0 |
| MTTR (Sev-1) | Hours | < 1 hour (stretch: < 30 min) |
| Engineers who can debug prod | ~3 | 15-20 (via SRE champions) |

## How to Read This Plan

Read in this order:

1. **[Assessment](01-assessment/README.md):** What's actually going on beneath the symptoms, and which problems to solve first.
2. **[Proposals](02-proposals/README.md):** Two concrete initiatives with expected impact: the deployment pipeline and the observability program.
3. **[Roadmap](03-roadmap/README.md):** Week-by-week execution plan with ownership and deliverables for each phase.
4. **[Communication](04-communication/README.md):** How I'd keep leadership, engineers, and cross-functional teams informed throughout.
5. **[Appendix](05-appendix/clarifying-questions.md):** Supporting material: clarifying questions, the [microservices conversation](05-appendix/microservices-conversation.md), [de-risking the 6-month feature](05-appendix/derisking-6-month-feature.md), [gaps analysis](05-appendix/gaps-and-considerations.md), and [references](05-appendix/references.md).
