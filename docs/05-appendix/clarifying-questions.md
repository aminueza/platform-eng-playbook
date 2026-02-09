---
description: Questions to validate assumptions before committing to the plan
---

# Clarifying Questions

These are the things I'd want to understand in the first week. Some of them might change the plan.

## Deployment Pipeline

**What are the specific steps in the 4-hour deployment, and where does the time go?** Build time? Test execution? Migration? Manual verification? Traffic shift? Understanding the bottleneck distribution determines whether we optimize the pipeline itself or the process around it.

**How long does the nightly test suite run, and what's the false positive rate?** If it's flaky, that's likely contributing to the slow deploy cadence. People wait and re-run rather than trusting results.

## The 6-Month Feature

**What's the current merge strategy?** If it's a long-lived feature branch, it's diverging from main daily, and the longer we wait, the harder the merge. If it's already being merged incrementally, the risk profile is very different.

## Database

**Is the 500k-record concern about query latency, migration speed, or storage?** 500k rows is small for PostgreSQL. This likely points to missing indexes or ORM query patterns rather than a data volume problem. Knowing which hot paths are slow would let us fix this quickly.

## Staging vs. Production

**What specifically differs?** Data volume, infrastructure topology, third-party integrations, configuration? The answer determines whether fixing staging parity is a 2-week project or a 2-month one.

## On-Call

**What's the current rotation structure?** How many people, what schedule, what's the escalation path? Is there an incident commander role, or does on-call handle everything solo?

## Compliance

**Which tables and services handle PHI?** Is there a data classification inventory? Are we under active HIPAA audit? This affects how we approach access management automation and staging data anonymization.

## Observability

**Which services produce the "failed to send 1 trace" logs, and how often?** Is this the Datadog agent sidecar on Fargate or the ddtrace library in the Django app? The fix is different depending on where the problem lives.

## Discovery Conversations

Beyond the specific questions above, I'd want to ask:

**Engineers (1:1s):** What's the most frustrating part of your day-to-day? When was the last time a deployment caused you problems? How often do you need to debug something in production? What tools do you wish you had?

**Engineering Leadership:** What are your top 3 infrastructure concerns? How do you measure engineering productivity today? What would success look like at 90 days?

**Product/Business:** What features are blocked by infrastructure limitations? How do deployment delays impact product timelines? What's the business cost of an hour of downtime during US business hours?
