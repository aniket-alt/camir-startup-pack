# CAMIR — The 20 Flagship Features

**What this is** — the twenty highest-leverage features in CAMIR, each with its mechanism, the first-principle it obeys, and the moment a named person literally sees it.
**Why it exists** — a router's feature list writes itself as classifier, gate, dashboard, and then the team spends a year on classifier accuracy, which [S4] shows moves ~2.13 percentage points at best. This file ranks the features by leverage instead of by engineering interest, which is why #1 is the probe that can tell a customer *not to buy*, and why four of the top twelve exist solely for Ravi Menon, who is not a customer and can stop everything.
**How to read it** — read the **Principle** column as an audit: a feature with no principle is decoration and this list is where it gets cut. A skeptic should attack F9 (the classifier route), which is deliberately demoted to an ablation, and F3 (the cost axis), which is the contribution claim.
**Depends on / feeds** — depends on [PRD.md](PRD.md) §3 and §5, [../strategy/value_prop_canvas.md](../strategy/value_prop_canvas.md), [../research/survey.md](../research/survey.md); feeds [features_prioritized.md](features_prioritized.md), [journeys/](journeys/), [ux_spec.md](ux_spec.md).

Principle codes PR1–PR10 are defined in [PRD.md](PRD.md) §3. Personas P1–P6 in [../strategy/personas.md](../strategy/personas.md).

---

## Tier A — the qualifying instruments (F1–F6)

These decide whether the customer should proceed at all. They ship first, because they are the only features that can return a cheap negative.

### F1 — Oracle Ceiling Probe
**Mechanism.** Runs every request in the pinned replay corpus through every tier in the pool manifest, judges each response under the forced protocol, and labels per request the set of tiers that answered correctly. The ceiling is the score achievable with perfect foreknowledge — the upper bound on every claim CAMIR could ever make.
**Principle.** PR7, PR10.
**Visible moment.** Marcus opens the Ceiling Report and reads one sentence: *"On the sampled requests, a perfect router resolves a measured share at the 8B tier within your declared tolerance. Everything CAMIR can do lives below that ceiling."* `(assumption: illustrative wording; no CAMIR run exists — see [G2])`

### F2 — Disqualification Report
**Mechanism.** If the gap between ceiling and fixed-model baseline is smaller than CAMIR's own operating cost for that deployment, the ceiling probe emits a **do-not-deploy** verdict with the difficulty histogram behind it, and refuses to generate a policy.
**Principle.** PR7.
**Visible moment.** Day two of an evaluation. The report header reads *"Routing is not recommended for this traffic"*, with the bimodal histogram beneath it. Nothing further unlocks. This is a product behaviour, not a sales stance — the walk-away condition in `../BRIEF.md` compiled into the tool.

### F3 — Self-Hosted Cost Axis
**Mechanism.** Cost per request = GPU-seconds consumed × amortised hourly rate ÷ declared utilisation, with the all-in multiplier over raw rental (3–5× once engineering time is counted [S27]) as an explicit, printed parameter. RouterBench prices routers against hosted API list prices [S7]; no published benchmark has derived a cost axis for self-hosted pools.
**Principle.** PR9.
**Visible moment.** Every frontier chart carries a footer naming its four cost-axis parameters — GPU hourly rate, assumed utilisation, all-in multiplier, pool manifest hash. Wen screenshots this footer, not the curve, because it is the part she can audit.

### F4 — Artifact Guard
**Mechanism.** Generous generation budgets with per-response truncation logging; strict output-format parsing with failure counts; verbosity-normalised judging. [S5] found truncation in 65% of MMLU and 57% of MedQA cases and 5–12% parse failures on MMLU across 206,000 query-model pairs — measured as "the small model cannot do this" when it is instrumentation.
**Principle.** PR6.
**Visible moment.** The Ceiling Report shows two bars, not one: *ceiling as measured* and *ceiling after artifact correction*, with the delta labelled. The delta is CAMIR's headline scientific claim rendered as a bar chart.

