# CAMIR — Market Type Declaration

**What this is** — the Steve Blank market-type decision for CAMIR (Existing / Re-segmented / New / Clone), the evidence behind it, and the strategic consequences that follow for sales cycle, positioning, capital and dominant risk.
**Why it exists** — the four market types demand incompatible playbooks, and CAMIR sits close enough to two of them to be misplayed. Treat it as an **existing** market and the plan becomes "beat Martian on router accuracy" — a claim the routing-plateau result falsifies in one citation [S4]. Treat it as a **new** market and the plan becomes years of category creation funded by nobody, which is how TensorZero died with $7.3M raised and 11,000 stars [S23]. Getting this wrong picks the wrong competitor, the wrong sales cycle, and the wrong burn rate simultaneously.
**How to read it** — §Decision first, then §Why not the other three, which is where the argument actually is. §Consequences is the operating table every downstream strategy artifact inherits.
**Depends on / feeds** — depends on [../research/competitors.md](../research/competitors.md), [../research/landscape.md](../research/landscape.md), [../BRIEF.md](../BRIEF.md); feeds [positioning.md](positioning.md), [market_sizing.md](market_sizing.md), [gtm.md](gtm.md), [channel_plan.md](channel_plan.md), [../validation/stage_gate.md](../validation/stage_gate.md) and [../financials/use_of_funds.md](../financials/use_of_funds.md).

---

## Decision

**CAMIR enters a RE-SEGMENTED market — niche re-segmentation of LLM inference cost optimisation, along the axis of who owns the weights and who sets the quality tolerance.**

The market exists, is large, has named incumbents and a revealed price. CAMIR does not create demand for cheaper inference; that demand is already served four ways. It carves off a segment the incumbents **cannot** serve without abandoning their own business model: teams running their own weights, who need a tolerance dial they set themselves and a savings number they can audit.

---

## The evidence

| Claim | Evidence |
|---|---|
| The market exists and buyers already pay | OpenRouter reached **$160M annualised revenue by August 2026**, up from $50M at end-2025, monetising at **~5% on top of inference spend** [S17]. Money already flows to a routing-adjacent layer at scale |
| The problem is recognised, not latent | Inference became the **second-largest line item in enterprise AI budgets in 2026**, behind talent only; 55–80% of enterprise AI GPU spend goes to inference [S25]. Nobody needs convincing that inference costs money |
| The category has incumbents with mindshare | Martian, Not Diamond, OpenRouter, LiteLLM, RouteLLM, plus vendor-native routing [S17][S18][S19][S20][S22] |
| **The incumbents are structurally confined to one segment** | Every commercial router's value *is* the hosted catalog. Serving self-hosters means giving up the catalog, the billing relationship and the intermediary position. This is a business-model wall, not a roadmap gap |
| **The segment is real and unmonetised** | The self-hosted end of the market is served only by free software — LiteLLM, RouteLLM, vLLM Semantic Router [S11][S22]. Nobody is charging there |
| The re-segmentation dimension is validated by a public failure | When vendor-native routing shipped with a vendor-set tolerance, users immediately reported quality degradation on complex queries and had no dial to adjust [S20][S21]. The dimension CAMIR re-segments on is one the market has already noticed hurts |

---

## Why not the other three

**Not an Existing market.** In an existing market you win on performance against a known metric. CAMIR's known metric would be router accuracy — and 21 routing methods across 5 benchmarks converge into a narrow band far below the oracle router, with the best available remedies buying **up to 2.13 percentage points** [S4]. There is no performance headroom to win on. A frontal existing-market entry is a race to a ceiling the field has already documented.

**Not a New market.** New markets require educating buyers that a problem exists. This problem is the second-largest line in the budget [S25] and has four active workarounds. Claiming a new market here would be self-flattery, and it would license the slow, capital-hungry adoption curve that new-market entrants are allowed — which CAMIR cannot afford and does not need.

**Not a Clone.** Cloning a proven model into an underserved geography is not the play; the underserved axis is architectural, not regional.

**The near-miss worth naming:** re-segmentation has two flavours, and CAMIR is the **niche** kind, not the **low-cost** kind. It does not win by being a cheaper router. It wins by serving a segment whose requirements — own the weights, set the tolerance, audit the saving — the incumbents cannot meet. Confusing the two would produce a price-war plan against a free product [S20], which is unwinnable.

