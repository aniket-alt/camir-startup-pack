# CAMIR — Competitor Teardown

**What this is** — the teardown table for everything a platform team could use instead of CAMIR, including doing nothing, with each one's pricing, funding, traction signal and the *mechanical* reason it fails; followed by the two axes that actually divide this market and where the white space sits.
**Why it exists** — the competitor question CAMIR loses on is not "who else routes" but "why would anyone pay for routing when a frontier vendor gives it away free [S20] and the open serving stack is absorbing it [S11]". If that answer is not written down before the strategy layer is built, positioning will be drawn against the wrong opponent — the other routers — instead of against free.
**How to read it** — the table's last column is the argument. Then read §Two axes: the conventional cost-versus-quality map is the *wrong* one, and §Two axes says why and what replaces it.
**Depends on / feeds** — depends on [landscape.md](landscape.md) and [sources.md](sources.md); feeds [../strategy/positioning.md](../strategy/positioning.md), [../strategy/market_type.md](../strategy/market_type.md), [../strategy/petal_diagram.md](../strategy/petal_diagram.md), [../narrative/vc_memo.md](../narrative/vc_memo.md) and [../financials/comps_exits.md](../financials/comps_exits.md).

---

## The teardown table

| Competitor | Segment it serves | Mechanism | Pricing | Funding / stage | Traction signal | Where it wins | Where it fails **mechanistically** |
|---|---|---|---|---|---|---|---|
| **Do nothing — one fixed frontier model** | Everyone, by default | No decision. Every request to the largest affordable model | 100% of list model price on every request | n/a | The default state of nearly all deployments | Guaranteed quality ceiling; zero engineering; no new failure mode | Pays the maximum price on the easy majority. FrugalGPT sent only **16.6%** of queries to GPT-4 while matching its quality [S3] — the other ~83% were overpaying by construction. Cost scales linearly with traffic forever |
| **Do nothing — one fixed cheap model** | Cost-constrained teams | No decision | Lowest available | n/a | Common in prototypes | Cheapest possible bill | Quality loss is not spread evenly — it lands entirely on the hard subset, which is the visible, complained-about, escalated subset. A 5% average quality drop can be a 40% drop on the traffic that matters |
| **Hand-written routing rules** | Teams that noticed the bill | Regex on prompt length, keyword lists, endpoint mapping | Engineering time only | n/a | The dominant workaround; four variants named in `BRIEF.md` | Ships in an afternoon; fully inspectable | Surface features are a *weaker* difficulty signal than learned routers, and learned routers already plateau far below oracle because of a predictability bottleneck [S4]. Rules decay silently as traffic shifts, and nobody re-measures — there is no frontier to check against |
| **GPT-5-style vendor-native routing** | Every team already on that vendor | Real-time router between an efficient model and a reasoning model, on complexity, tool needs and explicit intent | **No separate routing fee** — pay the underlying model tokens [S20] | Frontier lab | Shipped and default-on | Free, zero integration, tuned by the people who own the weights | Two structural failures. (1) **The tolerance is the vendor's, not yours** — when mandatory routing shipped, users immediately reported complex queries degraded by being sent to the smaller model [S21], and there is no customer-side dial. (2) **It cannot route off-vendor** — it optimises within one catalog, which is the opposite of what a self-hosting team needs. Also: the vendor is paid more when it routes up, and audits its own decisions |
| **Commercial hosted routers — Martian** | Enterprises on hosted catalogs | Real-time routing proxy, per-request cost/latency attribution, custom router training | Enterprise, contact sales [S19] | *Reported* to have neared ~$1.3B valuation April 2026 — **single weak secondary source, hedge required** [S18] | Category leader by mindshare | Genuinely solves the problem for hosted-catalog teams; real observability | **The savings are vendor-measured.** The party computing "what you would have spent" is the party paid the difference, and the buyer cannot audit the counterfactual. No self-hosting path — the value is catalog breadth. Adds a vendor dependency in the request path |
| **Commercial routers — Not Diamond** | Teams wanting recommendation, not proxying | Recommender: returns which model to call; you make the call | ~$0.05 per million tokens routed, on top of model cost [S19] — *competitor-authored source, verify* | Venture-backed | — | No proxy in the data path; lower blast radius | Same catalog assumption. Recommending a model you cannot serve is useless to a self-hoster, and the fee is charged per token routed whether or not the routing saved anything |
| **OpenRouter (gateway with routing)** | Teams wanting one API over many models | Unified API, hundreds of models, optional Auto Router | **~5% on top of inference spend** [S17] | **$160M annualised revenue Aug 2026, up from $50M end-2025; reported Stripe acquisition >$7B** [S17] | The category's commercial proof | Enormous catalog, billing consolidation, real revenue at real scale | Structurally opposed to self-hosting: its value *is* being the intermediary. A team that runs its own weights is deliberately removing the intermediary. Also: 5% of spend is charged on the whole bill, so its incentive is not to shrink the bill |
| **LiteLLM (self-hosted OSS proxy)** | Self-hosting teams — **CAMIR's exact users** | OpenAI-compatible proxy over many providers; enterprise tier [S22] | OSS free; enterprise tier priced separately | Commercial OSS | Widely deployed as default self-hosted proxy | Already installed where CAMIR wants to be; normalises interfaces | Does not decide tiers by difficulty — it is plumbing, not policy. **This is the most important row in the table and it is not really a competitor: it is CAMIR's likeliest distribution channel.** Competing with it for the same install is the losing move; shipping as a routing strategy inside it is the winning one |
| **vLLM Semantic Router / serving-native routing** | Self-hosting teams | Workload–Router–Pool inside the serving engine [S11] | Free, open source | Project within the dominant OSS serving stack | Early, vision-stage | Lives where the weights already are; can see internal state; zero extra hop | **The commoditisation clock, not a weakness.** Today it lacks a measurement layer — reproducible mixed-difficulty benchmark, judged correctness, published frontier, per-deployment savings attribution. If it acquires one, CAMIR's open half has no reason to exist |
| **Semantic caching (GPTCache et al.)** | Teams with repetitive traffic | Embed, similarity-search, return stored answer | OSS free; vector store cost | Mature OSS | **20–45% production hit rates**, 30–70% by traffic pattern; ~31% of queries semantically similar to a prior one [S35][S36] | Removes inference entirely on hits; 3–8ms vs 500–2000ms | Only repeats. **But it is not neutral toward CAMIR:** it sits upstream and harvests the repetitive, easy traffic first, adversely selecting what reaches the router toward the hard end where routing saves least. Composable, and it takes the cream |
| **RouteLLM (OSS reference implementation)** | Researchers, teams building their own | Preference-data classifier over a strong/weak pair | Free | LMSYS academic project | The reference citation in the field | Free, published, credible, reproducible on hosted models | Two tiers not a pool; evaluated GPT-4 Turbo vs Mixtral 8x7B [S1]; the 85% headline is MT-Bench-specific and drops to 1.41× on MMLU [S2]. **A team could fork it instead of buying CAMIR** — which is the correct read of CAMIR's moat |

