---
title: "The Missing Middle: Why Mid-Market Platform Teams Are Stuck Between Backstage and Vercel"
date: 2026-07-09T09:00:00+05:30
featured_image: "featured.webp"
tags: ["Platform Engineering", "Kubernetes", "GitOps", "Internal Developer Platform", "Backstage", "Vercel", "PaaS", "SRE", "Developer Experience"]
---

## Overview

There is a size of engineering organization that platform tooling has quietly failed.

If you are Google, you build your platform. You have twenty engineers who work on nothing but the platform, and the leverage is obvious: one paved road, used by a thousand developers, pays for itself many times over.

If you are four people with a Rails app and a deadline, you rent a platform. You push to a branch, a preview URL appears, and you have never once thought about an ingress controller. That is the correct trade.

The trouble starts in between — and "in between" is where most of us actually work. You have thirty to a hundred and fifty engineers. You are on Kubernetes, probably for good reasons. And your platform team is two or three people, one of whom is on call this week.

That team is asked to do the impossible: build Google's platform with Google's ambitions and none of Google's staffing, or migrate onto a hosted PaaS that they have already outgrown.

---

## Two roads, both dead ends

### Road one: build it yourself

Gartner projects that [80% of large software organizations will have platform engineering teams by 2026](https://www.signisys.com/blog/gartner-says-80-of-software-orgs-will-have-platform-teams-by-2026/), up from 45% in 2022. The discipline won the argument. What the projection does not say is how large those teams are.

At the tech giants, a platform team is ten to twenty engineers. At mid-market companies, it is two or three. The same job description, a fifth of the people.

So the two-person team starts building. Backstage for the catalog. ArgoCD for delivery. Some Helm charts and a Terraform module. Then a script to scaffold new services, because copy-pasting a chart is how drift starts. Then a way to spin up preview environments, because the frontend team keeps asking. Then a promotion flow, because someone `kubectl apply`-ed to prod at 2am and nobody wants to talk about it.

None of these decisions is wrong. Each is locally correct. And eighteen months later, you have a bespoke internal platform whose only documentation is in one engineer's head — and that engineer has a competing offer.

The failure mode of building your own platform is not that you cannot build it. It's that you cannot *staff* it. A platform is not a project; it is a product with users who file bugs, and a two-person team that ships features cannot also run a support desk, keep pace with upstream Kubernetes, and maintain the glue they wrote in a hurry in 2024.

### Road two: rent it

So you look at Vercel, or Heroku, or Render, or Railway. The developer experience is genuinely excellent. This is not a straw man — these products are good, and for the right team at the right time they are the obvious answer.

But two things happen as you scale.

The first is the bill. Per-seat and bandwidth pricing has a way of surprising you: there is a [well-documented case of a Vercel $20/month plan turning into $286](https://deploywise.dev/blog/vercel-pricing-explained), and a three-person SaaS with 5,000 monthly actives lands somewhere around $150–300/month. Those numbers are fine. They are also the numbers *before* you have a data team, a batch tier, or a staging environment per squad. The curve bends in a direction that does not favor you.

The second is the exit. You did not just deploy on the PaaS; you built on it. Edge Config, KV, private services — each is a good primitive and each is a thread stitching you in place. Leaving is not a migration, it is a rewrite. By the time the bill justifies the move, the move has become a quarter of engineering time you do not have.

The honest framing is not "hosted PaaS is bad." It's that **a hosted PaaS is the right answer early and the wrong answer later**, and nothing in the product tells you when you crossed over. You find out from a finance meeting.

---

## The gap nobody sells to

Put the two roads on the same axes and the shape of the problem shows up immediately.

```
                    complete / batteries-included
                              ▲
                              │
       Vercel · Heroku        │
       Render · Railway       │            ← the missing middle
       (rent, can't control)  │
                              │
  ◄───────────────────────────┼───────────────────────────►
     closed / vendor          │              open / yours
                              │
                              │      Backstage · raw ArgoCD
                              │      Helm + glue + hope
                              │      (yours, but you build it)
                              ▼
                      primitives / assemble-it-yourself
```

The top-left quadrant is complete but not yours. The bottom-right is yours but not complete. The mid-market platform team needs the top-right — a platform that is *both* productized and owned — and for years there has been very little there.

That quadrant has a name in the abstract: a **golden path**. Not a framework, not a catalog, not a control plane you rent. A paved road: one obvious, well-lit way to take an application from a git push to a running production workload, with previews and promotion in between, built on primitives you already trust and can walk away with.

---

## What the middle actually needs

I don't think the missing product is a new abstraction over Kubernetes. We have enough of those. I think it is a smaller and more boring list:

**A deployment object developers can reason about.** Not a Deployment, a Service, an HTTPRoute, a ConfigMap, and four annotations — one thing, called an app, that owns all of those. Developers should think in `app → environment`, not in eleven Kubernetes kinds.

**Previews as a primitive, not a project.** An ephemeral environment per change, torn down on merge. Every platform team builds this. Nobody should have to.

**Promotion as a first-class flow.** Dev to staging to prod, with gates where you want gates and automation where you don't. The 2am `kubectl apply` should be impossible, not merely discouraged.

**Secrets that never live in Git.** Not sealed, not encrypted-and-committed. Referenced from an external store, scoped sensibly, absent from the repo.

**An exit.** If the platform disappeared tomorrow, your applications should keep running and your manifests should stay legible. The test of a platform you own is whether you can leave it — and whether leaving is a Tuesday afternoon rather than a quarter.

Look at that list closely and notice what it is not asking for. Every item is achievable with tools that already exist and are already good: ArgoCD reconciles, Kargo promotes, External Secrets Operator resolves secrets, Gateway API splits traffic. The CNCF ecosystem solved these problems. What it did not do is *assemble* them into a road, and assembling them is precisely the work a two-person team cannot sustain.

The missing middle is not missing technology. It's missing a product.

---

## Where this is going

I've spent the last stretch of my career on the wrong end of both roads — maintaining glue nobody else understood, and watching a bill for a platform I couldn't change. My conviction is that the mid-market team does not need to pick. If you can run Kubernetes, you should be able to run a great platform, and it should not cost you a team you don't have.

Over the next few weeks I want to work through this in public: what the economics of a hosted PaaS actually look like at scale, why golden paths matter more than headcount, and how you would build this layer on ArgoCD and Kargo rather than reinventing continuous delivery from scratch.

I'm building something in this gap. It isn't ready to show you yet, and I'd rather earn the argument first.

If the shape of this problem is familiar — if you are the two-person team in the middle — follow along. And tell me where I've got it wrong.
