---
description: Two concrete technical proposals to address critical pain points
---

# Technical Proposals

Based on the assessment, I'm proposing two initiatives that tackle the highest-impact problems with the team we have (me + 2 engineers).

## Proposal 1: Safe & Fast Deployment Pipeline

4-hour deployments blocking 50 engineers + 2 Sev-1 incidents from migrations. This is the single biggest lever we can pull.

- Deploy time: 4 hours to under 1 hour (stretch: under 30 minutes)
- Sev-1 from deploys: 2/month to 0
- Engineers blocked: 50 to 0
- **Investment:** ~4 weeks (me + 1 engineer)

[Full proposal](deployment-pipeline.md)

## Proposal 2: Alert-to-Runbook Program & Observability Enablement

On-call is overwhelmed and developers can't debug production. We need to make observability an engineering-wide capability, not a platform-team monopoly.

- Alert noise: unknown baseline, target 80%+ actionable (stretch: 90%)
- MTTR: hours to under 1 hour (stretch: under 30 minutes)
- Devs who can debug: a handful to 15-20 via SRE champions program
- **Investment:** ~3 weeks (me + 1 engineer)

[Full proposal](observability-alerts.md)

## Why These Two First

They're foundational and everything else depends on them:

- Can't run microservices on a broken deployment pipeline
- Can't improve reliability without observability across the team
- Can't scale the platform team's impact without developer self-service

They also have immediate ROI. If 10-15 engineers are blocked from merging per deploy and each loses roughly 1 hour to waiting and context switching, that's 50-75 engineer-hours per week across 5 deploys. Proposal 2 makes on-call sustainable before someone burns out.

And practically: quick, visible wins in the first month build trust for the harder initiatives that come later.
