# Technique Wave 2 — Advanced and Theory-Grounded

**What this is** — the catalog of techniques that carry a guarantee, a decomposition, or an estimator behind them: conformal deferral, latent-ability routing, cost-aware bandits, Pareto-frontier construction, off-policy savings attribution, judge-protocol engineering, evaluation-artifact instrumentation, self-hosted cost-axis derivation, drift detection, and post-cache selection correction.
**Why it exists** — wave 1's techniques are all inside the routing plateau band [S4], so nothing there decides whether CAMIR is a company. The decisions this file informs are the two that do: *what does the escalation threshold guarantee* (cluster A) and *is the gap to the oracle ceiling capability or instrumentation* (clusters F–G). Without it, CAMIR ships a τ with no coverage claim and a savings number its own buyer cannot audit — the failure `../../strategy/value_prop_canvas.md` ranks #1 for Dana.
**How to read it** — clusters **F, G and H** are the ones no competitor publishes and where CAMIR's founder edge is real; read those first. A skeptic should attack cluster **E**: off-policy savings attribution is where a share-of-savings price becomes contestable, and [S38] says the definition of eligible savings is exactly where FinOps negotiations actually happen.
**Depends on / feeds** — depends on [wave1.md](wave1.md), [../../research/survey.md](../../research/survey.md) §5–§8, [../../research/capability_table.md](../../research/capability_table.md) C8–C11; feeds [wave3.md](wave3.md), [decision_tree.md](decision_tree.md), [technique_feature_matrix.md](technique_feature_matrix.md), [../deep_dives.md](../deep_dives.md), [../not_vaporware.md](../not_vaporware.md).

---

## Scope and counting rule

**50 techniques in 10 clusters** — the wave is full, and it is full honestly: clusters F, G and H alone could have carried more entries had they been split finer, and several plausible candidates were cut as duplicates of listed entries (Bayesian model averaging over judges → folded into W2-30; "confidence-interval overlap testing on the frontier" → W2-21).

Evidence anchors: `[Sn]` resolves to [../../research/sources.md](../../research/sources.md); a bare author/method name means the technique is standard in its own field and named there; `(assumption)` means CAMIR is reasoning, not citing.

| Cluster | # | Techniques |
|---|---|---|
| A | 6 | Conformal prediction and distribution-free deferral guarantees |
| B | 5 | Latent-ability and psychometric routing |
| C | 5 | Bandits and cost-aware online routing |
| D | 5 | Multi-objective frontier construction |
| E | 5 | Off-policy and counterfactual evaluation — savings attribution |
| F | 7 | Judge-protocol engineering |
| G | 5 | Evaluation-artifact instrumentation |
| H | 5 | Cost-axis derivation for self-hosted pools |
| I | 4 | Drift detection and recalibration triggers |
| J | 3 | Adverse-selection correction for post-cache traffic |
| | **50** | |

---

## A. Conformal prediction and distribution-free deferral guarantees (6)

Wave 1's threshold τ is a number an operator picks. Conformal turns it into a number an operator can *promise*: escalate enough to hold measured error at or below a declared level, with a finite-sample guarantee that needs no assumption about the model.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-1 | **Split conformal prediction** | Calibrate a nonconformity score on a held-out split; the resulting threshold guarantees marginal coverage ≥ 1 − α under exchangeability, with no distributional assumption. | Vovk et al.; Lei–Wasserman |
| W2-2 | **Conformal risk control** | Extends the guarantee from coverage to any monotone risk — including "fraction of requests answered below the quality tolerance", which is the risk a CAMIR operator actually declares. | Angelopoulos et al., conformal risk control |
| W2-3 | **Conformal deferral rule** | Escalate iff the small tier's conformal prediction set contains more than one admissible answer; makes the escalation decision a set-size test rather than a scalar threshold. | Composition of W2-1 with W1-3; `(assumption: not published for LLM tier routing)` |
| W2-4 | **Mondrian / group-conditional conformal** | Calibrate separately per endpoint or per task class, so coverage holds *for Ravi's endpoint* rather than only on average — the technical form of per-endpoint tolerance ownership. | Mondrian conformal prediction (Vovk); `../../strategy/value_prop_canvas.md` PR-R1 |
| W2-5 | **Adaptive conformal inference (ACI)** | Update α online from realised coverage errors, retaining validity when traffic drifts — the mechanism that keeps a tolerance honest between recalibrations. | Gibbs–Candès (2021) |
| W2-6 | **Weighted conformal under covariate shift** | Reweight calibration points by a likelihood ratio when the deployment distribution differs from the calibration corpus — the correction a benchmark-calibrated router needs on real traffic. | Tibshirani et al. (2019) |

**Why this cluster is first.** [S21] documents users revolting when routing shipped with a vendor-chosen tolerance and no dial. A conformal guarantee is what makes a customer-set dial mean something rather than being a slider with a vibe attached.

