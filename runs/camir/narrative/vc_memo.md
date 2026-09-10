# CAMIR — Technical VC memo

**What this is** — the teardown-then-build investment memo for CAMIR: what the routing field gets wrong, why each existing approach cannot close the gap, the architecture that follows, three operating examples drawn from the journeys, the arithmetic, the market, and the three risks that could end it.
**Why it exists** — the obvious pitch for an LLM router — "our router is smarter, here is a savings multiple" — is falsified by one citation [S4], and the obvious multiple is an MT-Bench number that collapses on knowledge tasks [S2]. A memo built on either loses a technical investor in the first ten minutes and takes the rest of the pack's honesty down with it. This memo exists to make the investable claim in its defensible form: a measurement business, with a disqualification path, whose venture case is conditional and says so.
**How to read it** — §Thesis, then §The arithmetic, then §Honest risks. A skeptic should attack the conservative column in §The arithmetic, which is a null result, and §Market, whose first finding is that routing fees alone are not a venture-scale business.
**Depends on / feeds** — depends on [../research/landscape.md](../research/landscape.md), [../research/competitors.md](../research/competitors.md), [../tech/whitepaper.md](../tech/whitepaper.md), [../product/journeys/](../product/journeys/), [../strategy/market_sizing.md](../strategy/market_sizing.md), [../financials/](../financials/), [../validation/stage_gate.md](../validation/stage_gate.md); feeds [one_pager.md](one_pager.md), [pitch_deck.md](pitch_deck.md) and [../README.md](../README.md).

**Status: pre-traction.** An SJSU CMPE 295A capstone. No customer, no revenue, no benchmark run of CAMIR's own. Every forward number carries `[Sn]` or `(assumption)`.

---

## Thesis

The routing field has spent three years optimising the *selector* and bought almost nothing: 21 methods across 5 benchmarks converge in a narrow band far below the oracle, from a predictability bottleneck, and the best remedies are worth up to 2.13 percentage points [S4]. Meanwhile the *measurement* the selector is judged by is broken in larger ways — truncation under fixed generation budgets in 65% of MMLU and 57% of MedQA cases, 5–12% parse failures on MMLU, and judges biased toward verbosity [S5].

**CAMIR's claim is therefore not "our router is more accurate." It is: your frontier is measurable on your pool, and a meaningful part of the apparent ceiling is your harness.** The router is the object under measurement. The asset is the measurement layer — a mixed-difficulty replay corpus, a judging protocol forced by the 2026 reliability results, and a cost axis derived for self-hosted pools, which no published benchmark has derived [S7]. And because that layer runs inside the customer's perimeter, CAMIR can do the one thing no competitor paid on routed volume can: tell a prospect on day two not to buy.

---

## Why existing approaches are insufficient

1. **Fixed endpoint assignment** — the incumbent. One tier per endpoint, chosen once. It misses the difficulty variance *inside* an endpoint and gives the consuming engineer no per-request explanation and no dial. Marcus's March A/B test is two model upgrades stale ([../strategy/personas.md](../strategy/personas.md) P2).
2. **Hosted-catalog routers.** RouteLLM's cost-performance multiple is **3.66× on MT-Bench, 1.41× on MMLU and 1.49× on GSM8K** [S2]; FrugalGPT reached up to 98% with 16.6% escalation under **2023-era hosted price ratios** [S3]. Both price against hosted list prices, and the tier spread they exploited is compressing [S29].
3. **Pre-generation learned routers** hit the predictability bottleneck [S4]. Simple calibrated confidence routes as well as trained routers [S8], so a classifier that beats a fixed model has cleared table stakes, not a bar.
4. **Serving-engine routing** — the commoditisation clock. The vLLM Semantic Router vision places the workload/router/pool decomposition inside the engine [S11]. It will take the dispatch half. It has no reason to run a customer's oracle ceiling probe, publish inter-judge agreement, derive an amortised cost axis, or disqualify a prospect.
5. **Naive evaluation** manufactures incapability [S5]. Judge test-retest is above 95% at temperature 0 and ~70% at temperature 1, with ~40% position inconsistency and ~15% verbosity inflation [S34]; judges agree with each other only ~76% of the time [S33]. A frontier published without inter-judge agreement is a chart.
6. **Commercial routers and gateways** have an incentive problem no methodology fixes. OpenRouter reached $160M annualised revenue by taking ~5% of the inference bill it sits on [S17] — paid more as the bill grows. A frontier vendor shipped routing built into GPT-5 at no separate fee [S20], and the documented backlash — complex queries degraded by routing to a smaller model, with no dial and no attribution [S21] — is what vendor-set tolerance produces. Martian is reported, by a single weak source, to have neared a ~$1.3B valuation [S18]; its revenue is unknown [G3].
7. **Open-core LLM infrastructure** has a direct post-mortem: TensorZero archived its repository in June 2026 after raising $7.3M and passing 11,000 stars, citing the difficulty of fitting an open project and a commercial product at once [S23]. It is the closest structural comparable in this memo, and it returned its capital.

