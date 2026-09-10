# CAMIR — Pitch deck

**What this is** — a claim-led pitch deck for CAMIR, arranged for a technical investor who will challenge the measurement and economics.
**Why it exists** — a routing demo can show a cheap answer while hiding judge bias, self-hosted utilization and the consuming engineer's veto; without claim-led evidence, the deck sells a savings multiple that cannot survive buyer scrutiny.
**How to read it** — each title is the slide's claim, each visual names the evidence to render, and the skeptic should attack the oracle ceiling, all-in cost and unvalidated commercial assumptions.
**Depends on / feeds** — depends on [one_pager.md](one_pager.md), [vc_memo.md](vc_memo.md), [tech/whitepaper.md](../tech/whitepaper.md), [strategy/market_sizing.md](../strategy/market_sizing.md), [financials/pricing.md](../financials/pricing.md) and [financials/revenue_build.md](../financials/revenue_build.md); feeds [visuals/visual_manifest.md](../visuals/visual_manifest.md) and [README.md](../README.md).

## 1. The bill is fixed-model spend, but request difficulty is not

- Platform teams send mixed-difficulty requests through one tier because their own frontier is not measured.
- The easy request pays for the hard request's capacity.
- The consuming engineer bears quality risk while the budget owner receives the saving.

visual: V01 — Mixed-difficulty requests cross a fixed-model baseline

## 2. Borrowed routing headlines do not transfer to self-hosted pools

- RouteLLM's CPT is 3.66x on MT-Bench, 1.41x on MMLU and 1.49x on GSM8K [S2].
- FrugalGPT reached up to 98% under 2023-era hosted price ratios, with 16.6% escalating [S3].
- Self-hosted cost is utilization-sensitive: raw H100 cost is about $0.10/M tokens versus about $0.60/M hosted [S26], and realistic all-in cost is 3–5x raw rental [S27].

visual: V02 — Hosted claims versus self-hosted cost axis

## 3. Routing accuracy is plateauing, so measurement becomes the wedge

- The Routing Plateau benchmarks 21 methods across 5 benchmarks and finds a predictability bottleneck [S4].
- The best remedies gain up to 2.13 percentage points [S4].
- CAMIR does not claim a smarter router.

visual: V03 — Plateau evidence and the measurable-frontier thesis

## 4. A meaningful part of the apparent ceiling is the harness

- The Unsolvability Ceiling reports truncation in 65% of MMLU and 57% of MedQA cases [S5].
- It reports 5–12% parse failures on MMLU [S5].
- CAMIR publishes the frontier with artifact flags, not one unqualified score.

visual: V04 — Artifact-controlled oracle ceiling decomposition

## 5. The first output is a disqualification report, not a router demo

- Replay corpus builder and oracle ceiling probe run the customer's own logged requests through every tier, guarded and unguarded.
- **Worked case:** on Marcus's six endpoints, `account-reasoning` — his most expensive — comes back at a 22% ceiling and is **disqualified in week one**; `structured-extraction` shows a 31-point artifact share, recovered by a generation-budget change he could have made himself ([../product/journeys/beachhead.md](../product/journeys/beachhead.md), illustrative).
- The disqualification lands before the saving, which is what makes the saving believable. No vendor paid on routed volume can render this page.

visual: V05 — Qualify, measure, disqualify

## 6. The loop turns a request into an auditable decision

- **Classify → Dispatch → Judge → Attribute → Recalibrate**
- Cascade route is primary; classifier route is an ablation against calibrated confidence [S8].
- Durable records connect the decision, judgment, savings ledger and frontier run.

visual: V06 — CAMIR core loop

## 7. Ravi can stop routing without asking the platform team

- The saving lands in Dana's budget; the risk lands on Ravi, in a different reporting line, who gains nothing and can pin his endpoint to large without a meeting. This is how routed deployments die [S21].
- Four P0 features, ranked above classifier accuracy: tolerance he owns (a `NOT NULL` owner column), the tier stamped on **his own** trace, shadow before enforcement, and a pin effective on the next request.
- **Worked case:** his thumbs-down rate jumps 1.9 points; grouping his own spans by `camir.tier` shows the regression is on the *large*-tier path. Router exonerated in four minutes, without opening CAMIR ([../product/journeys/day_in_life.md](../product/journeys/day_in_life.md), illustrative).

visual: V07 — Ravi's control path from shadow mode to pin

## 8. The base case is a 1.4–1.5× cut in GPU cost — and the conservative case is a null result we would publish

- Routed cost ratio `r + e − h`, blended by the share of traffic owners allow to route: **1.39× base with no credit for harness repair, 1.49× with it; corridor 1.08×–2.84×** ([../tech/whitepaper.md](../tech/whitepaper.md) §2.6 — every input is a stated assumption).
- **Conservative × conservative is 1.08×, which does not pay for a control plane.** In that world CAMIR reports a null result.
- The largest single swing is not the router: routed coverage alone moves the base from 1.33× to 1.58×. That is an organisational variable, which is why slide 7's features rank above classifier accuracy.
- The multiple is on token-proportional GPU cost. On the all-in line, after the 3–5× engineering multiplier [S27], a 33% cut is 7–11%.

