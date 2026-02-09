---
description: From alert fatigue to actionable observability for all engineers
---

# Proposal 2: Alert-to-Runbook Program & Observability Enablement

## The Problem

On-call is overwhelmed and doesn't know what to do with most alerts. Developers can't use Datadog. Staging doesn't reproduce production issues. The net effect: incidents take longer to detect, longer to resolve, and only a handful of people can help.

## What We Build

### Phase 1: Alert Audit and Intelligent Filtering (Weeks 2-4)

Every alert gets classified:

| Category | Definition | What Happens |
|----------|------------|--------------|
| **A (Actionable)** | Clear runbook, immediate action needed | Keep, document the runbook |
| **B (Unclear)** | Might matter, needs analysis | 2 weeks to become A, or it's deleted |
| **C (Noise)** | No clear action, historical cruft | Delete immediately |

In my experience, 40-60% of alerts in an unmanaged setup fall into category C. Deleting them isn't losing coverage, but it's restoring signal.

**Modern approach:** Evaluate AI-powered alert intelligence (Datadog AIOps, PagerDuty Event Intelligence, or open-source alternatives) to:
- Automatically correlate related alerts into single incidents
- Suppress known transient issues (e.g., brief CPU spikes during deployments)
- Learn patterns of what's actionable vs. noise over time
- Only page on-call for real incidents with aggregated context

This is increasingly how modern ops teams handle alert volume. Let the AI filter out the noise so on-call sees 10 meaningful alerts instead of 100 mixed signals.

**Going forward:** alerts either have runbooks or flow through intelligent filtering before reaching on-call.

### Phase 2: Structured Incident Response (Month 1)

We're missing basic incident infrastructure:

| What We Create | What It Does |
|----------------|-------------|
| On-call handoff doc | What's hot, what changed recently, what to watch |
| Escalation matrix | Who owns what domain, who to call at 2am |
| Incident commander checklist | Step-by-step for Sev-1/Sev-2 so on-call isn't improvising |
| Postmortem template | Blameless format with action item tracking |

None of this is exotic. It's table stakes that's currently missing. See the full [Incident Management Process](incident-process.md) for roles, severity levels, and the end-to-end flow.

**Past incident correlation:** Configure Datadog Incident Management (or PagerDuty Event Intelligence, depending on tooling choice) to surface similar past incidents when a new one is created. When on-call gets paged, they immediately see "this looks like the incident from 2 weeks ago where we did X." This turns postmortems from documents nobody re-reads into context that shows up exactly when it's useful.

### Phase 3: Developer Observability Package (Months 1-3)

**Dashboards:** Standard template per domain, such as request rate, error rate, latency (RED metrics), DB query performance, external dependency health. Roll these out to 2 pilot teams first, iterate, then expand.

**SRE Champions Program:** Recruit 1 engineer per product team (5-6 total) as observability champions. They get deeper training, early access to new dashboards, and become the first point of help for their teammates. The platform team trains the champions, the champions train their teams. This scales observability adoption without the platform team becoming a bottleneck.

**Training:** One lunch-and-learn per month. First session: "Observability 101", how to read a dashboard, how to trace a request, how to find slow queries. Record it for folks who can't attend live. Champions attend all sessions and run follow-up office hours with their teams.

**Immediate quick win:** The "failed to send 1 trace" issue. This is likely Datadog agent resource limits on Fargate or a VPC endpoint misconfiguration. Fix it in week 1. We're losing production telemetry and don't know what we're blind to.

### Phase 4: Staging/Prod Parity (Months 2-3)

The current gap is why devs can't reproduce production issues:

| Aspect | Staging Today | Production |
|--------|--------------|------------|
| Data volume | Minimal seed data | 500k+ records |
| Infrastructure | Likely single instance | Multi-AZ |
| Third-party services | Mocked/sandbox | Live |
| Datadog instrumentation | Partial | Full |

Fix: weekly anonymized snapshots from production loaded into staging (pg_dump with PII scrubbed via a custom script). We handle PHI, so this needs to be thorough. Match the infra topology proportionally, and wire up the same Datadog dashboards.

## Expected Impact

| Metric | Before | After (confident) | Stretch |
|--------|--------|-------------------|---------|
| Actionable alert ratio | Unknown (low) | > 80% | > 90% (needs new-alert review process to hold) |
| Pages per on-call week | Unknown (high) | Cut by 50%+ | Measured and trending down monthly |
| MTTR (Sev-1) | Hours | < 1 hour | < 30 min (depends on team Datadog fluency) |
| Devs comfortable debugging in Datadog | A few | 15-20 (via SRE champions + training) | All 50 (6-month goal, not 90-day) |
| Staging reproduces prod issues | Rarely | 60-70% of the time | 80%+ (needs infra topology match) |

## Investment

~3 weeks focused effort (me + 1 engineer), then ongoing lightweight maintenance.

| Week | Focus | Ships |
|------|-------|-------|
| 2 | Alert audit | Classification spreadsheet, noise identified |
| 3 | Rationalization | 40%+ alerts deleted, top 15 runbooks written |
| 4 | Incident response | Handoff template, escalation matrix, IC checklist |
| 5-8 | Observability rollout | Dashboards, training sessions, staging parity |

## How We Know It Worked

- Pages per on-call week are down 50%+ from baseline
- Every alert in Datadog has a linked runbook or flows through intelligent filtering
- SRE champions in each team can independently debug production issues in Datadog
- Staging reproduces production issues more than 60% of the time
- MTTR for Sev-1 is under 1 hour