---

## Core architecture

The loop, verbatim from [../product/PRD.md](../product/PRD.md): **Classify → Dispatch → Judge → Attribute → Recalibrate.**

1. **Replay corpus builder, oracle ceiling probe, artifact guard** — measure the ceiling twice, guarded and unguarded, before any router exists; emit a **disqualification report** when the gap is smaller than the cost of operating ([../tech/architecture/D01.md](../tech/architecture/D01.md)).
2. **Tolerance policy engine and pin registry** — read *before* any tier is chosen; the tolerance has a non-null named owner ([../tech/architecture/D03.md](../tech/architecture/D03.md), [D04.md](../tech/architecture/D04.md)).
3. **Dispatcher, confidence gate, model pool** — the cascade route is primary; the **difficulty classifier** runs as an ablation against calibrated confidence.
4. **Judge harness and judge interface** — temperature 0, position control, verbosity control, programmatic verification where the task admits it, two or more judges, agreement published.
5. **Cost meter, savings attributor, trace stamper** — price each request on amortised GPU-seconds with the utilisation and all-in multiplier declared ([../tech/architecture/D05.md](../tech/architecture/D05.md)); write the tier onto the caller's own span.
6. **Drift monitor, recalibration scheduler, frontier builder, frontier diff** — a pool change re-runs the pinned corpus and projects every owner's tolerance point onto both curves.
7. **The open/paid line** — the entire qualify path and the counterfactual computation are open, permanently; the control plane runs in the customer's VPC and never sees a prompt ([../tech/architecture/D06.md](../tech/architecture/D06.md)).

---

## Concrete operating examples

All three are from [../product/journeys/](../product/journeys/). The figures are illustrative — no CAMIR run exists — but the mechanism in each is specified to the component.

**Marcus, the beachhead — the disqualification lands first.** Marcus runs the harness on 8,000 stratified requests from six endpoints of his ~$50k/month shared service, overnight, on his own fleet. `account-reasoning`, his most expensive endpoint, comes back at a **22% ceiling and is disqualified**. `structured-extraction` shows a **31-point artifact share** — nearly a third of "the 8B can't do this" was truncation and parse failure, recovered by a generation-budget change he could have made himself. `classification` sits at 91% and has run on the 70B for eleven months. The order is the argument: CAMIR removed his largest line item from its own scope before showing a saving, and everything he believes afterwards is credible because of it.

**Ravi, the veto — four minutes, without opening CAMIR.** Month four, an ordinary Tuesday. Ravi's feature shows thumbs-down up 1.9 points. His instinct is the one that ends deployments: pin everything to large. Instead he groups his own spans by `camir.tier` — an attribute the trace stamper wrote four months earlier — and finds the regression concentrated in requests that went to the **large** tier. Router exonerated in four minutes; the actual cause was his own team's prompt-template change. Without the attribute he pins, the pin stays forever, and the real bug keeps shipping. **A per-request attribute read once a quarter is why the deployment survives.**

**Wen, the edge-high evaluator — the result that makes the rest believable.** Wen brings her own judge ensemble and a five-tier pool including a LoRA specialist. CAMIR's code computes her inter-judge agreement (0.86 against the ~0.76 field baseline [S33]). Her artifact share comes back at **6 points** — against Marcus's 14 and Priya's 19 — which is the correct shape: the effect shrinks as harness quality rises. Her trained classifier gains **1.4 points** over calibrated confidence, inside the plateau band [S4]; CAMIR reports it as a null-ish result and keeps the cascade primary. Her objection — *"I could build this in three weeks"* — is true, and the answer is not capability but the standing obligation to re-measure every time the pool moves.

---

## The arithmetic

From [../tech/whitepaper.md](../tech/whitepaper.md) §2.6. Four inputs, each a stated assumption: tier cost ratio `r`, post-cache escalation rate `e`, harness recovery `h`, and routed coverage `k` — the share of traffic whose owners allow routing.

| | Conservative | Base | Optimistic |
|---|---|---|---|
| Multiple on token-proportional GPU cost | **1.08×** | **1.39× without harness credit · 1.49× with** | **2.84×** |
| Cost reduction | 8% | 28–33% | 65% |

- **The conservative column is a null result.** 1.08× does not pay for a control plane. In that world CAMIR says so, which is the founder's declared walk-away condition.
- **The largest swing is coverage, not the router**: `k` alone moves the base from 1.33× to 1.58×. That is organisational, and it is why attribution and tolerance ownership outrank classifier accuracy.
- **On the all-in line the cut is 7–11%**, because realistic self-hosted cost is 3–5× raw GPU rental [S27] and engineering time does not shrink when a request routes down.

