---
description: How to address the CEO's microservices request
---

# The Microservices Conversation

The CEO wants microservices in 3 months. After two Sev-1s in one month, I understand why. But 50 engineers are shipping into a coupled monolith right now, and a full migration in 3 months isn't realistic. I'd rather be upfront about that and propose something that actually works.

---

## Why 3 Months Isn't Enough for a Full Migration

- 50 engineers are actively shipping features in a tightly coupled monolith
- No identified service boundaries or API contracts exist yet
- No independent deployment capability, which microservices fundamentally require
- No distributed tracing, no service mesh, no inter-service auth

Rewriting the monolith while 50 people are shipping into it would slow everything down and probably cause more incidents, not fewer.

## What the CEO Actually Wants

When a CEO says "microservices," they're usually not thinking about architecture. They're thinking about problems they want solved:

| What they're saying | What they need |
|---------------------|----------------|
| "Microservices" | Deploy one thing without risking everything else |
| "Modern architecture" | A bug in payments doesn't take down scheduling |
| "Scale" | Teams ship without coordinating deploys |

All of that is achievable. We just need to get there in a different order than "rewrite everything."

## The Phased Approach

### Months 1-3 (This Plan)

Fix the deployment pipeline, add feature flags, identify domain boundaries, extract one bounded context as a proof-of-concept. This is the foundation. Without fast deploys and observability, microservices just make things harder.

### Months 4-9

Extract 3-5 services using the Strangler Fig pattern, starting with whichever ones have the clearest boundaries. Each extraction gives that service its own deploy cycle, its own scaling, and its own failure boundary.

### Month 9+

Keep decomposing based on what we've learned. By this point we have real data on which extractions paid off and which ones aren't worth the effort.

## The Visual

![Microservices Evolution Path](../images/microservices-evolution.svg)

## What I'd Say to the CEO

"I'm with you on the direction. Let me walk you through how I'd get us there:

First 3 months, we fix the deploy pipeline, add feature flags, and pull out one service as a proof of concept. That alone cuts deploy time from 4 hours to under an hour and stops the kind of Sev-1s we just had.

Months 4 through 9, we extract 3-5 more services with the Strangler Fig pattern. Each one deploys on its own, fails on its own, scales on its own.

We don't freeze features and we don't rewrite. Teams keep shipping the whole time, and I'll have numbers to show you every month."

The metric that tells the story is deploy time. The more decoupled the system gets, the faster deploys become. If that number is dropping, we're on track.

| Metric | Month 1 | Month 3 | Month 9 |
|--------|---------|---------|---------|
| Deploy time | < 1 hr | < 30 min | < 15 min (per service) |
| Deploy frequency | 2x/day | 5x/day | Per-service, independent |
| Sev-1 from deploys | 0 | 0 | 0 |
| Independent services | 0 | 1 | 4-5 |
