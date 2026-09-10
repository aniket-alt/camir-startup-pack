# CAMIR — Product Requirements Document

**Version 0.1 · 2026-09-09 · status: pre-build.** No component described here has been built or measured. Every number carries `[Sn]` resolving to [../research/sources.md](../research/sources.md) or an explicit `(assumption)` tag.

**What this is** — the specification of CAMIR as a system: the closed loop it runs, the ten first-principles it obeys, the feature superset organised by loop phase, the durable records it writes, and the metrics that decide whether it worked.
**Why it exists** — the tempting build order is classifier accuracy first, because that is the interesting engineering. [S4] shows 21 routing methods converge into a narrow band far below the oracle and the best remedies bought ~2.13 percentage points, so that order spends year one on the field's least movable variable and ships a router no product engineer will let near their endpoint. This document fixes the opposite order — measurement and tolerance ownership first, routing accuracy as an ablation — and names the non-goals that keep it fixed.
**How to read it** — §3 (principles) then §5 (core feature set); a skeptic should attack §3's mapping rule, because if a P0 feature maps to no principle the principle list was wrong or the feature is decoration. §4's non-goals are where the scope is actually held.
**Depends on / feeds** — depends on [../BRIEF.md](../BRIEF.md), [../ASSUMPTIONS.md](../ASSUMPTIONS.md), [../research/survey.md](../research/survey.md), [../research/capability_table.md](../research/capability_table.md), [../strategy/personas.md](../strategy/personas.md), [../strategy/value_prop_canvas.md](../strategy/value_prop_canvas.md); feeds [features_flagship.md](features_flagship.md), [features_prioritized.md](features_prioritized.md), [journeys/](journeys/) and [ux_spec.md](ux_spec.md).

---

## 1. Executive summary and the core loop

CAMIR is a cost-aware inference router for platform teams running LLM features at volume on a **self-hosted model pool**. It sends each request to the smallest **tier** predicted to answer it correctly, via a **classifier route** (predict difficulty pre-generation, dispatch once) or a **cascade route** (try small first, escalate on low confidence). Its output is not a saving; it is a measured **cost-quality frontier** against which the customer sets a **quality tolerance** and picks the point.

**The claim, stated precisely.** Not "our router is more accurate" — [S4] makes that falsifiable in one citation. The claim is: **your frontier is measurable on your pool, and a meaningful part of the apparent ceiling is your harness** [S5]. The router is the object under measurement; the measurement layer is the asset, because the routing mechanism is being absorbed into the serving engine [S11].

### The core loop

**Classify → Dispatch → Judge → Attribute → Recalibrate**

| Phase | What happens | Component | Durable record written |
|---|---|---|---|
| **Classify** | Estimate difficulty pre-generation (classifier route) or accept the request unscored (cascade route); read the endpoint's tolerance and pin state | difficulty classifier, tolerance policy engine, pin registry | `decision_record` (opened) |
| **Dispatch** | Send to the chosen tier; on the cascade route, score the small-tier answer at the confidence gate and resolve or escalate | dispatcher, confidence gate, model pool | `decision_record` (tiers attempted, tokens, GPU-seconds, escalated y/n) |
| **Judge** | Score correctness under the forced protocol — temperature 0, fixed answer position or averaged permutations, verbosity control, two or more judges, truncation and parse guards | judge harness, artifact guard | `judgment_record` (per-judge verdict, inter-judge agreement, truncation flag, parse status) |
| **Attribute** | Price the request on the self-hosted cost axis, compute the fixed-model-baseline counterfactual, stamp the tier decision onto the caller's trace | cost meter, savings attributor, trace stamper | `savings_ledger` row; OpenTelemetry span attributes on the caller's own span |
| **Recalibrate** | Detect frontier drift from model upgrades or traffic shift; retrain the classifier, re-fit the confidence threshold, re-plot the frontier, re-publish the ceiling | drift monitor, recalibration scheduler, frontier builder | `frontier_run` (dated, corpus-hashed, pool-manifest-pinned) |

The loop closes because `judgment_record` labels are the classifier's training data and the confidence gate's calibration set. That is the compounding asset, and it is **per-deployment, not network-wide** — `../BRIEF.md` §Moat says so and this PRD does not improve on it.

---

## 2. Target users and the config surface

One system, six people. From [../strategy/personas.md](../strategy/personas.md); the router binary is identical for all of them and what varies is how much configuration is exposed and **who owns the tolerance**.

