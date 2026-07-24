# StoreLens — Onboarding & First-Session Design Rationale

*Analytics for small Shopify merchants. Design brief: merchants sign up, see a dashboard they don't understand, and never return.*

---

## a) Problem framing and intended flow

### The current-state problem

Today's flow is: **sign up → connect Shopify → sync → land on a generic dashboard** (a dozen widgets: revenue, orders, AOV, conversion, a big line chart, filters). It fails for a small merchant in four compounding ways:

1. **Cold start with no benchmark.** A solo merchant sees "2.1% conversion" and cannot answer the only question that matters — *is that good?* Numbers without a reference point carry no meaning, so they carry no motivation.
2. **No narrative.** The dashboard supplies data but offloads all interpretation onto the merchant. These users are not analysts; they run a shop. Self-interpretation is cognitive work they didn't sign up for, so they bounce.
3. **A passive surface.** There is nothing to *do*. A dashboard you only look at has no engagement loop, so there's no habit to form.
4. **No reason to return.** Nothing pulls the merchant back. Between sessions the product is silent, the merchant forgets, and churn follows.

**Root cause:** the product leads with *data supply*, but the merchant arrives with a *question* (demand). The dashboard answers a question nobody asked. Every fix below is a way to close that gap.

### The reframe

Three leverage points, applied in order:

- **Lead with the question, not the data.** Onboarding's central moment isn't "connect your store" — it's *"What do you want to know first?"* The merchant picks one goal, and the product organizes itself around that.
- **Do the interpretation for them.** The payoff screen is not a chart grid; it's **The Pulse** — a single plain-language sentence that states what changed and why it might be happening, in the voice of a knowledgeable friend who runs a shop.
- **Manufacture a reason to return.** The first meaningful action turns The Pulse into a recurring **weekly pulse** (email + optional alert). The return visit is then a *"Since you were away"* digest — never the cold dashboard again.

### Intended flow, with decision points and failure paths

The happy path is a straight line; every decision point has an explicit failure path so no one dead-ends into a blank screen. (Rendered visually as screen 0 of the prototype.)

```
Connect Shopify store
   │
   ▼
◆ Decision: OAuth succeeds?
   ├─ FAIL → Recovery screen: "We couldn't reach your store." Retry + Get help.
   ▼
◆ Decision: enough order history (≥ ~30 days)?
   ├─ FAIL (low data) → "Starter mode": set expectations, show what we'll watch,
   │                     no empty/broken-looking charts.
   ▼
◆ Reframe: "What do you want to know first?" → pick ONE goal
   ├─ FAIL (indecision / "Not sure yet") → default to "Are sales healthy?", continue.
   ▼
Build the first insight  (async)
   │
   ├─ FAIL (slow sync) → "Email me when it's ready." Keep the session alive; never a blank spinner.
   ▼
◆ Decision: is there a notable signal for the chosen goal?
   ├─ FAIL (nothing notable) → show the steady baseline as a positive state,
   │                            still offer the return hook.
   ▼
The Pulse  — one plain-language insight + likely causes   ← time-to-value / "aha"
   │
   ▼
First meaningful action: "Turn on the weekly pulse"        ← creates the return hook
   ├─ FAIL (skips setup) → enroll in a gentle default weekly digest, opt-out later.
   ▼
[ end of first session ]
   │  (trigger: weekly email OR alert fires)
   ▼
Return visit → "Since you were away" digest → dashboard, now framed by the goal
```

**Why this ordering.** Time-to-value is front-loaded: the merchant states a question, and within one short sync gets a sentence they understand. The habit loop (pulse → return → digest) is established *before* the full dashboard ever appears — and when the dashboard does appear, it leads with the merchant's chosen question and adds the missing benchmarks ("typical for home goods is 1.8–2.6% — you're fine"), which repairs the original "no benchmark" failure directly.

---

## b–c) Screens and prototype

Delivered as a single self-contained clickable prototype (`storelens_prototype.html`). It opens on the flow map, then walks the primary path. Eight screens plus failure paths:

| # | Screen | Role in the brief |
|---|--------|-------------------|
| 0 | Flow map | Overview / decision points / failure paths |
| 1 | Connect store | **Empty state / cold start** |
| 1b | Connection error | Failure path (clickable via "Simulate error") |
| 2 | "What do you want to know?" | The reframe — the key move |
| 3 | Building your insight | Sync that is never blank |
| 4 | **The Pulse** | The "aha" + **first meaningful action** |
| 4′ | KPI grid | **The rejected version** (see part d) |
| 5 | Weekly pulse setup | First meaningful action → **return hook** |
| 6 | "Since you were away" | **The return visit** |
| 7 | Dashboard, framed | Analytics, but contextualized + benchmarked |

**Design language (why it doesn't look templated):** the product thesis is *a plain-spoken human voice interpreting precise numbers for a time-poor shopkeeper*, so the typography encodes it — **Fraunces** (a warm, characterful serif) carries the spoken insight sentences, **Geist** handles the UI chrome, and **Geist Mono** sets every figure like a ledger. The palette stays calm ink-on-paper with a single restrained pine accent; **saturated color appears only at the insight moment** (a gold highlight under the key phrase, red/green only on the trend that's actually moving). That restraint *is* the signature — it visually enacts "one signal in the noise."

---

## d) The screen I iterated: the first payoff screen

This is the most important screen in the product — the moment that decides whether the merchant "gets it" — so it's the one I iterated hardest. Both versions are in the prototype (screen 4 = kept, screen 4′ = rejected).

### The version I rejected — the KPI dashboard

Four stat cards (Revenue, Orders, AOV, Conversion) with tiny up/down arrows, a comparison line chart, and a filter row.

It's competent, it's fast to build, and it's what almost every analytics product ships. I rejected it because **it recreates the exact failure in the brief.** Walking my own failure list against it:

- *Cold start / no benchmark* — "2.1%, ▼0.3pt" still doesn't tell a first-time user whether that's good. Unsolved.
- *No narrative* — the merchant still has to notice that AOV is down, connect it to revenue, and guess why. All the interpretation work is still theirs. Unsolved.
- *Passive* — nothing to do but adjust a date filter. Unsolved.
- *No reason to return* — a grid of numbers gives you no hook. Unsolved.

Shipping it would mean losing 30 of the 100 points *and* the actual users. A prettier version of the wrong screen is still the wrong screen.

### The version I kept — The Pulse

One sentence, set large in the serif voice: *"Your repeat customers are slipping — down 12% since last month."* Below it, a small sparkline, a plain-language translation ("1 in 5 buyers used to come back; now it's closer to 1 in 6"), three plausible **causes** in human terms, and one primary action: *Watch this each week.*

### Why the kept version wins, point by point

- **It does the interpretation.** The headline states the finding *and* its direction; the merchant reads it in about five seconds with zero analysis required.
- **It supplies meaning, not just a number.** "1 in 5 → 1 in 6" is a benchmark a shopkeeper feels, where "12%" is abstract.
- **It's causal, so it's actionable.** Listing likely causes ("Cedar Candle out of stock 9 days") turns a metric into a to-do, which is what a merchant actually wants.
- **It creates the loop.** The single CTA converts a one-time insight into a recurring pulse — the mechanism that fixes "never returns."
- **The craft reinforces the reframe.** Because color and the serif voice appear *only* here, the eye lands on the finding, not on chrome.

One honest trade-off I accepted: The Pulse shows less at once than the grid, and a data-literate power user might want the raw numbers immediately. I resolved that by keeping the full dashboard one click away ("See the full picture") and framing it around the chosen goal — so I optimize the *first* session for the non-analyst majority without stranding the minority who want depth.

---

## A note on the deliverable format

The brief asks for a Figma link. I can't author or host a Figma file, so I've delivered the equivalent substance instead: a working clickable prototype (this HTML file, openable in any browser) that contains the flow map, all screens, and the interactive primary path, plus this written rationale. The prototype is high-fidelity and self-contained, so it doubles as a faithful spec to rebuild in Figma if the reviewer requires the file natively.