---

## The two axes that actually matter

**The obvious map — cost reduction × quality retained — is the wrong one, and it is wrong in a specific way.** It is not that the axes are unimportant; it is that they are the *output* of every system in the table, so plotting competitors on them requires numbers nobody publishes comparably [S13], and the routing plateau finding says that on the quality axis **21 methods converge into a narrow band** [S4]. An axis on which everyone scores the same does not divide a market. Cost-versus-quality is the frontier CAMIR *plots*; it is not the map CAMIR *competes* on.

The two axes that do divide this market:

### Axis 1 — Who owns the weights: hosted catalog ←→ self-hosted pool

This is a hard structural boundary, not a preference. It determines what signals a router can see (a self-hoster can route on prefill activations [S10]; a hosted router cannot), who can audit the savings, whether data leaves the perimeter, and whether the vendor's incentive is aligned. **Every commercial router in the table sits at the hosted end and cannot cross**, because their value proposition is the catalog. Nobody is monetising the self-hosted end; the only things there are free — LiteLLM, RouteLLM, vLLM Semantic Router.

### Axis 2 — Who sets the quality tolerance: vendor-set and opaque ←→ customer-set and measured

This is the axis the GPT-5 backlash exposed [S21]. Every incumbent decides internally how much quality to trade for cost and does not show the customer the exchange rate. Fixed-model policies never state a tolerance at all. Commercial routers state a saving but compute it themselves. **Nobody hands the customer a plotted frontier for their own traffic and lets them pick the point.**

