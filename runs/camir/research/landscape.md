# CAMIR — Landscape

**What this is** — every approach that currently reduces the cost of serving mixed-difficulty LLM traffic, ordered from nearest to farthest from CAMIR's own mechanism, with each one's measured limit and the gap it leaves.
**Why it exists** — the router idea is not new, it is crowded: 21 routing methods were benchmarked in a single 2026 paper [S4], a frontier vendor ships routing as a default [S20], and an open serving stack is absorbing it [S11]. A pack that presents CAMIR as a fresh idea would be falsified by one search, and the honest reading — that the *idea* is settled and the *reproducible self-hosted evaluation* is not — is a stronger position than novelty would have been.
**How to read it** — read §1 and §7 and skip the middle if you are short of time: §1 is what already does CAMIR's job, §7 is what will do it for free in eighteen months. The gap column is the argument; attack it there.
**Depends on / feeds** — depends on [sources.md](sources.md) and [../BRIEF.md](../BRIEF.md); feeds [competitors.md](competitors.md), [survey.md](survey.md), [../strategy/positioning.md](../strategy/positioning.md), [../strategy/petal_diagram.md](../strategy/petal_diagram.md) and the teardown section of [../narrative/vc_memo.md](../narrative/vc_memo.md).

---

## How this is ordered

Distance from CAMIR's core loop — *classify a request's difficulty, dispatch to the smallest tier that will answer it correctly, measure the frontier*. Nearest first.

---

## 1. Direct — systems that route requests across model tiers

### 1.1 RouteLLM (LMSYS, open source)

**What it is.** An open framework for training and serving preference-data routers between a strong and a weak model, plus an evaluation harness.
**Maps onto CAMIR's loop.** Almost exactly, minus the pool: it is a **classifier route** with a binary tier decision.
**Mechanism.** Learn a win-probability model from human preference data (Chatbot Arena), route by predicted probability that the weak model suffices, threshold set by a target cost-performance point.
**Measured limits.** >85% cost reduction on MT-Bench, ~45% on MMLU, ~35% on GSM8K at ~95% of GPT-4 quality [S1]. In CPT terms: **3.66× on MT-Bench, 1.41× on MMLU, 1.49× on GSM8K** [S2]. Router serving cost is genuinely small — $3.19–$39.26 per million requests depending on router class [S1].
**The gap it leaves.** Three, and they compound. (a) **Two tiers, not a pool** — strong/weak binary, no medium tier and no notion of a configurable model pool. (b) **The evaluation pair is GPT-4 Turbo vs Mixtral 8x7B** [S1] — a hosted frontier model against a hosted open model; nothing in the published result tells a team what happens across three self-hosted open-weight tiers. (c) **The multiple collapses off-conversation**: quoting "85%" without saying "MT-Bench" is the single most common misreading in this space, and on knowledge tasks the real figure is under 1.5×.

### 1.2 FrugalGPT (Chen, Zaharia, Zou — Stanford)

**What it is.** The canonical LLM **cascade**: try cheap models in sequence, score each answer, stop when a scorer clears a threshold, escalate otherwise.
**Maps onto CAMIR's loop.** It *is* CAMIR's cascade route, with a learned answer-scorer as the escalation signal.
**Mechanism.** Per-query LLM sequence selection plus a trained generation scorer; the cascade's cost is the sum of attempts, so the scorer's precision is the whole economics.
**Measured limits.** Up to **98% cost reduction** matching GPT-4, or **+4% accuracy at equal cost**; 50–98% savings across benchmarks; **only 16.6% of queries reached GPT-4** [S3]. It also documents the finding that makes routing possible at all: a cheap model sometimes answers correctly where an expensive one fails, so the tiers are not strictly nested.
**The gap it leaves.** The results are from **2023-era hosted API price ratios**, where the frontier/cheap spread was enormous. Self-hosted open-weight tiers have a much narrower cost spread, and no published work re-derives the cascade economics under that compression — which is precisely the arithmetic CAMIR has to do. And the scorer is itself an inference call: the cascade pays for the failed small attempt *plus* the scorer *plus* the large answer.

### 1.3 Commercial hosted routers — Martian, Not Diamond

