# CAMIR — Future press release

**What this is** — an Amazon-style working-backwards press release dated September 2031, five years out, describing the outcome CAMIR is trying to earn: what exists, who uses it, and the unit of measurement the market adopted because the company existed. Plus the dated path that has to be true for any of it to happen.
**Why it exists** — writing the end state first is how the team finds out which present-day decisions it depends on. For CAMIR the answer is uncomfortable: the 2031 release only works if the company still issues disqualification reports at scale, still refuses to compute its own customers' savings, and has reached Product 2 — three things that each get harder to hold as revenue grows. A vision written without them would describe a gateway, which is the business this pack argues against.
**How to read it** — the release is aspirational and every number in it is the [../financials/revenue_build.md](../financials/revenue_build.md) Y5 scenario, not a forecast. Read §How we got here second: each step is a milestone that is not under CAMIR's sole control. A skeptic should look for any sentence that reads as present-day traction; there should be none.
**Depends on / feeds** — depends on [../BRIEF.md](../BRIEF.md) §Wedge, [../strategy/positioning.md](../strategy/positioning.md), [../tech/whitepaper.md](../tech/whitepaper.md), [../financials/revenue_build.md](../financials/revenue_build.md), [../validation/metrics_by_stage.md](../validation/metrics_by_stage.md); feeds [pitch_deck.md](pitch_deck.md), [mission_vision.md](mission_vision.md) and [../README.md](../README.md).

**Everything below is a hypothetical future.** Quotes are illustrative voices of the pack's personas, not statements by real people. CAMIR has no customers today.

---

## CAMIR: the frontier run becomes how platform teams decide what their models cost them

**San Jose, California — September 2031.** CAMIR today published its fifth annual *State of the Self-Hosted Frontier*, drawn from frontier runs that customers chose to make public. More than 200 platform teams now run CAMIR's control plane inside their own infrastructure, and the open-source harness underneath it is used by many times that number who pay nothing.

The report's headline is not a savings number. It is that **the typical team discovered, on its first run, that a material share of what it had measured as "the small model cannot do this" was its own evaluation harness** — generation budgets that truncated answers, parsers that rejected correct output, judges that rewarded length. The 2026 research that first documented those artifacts found truncation in 65% of MMLU and 57% of MedQA cases [S5]. Five years on, the artifact share is the first line of every frontier run, and it is expected to shrink as a team's harness improves.

The report also counts something no other vendor in the category publishes: **the share of evaluations in which CAMIR told the prospect not to deploy.** It has never been zero.

> "We used to argue every quarter about whose savings number was right. Now my own engineers run the counterfactual on our hardware, the report names who signed each tolerance, and the one endpoint CAMIR told us not to route is still on the page. That line is why I believe the rest."
>
> — *illustrative voice of Dana Okonkwo, Head of Platform (persona P4)*

> "When my feature's quality moves, I group my own traces by tier and I know in minutes whether it was the router. Usually it isn't. When it is, I pin my endpoint and nobody has to approve it."
>
> — *illustrative voice of Ravi Menon, product engineer (persona P5)*

> "I brought my own judge and my own five tiers. What I wanted was a harness somebody else wrote that computes my inter-judge agreement. Six points of our ceiling turned out to be our harness — on a team that builds evaluation for a living."
>
> — *illustrative voice of Wen Xu, ML infrastructure lead (persona P3)*

## The unit the market adopted

Platform teams no longer ask a vendor for a savings percentage. They ask for a **frontier run**: the fixed-model baseline, the oracle ceiling measured with and without evaluation artifacts, the route, the declared quality tolerance and its named owner, the self-hosted cost parameters — utilisation and the all-in multiplier stated, not assumed — the judge set, and the inter-judge agreement. A run missing any of these is labelled *not reproducible*.

The shift happened because the old numbers did not survive being re-run. A router that saved 3.66× on one benchmark saved 1.41× on another [S2]; savings quoted against raw GPU rental were 3–5× too large once engineering time was counted [S27]. The frontier run made the disagreement inspectable.

## How we got here

Each step is a milestone from [../financials/revenue_build.md](../financials/revenue_build.md). None was certain, and three of them depended on parties CAMIR did not control.

| Year | Milestone | What had to be true |
|---|---|---|
| **2026** | The capstone team measures an oracle ceiling on a self-hosted pool, guarded and unguarded, and publishes it — **or publishes the null result** | The ceiling existed on realistic traffic (E1) and part of it was harness (E2). Neither was known when this release was first drafted |
| **2027** | Routing ships as a strategy inside LiteLLM; the first consuming engineers enable enforcement on their own endpoints | A third-party maintainer accepted the contribution (M2) — unagreed at the time — and tolerance ownership held up against a real veto (E13) |
| **2028** | Three customers publish before-and-afters under signed eligible-savings definitions | The counterfactual survived customers re-deriving it themselves (E9) |
| **2029–2030** | Product 2: the same measurement applied to cache thresholds, quantisation and upgrade regression | Customers wanted levers nobody had asked for in 2026 (M4) |
| **2031** | Hybrid pools admitted; ~219 customers and ~$11M ARR in the revenue build's Y5 scenario | Hybrid routing proved commercially distinct from a frontier vendor's free router (M5, [S20]) |

## What did not happen, and why that matters

The router did not become CAMIR's product; the serving engines absorbed dispatch, as the 2026 vision paper said they would [S11]. CAMIR did not build a cache, host models, or run a multi-tenant cloud — the control plane still runs inside each customer's perimeter and still never sees a prompt. And CAMIR never computed a customer's savings on its own servers. Each of those refusals cost revenue at some point between 2026 and 2031. The release is only true if every one of them held.

---

## Recommended next 3

1. **Treat the "How we got here" table as the actual plan.** Every row is a dated, falsifiable milestone, and the first row resolves in about six weeks; the press release is only as real as that row.
2. **Publish the disqualification rate from the first evaluation onward.** The release's most distinctive sentence — "it has never been zero" — can only be true in 2031 if it is recorded from 2026.
3. **Re-write this release after E1 and E2.** If the ceiling or the artifact share comes back weak, the headline changes before anything else does, and it is better to find out here than in a deck.
