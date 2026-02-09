---
description: Stakeholder communication strategy
---

# Communication Plan

## Communication Matrix

| Audience | Cadence | Format | What I'm Communicating |
|----------|---------|--------|------------------------|
| **Manager** | Weekly 1:1 + async | Meeting + Slack | Metrics trending, what shipped, where I need help |
| **Manager** | End of Month 1 and 3 | Written review | Retro with adjusted plan; 90-day results with roadmap |
| **2 Engineers** | Daily standup (15 min) + weekly planning (1 hr) | Sync | Blockers, priorities, how their work fits the bigger picture |
| **Eng Leadership** | Biweekly (30 min) | Meeting with dashboard walkthrough | Platform health improving, how it helps their teams |
| **Dev Team** | Monthly | Newsletter + lunch-and-learn | What shipped, what it saves them, what's next |
| **Dev Team** | Continuous | Slack (#platform channel) | Real-time support, deployment announcements, incident comms |
| **CEO / Exec** | Month 1 and 3 (via manager) | Written summary | Deploy time cut by X, here's our path to service independence |

## Weekly Manager 1:1

Standard format, takes 5 minutes to prep:

- **Metrics:** deploy time, MTTR, alert count, toil hours. Trend arrows, not long narratives
- **Shipped this week:** 2-3 bullet points
- **Planned next week:** 2-3 bullet points
- **Blockers / needs:** anything requiring their help
- **Team health:** quick pulse on Engineer A and B

## Monthly Platform Newsletter

Short, scannable email to the broader dev team. Structure:

1. **What we shipped:** 3-4 items with before/after numbers where possible
2. **By the numbers:** key metrics table (deploy time, alert count, MTTR)
3. **What's next:** 2-3 items for the coming month
4. **Feedback welcome:** link to #platform channel

Keep it to one screen. If someone has to scroll, they won't read it.

## Biweekly Eng Leadership Meeting

30 minutes:

- 10 min: Dashboard walkthrough + trend analysis
- 10 min: What shipped, what's in progress, what's blocked
- 5 min: Priority check. Are we still working on the right things?
- 5 min: Q&A

## Slack Channel (#platform)

What goes here: deployment announcements, incident updates, new tooling announcements, maintenance windows, quick tips.

What doesn't: long technical debates (use threads or a meeting), unactionable information.

Response expectations:

| Type | Target Response |
|------|-----------------|
| Production issue | < 15 minutes |
| Blocking question | < 2 hours |
| General question | < 24 hours |

## Month 3 CEO Summary

One page. Structure:

1. **The problem we solved:** 2-3 sentences on where we started
2. **Results delivered:** before/after metrics table with business impact
3. **What this enables:** faster feature delivery, lower risk, foundation for service independence
4. **Recommended next steps:** link to the 6-12 month roadmap
