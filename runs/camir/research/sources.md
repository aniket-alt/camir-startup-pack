# CAMIR — Source Register

**What this is** — the numbered citation list every other artifact in this pack points at. Each entry: title, URL, date accessed, and what it is allowed to support.
**Why it exists** — CAMIR has zero measurements of its own, so every number in the pack is borrowed. Without a register a reader cannot tell the 85% cost reduction RouteLLM measured from a 40% saving the pack imagined, and the pack's whole credibility rests on that distinction being visible. It also records **confidence**, so the two competitor valuations sourced from secondary blogs cannot be quoted as though they were filings.
**How to read it** — check the Confidence column before quoting any figure downstream. `primary` = paper, vendor page or official post. `secondary` = credible aggregator or analyst blog. `weak` = single blog restating an unnamed source; usable only with the hedge attached.
**Depends on / feeds** — depends on nothing; feeds [landscape.md](landscape.md), [competitors.md](competitors.md), [capability_table.md](capability_table.md), [survey.md](survey.md) and, transitively, every sourced number in [strategy/](../strategy/), [tech/](../tech/) and [financials/](../financials/).

All sources accessed **2026-09-09** unless noted otherwise.

---

## Routing and cascading — the core literature

**[S1]** RouteLLM: An Open-Source Framework for Cost-Effective LLM Routing — https://www.lmsys.org/blog/2024-07-01-routellm/ — accessed 2026-09-09 — *primary* — Supports: >85% cost reduction on MT-Bench, ~45% on MMLU, ~35% on GSM8K versus GPT-4-only while retaining ~95% of GPT-4 performance; router deployment cost per million requests (SW Ranking $39.26, Causal LLM $5.23, Matrix Factorisation $3.32, BERT $3.19); the strong/weak pair used was GPT-4 Turbo vs Mixtral 8x7B.

**[S2]** RouteLLM: Learning to Route LLMs with Preference Data — arXiv:2406.18665 — https://arxiv.org/pdf/2406.18665 — *primary* — Supports: cost-performance-threshold (CPT) framing; 3.66× cost saving on MT-Bench at 95% GPT-4 quality, 1.41× on MMLU at 92%, 1.49× on GSM8K at 87%. **Note the collapse**: the headline 3.66× holds only on the most conversational benchmark; on knowledge and math it is under 1.5×. Any CAMIR claim must cite the benchmark alongside the multiple.

**[S3]** FrugalGPT: How to Use Large Language Models While Reducing Cost and Improving Performance — Chen, Zaharia, Zou — arXiv:2305.05176, TMLR 2024 — https://arxiv.org/abs/2305.05176 — *primary* — Supports: LLM cascade formulation; up to 98% cost reduction matching GPT-4 performance, or +4% accuracy at equal cost; 50–98% savings across benchmarks; only 16.6% of queries reached GPT-4; the observation that a cheap model sometimes answers correctly where an expensive one fails.

**[S4]** The Routing Plateau: Understanding and Breaking the Accuracy Limits of LLM Routers — Lu et al. — arXiv:2606.07587, May 2026 — https://arxiv.org/abs/2606.07587 — *primary* — Supports: study of **21 routing methods across 5 benchmarks**, spanning classifier, retrieval, ranking/pairwise, latent-factor/IRT, contrastive, expert-orchestration/cascade and bandit/cost-aware families proposed 2023–2026. Finding: a **routing plateau** — methods converge into a narrow accuracy band far below the oracle router, caused by a **predictability bottleneck** (routers learn global averaged model-performance trends, not query-specific signal). Remedies tested (more training data, stronger query encoders, encoder fine-tuning) bought **up to 2.13 percentage points**. **This is the strongest published evidence against CAMIR's riskiest assumption (A1) and must be cited wherever A1 is discussed.**

**[S5]** Unsolvability Ceiling in Multi-LLM Routing: An Empirical Study of Evaluation Artifacts — arXiv:2605.07395, May 2026 — https://arxiv.org/abs/2605.07395 — *primary* — Supports: 206,000 query-model pairs across MMLU, MedQA, HumanEval, MBPP, Alpaca, ShareGPT using Gemma 4 and Llama 3.1 families. Finding: a substantial share of measured "the small model cannot do this" is an **evaluation artifact**, not a capability limit — judge bias toward verbosity over correctness; **truncation under fixed generation budgets (65% of MMLU, 57% of MedQA cases)**; output-format mismatch (**5–12% parse failures on MMLU**). **This is the single most important source for CAMIR's founder edge**: it says the oracle ceiling is mostly a measurement problem, and measurement is what this team does.