---

## B. Latent-ability and psychometric routing (5)

The plateau study explicitly enumerates latent-factor / IRT routers among the 21 methods that converge [S4] — so this cluster is catalogued as **measurement structure**, not as a route to accuracy.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-7 | **Item response theory (2PL)** | Model each request as an item with difficulty and discrimination parameters and each tier as an ability; routing becomes a comparison of ability against difficulty. | [S4] (IRT family); Birnbaum 2PL |
| W2-8 | **Matrix factorisation over the correctness matrix** | Low-rank factorisation of `request × tier → correct?`, distinct from W1-25 which factorises *preference*; correctness is what CAMIR's frontier is built on. | [S1][S7] |
| W2-9 | **Low-rank completion for unobserved pairs** | Impute outcomes for request-tier pairs never run, so an oracle ceiling can be estimated without an exhaustive N×M sweep on production traffic. | [S7]; `(assumption: introduces imputation error into the ceiling and must be reported as a band)` |
| W2-10 | **Rasch difficulty calibration for corpus design** | Select benchmark items to span the difficulty range uniformly rather than sampling whatever the dataset ships — the fix for a corpus whose "mixed difficulty" is actually clustered. | Rasch model; W1-47 |
| W2-11 | **Co-failure structure estimation** | Factor-analyse the *overlap* in errors across tiers; models fail together, which bounds any combination strategy including routing. | 67-frontier-model co-failure ceiling [S15] |

**The finding this cluster forces into the open.** The oracle ceiling is lower than an independence assumption predicts [S15], **and** partly an artifact [S5]. Both corrections must be applied to the same number before anyone quotes it.

---

## C. Bandits and cost-aware online routing (5)

Declared out of scope for year one (`../../BRIEF.md` §Wedge names no RL); catalogued because the control plane's per-deployment training is where these arrive in year two.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-12 | **Contextual bandit (LinUCB)** | Treat tier choice as an arm, prompt embedding as context, and cost-adjusted correctness as reward; explores where the classifier is uncertain rather than where it is wrong. | [S4] (bandit family); Li et al. LinUCB |
| W2-13 | **Thompson sampling over per-tier success** | Sample from posterior success probabilities per context cluster; cheaper to implement than LinUCB and naturally handles the cold start a new deployment has. | Thompson sampling |
| W2-14 | **Bandits with knapsacks** | Enforce a hard spend constraint over a billing period rather than a per-request threshold — the formulation that matches how a budget is actually owned. | Badanidiyuru et al., BwK |
| W2-15 | **Lagrangian cost-constrained policy** | Attach a shadow price λ to the budget constraint; λ *is* the exchange rate between a quality point and a dollar, and is the number the frontier chart implicitly reports. | Constrained optimisation; connects to W2-19 |
| W2-16 | **Constrained MDP for cascade escalation** | Model multi-stage escalation as a CMDP with quality reward and cost constraint, yielding stage-wise thresholds jointly rather than tier-by-tier. | CMDP (Altman); generalises W1-5 |

---

## D. Multi-objective frontier construction (5)

CAMIR's unit of value is a curve, so the curve needs a construction method that survives review.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-17 | **Pareto-front extraction** | Keep only operating points not dominated on both cost and quality; everything else is noise on the chart. | RouterBench cost–quality space [S7] |
| W2-18 | **Convex-hull interpolation between operating points** | Randomising between two operating points reaches any point on the segment joining them, so the achievable frontier is the upper convex hull, not the raw point cloud. | RouterBench's interpolation construction [S7] |
| W2-19 | **Exchange-rate scalarisation** | Report "dollars per quality point" at the operator's chosen tolerance instead of a single savings percentage — makes two routers comparable when their curves cross. | Scalarisation; W2-15 |
| W2-20 | **Hypervolume indicator** | A single scalar summarising a whole frontier against a reference point, so router A vs router B is answerable without picking a tolerance first. | Zitzler–Thiele hypervolume |
| W2-21 | **Bootstrap confidence bands on the frontier** | Resample requests to put an interval around the whole curve. Non-optional: the plateau's best remedies moved accuracy by **up to 2.13 points** [S4], which is inside most unreported error bars. | [S4]; W1-48 |

---

## E. Off-policy and counterfactual evaluation — savings attribution (5)

