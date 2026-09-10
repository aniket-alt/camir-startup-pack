# CAMIR — Pricing

**What this is** — the value metric, the budget line the price comes out of, a sourced competitor price table, the tier design, willingness-to-pay per persona, and an honest account of how much pricing power CAMIR actually has.
**Why it exists** — CAMIR sells against a substitute that is free: a frontier vendor ships routing at no separate fee [S20], and the serving stack is absorbing routing as an open-source feature [S11]. A price asserted without naming the budget it comes out of would collapse in the first buyer conversation, and the failure would be silent — the deal simply never closes and nobody says why. This file fixes the one decision that survives that pressure: **price out of the inference bill or not at all**.
**How to read it** — §The one decision, then §The ACV arithmetic, which is the only place the $30k number is derived rather than quoted. A skeptic should attack the 25% savings rate: it is an assumption, not a measurement [G2].
**Depends on / feeds** — depends on [../strategy/petal_diagram.md](../strategy/petal_diagram.md), [../strategy/market_sizing.md](../strategy/market_sizing.md), [../strategy/personas.md](../strategy/personas.md), [../research/sources.md](../research/sources.md); feeds [revenue_build.md](revenue_build.md), [unit_economics.md](unit_economics.md), [../strategy/channel_plan.md](../strategy/channel_plan.md) and [../strategy/sales_roadmap.md](../strategy/sales_roadmap.md).

**No price in this file has been tested with any buyer** (assumptions A4, A5). Every figure is derived from a stated assumption or carries a source.

---

## The one decision

**The value metric is a share of measured savings — ~28% — charged against the customer's existing inference bill.**

The reason is not elegance. [../strategy/petal_diagram.md](../strategy/petal_diagram.md) examines five adjacent budgets and finds that **only one holds real, approved, transferable money: Petal 1, the inference spend itself.** Petal 2's gateway budget belongs to teams who chose *not* to self-host. Petal 3 is free software. Petal 4 is a pricing precedent, not a customer. Petal 5 is unbudgeted engineering time.

Every pricing model other than share-of-savings requires the buyer to **create a new line item**. Shrinking an existing line is a fundamentally easier sale than opening a new one, and at a $30k ACV there is no budget to fund the longer cycle that a new line item implies. That is the argument, and it is stronger than the incentive-alignment argument in `../BRIEF.md`.

**Declared fallback, decided now rather than under pressure:** if savings measurement proves contentious in the first three priced conversations, switch to **~5% of inference spend under management**, anchored to OpenRouter's revealed take rate [S17]. The fallback lands within a few percent of the same ACV (§The ACV arithmetic), which is what makes it a safe fallback rather than a repricing.

---

## The ACV arithmetic

The single derivation everything downstream depends on. Change any line and recompute.

| Line | Value | Basis |
|---|---|---|
| Beachhead customer's annual inference spend | **$600,000** | $50k/month, the beachhead profile in `../BRIEF.md` §Users and [../strategy/personas.md](../strategy/personas.md) P2 |
| × share surviving upstream semantic caching (f4) | ×0.70 | `[sourced]` production cache hit rates **20–45%** [S36]; base case takes the midpoint of the loss |
| = **Routable spend** | **$420,000** | Routing only ever prices cache-miss traffic [G4] |
| × realised saving rate on routable spend | ×25% | `(assumption: conservative against RouteLLM's 1.41× on knowledge tasks and 1.49× on math` [S2]`; no CAMIR measurement exists` [G2]`)` |
| = **Measured annual saving** | **$105,000** | |
| × CAMIR share of measured savings | ×28% | `(assumption: no universal rate is published by any share-of-savings vendor` [S38][S39]`; 28% sits inside the band those vendors are reported to negotiate)` |
| = **ACV** | **≈ $29,400 → $30,000** | Matches [../strategy/market_sizing.md](../strategy/market_sizing.md) |

**Note on f3.** [../strategy/market_sizing.md](../strategy/market_sizing.md) applies a second factor f3 (×0.70, share of spend above the volume floor) when sizing the *market*. It is deliberately **not** applied here: f3 is a population filter that decides which companies qualify, and the beachhead customer is by definition already above the floor of ~16M tokens/day [S27]. Applying it twice would understate ACV by 30%.

