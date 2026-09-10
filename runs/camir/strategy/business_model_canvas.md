# CAMIR — Business Model Canvas

**What this is** — Osterwalder's nine blocks for CAMIR, each carrying one falsifiable hypothesis and the cheapest test that would kill it. This is Blank's Customer Discovery Phase 1 artifact.
**Why it exists** — the Lean Canvas ([lean_canvas.md](lean_canvas.md)) drops **Key Partners, Key Activities and Customer Relationships**, and for CAMIR those three are not incidental blocks: Key Partners holds the LiteLLM distribution dependency that is the *only* channel whose CAC survives the TAM, Key Activities holds the GPU-hours that are the dominant variable cost, and Customer Relationships holds the open-core boundary whose blurring is what archived TensorZero's repository [S23]. Losing those three would hide the three most likely ways CAMIR fails.
**How to read it** — read the **Test** column, not the block contents. Blank's discipline is that a canvas is a set of guesses, and the only useful question about a guess is what would disprove it and what that costs. §Kill order ranks the nine by test cost against consequence.
**Depends on / feeds** — depends on [lean_canvas.md](lean_canvas.md), [market_sizing.md](market_sizing.md), [gtm.md](gtm.md), [personas.md](personas.md); feeds [../validation/experiment_board.md](../validation/experiment_board.md), [../validation/riskiest_assumptions.md](../validation/riskiest_assumptions.md), [../validation/stage_gate.md](../validation/stage_gate.md) and [../financials/unit_economics.md](../financials/unit_economics.md).

---

## The nine blocks

### 1. Customer Segments

- **Beachhead:** platform teams self-hosting an open-weight model pool at ≥~$50k/month inference spend, with mixed-difficulty traffic through a shared service (P2 Marcus).
- **Edge-low:** teams self-hosting for data residency rather than price [S28], who need defaults that cannot silently hurt them (P1 Priya).
- **Edge-high:** teams with their own tiers, judge and evaluation practice (P3 Wen).
- **Explicitly excluded:** anyone on a single hosted catalog — routing is free for them [S20] — and anyone below ~16M tokens/day, who should not self-host at all [S27].

> **Hypothesis:** at least **1,000 companies** globally self-host an open-weight model pool at ≥$50k/month with mixed-difficulty traffic. `(assumption A3; no denominator exists [G1])`
> **Cheapest killing test:** 20 discovery calls asking one question — *what share of your production LLM tokens runs on weights you operate, and what is the monthly spend?* — plus a count of companies visibly hiring for self-hosted inference roles. **Cost: 3 weeks, $0.** **Kill threshold: fewer than 6 of 20 qualified teams meet the profile.**

### 2. Value Propositions

Set your own quality tolerance; see the measured cost-quality frontier for your own traffic; audit the savings yourself because the harness runs in your perimeter. Per-endpoint tolerance ownership and per-request tier attribution so a shared-infrastructure change is survivable.

> **Hypothesis:** the **tolerance dial and auditable attribution** — not the saving — are what a buying unit responds to, because the saving is available free elsewhere [S20].
> **Cheapest killing test:** in discovery, present two one-paragraph descriptions — savings-first and tolerance-first — and ask which they would forward to their manager, then ask *why*. **Cost: inside the same 20 calls, $0.** **Kill threshold: savings-first preferred by more than 14 of 20.**

### 3. Channels

Proxy-native distribution as a routing strategy inside LiteLLM [S22]; the published self-hosted frontier as the credibility artifact [G2]; peer proof; conference talks. Paid advertising and outbound sales rejected on CAC ([gtm.md](gtm.md)).

> **Hypothesis:** a routing strategy shipped inside an installed proxy converts at open-core rates — **1–5% of active installs** reaching the paid control plane [S40].
> **Cheapest killing test:** ship the open harness first and instrument enable-rate and volume distribution before any paid layer exists. **Cost: 0 marginal, it is the product.** **Kill threshold: under 0.5% of installs cross the volume floor.**

### 4. Customer Relationships

Self-serve for the open router and harness; hands-on for the first ten paid deployments; a **hard, published open-core boundary** — router and harness open forever, control plane paid, no feature ever migrated from open to paid.

> **Hypothesis:** the boundary can be held without either starving the paid layer of value or hollowing out the free one — the failure that archived TensorZero after $7.3M and 11,000 stars [S23].
> **Cheapest killing test:** the **"one artifact, two audiences" test** applied at every release — if a paid feature requires code the open harness does not contain, the boundary has already broken. **Cost: $0, a release checklist.** **Kill threshold: two consecutive releases where the paid layer needs closed code.**

### 5. Revenue Streams

Share of measured savings (~28%), precedented in cloud FinOps where rates are set per customer and never published [S38][S39]. Fallback: per-request, or ~5% of inference spend, anchored to OpenRouter's revealed take rate [S17]. ACV ≈ $30k/yr at a $600k/yr-spend customer.

> **Hypothesis:** a self-hosting team will pay a **third-party fee for routing** when a frontier vendor offers routing at no separate charge to the adjacent segment [S20]. `(assumption A4 — untested with any buyer)`
> **Cheapest killing test:** after three shadow-mode installs produce real before-and-afters, put a priced proposal in front of each. **Cost: ~$0 beyond the installs.** **Kill threshold: zero of three will discuss a number after seeing their own measured saving.**