| Persona | Relationship to CAMIR | What the config surface exposes | The one requirement they impose |
|---|---|---|---|
| **P1 Priya** — drop-in, edge-low, self-hosts for data residency not price [S28] | Points a base URL at the ingress proxy | Defaults only: one tolerance, one pool manifest, shadow mode on by default | Defaults that cannot silently hurt her |
| **P2 Marcus** — beachhead, staff platform engineer, ~$50k/month | Runs the harness on his own logged traffic inside his perimeter | Tolerance per endpoint, corpus sampling, cost-axis parameters | He picks the point on the curve; CAMIR shows the exchange rate |
| **P3 Wen** — edge-high, own judge, five tiers | Replaces the judge and the tier registry, keeps the harness | Every component is an interface: judge, tier, cost model, classifier, gate | Published inter-judge agreement, or the frontier is noise [S33] |
| **P4 Dana** — economic buyer, never evaluates | Reads one report | Nothing. A single savings report with a named signer | The verifier and the vendor must be different parties |
| **P5 Ravi** — consumes the shared service, bears the risk, **can veto** | Sets his endpoint's tolerance; reads tier decisions in his own traces | Per-endpoint tolerance, pin flag, per-endpoint shadow report | Attribution within the hour, and an off switch he owns |
| **P6 Sam** — OSS adopter below the volume floor [S27] | Clones the repo, runs it on one GPU | Everything open: router, harness, frontier builder | A licence that stays put; no feature ever moves open → paid |

**Explicitly not separate products.** There is no "lite" router and no "enterprise" router. Priya and Wen run the same dispatcher; Priya never opens the tier registry. The only asymmetry that is real is **ownership of the tolerance**, and that is a permission, not a build.

---

## 3. First-principles grounding (non-negotiable)

Ten principles, drawn from [../research/survey.md](../research/survey.md). **Rule: every major feature maps to at least one principle. A feature mapping to none gets cut, or the principle list was wrong.** The mapping is enforced in [features_flagship.md](features_flagship.md) and in the `Principle` column of [features_prioritized.md](features_prioritized.md).

| # | Principle | Statement | Source | What it forces in the build |
|---|---|---|---|---|
| **PR1** | **Cascade cost is set by first-stage resolution, not last-stage accuracy** | In a detection cascade, total cost is dominated by how often stage one resolves confidently. A first stage that rarely resolves makes the cascade **more** expensive than going straight to the large tier | survey §2.1 | Escalation rate is a first-class reported metric, not a debug counter; the cascade's cost decomposition (failed small attempt + gate cost + large answer) is shown before deployment |
| **PR2** | **Deferral quality dominates model quality** | In learning-to-defer, the rule deciding when to hand off matters more than either predictor, because it sets the whole cost/coverage trade-off | survey §2.2, [S8] | The gate and its threshold are configurable, versioned, auditable objects — not a constant compiled into the router |
| **PR3** | **Calibrated confidence is the baseline to beat** | Simple confidence measures — max softmax probability, margin, predictive entropy — route as well as trained routing models | [S8] | The classifier route is reported as an **ablation against calibrated confidence**, never against the fixed-model baseline. Beating a fixed model is table stakes |
| **PR4** | **A budget is a declared tolerance against a named reference** | Anytime and budgeted inference supply the notion: acceptable loss is declared in advance, versus an unconstrained reference | [S14] | Tolerance is a stored, owned, versioned policy object with an owner field — not a slider in a dashboard |
| **PR5** | **Pre-generation difficulty prediction is near its ceiling — the predictability bottleneck** | 21 routing methods across 5 benchmarks converge into a narrow band far below the oracle; routers learn globally averaged model-performance trends, not query-specific signal. Best remedies gained **up to 2.13 percentage points** | [S4] | CAMIR never claims classifier superiority. The classifier route is scoped as an ablation, and the only credible escape on the roadmap is prefill activations [S10] — the one signal hosted routers structurally cannot see |
| **PR6** | **A substantial share of measured "the small tier cannot do this" is instrumentation, not capability** | Across 206,000 query-model pairs: truncation under fixed generation budgets in **65% of MMLU and 57% of MedQA** cases; **5–12% parse failures on MMLU**; judge bias toward verbosity over correctness | [S5] | Generous generation budgets with truncation logging, strict output parsing with failure counts, and a ceiling reported **twice** — with and without artifacts — from the first run |
| **PR7** | **The oracle ceiling bounds everything, and models co-fail** | Everything a router could claim lives between the fixed-model baseline and the oracle ceiling; across 67 frontier models failures overlap, so the ceiling is lower than independence predicts | survey §5.2, [S15] | The ceiling is measured **first**, before any router is deployed, and a low ceiling produces a disqualification, not a smaller pitch |
| **PR8** | **The quality axis is a judgment, and judges disagree with each other** | ~80% agreement with humans but only **~76% inter-judge agreement**; test-retest **above 95% at temperature 0, ~70% at temperature 1**; position bias ~40%, verbosity ~15%, self-enhancement 5–7% | [S33][S34] | Forced protocol: temperature 0, fixed answer position or averaged permutations, verbosity control, exact-match wherever the task admits it, two or more judges, and **inter-judge agreement published alongside every frontier** |
| **PR9** | **Models are points; routers are curves** | The evaluation frame is a two-dimensional cost–quality space. A router is not a number, it is a trade-off curve, and the customer picks the point | [S7] | The unit of output is a `frontier_run`, not a savings percentage. **And the cost axis must be re-derived**: RouterBench prices against hosted API list prices [S7]; a self-hosted pool costs amortised GPU-hours per token [S26][S27] — an unclaimed contribution |
| **PR10** | **Tiers are not strictly nested** | A cheap model sometimes answers correctly where an expensive one fails, which is why an oracle ceiling can exceed the large model's own score | [S3] | Routing is a per-request assignment problem, not a threshold on one competence ordering. The non-nested set is reported explicitly, because it is the evidence that routing is more than downgrading |