This cluster is what makes the paid control plane's savings-attribution feature defensible, and it is where the pricing model lives (`../../BRIEF.md` §Business model; assumption A5).

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-22 | **Counterfactual baseline replay** | Re-cost every logged request at the fixed-model baseline's price and quality, producing "what you would have spent" from the customer's own logs rather than the vendor's assertion. | `../../strategy/value_prop_canvas.md` PR2/PR-D1; FinOps precedent [S38][S39] |
| W2-23 | **Inverse propensity scoring** | Reweight logged outcomes by the probability the logging policy chose that tier, giving an unbiased estimate of a *different* routing policy's cost — how a new τ is evaluated without deploying it. | IPS / Horvitz–Thompson |
| W2-24 | **Self-normalised IPS** | Divide by the summed weights to control the variance blow-up that makes raw IPS useless when the new policy diverges from the logged one. | Swaminathan–Joachims SNIPS |
| W2-25 | **Doubly robust estimator** | Combine a learned outcome model with IPS so the estimate stays consistent if *either* is right — the estimator to publish, because a single-model estimate is the one a buyer's analyst will attack. | Dudík et al., doubly robust OPE |
| W2-26 | **Interleaved / switchback assignment** | Alternate routing policy by time block or by request hash on shared infrastructure, where a user-level split is impossible because six product teams share one endpoint. | Switchback experiments; `../../strategy/value_prop_canvas.md` §Marcus J2 |

**The honest limit.** Counterfactual savings are computable but disputable (A5). [S38] records that share-of-savings vendors publish **no universal rate** and that *the definition of eligible savings is negotiated per customer* — so the estimator choice above is a commercial term, not only a statistical one.

---

## F. Judge-protocol engineering (7)

The quality axis of every frontier is a judgment, and in 2026 that judgment was systematically audited and found wanting. **No competitor publishes a protocol.** This is the cluster where CAMIR's stated founder edge is operational rather than rhetorical.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-27 | **Temperature-0 judging with test-retest measurement** | Judge deterministically and re-run a sample to publish the same-verdict rate: **>95% at temperature 0, ~70% at temperature 1** [S34]. | [S34] |
| W2-28 | **Position-permutation averaging** | Score every pair in both orders and average, removing position bias that produces **~40% GPT-4 inconsistency** [S34]. | [S34] |
| W2-29 | **Length-controlled scoring** | Regress out or cap response length before scoring, countering **~15% verbosity inflation** [S34] — the same bias [S5] identifies as inflating apparent small-tier incapability. | [S34][S5] |
| W2-30 | **Multi-judge panels with aggregation** | Two or more judges from different families, verdicts combined by majority or by a Bayesian aggregate weighted on anchor-set accuracy. Required because judges are individually consistent yet mutually inconsistent [S33]. | [S33] |
| W2-31 | **Inter-judge agreement statistics** | Report Cohen's κ / Fleiss' κ / Krippendorff's α alongside the frontier. Baseline to beat: **~76% inter-judge agreement**, against ~80% judge-human agreement [S33]. **A frontier published without this is not reproducible.** | [S33] |
| W2-32 | **Human anchor-set calibration** | Hand-label a few hundred items and report each judge's accuracy against it, so judge choice is an evidenced decision rather than a default. A 2026 RAND study cited in [S33] finds no judge uniformly reliable across benchmarks. | [S33] |
| W2-33 | **Self-enhancement-bias control** | Never let a judge score answers from its own model family; **5–7% self-enhancement bias** [S34], which in a routing benchmark maps directly onto tier preference. | [S34] |

---

## G. Evaluation-artifact instrumentation (5)

[S5]'s finding — across **206,000 query-model pairs** — that much of measured unsolvability is instrumentation, not capability, is the central insight of this pack. These five techniques are how it is exploited.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-34 | **Finish-reason logging and truncation counting** | Record why each generation stopped and count length-stops; **65% of MMLU and 57% of MedQA cases** were truncated under fixed budgets [S5]. Costs one log field and invalidates a ceiling if omitted. | [S5] |
| W2-35 | **Generation-budget sensitivity sweep** | Re-run the corpus at 2×, 4×, 8× `max_tokens` and plot accuracy against budget; the flat region is where the ceiling is real and the rising region is where it was manufactured. | [S5]; W1-36 |
| W2-36 | **Parse-failure taxonomy and counting** | Classify extraction failures (wrong format, refusal, prefix chatter, no answer) and report the rate; **5–12% parse failures on MMLU** [S5]. Format failures scored as wrong answers are a routing signal made of nothing. | [S5] |
| W2-37 | **Artifact-vs-capability decomposition of the oracle gap** | Split the measured gap between the deployed router and the oracle ceiling into (a) genuine unpredictability, near its ceiling per [S4], and (b) instrumentation error, large and fixable per [S5]. **Nobody has published this decomposition on a self-hosted pool.** | [S4][S5]; survey §6.3 |
| W2-38 | **Re-scored ceiling after artifact remediation** | Recompute the oracle ceiling with W2-34 to W2-36 applied and publish both numbers — the before/after that turns a measurement claim into a demonstrated result. | [S5]; `(assumption: the size of the shift on a self-hosted pool is unmeasured — this is the experiment, not the finding)` |

---

