# Technique Wave 1 — Established Domain Science

**What this is** — the catalog of settled, published techniques CAMIR's router and its measurement harness are assembled from: cascade construction, deferral theory, calibration, confidence measures, preference and clustering routers, surface heuristics, budgeted inference, verification, and benchmark construction.
**Why it exists** — the field's own survey shows CAMIR occupies two of six routing paradigms and contributes no seventh [S6]; without this file the pack would describe the router in generic language and a reviewer could not tell whether the team knows the difference between margin sampling and a trained router — which matters, because [S8] says the two route about equally well and therefore fixes the baseline the classifier route must beat. Absent this catalog, engineering would rediscover Chow's rule badly.
**How to read it** — read cluster **D** (confidence measures) and cluster **J** (benchmark construction) first; they are the baseline and the deliverable. A skeptic should attack cluster **G**, which lists techniques that are documented to fail, and ask why any of them remain in the system.
**Depends on / feeds** — depends on [../../research/survey.md](../../research/survey.md) §2–§5, [../../research/capability_table.md](../../research/capability_table.md), [../../research/sources.md](../../research/sources.md); feeds [wave2.md](wave2.md), [decision_tree.md](decision_tree.md), [technique_feature_matrix.md](technique_feature_matrix.md), [../deep_dives.md](../deep_dives.md) and [../not_vaporware.md](../not_vaporware.md).

---

## Scope and counting rule

**In scope:** techniques for selecting among *independently trained* models at inference time, and for measuring the result. Following the field's convention this **excludes mixture-of-experts**, which routes within one model [S6].

**48 techniques in 10 clusters.** The wave stopped at 48 rather than 50 because the established literature genuinely runs out here: everything remaining in the 2023–2026 corpus either belongs in wave 2 (theory-grounded) or is a restatement of an entry below. Two near-misses were deliberately cut as duplicates — "nearest-centroid difficulty lookup" (identical mechanism to W1-32) and "length-normalised log-likelihood with a learned threshold" (W1-22 plus W1-3). **Padding this wave to 50 would have meant printing them.**

Evidence anchors: `[Sn]` resolves to [../../research/sources.md](../../research/sources.md); a bare author/method name means the technique is standard and named in the literature but not separately registered as a CAMIR source; `(assumption)` means CAMIR is reasoning, not citing.

| Cluster | # | Techniques |
|---|---|---|
| A | 6 | Cascade construction and stopping rules |
| B | 5 | Learning to defer / selective prediction |
| C | 5 | Calibration |
| D | 7 | Confidence and uncertainty measures |
| E | 5 | Preference-model routing |
| F | 3 | Clustering and instance-based routing |
| G | 4 | Surface-feature heuristics — and their documented failure |
| H | 3 | Budgeted and anytime inference |
| I | 4 | Verification without a judge |
| J | 6 | Benchmark construction and stratification |
| | **48** | |

---

## A. Cascade construction and stopping rules (6)

The **cascade route**, which [../../research/landscape.md](../../research/landscape.md) makes CAMIR's primary strategy and the classifier route its ablation.

| # | Technique | Mechanism (one line) | Evidence anchor |
|---|---|---|---|
| W1-1 | **Two-stage small→large cascade** | Send every request to the small tier; escalate the ones whose answer fails a stopping rule. | FrugalGPT LLM cascade [S3] |
| W1-2 | **Multi-stage ordered cascade** | An ordered list of tiers with a per-stage threshold, so a medium tier can absorb requests the small tier fails and the large tier never sees. | [S3] |
| W1-3 | **Scalar score-threshold deferral rule** | Escalate iff `score(query, answer) < τ`; τ is the single knob that maps to a point on the frontier. | [S3][S8] |
| W1-4 | **Learned answer scorer** | A small regression head (DistilBERT-class) over `(query, answer)` predicting correctness — the thing τ thresholds, distinct from the generating model's own confidence. | FrugalGPT's scorer [S3] |
| W1-5 | **Cost-constrained joint optimisation of sequence and thresholds** | Choose *which* tiers, in *what order*, at *what thresholds*, by maximising expected quality subject to a budget — a constrained program, not a hand-tuned τ. | [S3] |
| W1-6 | **Escalation-rate targeting** | Treat escalation rate, not τ, as the operator-facing control variable; FrugalGPT's headline operating point escalated **16.6%** of queries to GPT-4 [S3]. | [S3] |

**Inherited failure mode.** Cascade economics are dominated by the first stage's resolution rate, and FrugalGPT's arithmetic was derived under **2023 hosted price ratios** [S3]. Under compressed self-hosted tier spreads [S29][S26] the same structure can cost *more* than going straight to the large tier — nobody has re-derived it [G2].

