# Cost-Aware Routing for Mixed-Difficulty LLM Traffic — A Mini Survey

*Compiled 2026-09-09 for the CAMIR artifact pack. All citations resolve to [sources.md](sources.md).*

**What this is** — a dated survey of the science CAMIR rests on: the classical foundations of cascaded and deferred prediction, the 2023–2026 taxonomy of LLM routing, the enabling technology, the evaluation methods, and the evidence for *and against* the core mechanism.
**Why it exists** — this pack asserts that routing works, that difficulty is partly predictable, and that the remaining gap is measurable. Each of those is a scientific claim with a literature that contains at least one result pointing the other way, and the PRD, whitepaper and VC memo all cite this file rather than re-arguing from scratch. Without it, the pack's principles would be assertions the founders happen to believe, and its strongest counter-evidence — the routing plateau — would never appear in its own documents.
**How to read it** — §6 is the argument that matters: the evidence *against* the mechanism, stated at full strength before the evidence for it. A skeptic should start at §6.2 and §7. §8 lists what is genuinely open, which is where CAMIR's contribution has to live.
**Depends on / feeds** — depends on [sources.md](sources.md), [capability_table.md](capability_table.md), [landscape.md](landscape.md); feeds [../product/PRD.md](../product/PRD.md) §First-principles grounding, [../tech/whitepaper.md](../tech/whitepaper.md), [../tech/deep_dives.md](../tech/deep_dives.md), [../tech/techniques/](../tech/techniques/) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

---

## Abstract

Serving a mixed-difficulty request stream from a single large language model overpays on the easy majority; serving it from a single small model fails the hard minority, and fails it visibly. Routing — selecting a model per request — is the obvious response, and by 2026 it is a well-populated field: a recent survey organises it into six paradigms [S6], a benchmark provides 405,467 inference outcomes across 14 models [S7], a systematic study compares 21 methods [S4], and a frontier vendor ships routing as a free default [S20]. The empirical picture is not, however, uniformly encouraging. Reported savings are large but benchmark-dependent, collapsing from 3.66× on conversational tasks to 1.41× on knowledge tasks for the same router [S2]. More seriously, the 2026 "routing plateau" result finds that 21 methods across five benchmarks converge into a narrow accuracy band far below the oracle router, attributing this to a *predictability bottleneck* in pre-generation difficulty estimation [S4]. Against this, a companion 2026 result finds that a substantial fraction of measured unsolvability is an artifact of evaluation rather than model capability — truncation under fixed generation budgets in 65% of MMLU cases, 5–12% parse failures, judge bias toward verbosity [S5]. Read together, these relocate the open problem: the routing *decision* appears near its ceiling under current signals, while the routing *measurement* is demonstrably broken and therefore improvable. This survey covers the classical foundations (§2), the taxonomy (§3), enabling technology (§4), evaluation methodology (§5), evidence for and against (§6), risks (§7) and open questions (§8), and concludes that the defensible contribution available to a small team is a reproducible cost-quality frontier for **self-hosted open-weight pools**, whose cost axis has never been derived, rather than a superior routing algorithm.

---

## 1. Scope

**In scope:** selecting among *independently trained* models at inference time, on a per-request basis, to trade cost against quality. Following the field's own convention, this **excludes mixture-of-experts**, which routes within a single model [S6].

**Out of scope for CAMIR specifically:** multi-turn and agentic routing, latency-SLO-aware routing [S12], fine-tuning, and model hosting — all declared non-goals in `../BRIEF.md`.

**Terms** follow `../BRIEF.md` §Vocabulary: *tier*, *model pool*, *classifier route*, *cascade route*, *cost-quality frontier*, *quality tolerance*, *fixed-model baseline*, *oracle ceiling*, *escalation rate*.

---

## 2. Classical foundations

Routing is not new science; it is a recombination of three older ideas, and naming them correctly is what separates a technology claim from a wrapper claim.

**2.1 Cascaded classifiers.** Detection cascades — cheap, high-recall stages rejecting most candidates before expensive stages run — established the core economics: total cost is dominated by the *rejection rate of the first stage*, not by the accuracy of the last. CAMIR's cascade route inherits both the structure and the failure mode: a first stage that rarely resolves confidently makes the cascade more expensive than going straight to the large tier.

