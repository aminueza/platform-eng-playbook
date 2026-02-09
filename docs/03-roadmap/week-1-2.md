---
description: Listen, Learn, Ship Quick Wins
---

# Week 1-2: Listen, Learn, Ship Quick Wins

Phase 1 of 4. See the [timeline overview](README.md) for how this fits the full plan.

## Goals

- Understand the real problems firsthand, not from a scenario doc
- Build relationships with the engineering team
- Ship quick wins that demonstrate immediate value
- Create the foundation for the larger initiatives

## Activities by Owner

### Me

| Activity | Why |
|----------|-----|
| Shadow on-call for 2-3 rotations | Nothing replaces feeling the pain yourself |
| Run postmortems on both Sev-1s (or review existing ones) | Need to understand root causes deeply, not just the symptoms |
| Map the deployment pipeline end-to-end | Document every step, time each one, find the bottlenecks |
| 20-min 1:1s with 6-8 engineers | One question: "What slows you down most?" |
| Align with manager and eng leadership | Confirm priorities match, get buy-in before building |
| Agree on 3-5 success metrics with leadership | If the CEO's definition of success differs from ours, we'll find out in Month 3 the hard way |
| Quick security posture assessment | Does a security team exist? What are their requirements? Need to know before we ship pipeline changes |
| Document current DR procedures | Backups tested? RTO/RPO defined? If not, we need to know now |
| Map stakeholder dependencies | Who needs to approve what (security sign-off, finance for tooling, legal for data anonymization) |

### Engineer A

| Activity | Why |
|----------|-----|
| Document the current deployment process step-by-step with timing | We need a baseline before we can improve |
| Inventory every Datadog alert, classify as actionable/investigate/noise | Prep for alert rationalization in weeks 3-4 |
| Identify top 10 flakiest tests and their impact on deploy time | If 30% of the 4-hour deploy is flaky test retries, fixing the pipeline alone won't get us to 30 minutes |

### Engineer B

| Activity | Why |
|----------|-----|
| Audit current access management: tools, processes, approval chains | Understand the toil and compliance gaps |
| Research SSO/SCIM options for the current toolset | Prep for automation in weeks 3-4 |

## Quick Wins (Ship in Week 1-2)

These aren't big projects. They're targeted fixes that address real pain and can ship in hours.

### Fix "failed to send 1 trace"

Production telemetry is being lost silently. Likely cause: Datadog agent resource limits on Fargate (CPU/memory too low) or a VPC endpoint / security group issue. Investigate and fix. Probably 2-4 hours of work to restore visibility we don't know we're missing.

### Add Database Timeouts

This would have prevented Sev-1 #1. Add connection-level timeouts in Django:

```python
# settings.py
DATABASES = {
    'default': {
        ...
        'OPTIONS': {
            'options': '-c statement_timeout=30000 -c lock_timeout=4000'
        }
    }
}
```

A runaway query or script now fails after 4 seconds instead of locking half the database for 6 hours. 1-2 hours of work + testing.

### Add Migration Linter to CI

This would have caught Sev-1 #2. Add `django-migration-linter` to the CI pipeline:

```yaml
# .github/workflows/ci.yml
- name: Lint migrations
  run: |
    pip install django-migration-linter
    python manage.py lintmigrations --no-cache
```

Catches dangerous migrations (full table rewrites, NOT NULL without defaults) before they reach production. 2-4 hours.

## Deliverables by End of Week 2

| Deliverable | Owner |
|-------------|-------|
| Root cause analysis for both Sev-1s | Me |
| Deployment pipeline map with bottleneck annotations | Me + Engineer A |
| Alert audit spreadsheet with classifications | Engineer A |
| Access management audit with recommendations | Engineer B |
| Security posture assessment | Me |
| DR procedures documented (or gaps noted) | Me |
| Stakeholder dependency map | Me |
| Success metrics agreed with leadership | Me |
| Flaky test report with deploy time impact | Engineer A |
| Quick wins live in production | All |

## Key Meetings

| Meeting | Who | Outcome |
|---------|-----|---------|
| 1:1s with 6-8 engineers | Individual devs | Pain points documented, trust established |
| Manager sync | Direct manager | Priorities confirmed, expectations set |
| Leadership alignment | Eng leadership | Buy-in on 90-day plan |