**What they are.** Routing sold as a service. Martian operates as a real-time routing proxy with per-request cost and latency attribution; Not Diamond positions as a **recommender** — it returns which model to call and you make the call [S19].
**Maps onto CAMIR's loop.** Same decision, delivered as vendor infrastructure over the vendor's catalog.
**Mechanism.** Proprietary; both offer custom router training on customer traffic.
**Measured limits.** Not independently verifiable. Not Diamond lists ~$0.05 per million tokens routed on top of model costs; Martian is enterprise "contact sales" and is *reported* — single weak secondary source, hedge required — to have approached a $1.3B valuation in April 2026 [S18][S19].
**The gap it leaves.** Vendor dependency and a **vendor-measured saving**. The buyer cannot audit the counterfactual, because the party computing "what you would have spent" is the party being paid for the difference. And neither has a self-hosting path: their value is the catalog.

### 1.4 Vendor-native routing — GPT-5's built-in router

**What it is.** A frontier vendor shipping the router inside the model product: an efficient model, a deeper reasoning model, and a real-time router selecting between them on conversation type, complexity, tool needs and explicit intent [S20].
**Maps onto CAMIR's loop.** The same loop, closed inside the vendor, over the vendor's own tiers, **with no separate routing fee** [S20].
**Measured limits.** No published cost-quality frontier. The instructive datum is behavioural: when mandatory routing shipped, users complained immediately of quality degradation from complex queries being sent to the smaller model [S21].
**The gap it leaves.** This is simultaneously the biggest threat and the clearest evidence for CAMIR's wedge. The threat: routing is now a free default for anyone on that vendor. The evidence: **when the vendor sets the quality tolerance, the customer feels the loss and cannot adjust it.** CAMIR's declared-tolerance-per-deployment framing exists because of exactly this failure. It does nothing, however, for teams already locked into that vendor — it only serves the ones running their own models.

### 1.5 vLLM Semantic Router — routing absorbed into the serving stack

**What it is.** A vision paper and project from within the vLLM ecosystem proposing a **Workload–Router–Pool** architecture for inference optimisation [S11].
**Maps onto CAMIR's loop.** The same three-part decomposition CAMIR uses, being built into the open serving substrate CAMIR would sit on top of.
**Mechanism.** Semantic classification of requests, dispatch across a pool managed by the serving layer.
**Measured limits.** Early; a vision paper rather than a measured frontier.
**The gap it leaves.** **This is the commoditisation clock.** If the serving stack routes natively, a standalone open-source router has no reason to exist and the open-core plan loses its top of funnel. What it does not obviously provide is the *measurement* half — a reproducible mixed-difficulty benchmark, consistent judging, a published frontier and per-deployment savings attribution. CAMIR should assume the routing mechanism commoditises and that the defensible surface is evaluation and policy.

---

## 2. Adjacent — gateways that could route but mostly do not

### 2.1 OpenRouter

Unified API across hundreds of models with an optional Auto Router. **$160M annualised revenue as of August 2026, up from $50M at end-2025, monetising at ~5% on top of inference spend**, reportedly being acquired by Stripe for over $7B [S17]. Maps to CAMIR's loop only at the edge — its value is catalog access and billing consolidation; routing is a feature. **The gap:** it is a hosted aggregator. A self-hosting team's whole point is not sending traffic to an aggregator. **What CAMIR should take from it:** the 5% take rate is the market's revealed price for a routing/gateway layer, and it is the only real pricing anchor in this landscape.

### 2.2 LiteLLM

Self-hosted open-source proxy: one OpenAI-compatible interface over many providers, with an enterprise tier [S22]. Nearest structural analogue to CAMIR's open-core shape and its most likely distribution channel. **The gap:** it normalises interfaces, it does not decide tiers by difficulty. A CAMIR that ships as a routing strategy *inside* LiteLLM reaches its users without building a proxy; a CAMIR that ships as a competing proxy fights an established one for the same install.

### 2.3 Portkey and the observability/gateway cluster

Gateway plus observability, positioned as the monitoring layer for LLM applications. Relevant less as a competitor than as the category ClickHouse bought into when it acquired Langfuse in January 2026 [S23] — evidence that this layer consolidates into data infrastructure rather than standing alone.

---

## 3. Orthogonal — cost reduction that is not routing

### 3.1 Semantic caching (GPTCache and successors)

