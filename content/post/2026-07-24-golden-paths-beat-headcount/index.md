---
title: "Golden Paths Beat Headcount. The Platform Staffing Math Says So."
date: 2026-07-24T09:00:00+05:30
featured_image: "featured.webp"
tags: ["Platform Engineering", "Golden Paths", "Internal Developer Platform", "Backstage", "Developer Experience", "DORA", "Team Topologies", "Kubernetes", "SRE"]
---

## Overview

The most common reply to the last two posts was some version of the same sentence: *you're describing a staffing problem, not a product problem. Give me two more engineers and I'll build the road myself.*

It's a fair objection and it deserves a real answer, so this week I ran the staffing math.

The short version: you are more understaffed than you think — and the extra headcount, if you got it, is not what would save you. The industry already ran this experiment. Nearly everyone has a platform team now, and most are not producing the outcome they were funded for.

---

## The ratio nobody re-runs

There's a rule of thumb for platform team sizing that shows up consistently once you go looking: **one platform engineer per eight to twelve product engineers**, with [mature platform teams sitting "between 5 and 10 percent of the engineering organisation"](https://platformengineeringcost.com/team-structure).

Hold that against the team from the first post — thirty to a hundred and fifty engineers, two or three on platform — and something uncomfortable falls out.

```
  platform engineers needed  ·  industry ratio 1 : 8–12
        │
    15  ┤                                     ╱   8–15 needed
        │                                 ╱
    10  ┤                             ╱
        │                         ╱     the gap you're
     5  ┤                     ╱         quietly absorbing
        │                 ╱
     3  ┤             ╱
     2  ┼────────────────────────────────────────   what you have · 2–3
        └──┬────────┬────────┬────────┬────────┬──
          30       60       90      120     150
                    engineers in the org
```

At thirty engineers, a two-person platform team is *correctly staffed*. Bang on the ratio. At a hundred and fifty, that same two-person team should be eight to fifteen people. It is still two people.

Nobody decided this. There was never a meeting where someone said "let's run platform at a fifth of the recommended ratio." The org grew, the platform team didn't, and the ratio bent underneath them the same way the PaaS bill bent in [last week's post](/post/2026-07-16-priced-out-paas-at-scale/) — gradually, with no single month that looked like the month to act.

That's the second time this pattern has shown up in three weeks, and I don't think that's a coincidence. **The mid-market failure mode is never a bad decision. It's a good decision that quietly stopped being true.**

---

## The headcount isn't coming — and wouldn't fix it

Here's where I expected to write "so go ask for the headcount," and where the data stopped me.

By 2025, [90% of organizations reported using an internal developer platform, and 76% had established dedicated platform teams](https://dora.dev/capabilities/platform-engineering/). The staffing argument *won*. Gartner's projection from the first post landed. Companies went out and hired the platform people.

And the developer experience did not correspondingly improve. DORA is blunt about the failure mode: platforms can **decrease throughput and change stability** when they aren't carefully managed. The 2024 data put the gain from developer independence — the thing platforms exist to deliver — at [5% at the team and individual level](https://dora.dev/capabilities/platform-engineering/). Five percent is real and positive, and nowhere near what anyone thought they were buying.

Then there's the adoption number. Self-hosted Backstage — the default answer to "build your own platform," at roughly [89% share among organizations that adopted an IDP](https://roadie.io/blog/platform-engineering-in-2026-why-diy-is-dead/) — takes six to twelve months to stand up, sometimes eighteen or more, and lands at an [average internal adoption rate around 10% of engineers](https://medium.com/@samadhi-anuththara/backstage-backlash-why-developer-portals-struggle-cb82d4f082e1). Two independent sources put it in the same place. (The market-share figure is Roadie's, and they sell managed Backstage — hence the second source on adoption.)

Ten percent, after a year of work. That is not an understaffing outcome; a team too small to build anything would have produced *nothing*, which is cheaper. This is the signature of building the wrong thing competently.

So the honest answer to "give me two more engineers" is that the companies that got the engineers are, on average, at 10% adoption. **More headcount buys more platform surface area. It does not buy adoption, and adoption is the entire product.**

---

## A catalog is not a road

The difference between the platforms that work and the ones that sit at 10% isn't team size. It's what the team decided to build.

