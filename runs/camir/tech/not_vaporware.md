# CAMIR — Not vaporware: the stack, the loop, the cost model, and what is actually research risk

**What this is** — one page separating what an engineer could start building on Monday from what is a bet on unpublished results: named stack choices with the reason each was picked, the evaluation loop that runs continuously, the cost model at 2026 prices, and an explicit split of this-quarter work versus research risk.
**Why it exists** — CAMIR's pitch contains two claims of very different maturity, and a pack that presents them at the same confidence is dishonest in the direction that gets found out in due diligence. *"We can measure your oracle ceiling and decompose it into capability and instrumentation"* is a systems-engineering claim buildable with 2026 open-source components. *"A trained classifier will beat calibrated confidence on your traffic"* is a bet the published record currently argues against [S4][S8]. This document is where the two are separated by name, so a technical reviewer can see that the company's roadmap does not depend on winning the second one.
**How to read it** — §4's two columns are the document. If the this-quarter column cannot be built, the company is vaporware; if the research column fails entirely, the company still has a product. A skeptic should attack §3's cost model, and the honest place to attack it is the utilisation assumption.
**Depends on / feeds** — depends on [whitepaper.md](whitepaper.md), [deep_dives.md](deep_dives.md), [architecture/00_INDEX.md](architecture/00_INDEX.md), [../research/capability_table.md](../research/capability_table.md); feeds [../validation/mvp_definition.md](../validation/mvp_definition.md), [../financials/use_of_funds.md](../financials/use_of_funds.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

**Status: nothing below has been built.** This is a buildability argument, not a build report.

---

## 1. The stack, with the reason for each choice

| Layer | Choice | Why this one | The alternative, and why not |
|---|---|---|---|
| **Serving substrate** | **vLLM**, with SGLang supported behind the same tier interface | Multi-LoRA hot-reload and adapter rotation are more mature on vLLM [S32], and adapter-level tiering is what makes a multi-tier pool affordable on small hardware [S31] | SGLang has the edge at 50+ adapters with heterogeneous traffic [S32] — which is Wen's regime, not the beachhead's. It is an interface, not a rewrite |
| **Gateway integration** | **LiteLLM routing strategy** [S22], standalone proxy as fallback | The gateway is already deployed; shipping as a strategy makes the ask *configure*, not *replace*. The only channel whose economics survive ([../strategy/channel_plan.md](../strategy/channel_plan.md)) | A rival proxy asks a platform team to swap a working component for a feature |
| **Ingress protocol** | OpenAI-compatible `/v1/chat/completions` | Priya's whole adoption is one base-URL change. Anything else costs a code change in every caller | A custom SDK; rejected — it converts a config change into a migration |
| **Classifier** | DistilBERT-class encoder over derived features; **calibrated confidence always run as the comparison arm** | [S1] prices routers concretely: BERT $3.19, matrix factorisation $3.32, causal-LLM $5.23 per million requests. The cheap ones are competitive | A large-model router — [S1]'s SW-Ranking at $39.26/M is 12× the BERT router for the same plateau [S4] |
| **Confidence score** | Length-normalised sequence log-likelihood by default; semantic entropy and self-consistency behind explicit opt-in | The default must need **no extra generation** — the gate sits on the critical path of exactly the requests CAMIR is meant to make cheaper | Sampling-based scorers spend the saving before realising it |
| **Threshold fitting** | Split conformal + conformal risk control | The only method that turns a declared tolerance into a distribution-free finite-sample guarantee rather than a tuned constant | A hand-tuned τ, which cannot be audited, attributed or owned |
| **Judge** | Two open-weight judges + programmatic verification wherever the task admits it; judge interface for BYO | Judge choice may damage validity more than stochastic error within one judge [S33], so plurality is structural | A single frontier-model judge: one API dependency, one bias, and prompts leaving the perimeter |
| **Records** | Postgres for policies and ledgers; columnar store (ClickHouse-class) for `decision_record` | ~5M decision rows/month at beachhead volume is analytical, not transactional; policies are small, relational and constraint-heavy | One store for both; the `NOT NULL` guarantees that make the product work belong in a relational engine |
| **Telemetry** | OpenTelemetry span attributes into the customer's existing backend | Attribution must live in a tool the consuming engineer already opens, and must survive CAMIR's removal (O2) | A CAMIR dashboard, which the veto-holder will never open |
| **Language** | Python for harness and control plane; the request-path proxy in Go or Rust | The harness lives in the ML ecosystem; the proxy is latency-sensitive and sits in front of every request | All-Python request path — workable at beachhead volume, and a known ceiling |

**Nothing on this list is unavailable in 2026.** There is no component whose existence is a bet.

---

## 2. The evaluation loop, running continuously

**Named benchmarks, and why none of them is the product.** MMLU, MedQA, HumanEval, MBPP, GSM8K, MT-Bench, Alpaca, ShareGPT and the RouterBench composite [S7] are used for *development* — regression-testing CAMIR's own harness against results others have published. **They are not what a customer's frontier is measured on.** The customer's frontier is measured on their own replay corpus, because [S13]'s core complaint is that router evaluations are not comparable across papers and a public-benchmark frontier would inherit exactly that problem. RouteLLM's 3.66× is MT-Bench and collapses to 1.41× on MMLU [S2]: a benchmark result is a statement about a benchmark.

**The continuous loop, per deployment.**

1. **Golden set** — a stratified, pinned corpus with per-stratum error bars; re-drawn on traffic-mix change.
2. **Every `frontier_run` re-runs it** with corpus hash, pool manifest, cost parameters, judge set and inter-judge agreement pinned. A run missing agreement is flagged not reproducible [S13].
3. **Judge protocol regression tests** — test-retest at temperature 0 must stay above the ~95% [S34] band; position-permutation deltas and length-controlled scores are asserted in CI, so a judging regression fails a build rather than a customer.
4. **Artifact instrumentation asserted, not assumed** — truncation counts, parse-failure counts and generation-budget sweeps run every time. [S5] found truncation in 65% of MMLU and 57% of MedQA cases and 5–12% parse failures on MMLU; these are CI assertions in CAMIR, not findings.
5. **Production sampling** — a sampled share of live requests judged and folded back as calibration and training data.
6. **Drift** — EWMA/CUSUM on escalation rate, prequential accuracy on the classifier, embedding-shift tests, and an unconditional re-run on `pool_manifest` change [S29].

**The public artifact.** A reference `frontier_run` on public open-weight models over a public corpus, with the guarded/unguarded decomposition, published and re-runnable by strangers. It costs nothing to disclose and it is the only way the technical claim becomes checkable rather than asserted.

---

## 3. The cost model at 2026 prices

**Per-token economics** `(assumption: derived from [S26][S27][S28]; no CAMIR measurement exists [G2])`

| Quantity | Figure | Source |
|---|---|---|
| Raw batched H100, gpt-oss-120b class, high concurrency | **~$0.10 / M tokens** | [S26] |
| Hosted endpoint, same model | ~$0.60 / M output tokens | [S26] |
| B200 at ~$3.75/hr, 70B+ at high batch | below $0.05 / M output tokens | [S26] |
| **Idle penalty at 10% utilisation** | **10× per token** | [S27] |
| **Realistic all-in vs raw rental** | **3–5×** | [S27] |
| Self-hosting threshold | 16M+ tokens/day on H100; 22M+ on H200 | [S27] |
| Break-even vs frontier APIs, 70B class | ~100M tokens/month | [S27] |
| Against budget open-weight APIs ($0.14–0.50/M) | **self-hosting rarely wins on cost alone** | [S28] |

**The two corrections CAMIR must always apply.** (1) A saving quoted against *raw GPU cost* is 3–5× overstated [S27]. (2) The saving is on **token-proportional GPU cost**, not the all-in inference line — idle capacity, engineering time and the control plane do not shrink when a request routes down. [whitepaper.md](whitepaper.md) §4.1 makes this an explicit renunciation.

**CAMIR's own operating cost, per deployment.**

| Item | Cost | Note |
|---|---|---|
| Ceiling probe, first run | corpus × tiers full generations; 24,000 generations ≈ one night on a modest fleet | One-off per pool version |
| Judging | can exceed generation cost | Collapsed by programmatic verification where the task admits it |
| Shadow mode | **doubles generation on shadowed endpoints** | Real, ongoing, and the price of the evidence that unblocks the deal |
| Steady-state routing overhead | gate scoring on the critical path, ~1–5 ms for log-likelihood scores | Rises sharply for sampling estimators |
| Control plane | one small VM in the customer's VPC | Not multi-tenant (N4) |

**This is why the disqualification report exists as a product surface.** If the ceiling-to-baseline gap is smaller than the above, CAMIR costs more than it saves and says so.

---

## 4. Buildable this quarter versus research risk

| **Buildable this quarter — no unpublished result required** | **Research risk — the company must not depend on these** |
|---|---|
| Replay corpus builder, stratified, hashed, error-barred | **Classifier beating calibrated confidence.** [S8] finds simple confidence routes as well as trained routers; [S4] finds 21 methods converged in a narrow band far below oracle, remedies worth **up to 2.13 points**. Shipped as an ablation and expected to be null |
| Oracle ceiling probe (exhaustive cross-tier evaluation) | **Prefill-activation routing** [S10] — the one signal class self-hosting exposes and hosted routers cannot reach; the only credible escape from the predictability bottleneck, and unproven at production latency |
| Artifact guard: truncation logging, strict parse counting, budget sweeps, length control | **Cache-residual savings.** Nobody has measured routing savings on post-cache traffic, which is adversely selected toward the hard end [S36][G4] |
| Judge harness at the forced protocol, two judges, agreement published | **Cost-axis derivation as a *standard*.** Building it is engineering; getting anyone else to adopt it is not a technical question |
| Frontier builder, disqualification report, non-nested tier report | **Rising switching cost from accumulated labels** (A6). Plausible, untested, and per-deployment rather than network-wide |
| Cascade route, confidence gate, conformal thresholds | **Whether tolerance ownership actually stops a veto.** The four P0 features rest on one inference from [S21] and zero interviews — the largest non-technical risk in the pack |
| Ingress proxy, pin registry, shadow mode, trace stamper | |
| Cost meter and counterfactual on the open side | |
| Drift monitor, recalibration scheduler, frontier diff | |

**The load-bearing sentence.** Every item in the left column is a systems-engineering task using 2026 open-source components, and the left column alone produces the ceiling, the artifact decomposition, the disqualification verdict, the cascade and the attribution — that is a product. **The right column contains no item whose failure removes the product.** The classifier failing is the *expected* outcome and is published as such (N7); prefill routing failing costs an upside, not a business.

**Where the honest technical risk actually is** — and it is not in the right column. It is that the **serving layer absorbs the dispatch half** [S11], leaving CAMIR with the measurement half alone. That is not research risk; it is a clock, and [architecture/D07.md](architecture/D07.md) draws it. The measurement layer's survival of that absorption is the company's actual thesis.

---

## Recommended next 3

1. **Build the left column top to bottom and publish a reference `frontier_run` before writing any classifier code.** It produces the pack's contribution, it returns a negative result cheaply, and it makes the technical claim checkable by strangers — which is what the whole open-core distribution motion needs and what no competitor currently offers.
2. **Put the judge-protocol regression tests in CI on day one.** Test-retest, position-permutation delta, length-controlled scoring and parse-failure counts asserted in the build. A judging regression that reaches a customer's `frontier_run` invalidates every tolerance policy set against it, and it is silent.
3. **Time-box prefill-activation routing to one research spike with a stated kill criterion.** It is the only technical asymmetry CAMIR has against the predictability bottleneck [S4][S10] and it is exactly the kind of interesting problem that absorbs a year. A dated spike with a pre-declared threshold is how it stays an upside rather than becoming the roadmap.