**Two principles deliberately absent.** Latency is not a principle here — [S12] establishes it as a real third axis and `../BRIEF.md` declares it a year-one non-goal. Caching is not a principle here — [S36] puts production hit rates at 20–45%, it composes upstream and takes the easy traffic first; CAMIR prices only the residual and says so, gap [G4].

---

## 4. Goals and non-goals

### Goals, in priority order

| # | Goal | Done when |
|---|---|---|
| G1 | A team measures the oracle ceiling on **their own traffic** in under a week of wall-clock time on their own hardware | A `frontier_run` produced from a replay corpus of their logged requests, ceiling reported with and without evaluation artifacts (PR6) |
| G2 | Routing deploys in a shared-infrastructure organisation **without the consuming engineer blocking it** | Per-endpoint tolerance owned by the consuming team, tier decision on every trace, pin-to-large in one flag, shadow mode before any enforcement |
| G3 | A saving claim is **auditable by a party that is not CAMIR** | The counterfactual is computed by open-source code running inside the customer's perimeter, from records the customer holds |
| G4 | The cascade route works when the classifier route does not | Cascade ships as primary; classifier ships as an ablation reported against calibrated confidence (PR3, PR5) |
| G5 | A frontier produced by CAMIR is reproducible by a stranger | `frontier_run` pins corpus hash, pool manifest, cost-axis parameters, judge set and inter-judge agreement [S13] |

### Non-goals — real renunciations

Each is something CAMIR could plausibly build, will be asked for, and refuses in year one. From `../BRIEF.md` §Wedge.

| # | Non-goal | Why refused | What we say when asked |
|---|---|---|---|
| N1 | **No fine-tuning** | Fine-tuning the small tier raises the oracle ceiling and confounds the measurement CAMIR exists to produce; it also makes tiers non-comparable across runs | "We measure your pool. Changing the pool is your call, and we re-measure after you do." |
| N2 | **No model hosting** | Hosting puts CAMIR on the customer's uptime critical path and collapses G3 — the vendor would again be the party computing the bill | "The weights stay yours. We never see a token you don't send us." |
| N3 | **No caching layer** | Caching is upstream and orthogonal [S36], and building it would let CAMIR bank cache savings as routing savings — the exact incentive problem this pack accuses incumbents of [S17] | "Put your cache in front of us. We report the residual only." |
| N4 | **No multi-tenant SaaS** | The beachhead self-hosts for data residency, not price [S28]. Multi-tenancy means customer prompts leave the perimeter, which disqualifies P1 outright | "The control plane runs in your VPC. It reads records, not prompts." |
| N5 | **No agentic or multi-turn routing** | The decision unit is one request. Multi-turn needs state, a different cost model and a different evaluation frame; [S6] scopes the field the same way | "One prompt in, one tier decision out. Multi-turn is a different product." |
| N6 | **No latency-SLO management** | A third axis [S12] triples the frontier's dimensionality before the two-dimensional one has been measured once | "We report latency. We do not promise it." |
| N7 | **No claim of router-accuracy superiority** | The strongest renunciation and the least comfortable. [S4] would falsify it in one citation | "We do not claim a better router. We claim your frontier is measurable and part of your ceiling is your harness." |

