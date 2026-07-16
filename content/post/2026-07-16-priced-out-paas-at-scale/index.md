---
title: "We Priced Out 'Just Use a PaaS' at Scale. Here's Where It Breaks."
date: 2026-07-16T09:00:00+05:30
featured_image: "featured.webp"
tags: ["PaaS", "Vercel", "Heroku", "Render", "Cloud Costs", "Platform Engineering", "Vendor Lock-in", "Kubernetes", "SRE"]
---

## Overview

Last week I argued that mid-market platform teams are stuck between two roads — build an internal platform you can't staff, or rent a PaaS you outgrow. A few people pushed back on the second road: *is renting really that bad? The DX is incredible and the bill is tiny.*

They're right, for a while. So this post is the honest version of the rent-it math. Not "PaaS is bad" — it isn't — but *where the curve bends*, and why the bend catches teams by surprise.

I'll use real, cited numbers rather than a strawman. The uncomfortable part isn't any single line item. It's that every mechanism that makes a PaaS delightful at ten thousand requests a month is the same mechanism that bills you at ten million.

---

## The bill nobody models

Start with the number everyone quotes: the entry price. Vercel's Pro plan is $20/month per member. Render, Railway, Heroku all sit in the same neighborhood. At that price the decision is a no-brainer, and it should be — you get a CDN, TLS, previews, and zero ops for less than a team lunch.

The trouble is that $20 is a *floor*, not a *price*. It's the number before traffic, before seats, before the managed add-ons you'll probably turn on. One engineer's writeup walks through a [Vercel Pro plan that went from $20 to $286/month](https://deploywise.dev/blog/vercel-pricing-explained) once real usage arrived; a [three-person SaaS with 5,000 monthly actives lands around $150–300/month](https://designrevision.com/blog/saas-hosting-compared) on comparable hosting. Those are small teams and small numbers. The reason to care is the tail: keep scaling and public writeups of [four-figure bandwidth bills](https://deploybase.app/blog/vercel-bill-shock-1100-bandwidth-costs-alternatives-2026) and a [$23,000 month](https://usagebox.com/articles/vercel-23000-dollar-bill-usage-based-platform-bill-shock-2026) stop being hard to find. Fine at the bottom, frightening at the top — and the same pricing model draws the whole line.

Everyone thinks of the overage as just "bandwidth," but on Vercel alone the invoice meters something like seven separate dimensions. Three forces in particular bend the curve superlinear:

**Bandwidth.** PaaS egress is priced at a comfortable retail markup over what the underlying cloud charges. At a gigabyte a day you never notice. At a terabyte a day it's the biggest line on the invoice, and it grows with your success, not your headcount.

**Seats.** Per-member pricing means your bill scales with your *org chart*, not your infrastructure. Every engineer, every contractor, every "can you add marketing so they can check the preview" is another recurring line. The model is unpopular enough that the market is starting to walk away from it: [Render is dropping per-seat pricing in August 2026](https://render.com/blog/better-pricing-for-fast-growing-teams) for flat plans with unlimited members, and says most customers will pay the same or less. That's a genuine fix, and worth saying so plainly — but it's one vendor moving one axis. The floor gets fairer; the slope below doesn't.

**Managed add-ons.** The KV store, the edge config, the managed Postgres, the cron workers. Each is a few dollars and each is metered. Individually trivial; collectively the part of the bill you can't predict a quarter out.

None of these is a gotcha. They're all reasonable prices for real value. The problem is structural: **you're billed on the axes that grow when things go well** — traffic, team, features. The pricing page shows you the floor. The finance meeting shows you the slope.

---

## The exit you didn't price

Cost is the visible problem. Lock-in is the one that decides whether you can *do anything about it*.

When you adopt a PaaS, you don't just deploy on it — you build *into* it. The primitives that make the platform pleasant are proprietary surfaces:

- **Edge Config / KV / Blob** — your feature flags, sessions, and small hot data live in a store with a vendor-specific API.
- **Private services & internal networking** — your service-to-service wiring assumes the platform's networking model.
- **Build and routing config** — redirects, rewrites, headers, cron, and edge functions expressed in the vendor's schema.

You can resist this. Stick to plain Postgres and Redis, keep your routing in your own code, avoid the proprietary stores, and you stay portable — some disciplined teams do exactly that. But the platform is *designed* so the proprietary path is the easy one, and easy paths win under a deadline. The lock-in isn't forced; it's defaulted.

Individually, each is a good primitive. Together they're stitches. By the time the bill justifies leaving, "leaving" is no longer a migration — it's a rewrite of everything that touched a proprietary surface. You have to reimplement KV on Redis, rebuild the edge logic as real services, re-express the routing, and re-test all of it.

So the trap closes from both ends at once. The cost curve says *you should leave*. The integration depth says *leaving costs a quarter of engineering you don't have*. Teams sit in that vise for years, paying the bend, precisely because the exit was never on the pricing page.

---

## Right answer early, wrong answer later

Here's the part I want to be fair about, because it's the whole point.

A hosted PaaS is the *correct* choice early. When you're four people trying to find product-market fit, paying a retail markup to never think about ingress is a spectacular trade. You should take it. Ops time is the scarcest thing you have, and the PaaS sells it back to you cheap.

The failure isn't choosing a PaaS. It's that **nothing in the product tells you when you crossed the line** from "this is cheap leverage" to "this is an expensive dependency I can't leave." There's no dashboard for it. The DX that felt like a gift at seed stage feels like a hostage situation at Series B, and the transition is gradual enough that no single month looks like the month to act.

You find out you crossed the line the way most teams do: someone in finance circles a number, and someone in engineering estimates the rewrite, and the two numbers are both too big.

---

## What the mid-market team actually wants

The tell is what these teams ask for once they've felt the bend. It's never "give me a worse developer experience to save money." Nobody wants to go back to hand-rolling YAML. What they want is narrower and more specific:

- The **DX of Render or Vercel** — push, preview, promote, done.
- Running on **their own cluster**, so compute and traffic bill at their cloud's rates rather than a platform's markup on top — and the bill is predictable. Cloud egress still isn't cheap (AWS is ~$0.09/GB); the win is removing the middleman's margin and the seven-axis surprise, not making bandwidth free.
- With **no control plane they don't own**, so leaving is a migration, not a rewrite — the manifests and data stay in portable form the whole time.

That combination — the paved-road experience, on infrastructure you control, with no proprietary surfaces to rewrite your way out of — is the thing the market doesn't quite sell yet. It's the self-hosted answer to the rent-it road, and it's what I've been building toward.

There's an obvious objection here, and it's the one from last week: *someone still has to run that cluster, and a two-person team can't.* Exactly right — that's the hard part, not a detail. The combination above is only worth anything if running it is close to as cheap, in engineer-hours, as renting. Closing that gap is the whole problem, and renting-versus-building stops being the only choice on offer exactly when the third option gets genuinely low-effort.

More on how you'd actually build that on primitives you already trust — ArgoCD, Kargo, the CNCF stack — in the next few posts. For now: if you've watched a PaaS bill bend, I'd genuinely like to hear where yours broke. The numbers travel better when they're real.