---

## Market and business

**The sizing's first finding, stated first:** routing fees alone are not a venture-scale business at 2026 denominators — base-case routing TAM ~$70M/yr (corridor $10–360M), SAM ~$20M/yr, ~1,370 reachable companies ([../strategy/market_sizing.md](../strategy/market_sizing.md)). The venture case is **Product 2**: the same measurement applied to cache-policy thresholds, quantisation tier, batch and utilisation policy, and model-upgrade regression — decisions nobody measures today — sized at $460–760M by 2031, with demand unvalidated ([../financials/revenue_build.md](../financials/revenue_build.md)).

| | Figure | Basis |
|---|---|---|
| ACV | **$30,000** | 28% share of a measured saving on a $600k/yr pool (assumption; untested) |
| Blended CAC, Y1–Y3 | **~$4,200** (14% of ACV); ~$11,700 in year one alone | [../strategy/channel_plan.md](../strategy/channel_plan.md) — Stack A's build amortised |
| Gross margin | **42% in Y1 → 75% by Y3** | The bend is onboarding automation, not scale ([../financials/unit_economics.md](../financials/unit_economics.md)) |
| ARR | $0.15M Y1 · $2.38M Y3 · $10.95M Y5 | Conditional on three milestones not under CAMIR's sole control: the LiteLLM strategy merged (M2), Product 2 GA (M4), hybrid pools GA (M5). Each alone cuts the terminal number by more than half |

**Distribution** runs as a routing strategy inside LiteLLM [S22] — the only channel whose margin stack survives; marketplaces net 84.3%, system integrators 59.8%, outbound CAC ~117% of ACV. **That dependency is unagreed by anyone** and is first in the kill order.

---

## Why this can win where others have not

**The recombination is the asset, not any component.** Benchmark construction, artifact-aware judging, a self-hosted GPU-hour cost axis, customer-owned tolerance and an open counterfactual are each available somewhere; nobody combines them, because each one either costs a vendor revenue (the disqualification report, the open counterfactual) or has no commercial home (the judging protocol, the cost derivation).

**Why now.** A tier below frontier that is not embarrassing now exists — a 31B-class open model within ~10 LMArena Elo of models 10× its size, April 2026 [S29]. Local multi-model serving is practical, with multi-LoRA making adapter tiers cost megabytes [S31]. And inference became the second-largest line in enterprise AI budgets in 2026 [S25]. **The first of these also works against CAMIR**: as the spread compresses, the saving shrinks — which is why the conservative column exists and why Product 2 is not optional.

**The moat is weak, and per-deployment.** Accumulated labels, signed tolerance history and attribution continuity may raise switching cost; that is assumption A6 and it is untested. No cross-customer network effect exists by construction — the control plane never sees a prompt.

---

## Honest risks

1. **The oracle ceiling on real traffic may be too low** (risk R1, residual High). No mitigation exists; it is a fact about the world. The response is structural: measure it first, for $280k in four months, and publish the null result if it fails — [G2] makes the negative a contribution.
2. **The serving engine absorbs routing** (R2, residual High). It will take dispatch on someone else's schedule [S11]. The response is positional: own the half no engine has a reason to build — the customer's ceiling, published judge agreement, the self-hosted cost axis, the disqualification. A quarterly written check on the engine's scope is the tripwire.
3. **The consuming engineer vetoes and never un-vetoes** (R6). Four P0 features make the veto expensive to exercise rather than free; nothing makes it impossible, and the design rests on one public incident [S21] and zero interviews. The tripwire is the pin-to-large rate, read weekly; the test is E13.

---

## The ask

**$2.2M over 21 months, in three gated blocks.** Block 1 is **$280k** and answers only whether the ceiling exists; if it does not, ~87% of the raise is unspent and the result is published. Block 2 ($820k) tests whether a consuming engineer will enable enforcement on his own endpoint. Block 3 ($1.1M) tests whether it repeats and whether buyers pay ([../financials/use_of_funds.md](../financials/use_of_funds.md)). The metric to report is dollars spent per assumption killed; Block 1 kills the three that can end the company at 13% of the raise.

---

## Recommended next 3

1. **Fund Block 1 against its kill condition, not against the revenue build.** The investable object for the next four months is a pre-registered experiment with a publishable negative outcome, and the memo's strongest signal is that the team has committed to stop on it.
2. **Diligence the harness thesis, not the router.** Ask for the E2 protocol — naive and artifact-controlled arms on the same generations, with inter-judge agreement — and for the prediction that the artifact share shrinks on a well-instrumented pool. That is the claim the company rests on.
3. **Price the LiteLLM dependency explicitly.** Y2 ARR halves without it and it is unagreed; a conversation with the maintainers is the cheapest diligence item in the pack, and the answer changes the channel plan, not the pitch.