**What N7 costs.** It removes the demo every competitor gives. It is kept because a claim an investor can falsify in one search is worse than no claim, and because the measurement contribution [S13][G2] survives even if the router plateaus.

---

## 5. Core feature set, by loop phase

The superset. Ranking, dependencies and effort live in [features_prioritized.md](features_prioritized.md); the 20 highest-leverage are expanded in [features_flagship.md](features_flagship.md).

### 5.0 Pre-loop — Qualify

| Feature | Mechanism | Principles |
|---|---|---|
| **Oracle ceiling probe** | Run every request in the replay corpus through every tier; label per request which tiers answered correctly; the ceiling is the score of perfect foreknowledge | PR7, PR10 |
| **Disqualification report** | If the gap between ceiling and fixed-model baseline is smaller than the deployment's own operating cost, CAMIR emits *do not deploy*, with the histogram behind it | PR7 |
| **Non-nested tier report** | The set of requests where a smaller tier was right and a larger one wrong — evidence that routing is assignment, not downgrading | PR10 |
| **Replay corpus builder** | Stratified sample of the customer's logged requests by endpoint and length band, hashed and pinned | PR9 |
| **Pool manifest / tier registry** | Declared tiers with weights, hardware, resident VRAM and a utilisation assumption. Distinct base models do **not** share weights the way multi-LoRA adapters do [S31], so the manifest states resident cost per tier | PR9 |

### 5.1 Classify

| Feature | Mechanism | Principles |
|---|---|---|
| **Difficulty classifier** | Prompt-feature model predicting which tier resolves the request; trained per deployment on `judgment_record` labels | PR5 |
| **Calibrated-confidence baseline** | Max-softmax / margin / predictive-entropy scorer, always run alongside the classifier as the comparison arm | PR3 |
| **Tolerance policy engine** | Per-endpoint declared acceptable quality drop versus the fixed-model baseline, with an owner field and a version | PR4 |
| **Pin registry** | Per-endpoint pin-to-tier, honoured before any classification, effective on the next request | PR4, PR2 |

### 5.2 Dispatch

| Feature | Mechanism | Principles |
|---|---|---|
| **Cascade route** (primary) | Small tier first; the confidence gate resolves or escalates | PR1, PR2 |
| **Confidence gate** | Calibrated uncertainty threshold fitted on held-out `judgment_record` labels; the threshold is a versioned object | PR2, PR3 |
| **Classifier route** (ablation) | Single dispatch to the predicted tier; reported against the confidence baseline, never against a fixed model | PR3, PR5 |
| **Escalation-rate accounting** | Cascade cost decomposed into failed small attempt + gate cost + large answer, shown before deployment | PR1 |
| **Ingress proxy** | OpenAI-compatible endpoint; the drop-in surface for P1 | PR9 |

### 5.3 Judge

| Feature | Mechanism | Principles |
|---|---|---|
| **Judge harness** | Temperature 0; fixed answer position or averaged permutations; verbosity control; two or more judges; exact-match or programmatic verification preferred wherever the task admits it | PR8 |
| **Inter-judge agreement reporter** | Agreement statistic computed and attached to every `frontier_run`; a frontier without it is marked *not reproducible* | PR8 |
| **Artifact guard** | Generous generation budgets with truncation logging; strict output-format parsing with failure counts; ceiling recomputed with and without artifacts | PR6 |
| **Judge interface** | Wen's own judge plugs in behind the same interface; her agreement statistics are computed the same way | PR8 |

### 5.4 Attribute

| Feature | Mechanism | Principles |
|---|---|---|
| **Self-hosted cost meter** | Cost per request = GPU-seconds × amortised hourly rate ÷ utilisation, with the all-in multiplier (3–5× raw rental [S27]) exposed as a declared parameter rather than hidden | PR9 |
| **Trace stamper** | Tier decision, route type, confidence, escalation flag and policy version written as span attributes on the **caller's own** trace | PR2, PR4 |
| **Counterfactual savings ledger** | Per request: what the fixed-model baseline would have cost, what was actually spent, and the quality delta | PR9 |
| **Frontier builder** | Plots models as points and routes as curves in the cost-quality plane | PR9 |
| **Tolerance breach alert** | Measured quality on an endpoint crosses its declared tolerance → alert to the tolerance owner, with an auto-revert option | PR4 |
| **Savings report** | One page: baseline cost, cost at declared tolerance, measured quality delta, judge agreement, and who signed off | PR9 |