---

## B. Learning to defer / selective prediction (5)

The formal theory of a predictor allowed to say "not me". This is what a deferral rule *is*, and naming it correctly separates CAMIR's escalation logic from an if-statement.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-7 | **Rejection learning (Chow's rule)** | Under a fixed cost of rejection and calibrated class posteriors, the classical risk-optimal reject region is `max posterior < 1 − c`; CAMIR treats this as a bounded analogy, not an unqualified correctness rule for free-form generation. | Chow (1970), reject option |
| W1-8 | **Risk–coverage curves** | Plot error rate against the fraction of requests answered without deferral; the selective-prediction analogue of CAMIR's frontier and the correct way to compare two escalation rules. | Selective prediction / El-Yaniv–Wiener |
| W1-9 | **Learning to defer to an expert** | Train the small tier's deferral head *jointly with knowledge of the large tier's competence*, so it defers where the large tier actually helps — not merely where it is itself unsure. | Madras et al.; Mozannar–Sontag consistent surrogate loss |
| W1-10 | **Cost-sensitive deferral with asymmetric errors** | Weight a wrong cheap answer against a needless escalation separately, because in CAMIR they cost different things: quality-tolerance breach vs. money. | Cost-sensitive learning; `(assumption: the weights are a per-endpoint policy choice, untested)` |
| W1-11 | **Joint prediction-and-selection head** | One network with two outputs — answer and "should I answer" — trained together so the selector sees the predictor's internal state rather than only its output. | SelectiveNet (Geifman–El-Yaniv) |

**Central inherited result:** the quality of the deferral rule matters more than the quality of either model, because the rule alone determines the cost/coverage trade-off ([../../research/survey.md](../../research/survey.md) §2.2).

---

## C. Calibration (5)

Deferral requires a confidence score that means what it says. Every technique in cluster D is worthless uncalibrated.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-12 | **Temperature scaling** | One scalar divides the logits, fit on held-out data by NLL; the standard post-hoc fix. It preserves each request's argmax, so it never changes the *answer* — but a routing decision is a threshold on confidence, so rescaling moves requests across a fixed τ, and for multi-class max-softmax it can reorder requests. **τ must be refit after calibration, never carried over.** | Guo et al. (2017) |
| W1-13 | **Platt scaling** | Fit a logistic map from raw score to probability — used when the score is a scorer output (W1-4) rather than a softmax. | Platt (1999) |
| W1-14 | **Isotonic regression calibration** | Non-parametric monotone fit; more flexible than Platt, needs more held-out data, and is the right choice once a deployment has real routing history. | Zadrozny–Elkan |
| W1-15 | **Reliability diagrams + ECE / adaptive-ECE** | Bin predictions by confidence and plot observed accuracy against it; the number that tells an operator whether τ means anything. | Expected calibration error, Naeini et al. |
| W1-16 | **Brier score decomposition** | Split the score into reliability, resolution and uncertainty, separating "my confidences are miscalibrated" from "my confidences are uninformative" — two different bugs with two different fixes. | Murphy decomposition |

---

## D. Confidence and uncertainty measures (7)