**[S6]** Dynamic Model Routing and Cascading for Efficient LLM Inference: A Survey — arXiv:2603.04445 — https://arxiv.org/abs/2603.04445 — *primary* — Supports: the field's taxonomy — six paradigms (difficulty-aware routing, human-preference alignment, clustering, reinforcement learning, uncertainty quantification, cascading) and a three-dimension design frame (*when* the decision is made, *what* signals it uses, *how* it is computed). Scope note: routing between independently trained models at inference time, explicitly excluding mixture-of-experts.

**[S7]** RouterBench: A Benchmark for Multi-LLM Routing System — arXiv:2403.12031 — https://arxiv.org/abs/2403.12031 — *primary* — Supports: **405,467 inference outcomes** across 8 datasets (commonsense reasoning incl. HellaSwag/Winogrande/ARC-Challenge, knowledge, conversation, math, code, RAG) over 14 LLMs from Mistral-7B to GPT-4; formalises routers as **points or curves in two-dimensional cost–quality space** — the exact framing CAMIR calls the cost-quality frontier.

**[S8]** UCCI: Calibrated Uncertainty for Cost-Optimal LLM Cascade Routing — arXiv:2605.18796 — https://arxiv.org/pdf/2605.18796 — *primary* — Supports: calibrated-uncertainty thresholding as the escalation signal in cascades; the finding that simple confidence measures can route as well as trained routing models.

**[S9]** Forced Deferral: Manipulating Routing Decisions in Multimodal LLM Cascades — arXiv:2606.15308 — https://arxiv.org/html/2606.15308 — *primary* — Supports: **cascade deferral attacks** — semantics-preserving input perturbations that suppress small-tier confidence to force escalation. The adversarial cost risk in `financials/risk_matrix.md`: an attacker can inflate a victim's inference bill.

**[S10]** LLM Router: Rethinking Routing with Prefill Activations — arXiv:2603.20895 — https://arxiv.org/pdf/2603.20895 — *primary* — Supports: routing on internal prefill activations rather than surface prompt features — the signal class self-hosting makes available and hosted-API routers cannot reach.

**[S11]** The Workload-Router-Pool Architecture for LLM Inference Optimization — vLLM Semantic Router project vision paper — arXiv:2603.21354 — https://arxiv.org/pdf/2603.21354 — *primary* — Supports: routing is being absorbed into the open serving stack itself; the workload/router/pool decomposition CAMIR's architecture parallels. **Also the sharpest "why won't the serving layer just do this" threat.**

**[S12]** Beyond Accuracy and Cost: Latency-Aware LLM Query Routing for Dynamic Workloads — arXiv:2607.18253, July 2026 — https://arxiv.org/abs/2607.18253 — *primary* — Supports: latency as a third routing axis alongside cost and quality; relevant to CAMIR's declared non-goal of latency SLO management in year one.

**[S13]** Towards Fair and Comprehensive Evaluation of Routers in Collaborative LLM Systems — arXiv:2602.11877 — https://arxiv.org/pdf/2602.11877 — *primary* — Supports: existing router evaluations are not comparable across papers; the case for a standard harness.

**[S14]** SMART: Automatically Scaling Down Language Models with Accuracy Guarantees for Reduced Processing Fees — arXiv:2403.13835 — https://arxiv.org/pdf/2403.13835 — *primary* — Supports: the "accuracy guarantee" framing — a declared tolerance versus the strongest model, which is what CAMIR calls quality tolerance.

**[S15]** When Does Combining Language Models Help? A Co-Failure Ceiling on Routing, Voting, and Mixture-of-Agents Across 67 Frontier Models — arXiv:2606.27288 — https://huggingface.co/papers/2606.27288 — *primary* — Supports: models fail on overlapping queries, which bounds the benefit of any combination strategy including routing. Second-strongest evidence against A2 after [S4].

**[S16]** Awesome-Routing-LLMs — MilkThink-Lab — https://github.com/MilkThink-Lab/Awesome-Routing-LLMs — *secondary* — Supports: the field is large enough to sustain a curated tracker; useful as a completeness check on the landscape, not as a factual citation.