Spotify — where the term originated — defines a Golden Path as [the "opinionated and supported" path to build something](https://engineering.atspotify.com/2020/08/how-we-use-golden-paths-to-solve-fragmentation-in-our-software-ecosystem). Both words are load-bearing, and most internal platforms honour neither. *Opinionated* means there is one way — not a menu of blessed options, one way, chosen for you, so the decision is already made by the time you arrive. *Supported* means someone owns it when it breaks and keeps it true as the stack moves.

The problem Spotify was solving is the mid-market problem exactly. Before golden paths they had what they called **rumour-driven development**: engineers finding out how to do things by asking whoever sat nearby. Autonomous teams, decentralised tooling, no obvious road — so the road existed only in people's heads, and it was different in each head.

If your platform's real interface is a Slack channel where people ask which Helm chart to copy, you have rumour-driven development. It doesn't matter how good your Terraform modules are.

And here's the inversion that makes the staffing math work:

- **A catalog scales with the number of things you have.** Every service, template, and plugin is another entry to maintain and another thing that goes stale and erodes trust. Its cost grows with your surface area — which is why portal teams end up spending their capacity on the portal.
- **A road scales with the number of decisions you removed.** It gets *cheaper* to run as more people use it, because the alternatives stop being exercised.

One of those needs headcount proportional to the org. The other doesn't. That's not a philosophy, it's a cost structure.

The tell that Spotify got this right is what their golden path tutorials became: their most-read and most-used technical documentation, wired into the first two weeks of onboarding. The road was what developers reached for — not a portal they had to be reminded to visit.

---

## The threshold that changes the arithmetic

The sizing guide I opened with makes the point better than I can, almost in passing: organisations with strong golden paths that ["70-plus percent of product engineers actually use can operate at looser ratios because the platform scales through software rather than humans"](https://platformengineeringcost.com/team-structure).

Read that again with the earlier numbers in hand. The industry average for self-hosted developer portals is around 10% adoption. The threshold at which your staffing ratio stops mattering is 70%. **Most platform teams are seven times below the line where their headcount problem would dissolve** — and they're trying to close the gap by hiring, the one lever that provably doesn't move that number.

Adoption is the multiplier on everything a platform team does. Below the threshold, every engineer you add is another maintainer of software most of your org routes around. Above it, the platform does the work the humans would otherwise do, and the ratio genuinely relaxes.

Two or three people can pave one excellent road. What two or three people cannot do is run a catalog for a hundred and fifty engineers. The mid-market team keeps attempting the second thing because it looks more like what the big companies have.

---

## What makes a road get walked

If adoption is the whole game, the design question narrows: what makes developers use the paved route instead of routing around it?

DORA's 2025 answer is more specific than I expected. The platform capability most correlated with a positive user experience is [giving developers "clear feedback on the outcome of my tasks"](https://dora.dev/capabilities/platform-engineering/). Not breadth of features. Not catalog completeness. Feedback.

That's a very literal property of an actual road: **you can see where you are on it.** You pushed, and you can tell whether it built, where it deployed, what's running, and what happens next — without asking anyone. Rumour-driven development is what fills the silence when a platform won't answer that question, and no amount of catalog fixes it.

Which sends me back to the list from the [first post](/post/2026-07-09-the-missing-middle/), now with a reason attached rather than an assertion. One app object instead of eleven Kubernetes kinds. Previews as a primitive, promotion as a first-class flow. Secrets by reference. An exit you can take on a Tuesday. Every item is a decision removed or a piece of state made visible — that's what a golden path is made of. None of it requires a bigger team; all of it requires *someone to have paved it and to keep it true*.

---

## The objection I can't argue away

Here's the part I want to be fair about, because it's the same wall I hit at the end of last week.

Golden paths are a product. Products need an owner, a roadmap, and someone to keep them faithful to reality as Kubernetes moves underneath — Spotify's own principle was *fidelity to reality*, and that's a standing cost, not a launch cost. A two-person team can't sustain a product any more than it can sustain a catalog.

So "just build golden paths instead" is not, on its own, an answer. It's a better target with the same staffing problem attached.

The only way it resolves is if the road is something you **install** rather than something you assemble — paved upstream, maintained upstream, running on your cluster, with the opinions already made and no proprietary surface to rewrite your way out of. That's what makes the mid-market ratio survivable: not a bigger team, and not a rented control plane, but a road that arrives already paved.

Which is finally the interesting part, and it's where this series has been heading. Next: what that actually looks like built on ArgoCD, Kargo, External Secrets, and Gateway API — the primitives that already work, assembled into a road, rather than a twelfth abstraction over Kubernetes.

If you're running a platform team below the ratio, I'd like to know your real adoption number — not the one in the OKR doc. My hunch is that the honest ones cluster a lot closer to 10% than anyone says out loud, and that the teams above 70% did something specific to get there.