### The convergence check

Two independent anchors, computed separately:

- **Savings-share anchor:** 28% of $105,000 = **$29,400**
- **Spend-percentage anchor:** 5% of $600,000 = **$30,000**, using OpenRouter's revealed ~5% take rate on inference spend [S17]

They agree within 2%. That is mild evidence the number is not arbitrary — and no more than that, because both are anchored to adjacent categories [G5], and the second is a *gateway* price that bundles catalog access and billing consolidation, not routing alone.

**The uncomfortable corollary.** The convergence holds only at a 25% savings rate. At 15% the savings-share anchor gives $17,600 while the spend anchor still gives $30,000 — a 41% gap. **The two anchors converge at exactly one savings rate, and nobody has measured it.** Treat the agreement as a coincidence to be re-tested after the first frontier, not as corroboration.

---

## Competitor price table

| # | System | What it charges | Price | Confidence | What it does to CAMIR's price |
|---|---|---|---|---|---|
| 1 | **OpenRouter** | % of inference spend, gateway + routing | **~5%**, on $160M annualised revenue as of Aug 2026 [S17] | *secondary* | Sets the ceiling for a routing/gateway layer at ~5% of spend. The only revealed market price in the category |
| 2 | **GPT-5 unified system** | Nothing. Routing is built in | **$0 separate fee** — you pay underlying model tokens [S20] | *primary* | **Sets the adjacent segment's price expectation for routing at zero.** CAMIR is priced against a free substitute and must say why it is not the same thing: the vendor sets the tolerance, and the documented backlash [S21] is what that produces |
| 3 | **Not Diamond** | Per million tokens routed | ~**$0.05/M tokens** | **weak** — competitor-authored [S19]; verify before quoting | If real, this is a *recommender* price, ~1% of a $5/M hosted token cost. It prices the decision, not the outcome |
| 4 | **Martian** | Enterprise "contact sales", per-request attribution | Undisclosed; reportedly neared a **~$1.3B valuation April 2026** — single weak secondary source, hedge required [S18] | **weak** | Tells us enterprise routing supports an opaque price, nothing about its level |
| 5 | **LiteLLM** | Open-source proxy free; enterprise tier paid | Undisclosed tier pricing [S22] | *secondary* | The open-core shape CAMIR copies, and the channel it distributes through — a partner, not a competitor on price |
| 6 | **ProsperOps / nOps / Usage.ai** | Share of realised cloud savings | **No universal published rate.** The percentage *and* the definition of eligible savings are set per customer agreement [S38][S39] | *secondary* | The direct precedent for CAMIR's metric — and the warning: **the eligible-savings definition is what actually gets negotiated**, not the percentage |
| 7 | **vLLM Semantic Router** | Nothing — routing inside the serving engine | **$0** [S11] | *primary* | The long-run price of the routing mechanism is zero. Only the measurement layer can hold a price |

**What the table says.** Two of the seven charge nothing, and both are structurally positioned to keep charging nothing. Rows 1 and 6 are the only two priced references, and they price *different things* — a gateway's take of spend, and a FinOps vendor's take of savings. CAMIR sits between them and borrows from both.

---

## Tier design