## Commercial routers and gateways

**[S17]** OpenRouter — revenue, valuation & funding — Sacra — https://sacra.com/c/openrouter/ — *secondary* — Supports: **$160M annualised revenue as of August 2026, up from $50M at end-2025**; monetises at **~5% on top of customer inference spend**; reported $1.3B valuation round; Stripe reported to have agreed an acquisition above **$7B**. The 5% take rate is the single most useful pricing anchor in this register — it is a *revealed* market price for a routing/gateway layer, not a survey.

**[S18]** Martian — reportedly nearing $1.3B valuation, April 2026 — https://medium.com/@sarawgiapoorvwork347/martian-the-san-francisco-based-startup-that-invented-the-first-llm-router-is-reportedly-nearing-4211dd768296 — **weak** — Supports: only the hedged statement "a purpose-built commercial LLM router is reported to have reached roughly $1.3B valuation in April 2026." Single secondary blog restating an unnamed report. **Do not state as fact; always carry the hedge.**

**[S19]** Not Diamond vs Martian: Routing Layers, Not Gateways — Orca Router — https://www.orcarouter.ai/blog/not-diamond-vs-martian — **weak** (competitor-authored) — Supports, with hedge: Not Diamond positions as a recommender rather than a proxy, with a listed routing fee of ~$0.05 per million tokens routed on top of model costs; Martian as a real-time routing proxy with per-request cost attribution and enterprise "contact sales" pricing. Competitor marketing; verify before quoting a price.

**[S20]** Introducing GPT-5 — OpenAI — https://openai.com/index/introducing-gpt-5/ — *primary* — Supports: a frontier vendor shipped **routing as a built-in product feature** — a unified system with an efficient model, a deeper reasoning model, and a real-time router choosing between them on conversation type, complexity, tool needs and explicit intent. **No separate routing fee; you pay the underlying model tokens.** This is the incumbent counter-move from `BRIEF.md`, already executed.

**[S21]** GPT-5 Router — Inevitable Future of Chat Interfaces — https://dipkumar.dev/posts/llm/gpt5-router/ — *secondary* — Supports: the documented user backlash when mandatory routing shipped — complaints of quality degradation from complex queries being sent to the smaller model. **The clearest public evidence that routing fails loudly when the tolerance is set by the vendor rather than the customer**, which is CAMIR's positioning wedge.

**[S22]** LiteLLM pricing guide — TrueFoundry — https://www.truefoundry.com/blog/litellm-pricing-guide — *secondary* — Supports: LiteLLM as the self-hosted open-source proxy baseline; the open-source-gateway-plus-enterprise-tier shape CAMIR's open-core plan resembles.

## Post-mortems — why previous attempts died

**[S23]** TensorZero Shuts Down: What OSS LLMOps Can't Survive — byteiota — https://byteiota.com/tensorzero-shuts-down-what-oss-llmops-cant-survive/ — *secondary* — Supports: TensorZero **archived its GitHub repository 12 June 2026** after raising **$7.3M** and passing **11,000 GitHub stars**; founders returned unused capital and cited the difficulty of finding product-market fit for an open-source project *and* a commercial product simultaneously. Context in the same source: **ClickHouse acquired Langfuse in January 2026** as part of a $400M Series D at a $15B valuation, and the frontier vendors and hyperscalers are shipping native gateway, observability and evaluation features. **This is the direct post-mortem for CAMIR's chosen open-core shape and must be cited in `strategy/market_type.md`, `validation/riskiest_assumptions.md` and `financials/risk_matrix.md`.**

## Market structure and cost

**[S24]** Enterprise LLM Market Size, Share, Growth, Analysis, Report — Straits Research — https://straitsresearch.com/report/enterprise-llm-market — *secondary* — Supports: enterprise LLM market **$8.18B in 2026 → $51.66B by 2034, CAGR 25.9%**. Analyst top-down figure; used only as a sanity check against CAMIR's bottom-up sizing, never as the sizing itself.

**[S25]** AI Inference Cost Economics in 2026: GPU FinOps Playbook — Spheron — https://www.spheron.network/blog/ai-inference-cost-economics-2026/ — *secondary* — Supports: **inference cost overtook cloud infrastructure to become the second-largest line item in enterprise AI budgets in 2026, behind talent only**; analyst estimates that **55–80% of enterprise AI GPU spend goes to inference**. This is the load-bearing "why now" number for CAMIR's third shift.