### 5.5 Recalibrate

| Feature | Mechanism | Principles |
|---|---|---|
| **Drift monitor** | Watches escalation rate, classifier calibration error and per-endpoint quality against the frontier the policy was set on | PR5 |
| **Recalibration scheduler** | Re-runs the corpus on pool-manifest change or drift trigger and produces a new dated `frontier_run` | PR5, PR9 |
| **Frontier diff** | Old curve versus new, with the tolerance point projected onto both, so the owner sees whether their chosen point moved | PR4, PR9 |
| **Label recycler** | Promotes production `judgment_record` rows into the classifier's training set and the gate's calibration set | PR2, PR3 |

**Paid control plane (the open-core boundary).** Open forever: ingress proxy, both routes, confidence gate, judge harness, artifact guard, frontier builder, cost meter, replay corpus builder, oracle ceiling probe. Paid: per-deployment classifier training and gate fitting, tolerance policy management across endpoints and owners, savings attribution and the signed report, and routing observability. **No feature ever moves from open to paid** — the commitment [../strategy/personas.md](../strategy/personas.md) P6 requires and the TensorZero post-mortem [S23] explains.

---

## 6. Data and the learning flywheel

### What is written, per request

| Record | Written by | Contents | Retention |
|---|---|---|---|
| `decision_record` | router + dispatcher | request id, endpoint, route type, predicted tier, tiers attempted in order, confidence score, escalated y/n, tokens in/out, GPU-seconds per tier, latency, pin state, tolerance policy version, pool manifest hash | customer-controlled; prompt text optional and off by default |
| `judgment_record` | judge harness | request id, tier, judge ids, per-judge verdict, inter-judge agreement, truncation flag, parse status, verbosity-normalised score | run-scoped |
| `savings_ledger` | savings attributor | endpoint, day, baseline counterfactual cost, actual cost, quality delta, cost-axis parameters used | permanent |
| `tolerance_policy` | tolerance policy engine | endpoint, declared tolerance, **owner**, version, effective from/to, and the `frontier_run` it was set against | permanent, append-only |
| `frontier_run` | frontier builder | corpus hash, pool manifest, cost-axis parameters, judge set, inter-judge agreement, oracle ceiling with and without artifacts, curve points | permanent |
| `pool_manifest` | tier registry | tiers, weights, hardware, resident VRAM, utilisation assumption | versioned |

### How it compounds — and the honest limit

`judgment_record` labels train the classifier and calibrate the gate. A deployment that has judged a million of its own requests routes that customer's traffic better than a cold start, and the `tolerance_policy` history is expensive to recreate because it encodes decisions people signed. **This is per-deployment, not network-wide.** CAMIR cannot pool labels across customers without moving prompts out of the perimeter, which N4 forbids. `../ASSUMPTIONS.md` A6 records that rising switching cost is untested; nothing in this PRD makes it less untested.

**What a funded copycat lacks after two years:** the accumulated per-customer routing history and the calibration living in it. Not the algorithm. `../BRIEF.md` says so and the PRD does not upgrade it.

---

## 7. Oversight, safety, privacy, compliance

| # | Requirement | Mechanism |
|---|---|---|
| O1 | **Prompt text never leaves the perimeter** | The control plane reads `decision_record` and `judgment_record` metadata. Prompt storage is opt-in and off by default. Hard requirement, because P1 self-hosts for data residency, not price [S28] |
| O2 | **Every routed request is attributable** | Tier decision stamped on the caller's own trace, not only in CAMIR's store. Remove CAMIR and the historical attribution survives in the customer's telemetry |
| O3 | **The tolerance owner is a named person** | `tolerance_policy.owner` is required and non-null. Dana's "who checked that nothing got worse, in writing" is a schema constraint, not a report section |
| O4 | **Silent degradation is a defect class** | A quality drop with no breach alert is a P0 bug. The default tolerance ships conservative and shadow mode is on by default for new endpoints |
| O5 | **Adversarial cost inflation is a threat model** | Semantics-preserving perturbations can suppress small-tier confidence and force escalation, inflating the bill [S9]. Per-endpoint escalation-rate anomaly detection ships with the gate |
| O6 | **The counterfactual is open code** | The savings computation lives in the open-source half. The vendor must not be the only party able to compute the number [S17] |
| O7 | **Judge provenance is recorded** | Judge model, version and settings pinned in `frontier_run`. A frontier whose judge cannot be named is marked not reproducible [S13] |
| O8 | **No PII inference from prompt features** | The classifier consumes derived features; the feature list is inspectable and no feature may be free text |