### 6. Key Resources

The frontier harness (benchmark + judging protocol + self-hosted cost axis); accumulated per-deployment routing history and calibration; the published corpus of self-hosted frontiers; benchmark-engineering capability in the founding team.

> **Hypothesis:** per-deployment routing history creates enough **switching cost** to matter — a deployment that has seen a million of a customer's own requests routes them measurably better than a cold start. `(assumption A6, flagged untested by the founder)`
> **Cheapest killing test:** offline, on one customer's logged traffic — train on the first N requests, measure realised saving at fixed tolerance as N grows, and see whether the curve rises or flattens immediately. **Cost: GPU-hours only, one week.** **Kill threshold: cold-start performance within 3% of a million-request-warmed router. If it flattens, the moat is zero and the pack must say so.**

### 7. Key Activities

Benchmark construction and maintenance; judging-protocol engineering against the 2026 reliability findings [S33][S34]; per-deployment classifier training; savings attribution; continuous re-measurement as the frontier moves with each model release.

> **Hypothesis:** continuous re-measurement is a **recurring need**, not a one-off — the frontier moves enough with each model upgrade that a stale policy loses material money.
> **Cheapest killing test:** re-run one fixed benchmark against a pool before and after one model upgrade and measure how far the optimal tolerance point moves. **Cost: GPU-hours, days.** **Kill threshold: the optimal point moves less than 5% — in which case this is a one-time consulting engagement, not a subscription, and the entire revenue model changes shape.**

### 8. Key Partners

**LiteLLM** — the distribution channel, and the block CAMIR least controls [S22]. **vLLM / SGLang** — the serving substrate, and simultaneously the commoditisation threat [S11][S32]. Open-weight model publishers — the tiers themselves. Academic collaborators — the capstone advisor and the venue for the methodology publication.

> **Hypothesis:** the primary channel partner will accept an upstream routing strategy contributed by an outside team, and will not build an equivalent natively first.
> **Cheapest killing test:** open the conversation now — a proposal issue and a working prototype. **Cost: 1–2 engineer-weeks.** **Kill threshold: rejected, or a native equivalent lands first — in which case [gtm.md](gtm.md) has no primary channel and the whole GTM must be re-planned.**
> **This is the most under-controlled block in the canvas and the one to test first.**

### 9. Cost Structure

GPU-hours for benchmark runs and per-deployment training (dominant variable cost, scaling with *customers* rather than with their traffic); judge inference for continuous measurement (recurring); three founders' engineering; no sales headcount before evidence. **Not** model hosting — CAMIR routes across the customer's pool and never serves models.

> **Hypothesis:** gross margin stays above **70%** after GPU-hours for training and judging.
> **Cheapest killing test:** measure GPU-hours consumed per shadow install end-to-end, and divide by the $30k ACV. **Cost: metering already in place.** **Kill threshold: measurement cost exceeds 30% of ACV.** This is the AI-native margin trap and it is measurable from the very first install — see [../financials/unit_economics.md](../financials/unit_economics.md).

---

## Kill order — cheapest decisive test first

| # | Block | Test cost | Consequence if it fails | Run when |
|---|---|---|---|---|
| 1 | **Key Partners** | 1–2 engineer-weeks | GTM loses its only viable channel | **Now.** It depends on a third party and has the longest lead time |
| 2 | **Customer Segments** (A3) | 3 weeks, $0 | The segment is too small to be a company; TAM collapses from $70M toward $10M | Now, in parallel |
| 3 | **Value Propositions** | $0, same calls | The pitch is wrong; positioning is re-derived | Now, in parallel |
| 4 | **Key Resources** (A6) | 1 week GPU | The moat is zero; the pack must say so plainly | After the first customer traffic exists |
| 5 | **Cost Structure** | metering | Margin is services-shaped, not software-shaped | From the first shadow install |
| 6 | **Key Activities** | days | Subscription becomes one-off consulting | After one model-upgrade cycle |
| 7 | **Revenue Streams** (A4) | $0 after installs | No revenue layer; CAMIR is an OSS project | Day 61–90 |
| 8 | **Channels** | product-native | Distribution does not convert | Continuous |
| 9 | **Customer Relationships** | $0 checklist | Open-core boundary erodes; the TensorZero shape [S23] | Every release |

**Two of the top three tests cost nothing but conversation, and the third costs two engineer-weeks.** The entire business model is falsifiable for under a month of work before any product is finished — which is the point of the canvas and the reason it is worth writing before the build.

## Recommended next 3

1. **Open the LiteLLM partner conversation this week.** It is block 8, it is first in the kill order, it has the longest external lead time, and every channel assumption in [gtm.md](gtm.md) is downstream of it.
2. **Fold blocks 1, 2 and 5's tests into a single 20-call discovery script** so one activity kills or confirms three blocks. See [../validation/discovery_guide.md](../validation/discovery_guide.md).
3. **Run the cold-start-versus-warmed test (block 6) as soon as any real traffic exists.** It is the only experiment that can produce a *positive* moat finding, and the honest default in `../BRIEF.md` is that there is no moat — so a measured result either way improves the pack.