**[S26]** LLM Inference Cost 2026: Cost per Million Tokens — packet.ai — https://packet.ai/blog/llm-inference-cost — *secondary* — Supports: a single H100 serving gpt-oss-120b at high-concurrency batching reaches **~$0.10 per million tokens raw GPU cost**, against hosted endpoints serving the identical model at **~$0.60 per million output tokens**; on B200 at ~$3.75/hr, effective cost falls **below $0.05 per million output tokens** for 70B+ models at high batch.

**[S27]** Self-Hosting an LLM vs. API: Real Cost Math (2026) — Cloudzy — https://cloudzy.com/blog/self-hosting-open-weight-llm-gpu-vps-cost/ — *secondary* — Supports: break-even near **100M tokens/month** against frontier APIs for a 70B-class model on dedicated hardware; **16M+ tokens/day on H100 on-demand (22M+ on H200)** as the self-hosting threshold; **a GPU idle at 10% utilisation costs 10× per token**; realistic all-in cost is **3–5× the raw GPU rental** once engineering time is counted. The 3–5× multiplier is the honest correction CAMIR must apply to any self-hosted savings claim.

**[S28]** Self-Hosted LLM Costs 2026 — SitePoint — https://www.sitepoint.com/self-hosted-llm-costs-2026/ — *secondary* — Supports: against budget open-weight APIs (~$0.14–$0.50 per million tokens) self-hosting rarely wins on cost alone. **The uncomfortable corollary for CAMIR: the self-hosting population exists for reasons other than price — data residency, latency, control — which changes the pitch.**

## Enabling technology

**[S29]** The Best Open-Source Small Language Models (SLMs) in 2026 — BentoML — https://www.bentoml.com/blog/the-best-open-source-small-language-models — *secondary* — Supports: **Gemma 4 31B-thinking reaches LMArena Text Elo within ~10 points of 600B–1000B+ open-weight frontier models at roughly 10× fewer parameters (April 2026)**; SmolLM3 at 3B competitive with 4B-class models across 12 benchmarks; Gemma 3 sizes 1B/4B/12B/27B with 128K context. **This is the "a tier below frontier exists that is not embarrassing" evidence.**

**[S30]** The State of Open-Source LLM Inference Engines in 2026 — https://builderai.tools/blog/state-of-open-source-llm-inference-engines-2026 — *secondary* — Supports: **Ollama crossed 52 million monthly downloads in Q1 2026, from ~100K monthly in Q1 2023 (~520×)**. Proxy evidence for the growth of local/self-hosted serving. **Downloads are not production deployments** — this supports direction, not the size of the beachhead.

**[S31]** Efficiently serve dozens of fine-tuned models with vLLM — vLLM Blog, 26 Feb 2026 — https://vllm.ai/blog/2026-02-26-multi-lora — *primary* — Supports: multi-LoRA lets multiple custom models share one GPU with only adapters swapped per request, memory overhead per model reduced to megabytes; five customers each using 10% of a GPU consolidate onto one. The mechanism that makes a multi-tier **model pool** affordable on small hardware.

**[S32]** vLLM vs SGLang for Production LLM Serving in 2026 — https://devopsbeast.com/blog/vllm-vs-sglang-production-2026 — *secondary* — Supports: vLLM's multi-LoRA is more mature on hot-reload and adapter rotation; SGLang treats multi-LoRA as first-class with native batching across adapters and has an edge at 50+ adapters with heterogeneous traffic. Informs the serving-substrate choice in `tech/not_vaporware.md`.

## Evaluation and judging

**[S33]** Reliability without Validity: A Systematic, Large-Scale Evaluation of LLM-as-a-Judge Models Across Agreement, Consistency, and Bias — arXiv:2606.19544 — https://arxiv.org/html/2606.19544 — *primary* — Supports: LLM judges reach ~**80% agreement with human preferences**, matching human-to-human consistency, but **inter-judge agreement is only ~76%**; judges can be individually consistent yet mutually inconsistent, so **systematic error from judge choice may damage benchmark validity more than stochastic error within one judge**. A 2026 RAND study is cited finding no judge uniformly reliable across benchmarks, with frontier models exceeding 50% error on challenging bias benchmarks.