---

## 8. Success metrics

### 8.1 Outcome metrics — the domain's real "did it work"

Ranked. The first three decide whether CAMIR is a company.

| # | Metric | Definition | Target / basis |
|---|---|---|---|
| M1 | **Oracle ceiling on customer traffic** | Fraction of requests a perfect router would resolve at a lower tier within declared tolerance | Reported, not targeted. Below the disqualification threshold → *do not deploy*. PR7; the walk-away condition in `../BRIEF.md` |
| M2 | **Ceiling recovery fraction** | (route score − fixed-model baseline) ÷ (oracle ceiling − fixed-model baseline) | The honest denominator. [S4] says the field's methods sit far below 1.0; CAMIR reports where it lands `(assumption: no CAMIR measurement exists)` |
| M3 | **Artifact share of the apparent ceiling** | Ceiling with artifact guard minus ceiling without, as a fraction of the gap to oracle | **This is the contribution.** [S5] found truncation in 65% of MMLU cases and 5–12% parse failures; nobody has run the decomposition on a self-hosted pool |
| M4 | **Cost per request at declared tolerance** | Amortised GPU-hours per token, all-in multiplier declared [S26][S27] | Dana's unit — it separates growth from efficiency |
| M5 | **Inter-judge agreement** | Published with every frontier | Reported against the ~76% field baseline [S33] |
| M6 | **Escalation rate** | Share of cascade requests reaching a higher tier | Reported with the cost decomposition; PR1 says this variable, not classifier accuracy, sets cascade economics |
| M7 | **Time to first frontier** | Wall-clock from first logged request to a `frontier_run` | Under one week on the customer's own hardware (G1) |

### 8.2 Engagement metrics

M8 endpoints under a tolerance policy · M9 share of policies whose owner is the consuming team rather than the platform team (the Ravi metric) · M10 shadow-mode-to-enforcement conversion rate · M11 pin-to-large rate, where **rising is a product failure and must be visible as one** · M12 recalibration runs per quarter · M13 time from a quality question to an attributed answer, target under one hour.

### 8.3 Business metrics

M14 measured savings attributed, per the ledger · M15 OSS-to-control-plane conversion, against the 1–5% open-core band [S40] · M16 disqualification rate — the share of evaluations CAMIR itself ends, which should be **non-zero**; a zero rate means the ceiling probe is not being believed · M17 net revenue retention, once revenue exists.

**No target is set for M14.** [G2] records that no published routing savings exist for self-hosted open-weight pools, so any number here would be invented. RouteLLM's CPT is **3.66× on MT-Bench, 1.41× on MMLU, 1.49× on GSM8K** [S2]; FrugalGPT reached up to 98% cost reduction with only 16.6% of queries escalated to GPT-4 [S3] — both on hosted catalogs at 2023–2024 price ratios, and neither transfers.

---

## Recommended next 3

1. **Build the qualify phase before the router.** Replay corpus builder → oracle ceiling probe → artifact guard → disqualification report. It is the only sequence that returns a *negative* result cheaply, it produces M3 (the contribution) before any routing code exists, and it makes G1 true in week one. Everything in §5.1–5.2 is worthless if PR7 fails.
2. **Ship Ravi's four features in the same release as the first routing decision** — per-endpoint tolerance ownership, tier decision on every trace, shadow mode, unilateral pin-to-large. [../strategy/value_prop_canvas.md](../strategy/value_prop_canvas.md) ranks these above classifier accuracy for all three deciding personas, and [S21] is the public record of what happens when routing ships without them.
3. **Fix the cost-axis parameterisation in writing before the first `frontier_run`.** The utilisation assumption and the 3–5× all-in multiplier [S27] change the answer by more than any routing improvement will. Publishing the derivation is an unclaimed contribution [S7]; publishing a frontier with an undeclared cost axis reproduces exactly the incomparability [S13] complains of.
