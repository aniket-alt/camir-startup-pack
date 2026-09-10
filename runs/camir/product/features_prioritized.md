# CAMIR — 50 Features in Priority Order

**What this is** — the full feature superset, 50 rows in strict priority order across Now / Next / Later, each with its mechanism, the user value in a named persona's terms, its dependencies, effort and the principle it maps to.
**Why it exists** — the natural build order for a router is proxy → classifier → dashboard, and that order puts the first quality regression in production before anyone can attribute it, which is how the GPT-5 routing rollout was publicly criticised [S21]. This list re-orders around a different question — *what can return a cheap negative, and what stops Ravi Menon killing the deployment* — and the ordering is the argument. It also holds the line that the classifier route (#22) ships **after** its own baseline (#13), which is the ordering [S8] forces.
**How to read it** — the ordering is the content; read the **Depends on** column to check it is a real dependency graph and not a disguised calendar. A skeptic should attack the Now/Next boundary at #18/#19: everything above it must be defensible as "the first routed request cannot happen without this."
**Depends on / feeds** — depends on [PRD.md](PRD.md) §3 and §5, [features_flagship.md](features_flagship.md), [../strategy/value_prop_canvas.md](../strategy/value_prop_canvas.md); feeds [journeys/](journeys/), [ux_spec.md](ux_spec.md) and the tech layer's build sequence.

Principles PR1–PR10: [PRD.md](PRD.md) §3. Personas P1–P6: [../strategy/personas.md](../strategy/personas.md). Flagship IDs F1–F20: [features_flagship.md](features_flagship.md). Effort: S ≤ 1 week, M ≤ 1 month, L > 1 month, for a three-engineer team `(assumption: team of three, capstone cadence)`.

---

## NOW — #1–18 · nothing routes a real request until all eighteen exist

**Boundary rule:** a feature is Now only if the **first enforced routing decision is unsafe or unmeasurable without it**. That is why attribution and the off switch sit above the classifier.

| # | Feature | Mechanism | User value (persona) | Depends on | Effort | Principle |
|---|---|---|---|---|---|---|
| 1 | **Replay corpus builder** (F5) | Stratified sample of the customer's logged requests by endpoint and length band; content-hashed and pinned | P2: the frontier is on *his* traffic, which is the only kind Dana accepts | — | M | PR9 |
| 2 | **Pool manifest / tier registry** | Declares tiers, weights, hardware, resident VRAM, utilisation. Distinct base models do not share weights the way multi-LoRA adapters do [S31], so resident cost is per tier | P3: her five tiers are declarable, not assumed to be three | — | S | PR9 |
| 3 | **Judge harness, forced protocol** (F18) | Temperature 0 [S34]; fixed answer position or averaged permutations; verbosity control; exact-match or programmatic verification preferred | P3: a quality axis she can audit | 1 | L | PR8 |
| 4 | **Artifact guard** (F4) | Generous generation budgets with truncation logging; strict parse with failure counts. [S5]: truncation in 65% of MMLU cases, 5–12% parse failures | P2: his ceiling is not artificially low before he has decided anything | 3 | M | PR6 |
| 5 | **Inter-judge agreement reporter** | Two or more judges; agreement statistic computed and attached to the run; runs without it render as *Not reproducible* | P3: her stated bar — no agreement number, no frontier | 3 | S | PR8 |
| 6 | **Self-hosted cost meter / cost axis** (F3) | GPU-seconds × amortised hourly rate ÷ declared utilisation, with the 3–5× all-in multiplier [S27] printed as a parameter. No published benchmark derives this axis [S7] | P4: cost per request, a unit that separates growth from efficiency | 2 | M | PR9 |
| 7 | **Oracle ceiling probe** (F1) | Every corpus request through every tier; label which tiers resolved it; ceiling = perfect foreknowledge | P2: learns in week one whether routing can help at all | 1,2,3,4,6 | M | PR7, PR10 |
| 8 | **Disqualification report** (F2) | Ceiling minus baseline below CAMIR's own operating cost → *do not deploy*, with the histogram; no policy is generated | P2: a product that tells him not to buy it, which is why he believes the rest | 7 | S | PR7 |
| 9 | **Non-nested tier report** (F6) | The request set where a smaller tier was right and a larger wrong [S3] | P2/P3: evidence routing is assignment, not controlled degradation | 7 | S | PR10 |
| 10 | **Frontier builder** | Models as points, routes as curves, in a cost-quality plane [S7] | P2: he sees the exchange rate and picks the point himself | 6,7 | M | PR9 |
| 11 | **Ingress proxy** (F17) | OpenAI-compatible endpoint; change a base URL, keep the client | P1: one environment variable, one restart, one afternoon | 2 | M | PR9 |
| 12 | **Cascade route** (F13) | Small tier first; gate resolves or escalates. Observes an attempt instead of predicting one, sidestepping the predictability bottleneck [S4] | P2: savings that do not depend on difficulty being predictable | 11,13 | M | PR1, PR2 |
| 13 | **Confidence gate + calibration fit** (F14) | Calibrated uncertainty threshold (max softmax / margin / entropy) fitted on held-out labels; versioned object. Simple confidence routes as well as trained routers [S8] | P2: the escalation rule is inspectable, not a magic constant | 3,7 | M | PR2, PR3 |
| 14 | **Escalation-rate accounting + break-even** (F15) | Cascade cost decomposed into failed small attempt + gate + large answer; reports the escalation rate above which cascade costs more than always-large | P2: PR1's arithmetic, which FrugalGPT's 2023 hosted ratios no longer supply [S3][G2] | 12,6 | S | PR1 |
| 15 | **Per-endpoint tolerance policy** (F7) | Append-only versioned object keyed by endpoint, required non-null `owner`, declared drop versus the fixed-model baseline, and the `frontier_run` it was set against | **P5: his name, his number, no ticket.** Also P4's "who checked, in writing" as a schema constraint | 10 | M | PR4 |
| 16 | **Trace stamper** (F8) | Route type, tiers attempted, confidence, escalation flag, policy version as span attributes on the **caller's own** trace | P5: a quality question answered in minutes inside his existing tracing tool | 12 | S | PR2, PR4 |
| 17 | **Shadow mode** (F9) | Decisions computed and logged while all traffic still goes to the fixed-model baseline; full counterfactual, zero production risk; on by default for new endpoints | P5: consulted rather than informed. P1: defaults that cannot silently hurt her | 12,16 | M | PR9, PR4 |
| 18 | **Pin-to-large registry** (F10) | Per-endpoint pin read before classification, effective next request, no restart, no approval path; pin state stamped on the trace | **P5: the veto becomes a setting he owns instead of an escalation he files** | 15,16 | S | PR4, PR2 |

---

## NEXT — #19–35 · the deployment survives month three and someone pays

| # | Feature | Mechanism | User value (persona) | Depends on | Effort | Principle |
|---|---|---|---|---|---|---|
| 19 | **Tolerance breach alert + auto-revert** (F11) | Continuous measured quality per endpoint against declared tolerance; crossing pages the owner and optionally reverts that endpoint to baseline | P1: her stated objection — finding out from a support ticket — answered in the product | 15,17 | M | PR4 |
| 20 | **Counterfactual savings ledger** (F12) | Per request, baseline cost versus actual on the cost axis, plus quality delta. **Computation lives in the open half, inside the customer's perimeter** | P4: the party claiming the saving is not the party verifying it [S17] | 6,12,16 | M | PR9 |
| 21 | **Signed savings report** | One page: baseline, cost at declared tolerance, quality delta, judge agreement, named signer | P4: the slide, and the name attached to "who checked" | 20,5 | S | PR9, PR4 |
| 22 | **Classifier route (ablation)** (F16) | Prompt-feature model predicts the resolving tier; single dispatch, no wasted generation | P2/P3: the second strategy, honestly scoped. **Not shipped before #13** | 13,7 | L | PR3, PR5 |
| 23 | **Ablation table** | Four rows on one axis: fixed-model baseline, calibrated confidence, trained classifier, oracle ceiling — with gap-to-oracle per row | P3: the only honest way to read a classifier result [S8]. If the classifier does not beat confidence, CAMIR reports it | 22,13,7 | S | PR3, PR5 |
| 24 | **Drift monitor** (F19) | Watches escalation rate, calibration error and per-endpoint quality against the `frontier_run` the active policy cites | P2: his March A/B test is two model upgrades stale and nothing told him | 13,15 | M | PR5 |
| 25 | **Recalibration scheduler** | Pool-manifest change or drift trigger queues a re-run and emits a new dated `frontier_run` | P2: the policy stops being a one-time guess | 24,7 | M | PR5, PR9 |
| 26 | **Frontier diff** (F20) | Old curve against new with the chosen tolerance point projected onto both | P2/P5: "did *my* point move", not "did the curve move" | 25,15 | S | PR4, PR9 |
| 27 | **Label recycler** | Promotes production `judgment_record` rows into the classifier's training set and the gate's calibration set | P2: the per-deployment loop that is the only thing that compounds | 3,13,22 | M | PR2, PR3 |
| 28 | **Bring-your-own judge interface** | Wen's judge behind the same interface; her agreement statistics computed identically | **P3: the reason she evaluates at all** | 3,5 | M | PR8 |
| 29 | **N-tier registry** | Arbitrary tier counts and specialist tiers, not a fixed small/mid/large | P3: five tiers including a fine-tuned specialist. She abandoned RouteLLM partly because two were not enough | 2 | M | PR10 |
| 30 | **Shadow report distribution** | Per-endpoint shadow report pushed to the tolerance owner before any enforcement, on a schedule | P5: consultation as a mechanism, not a courtesy | 17,15 | S | PR4 |
| 31 | **Escalation anomaly detection** | Per-endpoint escalation-rate anomaly alerts. Semantics-preserving perturbations can suppress small-tier confidence and force escalation, inflating the bill [S9] | P2: a cost-optimising router is a new attack surface and he is the one paged | 14,24 | M | PR1 |
| 32 | **Cost-axis sensitivity sweep** | Re-renders the frontier across a utilisation range and an all-in multiplier range [S27], showing where the ranking of routes flips | P3: an idle GPU at 10% utilisation costs 10× per token; the frontier is a function of that assumption | 6,10 | S | PR9 |
| 33 | **Policy audit log** | Full append-only history: who changed which tolerance, when, against which frontier | P4: budget-review defensibility. P5: proof nobody moved his number quietly | 15 | S | PR4 |
| 34 | **Label-driven corpus stratification** | Re-samples the replay corpus by measured difficulty stratum once labels exist, so easy traffic stops dominating the run | P2: a corpus that keeps testing the hard end where routing actually decides | 1,7 | M | PR9, PR7 |
| 35 | **One-GPU quickstart CLI** | Clone, point at a log file, get a frontier chart on a single GPU in an afternoon | **P6: the distribution channel.** Open-core conversion runs 1–5% [S40]; Sam is the 95–99% and he writes the post Marcus reads | 1,7,10 | S | PR9 |

---

## LATER — #36–50 · only after the frontier is real and someone has paid for it

| # | Feature | Mechanism | User value (persona) | Depends on | Effort | Principle |
|---|---|---|---|---|---|---|
| 36 | **Control plane, VPC-deployed** | The paid layer installed inside the customer's network; reads records, never prompts | P1/P4: non-goal N4 held. Multi-tenant SaaS would disqualify the beachhead outright [S28] | 20,21 | L | PR4, PR9 |
| 37 | **Per-deployment classifier training service** | Managed retraining on the customer's own labels, versioned and rollback-able | P2: the paid half of the open-core line | 27,36 | L | PR5, PR3 |
| 38 | **Multi-endpoint policy console** | Tolerance ownership across dozens of endpoints and owners in one view | P2: six product teams, six owners, one platform engineer | 15,33,36 | M | PR4 |
| 39 | **Savings-share billing meter** | Meters attributed savings against the ledger for share-of-savings pricing, with the eligible-savings definition printed. FinOps precedent publishes no universal rate [S38][S39] | P4: pricing anchored to a number she can re-derive | 20,36 | M | PR9 |
| 40 | **Hybrid tier — hosted API as top tier** | Admits a hosted frontier model as an additional top tier; the cascade's "escalate to frontier" becomes a feature rather than a compromise | P2: the frontier escape hatch, per `../BRIEF.md` §Business model (A9, self-hosted first, hybrid later) | 2,6,29 | M | PR9, PR10 |
| 41 | **Prefill-activation routing signal** | Routes on internal prefill activations rather than surface prompt features [S10] | P3: the one signal class a hosted router structurally cannot reach — and the only credible escape from the plateau [S4]. Requires a serving-engine patch, not a proxy feature | 22,2 | L | PR5 |
| 42 | **Cache-residual measurement mode** | Measures the routing saving on post-cache traffic only. Semantic caching removes 20–45% of production traffic upstream [S36] and adversely selects the remainder toward the hard end; the residual is unmeasured [G4] | P2: an honest number rather than one that double-counts his cache | 20,1 | M | PR9 |
| 43 | **Published frontier registry** | Opt-in public registry of `frontier_run` manifests with corpus hashes and judge-agreement statistics | P3/P6: the standard harness [S13] says the field lacks. Contribution survives even if the router plateaus | 5,10,35 | M | PR8, PR9 |
| 44 | **Judge-disagreement triage** | Routes the requests where judges disagree into a separate bucket rather than averaging the disagreement away | P3: judges are individually consistent yet mutually inconsistent [S33]; averaging hides exactly that | 5,28 | M | PR8 |
| 45 | **Human spot-check workflow** | Samples judged requests for human review and reports human-judge agreement alongside inter-judge agreement | P3: judges reach ~80% agreement with humans [S33]; the residual 20% needs an owner | 44 | M | PR8 |
| 46 | **Per-endpoint frontier** | A separate curve per endpoint rather than one per deployment | P5: his endpoint's exchange rate, not the shared service's average | 10,34 | M | PR9, PR4 |
| 47 | **Per-endpoint spend guardrail** | Hard daily spend ceiling per endpoint, enforced by forcing the small tier or shedding | P4: a budget line that cannot surprise her twice | 20,15 | S | PR4 |
| 48 | **Pool federation** | Routing across multiple clusters or regions with per-cluster cost axes | P3: high-volume, multi-region, heterogeneous hardware | 2,6,32 | L | PR9 |
| 49 | **In-process router SDK** | Non-proxy mode for teams unwilling to add a network hop | P3: removes the proxy's latency and failure domain. Deliberately Later — the proxy is what makes P1's afternoon possible | 12,13 | M | PR2 |
| 50 | **Tolerance recommender** | Given a target spend, proposes the tolerance that reaches it and shows the projected quality delta | P4: inverts the question from "what does this cost" to "what do I give up to hit my number" | 10,15,26 | M | PR4, PR9 |

---

## Cut — features that map to no principle

The mapping rule has teeth only if something fails it. Each of these was proposed, mapped to nothing in [PRD.md](PRD.md) §3, and was cut.

| Proposed | Why it was cut |
|---|---|
| Built-in semantic cache | Maps to no principle and violates non-goal N3. It would also let CAMIR bank cache savings as routing savings — the incentive problem this pack accuses incumbents of [S17] |
| Prompt optimisation / rewriting | Changes the request, so the counterfactual in #20 stops being computable. Maps to nothing |
| Latency-aware routing | A real third axis [S12] and an explicit non-goal (N6). Deferred by decision, not by mapping failure — recorded here so it is not smuggled back in as a "small addition to #13" |
| A public model leaderboard | Ranks models, not routes on a customer's traffic. Contradicts PR9: models are points, routers are curves, and CAMIR's unit is the curve |
| Multi-turn conversation routing | Non-goal N5; needs a different cost model and evaluation frame [S6] |
| A chat UI over the pool | Would make CAMIR a consumer of its own router rather than a measurement layer. Maps to nothing |

---

## Recommended next 3

1. **Freeze the Now list at 18 and cut #22 from any conversation about the first release.** The classifier route is the most interesting feature here and the one with the weakest evidence behind it [S4]; it depends on #13, which is also its baseline. Building it first inverts the ordering [S8] forces.
2. **Move #35 (one-GPU quickstart) up if the first month of discovery is slow.** It is S effort, it depends only on #1, #7 and #10, and it is the only row in the table that manufactures distribution. Sam never pays [S40] and Sam is how Marcus hears about this.
3. **Put a date on #42 before quoting any savings number publicly.** Caching takes 20–45% of traffic upstream and skews the rest hard [S36][G4]; a savings figure that ignores it is the first number a technical buyer will attack, and it is cheap to measure.