### F5 — Replay Corpus Builder
**Mechanism.** Stratified sample of the customer's own logged requests by endpoint, length band and (once labels exist) difficulty stratum; content-hashed and pinned into every `frontier_run`, so a run is reproducible or explicitly is not.
**Principle.** PR9.
**Visible moment.** Marcus points it at a week of production logs and gets a corpus card: 8,000 requests, 6 endpoints, 4 length bands, hash `c4f19a…`. Everything downstream cites that hash. This is what makes his answer *his traffic*, which is the only frontier Dana will accept.

### F6 — Non-Nested Tier Report
**Mechanism.** Extracts the request set where a smaller tier answered correctly and a larger tier did not — the phenomenon FrugalGPT documented [S3], and the reason an oracle ceiling can exceed the large model's own score.
**Principle.** PR10.
**Visible moment.** A tab labelled *"Where small beat large: 340 requests"*, with examples. It is the single most persuasive screen for a skeptic who believes routing is just controlled degradation.

---

## Tier B — surviving contact with the organisation (F7–F12)

These are why the deployment is still live in month three. All four of Ravi's requirements are in this tier, and [../strategy/value_prop_canvas.md](../strategy/value_prop_canvas.md) ranks them above classifier accuracy for every deciding persona.

### F7 — Per-Endpoint Quality Tolerance Ownership
**Mechanism.** `tolerance_policy` is a versioned, append-only object keyed by endpoint, carrying a required non-null `owner` field, the declared acceptable drop versus the fixed-model baseline, and the `frontier_run` it was set against. The platform team cannot silently change another team's tolerance; a change writes a new version with a new signer.
**Principle.** PR4.
**Visible moment.** Ravi opens his endpoint's policy page and sees his own name in the Owner field, a tolerance he typed, and the curve it was chosen from. He did not file a ticket to get there.

### F8 — Tier Decision Stamped on Every Trace
**Mechanism.** Route type, predicted tier, tiers actually attempted, confidence score, escalation flag and policy version written as OpenTelemetry span attributes on **the caller's own span** — not only into CAMIR's store. Remove CAMIR and the history survives in the customer's telemetry.
**Principle.** PR2, PR4.
**Visible moment.** Ravi's thumbs-down rate ticks up. He filters his existing tracing tool by `camir.tier = 8b` and has the answer in four minutes, without opening a CAMIR screen or messaging Marcus. Target M13 is one hour; the mechanism is what makes four minutes possible.

### F9 — Shadow Mode
**Mechanism.** The router computes and logs a full tier decision for every request while **all traffic still goes to the fixed-model baseline**. Produces a complete counterfactual `savings_ledger` and a per-endpoint quality projection with zero production risk. On by default for every new endpoint.
**Principle.** PR9, PR4.
**Visible moment.** Two weeks before any traffic moves, Ravi receives a per-endpoint shadow report: *"Had routing been enforced on your endpoint, 77% of requests would have resolved at the 8B tier; projected quality delta −0.3 points against your declared tolerance of −0.5."* — the same shadow output as [journeys/beachhead.md](journeys/beachhead.md) Week 3 He is consulted, not informed — his stated gain G3.

### F10 — Unilateral Pin-to-Large
**Mechanism.** A per-endpoint pin flag in the pin registry, read before any classification, effective on the next request with no restart, no deploy and no approval path. Settable by the endpoint's tolerance owner.
**Principle.** PR4, PR2.
**Visible moment.** 11:40 on a Tuesday, Ravi sets `pin=large` on his endpoint. At 11:40:02 the next request goes large. The pin is stamped on the trace, so the dashboard shows the pin, the reason field, and who set it. **Pin rate rising is a product failure and the dashboard says so** — metric M11.

### F11 — Tolerance Breach Alert with Auto-Revert
**Mechanism.** Continuous measured quality per endpoint against its declared tolerance; a crossing reverts that endpoint to the fixed-model baseline automatically and then pages the tolerance owner. **Auto-revert is on by default**; the owner may disable it for their own endpoint, never for anyone else's. A quality drop with no alert is classified as a P0 defect.
**Principle.** PR4.
**Visible moment.** Priya, who tunes nothing, gets one alert in six months: *"faq-endpoint crossed its tolerance at 03:12; reverted to 70B; 41 requests affected; here they are."* Her stated objection — *"I find out from a support ticket, not from a dashboard"* — is answered in the product.