**The baseline the classifier route must beat.** [S8] finds simple confidence measures route as well as trained routing models — so beating a fixed-model baseline is table stakes and beating *free confidence* is the real bar ([../../research/survey.md](../../research/survey.md) §Recommended next 3).

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-17 | **Maximum softmax probability** | Maximum token probability, evaluated as a candidate signal rather than a calibrated probability of answer correctness; the cheapest escalation baseline when the backend exposes logprobs. | [S8] |
| W1-18 | **Top-2 margin** | Top-1 minus top-2 probability (the score behind active learning's "margin sampling"); separates "confident" from "one of two plausible answers", which MSP conflates. | [S8] |
| W1-19 | **Predictive entropy** | Entropy of the output distribution; sensitive to diffuse uncertainty across many options rather than a single rival. | [S8] |
| W1-20 | **Distance-to-uniform** | How far the distribution sits from maximum ignorance; robust when vocabulary size makes entropy hard to threshold across tasks. | [S8] |
| W1-21 | **Length-normalised sequence log-likelihood** | Mean token log-prob over the generated answer, correcting the bias that makes long answers look uncertain. | Standard sequence scoring |
| W1-22 | **Self-consistency agreement rate** | Sample *k* answers at temperature > 0 in an explicitly costed offline or opt-in arm; agreement is a candidate signal, not proof of correctness, and conflicts with the temperature-zero judging protocol if used unpriced in production. | Wang et al., self-consistency |
| W1-23 | **Semantic entropy** | Cluster the *k* samples by meaning-equivalence, then take entropy over clusters — so five phrasings of one right answer read as confident, which token entropy does not. | Kuhn et al.; Farquhar et al. (2024) |

---

## E. Preference-model routing (5)

The nearest published comparator family [S1][S2]. All five are *classifier-route* techniques: decision before generation.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-24 | **Bradley–Terry win-probability model** | Fit a win model over preference pairs and route by predicted probability the small tier wins; the routing decision becomes a threshold on that probability. In RouteLLM it is the preference model inside the similarity-weighted router (W1-28), not a separately benchmarked fifth router. | RouteLLM [S2] |
| W1-25 | **Matrix factorisation router** | Factor the `query × model` preference matrix into latent factors and score unseen pairs — **$3.32 per million requests** to serve [S1]. | [S1] |
| W1-26 | **Causal-LLM router** | Fine-tune a small generative model to emit the win probability directly — **$5.23 per million requests** [S1]. | [S1] |
| W1-27 | **BERT-classifier router** | Encoder classifier over the raw prompt — the cheapest of RouteLLM's four at **$3.19 per million requests** [S1]. | [S1] |
| W1-28 | **Similarity-weighted ranking router** | Weight training-set preferences by embedding similarity to the incoming query, fitting a Bradley–Terry model (W1-24) on the weighted set — the most expensive of the four to serve at **$39.26 per million requests** [S1]. [S1] is cited for serving cost only; it does not license an accuracy ranking among the four. | [S1] |

**The number discipline that applies to this whole cluster.** RouteLLM's CPT is **3.66× on MT-Bench, 1.41× on MMLU, 1.49× on GSM8K** [S2]. The headline holds only on the most conversational benchmark. Any CAMIR artifact quoting a multiple without its benchmark is misleading.

---

## F. Clustering and instance-based routing (3)

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-29 | **kNN routing over query embeddings** | Route by the observed tier outcomes of the *k* nearest historical requests — the cheapest classifier-route baseline, and the one that improves automatically as per-deployment history accumulates. (The bar CAMIR's classifier must clear is still calibrated confidence, cluster D — not kNN.) | [S4][S6] |
| W1-30 | **Cluster-prototype routing** | k-means the embedding space offline, assign a tier per cluster, route by nearest centroid; O(1) at request time and inspectable by an operator. | [S6] |
| W1-31 | **Semantic cache as a zeroth stage** | Vector-similarity lookup that answers before any tier runs; production hit rates **20–45%** [S36]. Composes with routing but **takes the easy traffic first**, so it is a technique CAMIR must model rather than one it can ignore. | [S35][S36][S37] |

---

## G. Surface-feature heuristics — and their documented failure (4)

Listed because they are the incumbent (`../../BRIEF.md` §Problem, workaround 2) and because CAMIR's teardown depends on naming them precisely.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-32 | **Prompt-length thresholding** | Longer prompt → larger tier. | Surveyed as the weakest signal class; the predictability bottleneck applies most strongly [S4] |
| W1-33 | **Keyword / regex domain rules** | "Anything mentioning code goes large." | [S4]; `../../BRIEF.md` §Competition row 3 |
| W1-34 | **Endpoint-level static tier assignment** | One tier per API endpoint, no per-request decision. | Misses the difficulty variance *inside* each endpoint (`../../BRIEF.md` §Problem, workaround 3) |
| W1-35 | **Token-count × task-type lookup table** | A hand-maintained grid mapping declared task type and size to a tier. | `(assumption: common practice; no published measurement of its accuracy)` |

**Why they stay in the catalog anyway.** All four are ~zero-cost and therefore usable as *pre-filters* and as the floor in an ablation table. What they cannot be is the routing policy: they correlate weakly with real difficulty and decay as traffic shifts [S4].

---

## H. Budgeted and anytime inference (3)

The tradition that supplies the notion CAMIR calls **quality tolerance**.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-36 | **Fixed generation budgets** | Cap `max_tokens` per tier to bound cost per request. **This is also a measurement hazard**: fixed budgets truncate **65% of MMLU and 57% of MedQA cases** in [S5], manufacturing apparent incapability. | [S5] |
| W1-37 | **Anytime prediction** | Produce a usable answer at any interruption point, improving monotonically with compute — the property that makes an escalation deadline safe. | Anytime algorithms (Dean–Boddy) |
| W1-38 | **Accuracy-guaranteed scale-down** | Declare an acceptable accuracy loss versus the strongest model and scale down only within it — the direct ancestor of CAMIR's quality tolerance. | SMART [S14] |

---

## I. Verification without a judge (4)

Preferred over LLM-as-judge wherever the task admits it, because the judge is the weakest link in the whole apparatus ([../../research/survey.md](../../research/survey.md) §5.3).

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-39 | **Exact-match / normalised string match** | Deterministic correctness on multiple-choice and short-answer items; zero judge variance. | RouterBench's knowledge and commonsense datasets [S7] |
| W1-40 | **Programmatic unit-test execution** | Run the generated code against tests — pass@1 as ground truth on HumanEval / MBPP. | [S5][S7] |
| W1-41 | **Grammar-constrained decoding** | For tasks with a supported formal schema and backend, constrain output so it is extractable; it cannot remove parse failures for unconstrained prose, so unsupported cases fall back to counted parsing. | [S5] |
| W1-42 | **Schema extraction with explicit parse-failure counting** | Where constrained decoding is not available, extract with a strict parser and **count and publish the failures** instead of scoring them as wrong. | [S5] |

**This cluster is the founder edge in operational form.** [S5]'s finding — that much of measured unsolvability is instrumentation, across 206,000 query-model pairs — is only exploitable by a team that instruments first.

---

## J. Benchmark construction and stratification (6)

The deliverable, not the appendix. Open question #1 in [../../research/survey.md](../../research/survey.md) §8 is that **the cost axis itself has not been derived** for self-hosted pools.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W1-43 | **Exhaustive cross-tier evaluation** | Run every benchmark request through every tier and label each outcome — the raw material for everything else in this cluster. | RouterBench: **405,467 inference outcomes, 8 datasets, 14 models** [S7] |
| W1-44 | **Oracle-ceiling computation** | Score a hypothetical router with perfect foreknowledge; the upper bound on the entire venture and the two-week experiment in `../../BRIEF.md` §Riskiest assumption. | [S7]; survey §5.2 |
| W1-45 | **Fixed-model baseline construction** | Each tier alone as a *point* in cost–quality space; routers are *curves* through the same space. | RouterBench formalism [S7] |
| W1-46 | **Cost-performance threshold (CPT)** | The cost multiple achieved while retaining a stated fraction of the strongest tier's quality — e.g. **3.66× at 95% of GPT-4 on MT-Bench** [S2]. | [S2] |
| W1-47 | **Difficulty stratification by oracle label** | Partition the corpus into small-solves / large-only / neither, and report the frontier per stratum — the partition that reveals whether a low ceiling is bimodality (the walk-away condition) or noise. | [S7]; `../../BRIEF.md` §Riskiest assumption |
| W1-48 | **Stratified sampling with per-stratum error bars** | Sample within strata and report intervals, so a 2-point frontier difference is not read as a result. Directly relevant: the plateau paper's best remedies bought **up to 2.13 percentage points** [S4]. | [S4] |

---

## What wave 1 does *not* contain

Stated because a catalog that lists only what it has is a brochure.

1. **No technique here breaks the routing plateau.** 21 methods across 5 benchmarks converge into a narrow band far below oracle [S4]; clusters A, E, F and G are all inside that band by construction. Wave 1's contribution to CAMIR is *measurement*, not accuracy.
2. **No technique here is novel to CAMIR.** Every entry is published. That is the point of calling it wave 1.
3. **Mixture-of-experts, multi-turn and agentic routing are out of scope** [S6]; `../../BRIEF.md` §Wedge declares the last two non-goals for year one.

## Recommended next 3

1. **Implement cluster D before cluster E.** Calibrated confidence (W1-17 to W1-20) is the baseline [S8] and costs nothing; a preference-model router that does not beat it is not worth serving at $3.19–$39.26 per million requests [S1].
2. **Ship cluster I with the very first benchmark run.** Grammar-constrained decoding (W1-41) and parse-failure counting (W1-42) must be in place before the first oracle ceiling is computed, or that ceiling reproduces [S5]'s artifacts and reads artificially low — and every downstream number in the pack inherits it.
3. **Make W1-47 the headline chart, not the frontier.** The difficulty stratification answers the walk-away question ("is the traffic bimodal?") in week one; the frontier only answers it in month three.

<!-- critic: unresolved — none. Round-2 minor issue logged and accepted: cluster G (surface heuristics) mixes "techniques CAMIR uses" with "techniques CAMIR teardowns"; kept in one cluster deliberately because splitting them would imply the pre-filter use is endorsed rather than tolerated. -->

<!-- critic: round 1 recorded 2026-09-10 in ../../audit/CRITIC_LOG.md — 1 major, 4 minor fixed. Round 0 (commit 712241c) edits were retained but left no verdict record. -->