**Mechanism.** Embed the request, similarity-search prior requests, return the stored answer above a threshold — avoiding inference entirely.
**Measured limits.** **Production hit rates of 20–45%**, with a 30–70% range by traffic pattern; ~31% of LLM queries are semantically similar to a previous one; research implementations reach 61.6–68.8% hit rates; hits return in 3–8ms against 500–2000ms [S35][S36].
**Relation to CAMIR.** Genuinely orthogonal and genuinely composable — but **it goes first in the pipeline, and it eats the easy traffic**. Cached requests are disproportionately the repetitive, easy ones a router would have sent to the small tier anyway. So caching does not merely reduce CAMIR's volume, it **adversely selects** the remaining traffic toward the hard end, which is where routing saves least. This is the honest correction assumption A14 needs and no published work measures it [G4].

### 3.2 Quantisation, distillation, speculative decoding, batching

Make each tier cheaper rather than choosing between tiers. Multi-LoRA serving lets many models share one GPU with megabyte-scale per-model overhead [S31]; high-concurrency batching gets a single H100 serving gpt-oss-120b to ~$0.10 per million tokens [S26]. **Relation to CAMIR:** these are *enablers*, not rivals — they are why a three-tier self-hosted pool is affordable at all. They also compress the cost spread between tiers, which shrinks the prize.

### 3.3 Prompt compression and context trimming

Reduce tokens per request rather than model size per request. Composable, smaller effect, and orthogonal to difficulty.

---

## 4. Research systems — the academic frontier

### 4.1 The survey's six paradigms

The field's own taxonomy: difficulty-aware routing, human-preference alignment, clustering, reinforcement learning, uncertainty quantification, and cascading — with a design frame of *when* the decision is made, *what* signals it uses, *how* it is computed [S6]. **CAMIR occupies two of the six** (difficulty-aware routing = classifier route; uncertainty quantification + cascading = cascade route) and its contribution is not a seventh paradigm but the comparison of these two under self-hosted conditions.

### 4.2 The routing plateau — the field's most damaging result

21 routing methods across 5 benchmarks converge into a **narrow accuracy band far below the oracle router**, caused by a **predictability bottleneck**: routers learn globally averaged model-performance trends rather than query-specific signal, so they all solve the same easy queries and collectively fail the hard ones. Scaling training data, using stronger encoders and fine-tuning them bought **up to 2.13 percentage points** [S4].

**This is the most important entry in this landscape.** It says CAMIR's assumption A1 is, as literally stated, largely already answered — and answered against. The honest CAMIR response is not to dispute it but to change what is being claimed: if all routers plateau at similar accuracy, then **the differentiator is not router accuracy but where the plateau sits on the cost axis for a given pool**, and that is a per-deployment measurement, not a universal algorithm. It also strengthens the case for the cascade route, whose escalation signal comes from an actual generation attempt rather than a pre-generation prediction.

### 4.3 Calibrated uncertainty and deferral

UCCI formulates cascade routing as calibrated-uncertainty thresholding and finds **simple confidence measures can route as well as trained routing models** [S8]. Cost-effective human-AI cascades and uncertainty-aware agent deferral extend the same idea. **Consequence for CAMIR:** the cheap baseline is strong, so a trained classifier must beat calibrated confidence, not just beat a fixed model.

### 4.4 Prefill-activation routing

Routing on the model's internal prefill activations rather than surface prompt features [S10]. **Strategically important to CAMIR:** activations are available only to whoever runs the weights. A hosted router cannot see them; a self-hosted router can. This is the one signal class where self-hosting is an *advantage* rather than a constraint, and it is the most defensible technical direction available to this project.

### 4.5 Ceiling papers

The **unsolvability ceiling** work finds a substantial share of measured "the small model cannot do this" is evaluation artifact rather than capability limit: judge bias toward verbosity, **truncation under fixed generation budgets in 65% of MMLU and 57% of MedQA cases**, and **5–12% parse failures on MMLU** [S5]. The **co-failure ceiling** work across 67 frontier models finds models fail on overlapping queries, bounding any combination strategy [S15].

Read together with §4.2: the plateau is real, but part of the gap to oracle is **mis-measurement**, and mis-measurement is fixable by careful harness engineering. That is the single most favourable fact in this landscape for a team whose stated edge is benchmark engineering.

### 4.6 Evaluation infrastructure