Three tiers, and the boundary between them is a commitment, not a packaging exercise. `../BRIEF.md` and [../strategy/market_type.md](../strategy/market_type.md) commit that **no feature ever moves from the open tier to a paid one** — the one promise whose breach kills open-core credibility (P6 Sam's objection in [../strategy/personas.md](../strategy/personas.md)).

| Tier | Who | What it contains | Price | Why the boundary sits here |
|---|---|---|---|---|
| **Open** | P6 Sam, P1 Priya, every evaluator | Router, tier abstraction, **classifier route** and **cascade route**, benchmark harness, judging protocol, published frontiers, single-deployment observability | **$0, permanently** | Open-core conversion runs 1–5% of active users to hosted SaaS [S40]. The free base must be large or the paid layer has no funnel. Gating this is how the funnel dies |
| **Control Plane** | P2 Marcus evaluates, P4 Dana signs | Per-deployment classifier training, quality-tolerance policy management, **savings measurement and attribution**, routing observability across deployments, continuous re-measurement on model upgrade, per-endpoint tolerance ownership for P5 Ravi | **28% of measured savings**, floor **$12,000/yr**, cap **$120,000/yr** | The data loop and the audit trail live here (A7). This is the only place switching cost can accumulate (A6) |
| **Enterprise** | P3 Wen, multi-pool platform orgs | Bring-your-own judge certification, multi-pool and multi-tenant attribution, air-gapped deployment, contractual savings guarantee, named support | **Negotiated**, floor **$60,000/yr** | Enterprise open-source licences convert at 0.01–0.1% of active users at much higher value [S40]; this tier exists for the ten accounts that number implies, not as a growth engine |

### Why a floor and why a cap

**The floor at $12,000/yr** is not a packaging convenience — it is a margin requirement. [../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) block 7 sets a kill threshold: **measurement cost exceeding 30% of ACV breaks the margin.** [unit_economics.md](unit_economics.md) computes fully-loaded per-customer cost of **~$8,800/yr at 68 customers**. A customer whose measured savings imply a fee below the floor is a customer CAMIR loses money serving. The floor is where that line sits, with a thin margin above it.

**The cap at $120,000/yr** protects the customer, and therefore CAMIR. A share-of-savings contract with no cap on a customer whose spend grows 26% a year [S24] produces an invoice that eventually looks absurd relative to the work done, and the buyer cancels — this is the well-known failure mode of outcome pricing at scale. The cap corresponds to a customer spending ~$2.4M/yr on inference. Above it, the contract converts to the Enterprise tier's negotiated form, which is the honest way to reprice.

### What is deliberately not a tier

- **A per-seat tier.** Nobody sits in front of CAMIR. The buyer is the bill, not the user.
- **A per-request tier as the primary metric.** It is the *fallback* (A5), and it has a specific defect: it charges most when routing is working hardest, which inverts the alignment argument.
- **A free-trial-to-credit-card motion.** [../strategy/channel_plan.md](../strategy/channel_plan.md) rejects self-serve for the paid layer: the buyer is an engineering leader signing against a measured saving, not a developer swiping a card.

---

## Willingness to pay, per persona

| Persona | Budget they control | Would they pay? | What they would pay for | The number |
|---|---|---|---|---|
| **P1 Priya** — drop-in, 60-person SaaS | None directly; ~$8–15k/mo of GPU | **Rarely.** She self-hosts for a contractual data-residency reason [S28], not for cost | A default configuration that cannot silently hurt her, and shadow mode | Below the floor. **She is an Open-tier user, and the pack should stop pretending otherwise** |
| **P2 Marcus** — beachhead, ~$50k/mo | Recommends; does not sign | **Yes, and he is the one who computes the number.** His stated objection is that vendors do their own math [S17][S20] | The harness running inside his perimeter so he can re-run it | Evaluates at $30k; his test is whether he can reproduce the saving himself |
| **P3 Wen** — knobs, very high volume | Real budget, own eval team | **Yes, at 2–4× the beachhead ACV** — she has the spend. But she is also the most likely to build it | Multi-pool attribution, judge certification, published inter-judge agreement [S33][S34] | **$60–120k** Enterprise. Her real objection is "I could build this in three weeks", which is true; the counter is maintenance, not price |
| **P4 Dana** — Head of Platform, signs | **Owns the infrastructure budget** | **Yes, if the saving is attributable in writing** | Attribution she can defend in a quarterly budget review, and a name against "who checked" | Indifferent between $30k and $45k if the attested saving is $105k. **Sensitive to the *definition*, not the price** [S38] |
| **P5 Ravi** — the veto | None | **Never.** He gains nothing and carries the quality risk | Per-endpoint tolerance and a unilateral off switch | $0, and he can stop the deal. Pricing cannot reach him; product must |
| **P6 Sam** — OSS adopter | Below the volume floor [S27] | **Never, by construction** | An afternoon to a chart, and a licence that stays put | $0 forever. He is the channel [S40] |

**The pattern.** Only two of six personas hold budget, and the one who signs (Dana) is **price-insensitive and definition-sensitive**. That is the single most actionable finding in this file: the negotiation is over what counts as an eligible saving, exactly as the FinOps precedent predicts [S38][S39], and [../strategy/sales_roadmap.md](../strategy/sales_roadmap.md) already makes a signed eligible-savings definition a required deal step.

---

## Pricing power — stated honestly, it is weak today

A pricing-power section usually argues that price rises as a data moat compounds. For CAMIR that argument is available but thin, and overstating it here would contradict `../BRIEF.md` §Mechanism & moat, which calls the moat weak in the founder's own words.

**What genuinely supports price:**

1. **The counterfactual is expensive to compute independently.** To dispute a savings invoice, a customer must re-run the fixed-model baseline on their own traffic — which costs real GPU-hours. That is not a moat, but it is friction in CAMIR's favour, and it is the same friction that makes FinOps share-of-savings contracts stick [S38].
2. **Per-deployment routing history (A6).** A deployment that has seen a million of a customer's own requests should route them better than a cold start. **Untested** — [../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) block 6 sets the kill threshold at cold-start performance within 3% of a warmed router. If it flattens, this line disappears from the pricing argument entirely.
3. **The audit trail Dana defends in a budget review.** Continuity has value independent of the router: switching vendors means the before-and-after series breaks, which is precisely the artifact she needs.

**What actively erodes price, and is better evidenced than any of the above:**

1. **The routing mechanism commoditises into the serving engine** [S11]. When vLLM ships routing natively at $0, the mechanism's price is zero. Only measurement holds a price. This is the locked strategic position and it is a concession, not a strength.
2. **The tier cost spread is compressing.** A Gemma 4 31B-thinking model sits within **~10 LMArena Elo points of 600B–1000B+ open-weight frontier models at ~10× fewer parameters** [S29]. As the spread narrows, the saving narrows, and a share-of-savings price falls with it — automatically, without a renegotiation.
3. **Free routing from the frontier vendor** [S20] anchors the adjacent segment's expectation at zero.

### The repricing trigger — declared in advance

The denominator grows at ~26% CAGR [S24] while the savings rate per dollar of spend probably shrinks [S29]. **These pull in opposite directions and no source resolves them.** Rather than pretend to know which wins, declare the trigger now:

> **If measured realised savings fall below 15% of routable spend across three consecutive customers, the value metric converts from share-of-savings to a percentage of inference spend under management (2–3%), and the pitch converts from "we cut your bill" to "we govern your efficiency frontier."**

That is not a fallback, it is the bridge to the second product in [revenue_build.md](revenue_build.md) — and the arithmetic is why: at a 15% savings rate the share-of-savings ACV falls to ~$17,600, while 2.5% of a $600k spend holds $15,000 **and rises with the customer's spend rather than falling with the model price curve.**

---

## Recommended next 3

1. **Draft the eligible-savings definition before the first priced conversation, not during it.** The FinOps precedent is unambiguous that this is where the negotiation happens [S38][S39]. It needs: the baseline (always-large fixed-model cost on the same traffic), the measurement window, who runs the judge, what happens when the pool changes, and what happens when the customer's traffic mix shifts. One page, signed, per [../strategy/sales_roadmap.md](../strategy/sales_roadmap.md).
2. **Put the floor and the cap in front of the first three shadow-mode installs as a priced proposal.** [../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) block 5 already specifies this test at ~$0 marginal cost; the kill threshold is zero of three willing to discuss a number after seeing their own measured saving.
3. **Re-derive this entire file the day the first self-hosted frontier is measured.** Every number here descends from the 25% savings assumption [G2]. If the oracle ceiling comes in low, the ACV is not $30k and the tier design is wrong, not just the price.

<!-- critic: unresolved — the 28% share rate has no published comparable at any specificity; [S38][S39] establish only that share-of-savings pricing exists and that rates are private. The number is defensible as "inside the band" only by inference. It should be treated as the first thing a discovery call tests, not as a derived figure. -->