**2.2 Learning to defer / selective prediction.** The literature on abstention and rejection learning formalises a predictor that may decline and pass the instance to a more capable decider, at a cost. The modern LLM instantiation is explicit: uncertainty-driven selective prediction and deferral [S8], including deferral for agents [S8-adjacent work catalogued in S6]. The central result carried into CAMIR: **the quality of the deferral rule matters more than the quality of either model**, because it determines the cost/coverage trade-off.

**2.3 Calibration.** Deferral requires a confidence score that means what it says. The modern finding is that **simple confidence measures — maximum softmax probability, margin, predictive entropy, distance-to-uniform — can route as well as trained routing models** [S8]. This is the single most important classical result for CAMIR, because it sets the *baseline the classifier route must beat*: not a fixed model, but calibrated confidence from the small tier itself.

**2.4 Anytime and budgeted inference.** The tradition of producing an answer under an explicit compute budget supplies the notion CAMIR calls **quality tolerance** — a declared acceptable loss against an unconstrained reference [S14].

---

## 3. Taxonomy of approaches (2023–2026)

The survey literature organises the field into six paradigms, plus a three-dimensional design frame [S6].

### 3.1 The six paradigms [S6]

| Paradigm | Decision signal | Representative work | CAMIR's relation |
|---|---|---|---|
| **Difficulty-aware routing** | Predicted query difficulty from prompt features | The classifier-router family surveyed in [S4] | **This is CAMIR's classifier route** |
| **Human-preference alignment** | Learned win-probability from preference data | RouteLLM [S1][S2] | Nearest published comparator |
| **Clustering** | Query embedding neighbourhood | kNN routers, evaluated in [S4] | Cheap baseline CAMIR must beat |
| **Reinforcement learning** | Long-run cost/quality reward | Bandit and cost-aware routers in [S4] | Out of scope year one |
| **Uncertainty quantification** | Post-generation calibrated confidence | UCCI [S8] | **CAMIR's cascade escalation signal** |
| **Cascading** | Sequential attempts with a stopping rule | FrugalGPT [S3] | **This is CAMIR's cascade route** |

CAMIR occupies **two of six** and contributes no seventh. Its claim is comparative and conditional — these two, under self-hosted conditions, on a measured frontier — not novel.

### 3.2 The three design dimensions [S6]

- **When** the decision is made — *before* generation (classifier route) or *during/after* a first attempt (cascade route). This is the axis CAMIR's whole experiment is organised around.
- **What signals** it uses — surface prompt features, embeddings, preference models, post-hoc confidence, or **internal prefill activations** [S10].
- **How** it is computed — heuristic, trained classifier, matrix factorisation, learned scorer, or bandit.

### 3.3 Signal classes, ranked by what they cost and what they can see

| Signal | Cost | Available to hosted routers? | Ceiling evidence |
|---|---|---|---|
| Surface features (length, keywords) | ~0 | Yes | Weakest; the predictability bottleneck applies most strongly [S4] |
| Query embeddings / kNN | Low | Yes | Converges into the plateau band with everything else [S4] |
| Preference-model win probability | Low ($3.19–$39.26 per M requests) [S1] | Yes | 3.66× on MT-Bench, 1.41× on MMLU [S2] |
| **Prefill activations** | Marginal (already computed) | **No** | Early, 2026 [S10] |
| Post-generation calibrated confidence | Cost of one small-tier generation | Yes | Matches trained routers [S8] |
| Full verification / scorer model | Cost of an extra inference | Yes | FrugalGPT's mechanism [S3] |

**The strategic reading:** the only signal a hosted competitor structurally cannot use is prefill activations, and it is available precisely because CAMIR's users run their own weights.

---

## 4. Enabling technology

Detailed in [capability_table.md](capability_table.md); summarised here as the four preconditions.