RouterBench provides **405,467 inference outcomes across 8 datasets and 14 models**, formalising routers as points or curves in cost-quality space [S7]; a 2026 paper argues router evaluations are not comparable across papers and calls for a standard harness [S13]. **CAMIR's benchmark should extend RouterBench's format rather than invent one**, and the "fair evaluation" gap is an open, citable need.

---

## 5. Classical and manual — what teams actually do today

| Approach | Mechanism | Why it persists | Measured limit |
|---|---|---|---|
| One fixed frontier model | No decision at all | Guarantees quality; zero engineering | Pays frontier price on every easy request. FrugalGPT's 16.6% figure implies ~83% of queries were overpaying in that setting [S3] |
| One fixed cheap model | No decision at all | Cheapest possible | Fails the hard subset, and the failures are concentrated where they are most visible |
| Prompt-length / keyword rules | Surface heuristics | Ten lines of code, ships in an afternoon | Surface features correlate weakly with difficulty — the predictability bottleneck applies to them *more* than to learned routers [S4]. Decays as traffic shifts |
| Endpoint-level model assignment | Static per-route model choice | Matches how teams already organise services | Misses the difficulty variance *inside* each endpoint, which is where most of it lives |
| Manual A/B "cheapest model that seems fine" | Human evaluation, once | Actually works, briefly | Point estimate, not a frontier; stale on the next model release or traffic shift; no per-request adaptation |

**The gap they leave, collectively.** All five are *static policies over dynamic traffic*. Every one of them is a routing decision — made once, by a person, without measurement. The existence of four separate workarounds is the demand evidence from `BRIEF.md`; their common failure is that nobody knows where their policy sits on the cost-quality frontier because nobody has plotted it.

---

## 6. Emerging hybrids

- **Latency-aware routing** — cost, quality *and* latency as a three-axis problem [S12]. CAMIR excludes latency in year one; this is where the frontier becomes a surface.
- **Adversarial routing** — cascade deferral attacks force escalation with semantics-preserving perturbations [S9]. A cost-optimising router is a new attack surface: an adversary can inflate a victim's bill.
- **Routing inside the serving engine** — [S11], §1.5. The commoditisation path.
- **Difficulty-aware self-routing** — models estimating their own competence before answering. Collapses the router into the model and would remove the need for a separate component entirely.

---

## 7. The gap CAMIR actually occupies

After the above, the honest statement of white space is narrow and it is **not** "nobody has built a router."

1. **No published cost-quality frontier on a self-hosted open-weight pool.** Every headline number in §1 comes from a hosted catalog with a wide price spread [S1][S2][S3]. Self-hosted tiers have a compressed spread, and nobody has re-derived the economics under compression [G2].
2. **The measurement layer is the unsolved part, and it is measurably broken.** 65% truncation on MMLU, 5–12% parse failures, judges agreeing with each other only 76% of the time, verdicts flipping 30% of the time at temperature 1 [S5][S33][S34]. A large fraction of the gap to the oracle ceiling is instrumentation error.
3. **Nobody lets the customer set the quality tolerance and see the cost of it.** Vendor routing sets it silently and users revolt [S21]; commercial routers measure their own savings; fixed-model policies never state a tolerance at all.
4. **Prefill-activation signal is available only to self-hosters** [S10] and is unexploited commercially.

**Where this leaves the wedge.** CAMIR's defensible contribution is a **reproducible measurement apparatus and a declared-tolerance policy surface for self-hosted pools** — with the router as the thing being measured, not the thing being sold. The routing algorithm plateaus [S4]; the harness does not have to.

---

## Recommended next 3

1. **Re-scope the technical claim from "our router is better" to "our frontier is measurable on your pool."** [S4] makes router-accuracy superiority an unwinnable claim and [S5] makes measurement quality a winnable one. This decision propagates into `strategy/positioning.md` and `tech/whitepaper.md`.
2. **Benchmark the cascade route as the primary strategy and the classifier route as the ablation**, inverting the abstract's implicit ordering. The plateau [S4] and calibrated-confidence results [S8] both favour post-generation signal over pre-generation prediction.
3. **Measure the caching interaction before pricing anything.** [S36] says 20–45% of production traffic never reaches a router, adversely selected toward the easy end. Until residual savings on cache-miss traffic are known, every figure in `financials/` inherits gap G4.