### F12 — Counterfactual Savings Ledger
**Mechanism.** Per request, the cost the fixed-model baseline would have incurred against the cost actually incurred, priced on F3's axis, plus the quality delta. **The computation is in the open-source half and runs inside the customer's perimeter**, so the party claiming the saving and the party verifying it are different.
**Principle.** PR9.
**Visible moment.** Dana's slide: baseline $50,100/month, actual $41,800 — a 16.6% saving, in line with the ~17.5%-of-spend derivation in [../financials/pricing.md](../financials/pricing.md) — quality delta −0.2 points against declared tolerances, **signed by the endpoints' tolerance owners, not by Marcus and not by CAMIR** (O3). `(assumption: illustrative; no CAMIR measurement exists [G2]; same figures as [journeys/day_in_life.md](journeys/day_in_life.md) 16:20)` Her objection — *"the biller computes the counterfactual"* [S17] — dies on the word *open*.

---

## Tier C — the routing mechanism itself (F13–F17)

Note the position. The router is the object under measurement, not the product [S11].

### F13 — Cascade Route (primary)
**Mechanism.** Dispatch to the smallest tier; score the answer at the confidence gate; resolve or escalate. It changes the signal available under unpredictable difficulty because it observes an actual attempt rather than predicting one; it does not remove calibration, judge validity or distribution-shift risk from the predictability bottleneck [S4]. Its break-even condition is measured escalation below the pool-specific threshold in F15.
**Principle.** PR1, PR2.
**Visible moment.** The route selector shows *Cascade (recommended)* with its measured curve, and *Classifier (ablation)* with its own, on the same axes. The recommendation is a measurement, not a default.

### F14 — Confidence Gate
**Mechanism.** Calibrated uncertainty threshold — max softmax probability, margin, or predictive entropy — fitted on held-out `judgment_record` labels and stored as a versioned object. [S8] finds simple confidence measures route as well as trained routing models, which makes this both the mechanism and the bar.
**Principle.** PR2, PR3.
**Visible moment.** A calibration plot: predicted confidence against observed correctness, with the chosen threshold as a vertical line and the escalation rate it implies printed beside it. Wen moves the line and watches both numbers move.

### F15 — Escalation-Rate Accounting
**Mechanism.** Decomposes cascade cost into failed small attempt + gate cost + large answer, and reports the break-even escalation rate above which the cascade costs **more** than going straight to the large tier. This is PR1 made arithmetic, and it is the number FrugalGPT's 2023 hosted price ratios [S3] no longer supply for a compressed self-hosted spread [G2].
**Principle.** PR1.
**Visible moment.** A single line above the frontier: *"Break-even escalation rate 80% (small/large cost ratio 0.20). Yours is 23% — saving 58% of token-proportional GPU cost on this endpoint."* The break-even is `1 − r` ([../tech/whitepaper.md](../tech/whitepaper.md) §2.1, worked in [../tech/architecture/D05.md](../tech/architecture/D05.md)); the saving erodes long before it is reached If yours exceeds break-even, CAMIR recommends the classifier route or no routing at all.

### F16 — Classifier Route, reported as an ablation
**Mechanism.** Prompt-feature model predicts the resolving tier; one dispatch, no wasted generation. **Its result is reported against the calibrated-confidence baseline (F14), never against a fixed model**, because beating a fixed model is table stakes and beating free confidence is the real bar [S8].
**Principle.** PR3, PR5.
**Visible moment.** An ablation table with three rows — fixed-model baseline, calibrated confidence, trained classifier — and a column for the gap to oracle. If the classifier row does not beat the confidence row, CAMIR says so in its own report.

### F17 — Ingress Proxy
**Mechanism.** OpenAI-compatible endpoint. Change a base URL, keep the client library, keep the request shape.
**Principle.** PR9.
**Visible moment.** Priya changes one environment variable, restarts one service, and her afternoon is over. She never opens the tier registry. **This is the entire edge-low experience** and it is deliberately unremarkable.

---

## Tier D — keeping the frontier true (F18–F20)