1. **A non-embarrassing small tier.** Gemma 4 31B-thinking sits within ~10 LMArena Elo points of 600B–1000B+ open-weight frontier models at roughly 10× fewer parameters (April 2026) [S29]. *Caveat carried forward:* Elo is a preference metric, not a correctness metric, and convergence compresses the very cost spread routing arbitrages.
2. **Affordable multi-model serving.** vLLM multi-LoRA reduces per-model memory overhead to megabytes and consolidates five 10%-utilised models onto one GPU [S31]; SGLang batches across adapters natively [S32]. *Caveat:* this shares one base model; CAMIR's tiers are distinct base models requiring separate resident VRAM.
3. **Self-hosted cost floors low enough to matter.** ~$0.10 per million tokens raw on a batched H100 for gpt-oss-120b against ~$0.60 per million output tokens hosted; under $0.05 on B200 at high batch [S26]. *Caveats:* idle GPUs bill in full (10% utilisation → 10× per-token cost), and all-in cost is 3–5× raw rental once engineering time is included [S27].
4. **Inference as a visible budget line.** Second-largest line item in enterprise AI budgets in 2026; 55–80% of enterprise AI GPU spend [S25].

---

## 5. Evaluation methods and benchmarks

### 5.1 The standard frame

RouterBench formalises the evaluation as a **two-dimensional cost–quality space**, in which individual models are *points* and routers are *curves* [S7]. Every claim in this pack about a "frontier" is this construction. RouterBench supplies 405,467 inference outcomes over 8 datasets — commonsense reasoning (HellaSwag, Winogrande, ARC-Challenge), knowledge, conversation, math, code and RAG — across 14 models from Mistral-7B to GPT-4 [S7].

The derived metric in common use is the **cost-performance threshold (CPT)**: the cost multiple achieved while retaining a stated fraction of the strongest model's quality — e.g. 3.66× at 95% of GPT-4 on MT-Bench [S2].

### 5.2 The oracle ceiling

The reference point that bounds the whole enterprise: the score a router would achieve with perfect foreknowledge of which tier answers each request correctly. It is computable offline by running every request through every tier and labelling outcomes — which is exactly the two-week experiment specified in `../BRIEF.md` §Riskiest assumption. **Everything CAMIR could ever claim lives between the fixed-model baseline and the oracle ceiling**, and if that interval is narrow the project reports a null result.

### 5.3 Judging — the weakest link, quantified

The quality axis of every frontier is a judgment about correctness, and in 2026 that judgment was systematically audited:

- LLM judges reach **~80% agreement with human preferences**, matching human-to-human consistency — but **inter-judge agreement is only ~76%**, and judges can be individually consistent yet mutually inconsistent, so **systematic error from judge choice may damage benchmark validity more than stochastic error within a single judge** [S33].
- Test-retest same-verdict rates are **>95% at temperature 0 and fall to ~70% at temperature 1** [S34].
- Documented biases: **position bias producing ~40% GPT-4 inconsistency, verbosity bias ~15% inflation, self-enhancement bias 5–7%** [S34].
- A 2026 RAND study, cited in [S33], finds no judge uniformly reliable across benchmarks, with frontier models exceeding 50% error on challenging bias benchmarks.

**The forced protocol.** Any credible CAMIR result must: judge at temperature 0; fix answer position or average over permutations; control for response length; prefer exact-match or programmatic verification wherever the task admits it; use more than one judge and **publish inter-judge agreement alongside the frontier**. A frontier without judge-agreement statistics is not reproducible.

### 5.4 Comparability

A 2026 paper argues directly that router evaluations across papers are **not comparable**, and calls for a standard harness [S13]. This is both a criticism of the literature CAMIR cites and an opening: the harness is missing, and building it is a contribution independent of whether CAMIR's router wins.

---

## 6. Evidence for and against the core mechanism

`../BRIEF.md` stakes the venture on: *prompt difficulty is predictable from the prompt alone, accurately enough that routing beats a fixed model on the cost-quality frontier.* The literature does not settle this cleanly, and the strongest counter-evidence is stated first.

### 6.1 Evidence FOR