## H. Cost-axis derivation for self-hosted pools (5)

Open question #1 in [../../research/survey.md](../../research/survey.md) §8: every published frontier prices against hosted list prices [S7], so **the cost axis for self-hosted pools does not exist yet**. It is the most concrete unclaimed contribution available.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-39 | **Amortised GPU-hour to per-token costing** | Cost = (GPU hourly rate × wall-clock) ÷ tokens served, under a *declared* utilisation assumption. Reference points: **~$0.10 per million tokens** raw on a batched H100 for gpt-oss-120b, **below $0.05** on B200 at high batch [S26]. | [S26] |
| W2-40 | **Utilisation-sensitivity sweep** | Report the whole frontier at several utilisation levels, because **a GPU idle at 10% utilisation costs 10× per token** [S27] — utilisation moves the cost axis more than routing does. | [S27] |
| W2-41 | **All-in cost multiplier** | Apply the **3–5× raw-rental multiplier** [S27] for engineering time before quoting any saving; a saving quoted against raw GPU cost overstates by that factor. | [S27] |
| W2-42 | **Per-tier resident-VRAM accounting** | Charge each tier for the memory it holds resident, not for the tokens it emits. CAMIR's tiers are **distinct base models** that do not share weights, so the multi-LoRA consolidation figures [S31] do **not** apply. | [S31][S32]; capability table C2 |
| W2-43 | **Marginal-cost accounting for escalation** | Price an escalation at the *marginal* extra work — the small tier's prefill and decode are already spent — rather than as the sum of two full requests; the arithmetic that decides whether the cascade route survives compressed tier spreads [G2]. | `(assumption: no published cascade cost model for self-hosted pools — gap G2)` |

---

## I. Drift detection and recalibration triggers (4)

Marcus's pain Pn3: static policies decay silently, and his March A/B test is two model upgrades stale.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-44 | **Distribution shift tests on prompt embeddings** | Population stability index or a two-sample KS/MMD test between the calibration corpus and live traffic; fires when the *input* has moved. | Standard drift monitoring; `../../strategy/value_prop_canvas.md` Pn3 |
| W2-45 | **Change detection on escalation rate** | ADWIN or Page–Hinkley on the escalation-rate stream; fires when the *router's own behaviour* has moved, which is cheaper to observe than quality. | ADWIN (Bifet–Gavaldà); Page–Hinkley |
| W2-46 | **Prequential accuracy tracking** | Score the router's decisions online against whatever ground truth arrives late (verifications, user signals), giving a running quality estimate between full re-measurements. | Prequential evaluation (Dawid) |
| W2-47 | **Version-pinned recalibration triggers** | Any change to a tier's weights, quantisation, serving engine or sampling parameters invalidates the calibration and forces a re-run; the trigger is a config diff, not a schedule. | `(assumption: operational policy; no published guidance)` |

---

## J. Adverse-selection correction for post-cache traffic (3)

Gap **G4**: nobody has measured what routing saves *after* aggressive caching. Cache hits are disproportionately the repetitive easy requests routing would have sent small, so post-cache traffic is adversely selected toward the hard end.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W2-48 | **Cache-miss-conditioned frontier estimation** | Build the frontier only on traffic that actually reaches the router, with the cache in front, rather than on the raw request stream. Hit rates of **20–45%** in production [S36] make this a first-order correction. | [S36][G4] |
| W2-49 | **Difficulty-stratum reweighting** | Reweight the benchmark's strata (W1-47) to match the observed post-cache difficulty mix, so a public-benchmark frontier transfers to a cached deployment. | [S36]; `(assumption: reweighting scheme untested)` |
| W2-50 | **Selection-model correction** | Model cache-hit probability explicitly and correct the routing-savings estimate for non-random selection into the router's input stream. | Heckman-type selection correction; `(assumption: not applied to LLM caching in any published work)` |

---

## Recommended next 3

1. **Build cluster G before cluster A.** Instrumentation (W2-34 to W2-36) is three log fields and a sweep; it determines whether the oracle ceiling every other artifact quotes is real. A conformal guarantee (W2-1) placed on top of a truncation-contaminated corpus guarantees the wrong number precisely.
2. **Publish W2-37 — the artifact-vs-capability decomposition — as a standalone result.** [S4] and [S5] appeared within roughly a month of each other pointing opposite ways and nobody has run the decomposition on a self-hosted pool. It is citable whether or not CAMIR's router beats anything, which makes it the highest expected-value item in this file.
3. **Settle W2-25 and W2-22 before quoting a price.** Share-of-savings pricing (A5) is only as strong as the counterfactual estimator behind it, and [S38] says the eligible-savings definition is where the negotiation lands. Choose the estimator now, in public, or lose the argument in a procurement call later.

<!-- critic: unresolved — none outstanding after round 2. -->