visual: V08 — Self-hosted cost-quality frontier

## 9. The control plane is priced out of the inference bill, as a share of a saving the customer computes

- **$30,000 ACV** = $600k spend × 70% post-cache × 25% saving × 28% share (assumption; untested with any buyer). The 25% sits below the whitepaper's own 28–33% base.
- The customer's engineer computes the counterfactual with open code in their own perimeter — which is what makes share-of-savings credible where a vendor-computed saving is not.
- **Blended CAC ~$4,200 (14% of ACV); payback ~2.2 months at steady-state margin.** Year-one gross margin is **42%**, not 75% — onboarding is still a service until it is automated ([../financials/unit_economics.md](../financials/unit_economics.md)).
- Declared fallback: ~5% of spend under management, OpenRouter's revealed take [S17].

visual: V09 — ACV derivation and open-core boundary

## 10. Routing fees alone are not venture-scale; the control plane is the option

- Base routing TAM is about $70M/year, with a $10–360M corridor; SAM is about $20M/year (assumption: market-sizing model).
- Product 2 applies the same measurement machinery to cache policy, quantization, batching and upgrade regression, but demand is unvalidated.
- Hybrid expansion is a spend-pool scenario, not a company-count multiplier.

visual: V10 — Product 1 ceiling and conditional Product 2 expansion

## 11. Distribution depends on an upstream proxy, so the dependency is explicit

- The proposed channel is a routing strategy inside LiteLLM, not a rival proxy.
- Maintainer acceptance is an experiment and appears in the kill order.
- No merge, no base-case channel claim.

visual: V11 — Channel dependency and fallback path

## 12. Every competitor is paid by the volume it routes, so none can tell a customer not to buy

- **OpenRouter** takes ~5% of the bill it sits on, $160M annualised by Aug 2026 [S17]. **GPT-5** ships routing free, with a vendor-set tolerance — and a documented backlash over degraded complex queries [S20][S21]. **Martian** is reported by one weak source to have neared ~$1.3B [S18]; its revenue is unknown [G3].
- **The serving engine** is absorbing dispatch [S11] — the real clock. It will not run a customer's ceiling probe, publish judge agreement or issue a disqualification.
- **The closest comparable is a shutdown:** TensorZero archived after $7.3M and 11,000 stars [S23]. CAMIR's open/paid line is published before release for that reason.
- The moat is weak and per-deployment: labels, tolerance history and signed attribution may compound (A6, untested). The claim is measurement credibility, not algorithmic superiority.

visual: V12 — Commodity mechanism versus customer-owned records

## 13. The capstone starts with a falsifiable experiment, not traction

- **Abhishek Darji, Aniket Anil Naik, Tamizh Selvan Manivannan** — an SJSU CMPE 295A capstone team advised by Prof. Vijay Eranti. Pre-traction: no customers, no revenue, no measurement of our own.
- **Weak on domain, real on measurement:** new to LLM routing, but two years of professional test automation — data-driven harnesses at 1,000+ test cases, CI pipelines, evaluation infrastructure. The hard part of CAMIR is the evaluation, and that is the part we have built before (A13, an assumption until E2 runs).
- E1 (is there a ceiling), E2 (is part of it the harness) and E4 (does the segment exist) decide in about six weeks whether this should continue.

visual: V13 — Evidence ladder from capstone to customer proof

## 14. $2.2M over 21 months, staged so 87% is unspent if the ceiling is not there

- **Block 1 — $280k, months 1–4:** does the oracle ceiling exist? Low-fidelity MVP, GPU time for ≥ 3 corpora, 20 discovery screens. **If the ceiling fails, publish the negative result and stop** — [G2] makes the null a contribution.
- **Block 2 — $820k, months 5–12:** will a consuming engineer enable enforcement on his own endpoint? High-fidelity MVP, five design partners, and $120k ring-fenced for onboarding automation — the whole 42%→75% gross-margin bend.
- **Block 3 — $1.1M, months 13–21:** does it repeat, and will buyers pay? Control plane, ≥ 3 paid pilots at ≥ $30,000 ACV, one account surviving a pool change.
- Every figure is `(assumption)`; no raise, term sheet or conversation has occurred. See [../financials/use_of_funds.md](../financials/use_of_funds.md).

visual: V14 — Three-gate validation sequence

## Recommended next 3

1. Render V03, V04 and V06 first because they carry the technical thesis.
2. Validate E1/E2 before using the base-case frontier or savings scenario as evidence.
3. Test the LiteLLM channel and the $30,000 ACV with named decision-makers, while preserving the pre-traction status.