1. **Cascades achieve large savings.** FrugalGPT matched GPT-4 performance with **up to 98% cost reduction**, or improved on GPT-4 by 4% at equal cost; 50–98% savings across benchmarks; **only 16.6% of queries escalated to GPT-4** [S3].
2. **Classifier routing achieves real, if smaller, savings.** RouteLLM: >85% cost reduction on MT-Bench, ~45% on MMLU, ~35% on GSM8K at ~95% of GPT-4 quality [S1]; CPT 3.66× / 1.41× / 1.49× [S2].
3. **Router overhead is negligible relative to generation.** $3.19–$39.26 per million requests [S1] — the decision is essentially free, so the economics turn entirely on decision *quality*.
4. **Tiers are not strictly nested.** A cheap model sometimes answers correctly where an expensive one fails [S3]. This is what makes routing more than thresholding a single competence ordering, and it is why an oracle ceiling can exceed the large model's own score.
5. **Simple calibrated confidence is competitive with trained routers** [S8] — so a working system does not depend on solving the hard prediction problem.
6. **The market validated the shape.** A frontier vendor shipped routing as a default [S20]; a gateway monetising adjacent to routing reached $160M annualised revenue [S17].

### 6.2 Evidence AGAINST — stated at full strength

1. **The routing plateau.** 21 routing methods across 5 benchmarks — classifier, retrieval, ranking, latent-factor/IRT, contrastive, expert-orchestration/cascade and bandit families from 2023–2026 — **converge into a narrow accuracy band that remains far below the oracle router**. The diagnosed cause is a **predictability bottleneck**: routers learn globally averaged model-performance trends rather than query-specific signal, so they collectively solve the same easy queries and collectively fail the hard ones. Scaling training data, using stronger query encoders and fine-tuning them yielded **up to 2.13 percentage points** [S4]. *Direct consequence: CAMIR's assumption A1, as literally worded, has largely been tested by the field and the answer is "only weakly".*
2. **The co-failure ceiling.** Across 67 frontier models, failures overlap — models are not independent, so combining them (routing, voting, mixture-of-agents) has a bounded benefit [S15]. The oracle ceiling itself is lower than a naive independence assumption predicts.
3. **The headline multiples do not generalise.** The same router that achieves 3.66× on MT-Bench achieves **1.41× on MMLU** [S2]. Conversational preference tasks flatter routers; knowledge and reasoning tasks do not. Any CAMIR figure quoted without its benchmark is misleading.
4. **The cost spread is compressing.** If a 31B model sits within ~10 Elo of a 1T-class model [S29], the price ratio between tiers narrows, and routing's prize narrows with it. Routing was most valuable when the frontier/cheap spread was widest — which was 2023, when FrugalGPT was measured.
5. **Cascade arithmetic under compression.** The cascade pays for the failed small attempt, plus the scorer, plus the large answer. FrugalGPT's economics were derived under 2023 hosted price ratios [S3]; nobody has re-derived them for compressed self-hosted spreads [G2].
6. **Caching gets there first and takes the easy traffic.** Semantic caching achieves **20–45% production hit rates** [S36], and hits are disproportionately the repetitive easy requests a router would have sent small. Post-cache traffic is adversely selected toward the hard end. The residual saving available to routing is unmeasured [G4].
7. **Routing is free for the largest segment.** No separate routing fee on the vendor-native path [S20].

### 6.3 The reconciliation — where the open problem actually is

The two 2026 ceiling papers appeared within roughly a month of each other and point in opposite directions, which is the most interesting fact in this survey.

[S4] says: routers converge well below oracle because difficulty is hard to predict.
[S5] says: a substantial part of what is measured as "the small tier cannot do this" is **not capability but instrumentation** — judge bias toward verbosity over correctness, **truncation under fixed generation budgets affecting 65% of MMLU and 57% of MedQA cases**, and **5–12% parse failures on MMLU** — across 206,000 query-model pairs on Gemma 4 and Llama 3.1 families [S5].

If both hold, then the gap between deployed routers and the oracle decomposes into (a) genuine unpredictability, which [S4] says is near its ceiling under current signals, and (b) measurement error, which [S5] says is large and fixable. **The field has been optimising (a) and neglecting (b).** Nobody has published the decomposition on a self-hosted pool.

