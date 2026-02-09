---
description: Executive one-pager for PDF export
---

<style>
/* PDF-friendly single-page layout */
@media print {
  nav, .md-sidebar, .md-header, .md-footer, .md-tabs, .md-top { display: none !important; }
  .md-content { margin: 0 !important; padding: 0 !important; max-width: 100% !important; }
  .md-main__inner { margin: 0 !important; }
  body { font-size: 7pt; line-height: 1.15; }
  h1 { font-size: 12pt; margin: 0 0 1px 0; }
  h2 { font-size: 8.5pt; margin: 3px 0 1px 0; border-bottom: 0.5pt solid #ddd; padding-bottom: 1px; }
  table { font-size: 6.5pt; margin: 1px 0 3px 0; }
  table th, table td { padding: 1px 3px !important; }
  p, li { font-size: 7pt; margin: 1px 0; }
  hr { margin: 2px 0; }
  img { max-height: 80px !important; width: auto !important; margin: 2px 0 !important; }
}
</style>

# Platform Engineering: 90-Day Plan

**Amanda Souza** · Senior Platform Engineer · February 2026 <br>
**Full plan:** [amandasouza.app/platform-eng-playbook](https://www.amandasouza.app/platform-eng-playbook)
---

## The Problem

Two Sev-1 incidents last month, both from deployments. 4-hour deploy cycles block 50 engineers from merging (~50-75 lost engineer-hours/week). On-call can't keep up with alert noise, and only 3 people can debug production.

## Proposal 1: Safe & Fast Deployment Pipeline

Blue/green deployments, DB migration linter + lock timeouts in CI, feature flags, automated rollback. ~4 weeks, me + one engineer.

![Blue/Green Deployment Flow](images/blue-green-deployment.svg)

| Metric | Today | Target | Stretch |
|--------|-------|--------|---------|
| Deploy time | ~4 hours | **< 1 hour** | < 30 min |
| Sev-1 from deploys | 2/month | **0** | — |
| Deploy frequency | 1x/day | **2-3x/day** | 5+/day |
| Engineers blocked | ~50 | **0** | 0 |

## Proposal 2: Observability & Incident Response

Alert audit + cleanup, runbooks for actionable alerts, SRE champions program (1-2 per team), staging parity for prod debugging. ~3 weeks, me + one engineer.

![Observability & Incident Response Flow](images/observability-flow.svg)

| Metric | Today | Target | Stretch |
|--------|-------|--------|---------|
| Actionable alerts | Unknown (low) | **> 80%** | > 90% |
| MTTR (Sev-1) | Hours | **< 1 hour** | < 30 min |
| Engineers who debug prod | ~3 | **15-20** | All 50 (6-mo) |
| Staging reproduces prod | Rarely | **60-70%** | 80%+ |

## 90-Day Roadmap

![90-Day Platform Engineering Roadmap](images/90-day-timeline.svg)

| Phase | When | Key Deliverables |
|-------|------|-----------------|
| **Listen & Learn** | Week 1–2 | Pipeline mapped, DB timeouts + migration linter live, alert audit complete, stakeholder alignment |
| **Foundations** | Week 3–4 | Blue/green deploys, deploys < 1 hr, 40%+ alert noise cut, incident response framework, rollback drill |
| **Acceleration** | Month 2 | Feature flags operational, automated rollback, staging parity, SRE champions trained |
| **Scale** | Month 3 | 1 service extracted, SLO dashboards, leadership review against success criteria |

## Team

| Person | Focus |
|--------|-------|
| **Me** | Pipeline architecture, blue/green, feature flags, service extraction, leadership alignment |
| **Engineer A** | Alert audit, runbooks, deployment automation, SLO dashboards |
| **Engineer B** | Access automation, staging parity, developer self-service |

## Top Risks

| Risk | What we do about it |
|------|---------------------|
| Scope creep from ad-hoc requests | Strict prioritization, weekly review with manager |
| Engineer pulled to other work | Highest-impact items first; defer, don't dilute |
| Resistance to new deploy process | We ship unblocked PRs and faster merges first. Once engineers feel that, they're on board |

## Why This Order

Pipeline first because nothing else works without reliable deploys. Can't run microservices on a 4-hour deploy cycle. Can't improve reliability if only 3 people can see what's happening. Quick wins in month 1 build credibility for harder changes later.

---

*This is a living document. The full plan has proposals, roadmaps, technical details, and supporting material.*