### The map

```
                      customer-set tolerance, measured frontier
                                      ▲
                                      │
                        (empty)       │       ◆ CAMIR — the open quadrant
                                      │
   hosted catalog ───────────────────┼───────────────────► self-hosted pool
                                      │
     ◆ Martian   ◆ Not Diamond        │   ◆ LiteLLM        ◆ vLLM Semantic Router
     ◆ OpenRouter ◆ GPT-5 router      │   ◆ RouteLLM       ◆ hand-written rules
     ◆ fixed frontier model           │   ◆ fixed cheap model
                                      │
                                      ▼
                       vendor-set tolerance, opaque or absent
```

**The white space is the upper-right quadrant: self-hosted pools, with a customer-set quality tolerance against a frontier measured on the customer's own traffic.** It is empty for a reason worth stating plainly rather than celebrating — the commercial incumbents cannot cross Axis 1 without abandoning their business model, and the self-hosted incumbents are free open-source projects with no commercial motive to build the measurement layer on Axis 2. CAMIR's quadrant is empty because it is hard to monetise, not because nobody thought of it.

---

## The three competitive facts that hurt most

Stated at full strength, because a teardown that only wounds the competition is marketing.

1. **Routing is already free for the largest segment.** A frontier vendor ships it at no separate charge [S20]. Any pitch that opens on "routing saves money" is answering a question the market has already been given a free answer to. CAMIR's opening line has to be about *whose* tolerance and *whose* measurement, not about routing.
2. **The algorithm plateaus.** 21 methods, 5 benchmarks, a narrow band far below oracle, with the best remedies worth ~2.13 percentage points [S4]. "Our router is smarter" is a claim a reviewer can falsify with one citation. The pack must not make it.
3. **The nearest business-model analogue died three months ago.** TensorZero — open-source LLMOps, $7.3M raised, 11,000 GitHub stars — archived its repository on 12 June 2026 and returned capital, citing the difficulty of finding product-market fit for an open-source project *and* a commercial product at once [S23]. In the same window ClickHouse absorbed Langfuse into a $15B data-infrastructure company [S23]. **This is the specific failure mode of CAMIR's chosen open-core shape, with a date on it.**

## Where CAMIR still wins, mechanically

1. **Self-hosters are structurally unserved and structurally unmonetised**, and they are growing — Ollama went from ~100K monthly downloads in Q1 2023 to **52 million in Q1 2026** [S30] (downloads, not deployments — direction only, see gap G1).
2. **The measurement problem is real, quantified, and unowned.** 65% of MMLU cases truncated under fixed generation budgets, 57% on MedQA, 5–12% parse failures [S5]; judges agree with each other only ~76% of the time and flip ~30% of verdicts at temperature 1 [S33][S34]. A meaningful share of the gap to the oracle ceiling is instrumentation error, and instrumentation is CAMIR's team's actual professional background.
3. **Prefill-activation routing is available only to whoever runs the weights** [S10] — the one signal advantage that no hosted competitor can copy, because copying it would require them to stop being hosted.
4. **Nobody's savings number is auditable.** Every commercial router computes the counterfactual it bills against. An open harness that a customer runs themselves is a different product from a dashboard, and it is the one a skeptical platform engineer would trust.

---

## Recommended next 3

1. **Position against "free and opaque", not against Martian.** The competitor that decides CAMIR's fate is the vendor-native router at zero marginal cost [S20], and the counter is the tolerance dial plus the auditable frontier, not a better classifier.
2. **Treat LiteLLM as a channel and vLLM as the clock.** Ship the routing strategy as a plugin into the proxy self-hosters already run [S22] rather than as a rival proxy, and assume serving-native routing [S11] commoditises the mechanism within 18 months — so build the measurement layer as the asset from day one.
3. **Write the TensorZero post-mortem into the plan before an investor does** [S23]. The open-core shape has a recent, well-documented death in this exact category; `strategy/market_type.md` and `financials/risk_matrix.md` must name it and say what CAMIR does differently, or the first partner meeting will.
