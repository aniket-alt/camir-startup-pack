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

- Replay corpus builder and oracle ceiling probe run the customer's request distribution across the model pool.
- If the ceiling cannot support a meaningful frontier, CAMIR says do not deploy.
- A negative result is cheaper than a production regression.

visual: V05 — Qualify, measure, disqualify

## 6. The loop turns a request into an auditable decision

- **Classify → Dispatch → Judge → Attribute → Recalibrate**
- Cascade route is primary; classifier route is an ablation against calibrated confidence [S8].
- Durable records connect the decision, judgment, savings ledger and frontier run.

visual: V06 — CAMIR core loop

## 7. Ravi can stop routing without asking the platform team

- Per-endpoint quality tolerance belongs to the consuming engineer.
- Every tier decision is stamped on the caller's trace.
- Shadow mode precedes enforcement; unilateral pin-to-large works on the next request.

visual: V07 — Ravi's control path from shadow mode to pin

## 8. The customer sees a cost-quality frontier, not a magic percentage

- Cost uses amortized GPU-hours per token and declares utilization and the all-in multiplier [S26][S27].
- Judge harness uses temperature 0, position control, verbosity control and agreement reporting [S33][S34].
- The fixed-model baseline and oracle ceiling bound every route.

visual: V08 — Self-hosted cost-quality frontier

## 9. The control plane is priced from the inference bill

- Open-core keeps router, harness and frontier builder open.
- Paid control plane covers tolerance management, savings measurement and routing observability.
- The $30,000/year ACV is a scenario: $600,000 annual spend × 70% cache-miss traffic × 25% saving assumption × 28% pricing hypothesis.

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

## 12. The moat is weak; the measurement history may still compound per deployment

- The routing mechanism commoditizes into the serving engine [S11].
- Per-deployment labels, tolerance history and signed attribution may create switching friction, but A6 is untested.
- The claim is measurement credibility, not algorithmic superiority.

visual: V12 — Commodity mechanism versus customer-owned records

## 13. The capstone starts with a falsifiable experiment, not traction

- CAMIR is a pre-traction SJSU CMPE 295A capstone.
- A13 says test-automation and evaluation-infrastructure background transfers to benchmark engineering; it is an assumption.
- E1, E2 and E4 decide whether the venture should continue.

visual: V13 — Evidence ladder from capstone to customer proof

## 14. The ask is to measure the ceiling before scaling the story

- Complete the artifact-controlled self-hosted oracle experiment.
- Run shadow mode with endpoint ownership, traces and pin-to-large.
- Validate eligible savings, price and the buyer's willingness to sign.

visual: V14 — Three-gate validation sequence

## Recommended next 3

1. Render V03, V04 and V06 first because they carry the technical thesis.
2. Validate E1/E2 before using the base-case frontier or savings scenario as evidence.
3. Test the LiteLLM channel and the $30,000 ACV with named decision-makers, while preserving the pre-traction status.