This is the scientific basis for CAMIR's re-scoped claim: not *our router is smarter*, but *your frontier is measurable, and a meaningful part of the apparent ceiling is your harness*.

---

## 7. Risks to the mechanism

| # | Risk | Evidence | Severity |
|---|---|---|---|
| R1 | Difficulty is not predictable enough pre-generation; the classifier route adds nothing over calibrated confidence | [S4][S8] | **Fatal to half the thesis.** Mitigated only by the cascade route surviving independently |
| R2 | The oracle ceiling on realistic self-hosted traffic is too low to be worth routing | [S15][S4] | **Fatal.** This is the walk-away condition in `../BRIEF.md` and the first experiment must measure it |
| R3 | Judge unreliability makes the quality axis untrustworthy, so the frontier is noise | [S33][S34] | High — and it is the risk most under this team's control |
| R4 | Compressing tier cost spreads shrink the prize faster than routing improves | [S29][S26] | Medium-high, structural, and *worsens* over time |
| R5 | Semantic caching pre-harvests the easy traffic; residual routing savings are small | [S36], gap [G4] | Medium, unmeasured, and cheap to measure |
| R6 | Routing commoditises into the serving engine | [S11] | High on an 18-month horizon |
| R7 | Adversarial cost inflation — cascade deferral attacks force escalation with semantics-preserving perturbations | [S9] | Low now, real at scale; a cost-optimising router is a new attack surface |
| R8 | Published prior results are themselves contaminated by the artifacts [S5] identifies, so CAMIR cannot borrow them as evidence | [S5] | Medium — it lengthens the project but strengthens the contribution |

---

## 8. Open questions — where a contribution is still available

1. **What is the cost-quality frontier on a self-hosted open-weight pool?** Every published frontier prices against hosted API list prices [S7]; self-hosted cost is amortised GPU-hours per token under a utilisation assumption [S26][S27]. **The cost axis itself has not been derived.** Most concrete opportunity on this list.
2. **How does the plateau/artifact decomposition [S4] vs [S5] split on a self-hosted pool?** Nobody has run it. It is a publishable result independent of whether CAMIR's router wins.
3. **What are cascade economics under compressed tier spreads?** FrugalGPT's arithmetic assumed 2023 hosted ratios [S3].
4. **What savings remain after aggressive semantic caching?** [G4].
5. **Can prefill-activation routing [S10] break the plateau?** It is the one signal class hosted routers cannot access, and the plateau paper's diagnosis — routers see only global averages, not query-specific state — is precisely what an internal-state signal would address.
6. **What judging protocol makes a routing frontier reproducible?** [S13][S33][S34] make the need explicit and no standard exists.

---

## 9. Conclusion

The mechanism is real: cascades and classifier routers demonstrably reduce cost at controlled quality loss, sometimes dramatically [S1][S2][S3]. The mechanism is also, on current evidence, **near its ceiling as an algorithmic problem** [S4][S15], and its economic prize is **compressing** as small models close on frontier quality [S29]. What is *not* near its ceiling is the measurement: a sixth of MMLU's apparent unsolvability is truncation, judges disagree with each other a quarter of the time, and no two papers' numbers are comparable [S5][S33][S13].

For a small team, the defensible contribution is therefore not a better router. It is a **reproducible cost-quality frontier for self-hosted open-weight model pools** — a derived cost axis that does not yet exist, a judging protocol that survives the 2026 reliability results, and an honest decomposition of the gap to the oracle ceiling into unpredictability and instrumentation error. The router is the object under measurement, not the product.

## Recommended next 3

1. **Run the oracle-ceiling experiment before anything else** (§5.2, `../BRIEF.md` §Riskiest assumption). It bounds every claim in this pack and it is two weeks of local GPU time.
2. **Instrument against [S5]'s three artifacts from the first run** — generous generation budgets with truncation logging, strict output-format parsing with failure counts, verbosity-controlled judging. Otherwise the first frontier will reproduce the field's error and the ceiling will read artificially low.
3. **Report the classifier route as an ablation against calibrated confidence** [S8], not against the fixed-model baseline. Beating a fixed model is table stakes; beating free confidence is the real bar.