**[S34]** The Coin Flip Judge? Reliability and Bias in LLM-as-a-Judge Evaluation — arXiv:2606.13685 — https://arxiv.org/pdf/2606.13685 — *primary* — Supports: test-retest same-verdict rates **above 95% at temperature 0, falling to ~70% at temperature 1**; position bias producing ~40% GPT-4 inconsistency, verbosity bias ~15% inflation, self-enhancement bias 5–7%. **Directly sets CAMIR's judging protocol: temperature 0, fixed position, verbosity-controlled.**

## Adjacent techniques

**[S35]** GPT Semantic Cache: Reducing LLM Costs and Latency via Semantic Embedding Caching — arXiv:2411.05276 — https://arxiv.org/pdf/2411.05276 — *primary* — Supports: API-call reduction up to **68.8%**, cache hit rates **61.6–68.8%** in the paper's setting.

**[S36]** Semantic Caching for LLM Inference — Spheron, 2026 — https://www.spheron.network/blog/semantic-cache-llm-inference-gpu-cloud/ — *secondary* — Supports: **production semantic-cache hit rates of 20–45%** (citing 2026 Technion benchmarks), with a wider 30–70% range by traffic pattern; **~31% of LLM queries are semantically similar to a previous request**; cache hits return in 3–8ms vs 500–2000ms. **The number that shrinks CAMIR's addressable saving: routing only ever prices the cache-miss traffic.**

**[S37]** GPTCache — https://github.com/zilliztech/gptcache — *primary* — Supports: the reference open-source semantic cache; embedding + vector-store similarity lookup.

## Business model comparables

**[S38]** ProsperOps Pricing: Fees, Savings Share & Hidden Cost (2026) — Usage.ai — https://www.usage.ai/blogs/finops/tools/prosperops-pricing/ — *secondary* — Supports: share-of-savings ("Savings Share") is an established, funded pricing model in cloud FinOps; **no universal public rate — the percentage and the eligible savings categories are defined per customer agreement**. Precedent for CAMIR's preferred pricing, and warning that the definition of "eligible savings" is where the negotiation actually happens.

**[S39]** nOps Pricing: Fees, Savings Share & Hidden Costs (2026) — Usage.ai — https://www.usage.ai/blogs/finops/tools/nops-pricing/ — *secondary* — Supports: a second share-of-savings vendor, likewise not publishing a universal rate; Usage.ai itself charges a percentage of realised savings with no charge when none are realised.

**[S40]** Monetizing Open-Source Software: Pricing Strategies for Open-Core SaaS — Monetizely — https://www.getmonetizely.com/articles/monetizing-open-source-software-pricing-strategies-for-open-core-saas — *secondary* — Supports: typical open-source conversion rates — **hosted SaaS 1–5% of active users, enterprise licences 0.01–0.1% at much higher value, sponsorship 0.1–1%**; open core is the dominant developer-tool monetisation model in 2026; infrastructure projects convert better than general developer tools.

---

## Gaps — facts we looked for and could not source

Stated explicitly, because a marked gap beats a confident guess.

| # | What we could not find | Why it matters | Consequence |
|---|---|---|---|
| [G1] | Any survey giving the **share of production LLM workloads served from self-hosted open-weight pools**, with a denominator | This is assumption **A3** — the size of CAMIR's beachhead | `strategy/market_sizing.md` must build bottom-up from Ollama/vLLM proxies [S30] and state the range honestly; no top-down number exists |
| [G2] | Published **routing savings measured on a self-hosted open-weight pool** rather than a hosted catalog | It is exactly the gap CAMIR claims to fill — so its absence is the opportunity *and* means no prior number can be borrowed | Every CAMIR savings figure is `(assumption)` until its own benchmark runs |
| [G3] | Martian's or Not Diamond's **actual revenue** | Would size the commercial routing category | Category size is inferred from OpenRouter [S17] only, and OpenRouter is a gateway with routing, not a router |
| [G4] | Measured **residual savings from routing after aggressive semantic caching** on the same traffic | Assumption A14; determines whether CAMIR's saving is additive or largely pre-harvested | `financials/unit_economics.md` must model cache-miss traffic only and say so |
| [G5] | Any **pricing evidence for routing sold as a separate line item** to a self-hosting team | Assumption A4 | Pricing is anchored to OpenRouter's 5% [S17] and FinOps share-of-savings precedent [S38][S39], both from adjacent categories |
