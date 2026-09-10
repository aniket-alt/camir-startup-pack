# CAMIR — One-pager

**What this is** — a one-page investment and operating brief for CAMIR, a cost-aware inference router for self-hosted open-weight model pools.
**Why it exists** — a team can cut inference spend while the consuming engineer absorbs quality risk; without a customer-owned tolerance, trace attribution and a disqualification test, routing gets pinned or rejected before savings are billable.
**How to read it** — start with the technical claim and operating loop, then attack the oracle ceiling, self-hosted cost axis and $30,000 ACV assumption.
**Depends on / feeds** — depends on [BRIEF.md](../BRIEF.md), [research/](../research/), [strategy/](../strategy/), [product/PRD.md](../product/PRD.md), [tech/whitepaper.md](../tech/whitepaper.md) and [financials/pricing.md](../financials/pricing.md); feeds [vc_memo.md](vc_memo.md), [pitch_deck.md](pitch_deck.md) and [README.md](../README.md).

## The claim

**Your frontier is measurable on your pool, and a meaningful part of the apparent ceiling is your harness.** CAMIR does not claim a more accurate router. The Routing Plateau finds 21 routing methods converged in a narrow band far below the oracle, with best remedies worth up to 2.13 percentage points [S4]. The Unsolvability Ceiling finds evaluation artifacts: truncation in 65% of MMLU and 57% of MedQA cases, plus 5–12% parse failures on MMLU [S5].

## The problem

Platform teams run mixed-difficulty requests through one fixed model because they cannot measure the cost-quality frontier on their own pool. Hosted routing results do not transfer cleanly: RouteLLM's cost-performance multiple is 3.66x on MT-Bench, 1.41x on MMLU and 1.49x on GSM8K [S2]. Self-hosted economics also change at utilization: raw H100 cost is about $0.10/M tokens versus about $0.60/M hosted [S26], while idle GPU capacity can make cost per token 10x higher and realistic all-in cost 3–5x raw rental [S27].

## The mechanism

**Classify → Dispatch → Judge → Attribute → Recalibrate**

1. Build a replay corpus and run the oracle ceiling probe across the model pool.
2. Use the cascade route first: small tier, confidence gate, escalation; report the classifier route only as an ablation against calibrated confidence [S8].
3. Judge with temperature 0, position control, verbosity control, exact-match or programmatic verification where available, and published inter-judge agreement [S33][S34].
4. Price each request on amortized GPU-hours, stamp the tier decision on the caller's trace, and let the consuming engineer own the quality tolerance.
5. Recalibrate after drift or a pool change. If the ceiling is too low, issue a do-not-deploy report.

## Why now

The routing mechanism is moving into serving engines [S11]. The defensible contribution is the measurement layer: mixed-difficulty benchmark, judging protocol, and a self-hosted cost axis that published router benchmarks have not derived [S7]. Semantic caching already removes 20–45% of production traffic upstream [S36], so CAMIR measures the residual rather than claiming cache savings.

## Commercial shape

Open-core: router, tier abstraction, benchmark harness and published frontier remain open. The commercial control plane provides per-deployment tolerance management, savings measurement and attribution, and observability. The beachhead is a staff platform engineer managing roughly $50,000/month of inference spend (assumption: P2 profile), with a $30,000/year ACV scenario derived from $600,000 annual spend, 70% cache-miss traffic, 25% realized saving and a 28% tested pricing hypothesis (assumption: no CAMIR measurement or buyer validation; see [financials/pricing.md](../financials/pricing.md)).

## Evidence and edge

CAMIR is pre-traction and originates as an SJSU CMPE 295A capstone. The claimed edge is test-automation and evaluation-infrastructure experience transferring to benchmark engineering, recorded as assumption A13. No customer, logo, advisor, testimonial or CAMIR measurement is claimed.

## Recommended next 3

1. Run the artifact-controlled oracle ceiling experiment and publish the self-hosted cost derivation.
2. Put the shadow-mode report, unilateral pin and trace attribution in front of the consuming engineer before enforcement.
3. Test the eligible-savings definition and the $30,000 ACV scenario with the economic buyer and finance/procurement.