---

## Strategic consequences of niche re-segmentation

| Dimension | Consequence for CAMIR | Why |
|---|---|---|
| **Sales cycle** | Short for the technical evaluation (a day to point traffic at a proxy and read a frontier), **long for the budget decision** (a quarter or more, engineering-leader sign-off). Assume 60–120 days from first install to first paid contract `(assumption: no deal has been run)` | Re-segmented markets have a pre-existing budget line, so the buyer does not need convincing that the category matters — only that this entrant is the right one |
| **Positioning approach** | Against the **segment boundary**, never against a rival's feature list. The message is "for teams that run their own weights", not "more accurate routing" | The re-segmentation dimension is the whole differentiation; feature comparison drags the argument back onto the axis where everyone plateaus [S4] |
| **Competitor to plan against** | **Free**, not Martian. Vendor-native routing at no separate fee [S20] and free OSS proxies [S11][S22] set the price expectation | The commercial incumbents cannot follow CAMIR into the segment; the free ones are already in it |
| **Capital needs** | **Low relative to a new-market entrant, and that is the point.** The market exists, so spend goes to product and proof rather than category education. The first milestone is a published frontier, which costs GPU-hours and time, not sales headcount | Re-segmented markets reward evidence over evangelism |
| **Dominant risk** | **Segment size, not adoption.** The question is not "will self-hosters use this" — free tools already show they will — but **"is the segment large enough, and will it pay?"** Assumptions A3 and A4 | See [market_sizing.md](market_sizing.md), which finds a routing-fee TAM in the tens of millions and says so plainly |
| **Second risk** | **Commoditisation clock.** If serving-native routing [S11] acquires a measurement layer, the open half of CAMIR loses its reason to exist | Build measurement as the asset from day one, not routing |
| **Time-to-revenue posture** | Land on free open source, monetise the control plane. Expect a long free-to-paid lag | Open-core conversion runs **1–5% of active users for hosted SaaS, 0.01–0.1% for enterprise licences** [S40] |

---

## The post-mortem this type must survive

Re-segmentation via open core has a recent, dated death in this exact category, and naming it here is cheaper than being told it in a partner meeting.

**TensorZero** — open-source LLMOps, **$7.3M raised, 11,000 GitHub stars** — archived its repository on **12 June 2026**, returned unused capital, and cited the difficulty of finding product-market fit for an open-source project *and* a commercial product at the same time [S23]. In the same window, ClickHouse acquired Langfuse as part of a $400M Series D at a $15B valuation, and the frontier labs and hyperscalers shipped native gateway, observability and evaluation features [S23].

**What CAMIR must do differently, stated as commitments rather than hopes:**

1. **One artifact, two audiences — not two products.** TensorZero's diagnosis was carrying an OSS project and a commercial product simultaneously. CAMIR's open half and paid half are the *same measurement apparatus*: the harness that produces a public frontier is the harness that produces a customer's private one. If the paid layer ever requires code the open layer does not, that is the failure signal.
2. **Monetise the measurement, not the mechanism.** The mechanism commoditises [S11]; the audited savings number and the tolerance policy do not, because they are per-deployment and adversarial-proof by construction.
3. **Declare the consolidation exit as a real outcome, not a fallback.** Langfuse into ClickHouse is the base case for this category [S23]. `../financials/comps_exits.md` must model it as such rather than treating standalone scale as the only success.

---

## Recommended next 3

1. **Adopt "self-hosted pools with a customer-set tolerance" as the segment boundary in every artifact** — it is the re-segmentation dimension, and every positioning, channel and sales decision derives from it.
2. **Size the segment before committing capital.** The dominant risk of this market type is segment size (A3), and it is answerable with desk research plus twenty discovery calls, not with a build. See [market_sizing.md](market_sizing.md) and [../validation/riskiest_assumptions.md](../validation/riskiest_assumptions.md).
3. **Write the TensorZero commitments into the operating plan**, not just this file — specifically the "one artifact, two audiences" test, which is the earliest available warning that the open-core shape is failing the same way.
