# CAMIR — Technical VC memo

**What this is** — a teardown-then-build memo for CAMIR's measurement-first inference router and its commercial control plane.
**Why it exists** — a routing demo can look cheap while hiding judge error, utilization waste and a consuming engineer's veto; without the memo's failure analysis, the investment case confuses a curve with a company.
**How to read it** — read the thesis, then the competitor teardown and operating examples; attack the oracle ceiling, the all-in cost correction and the unvalidated $30,000 ACV.
**Depends on / feeds** — depends on [research/landscape.md](../research/landscape.md), [research/competitors.md](../research/competitors.md), [tech/whitepaper.md](../tech/whitepaper.md), [product/](../product/), [validation/](../validation/) and [financials/](../financials/); feeds [one_pager.md](one_pager.md), [pitch_deck.md](pitch_deck.md) and [README.md](../README.md).

## Thesis

Most routing work optimizes the selector before measuring the ceiling. The Routing Plateau tested 21 methods across 5 benchmarks and found a predictability bottleneck; the best remedies gained up to 2.13 percentage points [S4]. CAMIR's claim is narrower: **your frontier is measurable on your pool, and a meaningful part of the apparent ceiling is your harness**. The harness must expose truncation, parse failure, judge disagreement and self-hosted GPU economics before a route is allowed to claim savings. CAMIR is pre-traction; every forward result remains an assumption until the pool-specific experiment runs.

## Why existing approaches are insufficient

1. **Fixed endpoint assignment** sends every request to one tier. It misses difficulty variance inside an endpoint and gives Ravi Menon no trace-level explanation or unilateral pin.
2. **Hosted catalog routers** provide useful external evidence but not a customer's self-hosted cost axis. RouteLLM reports 3.66x on MT-Bench, 1.41x on MMLU and 1.49x on GSM8K [S2]; FrugalGPT reports up to 98% under 2023-era hosted price ratios and 16.6% escalation [S3]. Neither is a CAMIR result.
3. **Pre-generation learned routers** face the predictability bottleneck [S4]. CAMIR therefore makes the cascade route primary and compares any classifier route against calibrated confidence [S8].
4. **Serving-engine routing** is the commoditisation clock: vLLM's Semantic Router vision places routing inside the serving stack [S11]. It can dispatch; it does not necessarily provide a customer-owned judging protocol, self-hosted cost derivation, signed counterfactual or disqualification report.
5. **Naive evaluation** can manufacture incapability. The Unsolvability Ceiling reports truncation in 65% of MMLU and 57% of MedQA cases and 5–12% parse failures on MMLU [S5]. Judge test-retest is above 95% at temperature 0 but about 70% at temperature 1, with position and verbosity effects [S34]; inter-judge agreement is about 76% [S33].

## Core architecture

1. **Ingress proxy** accepts the request without moving prompt text out of the customer's perimeter.
2. **Replay corpus builder, oracle ceiling probe and artifact guard** establish whether routing is worth deploying.
3. **Tolerance policy engine and pin registry** give the consuming endpoint owner control.
4. **Dispatcher, confidence gate and model pool** execute the cascade route; the difficulty classifier is an ablation.
5. **Judge harness and judge interface** record correctness, artifact flags and inter-judge agreement.
6. **Cost meter, savings attributor and trace stamper** write the `savings_ledger` and caller-visible tier decision.
7. **Drift monitor, recalibration scheduler and frontier builder** turn model changes into a new `frontier_run`, not a stale promise.

## Concrete operating examples

### Marcus: the beachhead

Marcus Bell points CAMIR at a replay corpus from the shared inference service. The oracle ceiling probe runs every request through each declared tier, the judge harness records per-tier correctness, and the cost meter derives GPU-seconds from the pool manifest. Before enforcement, shadow mode writes the proposed tier and counterfactual to the trace. Marcus sees the cost-quality frontier; Dana receives a signed savings report; Ravi can pin his endpoint without a ticket.

### Ravi: the veto

Ravi Menon sees a quality question in his endpoint trace. The trace stamper shows the tier, escalation flag and tolerance policy version. He sets unilateral pin-to-large. The next request follows the pin, and the event remains in the trace. CAMIR treats a pin as a safety control to measure, not as evidence to suppress.

### Wen: the edge-high evaluator

Wen Xu supplies her own judge behind the judge interface and declares a five-tier pool. CAMIR publishes inter-judge agreement and the cost axis rather than asking her to trust a headline. If her oracle ceiling is too low, the disqualification report tells her not to deploy. That negative result is a valid output.

## Why this can win where others have not

The recombination is the asset: benchmark construction, artifact-aware judging, a self-hosted GPU-hour cost axis, tolerance ownership and open counterfactual attribution. The router itself is deliberately not the moat. The control plane can charge only if it survives the serving-engine commoditisation clock and if a buyer accepts the eligible-savings definition. Open-core distribution through a self-hosted proxy is a dependency, not a settled advantage.

## Honest risks

1. **The oracle ceiling may be too low.** E1 kills the company case if a perfect router cannot reach the declared quality at a materially lower cost.
2. **The cost spread may compress or utilization may invert it.** Idle GPU cost can be 10x per token and all-in cost can be 3–5x raw rental [S27]. The pool manifest must admit a tier only after measurement.
3. **The buyer may reject the price or measurement.** The $30,000 ACV scenario is derived from $600,000 spend, a 25% saving assumption and a 28% pricing hypothesis. E8 and the signed eligible-savings definition decide it; no current buyer validation is claimed.

## Recommended next 3

1. Execute E1 and E2 with raw token volume, GPU-hours, judge cost, utilization and inter-judge agreement published.
2. Run shadow mode with Marcus, Ravi and Dana's distinct decision rights visible in the records.
3. Test whether the commercial control plane earns a fee after the buyer can reproduce the counterfactual.