### F18 — Judge Harness with Published Inter-Judge Agreement
**Mechanism.** Temperature 0 (test-retest above 95%, versus ~70% at temperature 1 [S34]); fixed answer position or averaged permutations (position bias produces ~40% GPT-4 inconsistency [S34]); verbosity control (~15% inflation [S34]); exact-match or programmatic verification wherever the task admits it; two or more judges with the agreement statistic computed and attached to the run. Judges agree with each other only ~76% of the time [S33].
**Principle.** PR8.
**Visible moment.** Every frontier chart carries *"inter-judge agreement 0.79 (n=8,000, 2 judges)"* in its header. A run without it renders with a **Not reproducible** badge. Wen's stated bar — *"if you don't publish judge agreement, your frontier is noise"* — is a rendering rule.

### F19 — Drift Monitor and Recalibration Scheduler
**Mechanism.** Watches escalation rate, classifier calibration error and per-endpoint measured quality against the `frontier_run` the active policy was set on; a pool-manifest change or a drift trigger schedules a re-run and produces a new dated frontier. Static policies decay silently — Marcus's March A/B test is two model upgrades stale.
**Principle.** PR5, PR9.
**Visible moment.** A banner: *"Your policy was set against frontier run 2026-09-14. The 8B tier was upgraded 41 days ago. Re-measure."* with a button that queues the run.

### F20 — Frontier Diff
**Mechanism.** Old curve against new, with the customer's chosen tolerance point projected onto both, so the question is not *did the curve move* but *did my point move*.
**Principle.** PR4, PR9.
**Visible moment.** Two curves, one marker, one sentence: *"At your declared tolerance of −2.0, cost per request moved from $0.0041 to $0.0036. Your point is still on the curve."* This is the artifact that survives a model upgrade without a renegotiation.

---

## The integration argument — why the loop, not any feature

The first executable slice is F5 → F1 → F4 → F2, then F9/F10/F13 with a two-tier pool. The twenty-feature set is not a delivery promise: judging, tracing, recalibration and attribution need decomposition and measured effort. Wen says *"I could build this in three weeks"*; the credible response is to constrain the supported interfaces and ship the qualifying slice before expanding it.

The power is that the loop closes, and it closes in exactly one place: **the labels.**

1. **F18** produces `judgment_record` labels under a protocol strong enough that the labels mean something. Without PR8's protocol they are noise, and everything downstream inherits the noise.
2. Those labels are what **F1** turns into an oracle ceiling — the denominator for every claim.
3. The same labels **calibrate F14's gate** and **train F16's classifier**. A deployment with enough held-out, judge-reviewed history may be better calibrated than a cold start; the magnitude and data requirement are an experiment, not a premise.
4. **F12** prices the outcome on **F3**'s axis, and because the ledger and the axis are open, the number is verifiable by the buyer's own engineer rather than asserted by the vendor.
5. **F19** notices the pool moved, re-runs **F5**'s pinned corpus, and the loop starts again — which is the only defence against the silent decay of a policy set once.

Break any link and the rest degrades to something free: without F18 the frontier is unreproducible and Wen leaves; without F3 the cost axis is a hosted-API list price and the whole self-hosted claim collapses [S7]; without F1 the team optimises a router against an unknown ceiling, which is what [S4] documents 21 methods doing.

**And the loop is not the moat.** It is a per-deployment loop, not a network one — `../BRIEF.md` §Moat says the algorithm is not the defensible part and this file does not upgrade that. What the loop buys is that a customer who has run it for a year cannot cheaply recreate the labels, the calibration or the signed tolerance history. That is the whole claim, and it is `../ASSUMPTIONS.md` A6, still untested.

---

## Recommended next 3

1. **Build F5 → F1 → F4 → F2 as one release and ship nothing else.** Four features, one deliverable: a customer learns in under a week whether routing can help them, and CAMIR learns M3 — the artifact share of the apparent ceiling — which is the publishable contribution regardless of what the router does.
2. **Refuse to build F16 until F14 exists and is measured.** The ablation is meaningless without its baseline, and building the classifier first is the single most likely way this team spends year one on the ~2.13 percentage points [S4] says are available.
3. **Treat F8 and F10 as a single indivisible unit with F13.** Shipping a routing decision without per-request attribution and a unilateral off switch is the GPT-5 rollout [S20][S21], and it fails in the same documented way. If the schedule forces a cut, cut F16, not F10.

<!-- critic: round 1 recorded 2026-09-10 in ../audit/CRITIC_LOG.md — 2 major, 3 minor fixed. Round 0 (commit 712241c) edits were retained but left no verdict record. -->
