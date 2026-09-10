# Technique Wave 3 — Frontier and Cross-Domain Imports

**What this is** — the catalog of techniques that are either too new to be settled (internal-state routing, model introspection, deferral attacks) or borrowed intact from a field that solved a structurally identical problem decades ago (detection cascades, signal detection theory, option pricing, FinOps counterfactual accounting, statistical process control, sequential experimentation).
**Why it exists** — waves 1 and 2 sit inside a plateau the field has already measured [S4], so the only two places a genuinely new result can come from are a signal class competitors cannot access and a discipline that has not been imported yet. This file names both, with their risk stated. Without it, prefill-activation routing gets pitched as a differentiator rather than as the research bet it is, and CAMIR's tolerance-breach alerting gets invented from scratch instead of lifted from control charts.
**How to read it** — cluster **A** is the strategic bet and cluster **D** is the one a security reviewer will ask about; read those two. A skeptic should attack cluster **A**: it is one 2026 paper [S10], it requires a serving-engine patch rather than a proxy feature, and if it does not clear the plateau CAMIR has no accuracy story left at all.
**Depends on / feeds** — depends on [wave1.md](wave1.md), [wave2.md](wave2.md), [../../research/capability_table.md](../../research/capability_table.md) C7/C12, [../../strategy/positioning.md](../../strategy/positioning.md) §Axis X; feeds [decision_tree.md](decision_tree.md), [technique_feature_matrix.md](technique_feature_matrix.md), [../not_vaporware.md](../not_vaporware.md) §research risk.

---

## Scope and counting rule

**41 techniques in 9 clusters. This wave stopped early, and the reason is worth stating.** Frontier LLM-routing work that is not already covered by waves 1–2 amounts to roughly one live signal class (prefill activations, one paper [S10]), one adversarial thread (one paper [S9]), and one axis extension (latency, one paper [S12]). Everything else here is imported from another discipline, and imports have to be *load-bearing* to earn a line — a technique that could be applied to CAMIR but changes no decision is padding. Nine candidates were cut on that test, including genetic-algorithm threshold search, simulated annealing over tier orderings, and three further control-chart variants beyond the four listed.

Evidence anchors: `[Sn]` resolves to [../../research/sources.md](../../research/sources.md); a bare name means a standard method in its home field; `(speculative)` means the application to CAMIR is CAMIR's own idea and has no published support.

| Cluster | # | Techniques |
|---|---|---|
| A | 5 | Internal-state signals — the self-hosting-exclusive class |
| B | 4 | Model introspection and self-estimated competence |
| C | 4 | Speculative decoding as an implicit cascade |
| D | 5 | Adversarial robustness of deferral |
| E | 3 | Latency as a third frontier axis |
| F | 5 | Import — signal detection theory and detection cascades |
| G | 3 | Import — hedging and option pricing for tolerance policy |
| H | 4 | Import — FinOps counterfactual accounting |
| I | 4 | Import — statistical process control |
| J | 4 | Import — sequential and shadow-mode experimentation |
| | **41** | |

---

## A. Internal-state signals — the self-hosting-exclusive class (5)

**The one signal class a hosted competitor structurally cannot use**, and it is available precisely because CAMIR's users run their own weights [S10]; `../../strategy/positioning.md` §Axis X makes this the boundary no commercial incumbent can cross. **Flagged as high-upside research risk, not as a shipped capability.**

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-1 | **Prefill-activation routing** | Decide the tier from the small tier's internal activations after prefill, before any tokens are decoded — a signal that is query-specific rather than the globally averaged trend [S4] diagnoses as the plateau's cause. | [S10] — single 2026 paper; **research risk** |
| W3-2 | **Linear probes for correctness prediction** | Train a linear classifier on hidden states to predict whether this generation will be correct; cheap, inspectable, and testable on logged activations without touching the serving path. | Standard probing methodology; `(speculative for tier routing)` |
| W3-3 | **Layer-wise early-exit confidence** | Read the confidence of intermediate-layer predictions; agreement across depth is a known correctness correlate and needs no extra forward pass. | Early-exit / logit-lens literature; `(speculative)` |
| W3-4 | **Attention-entropy difficulty proxy** | Diffuse attention over the prompt as a proxy for the model not knowing what matters. | `(speculative — no published validation as a routing signal)` |
| W3-5 | **Prefill-cost-aware dispatch** | The small tier's prefill is already paid at escalation time; reuse its KV cache or its activation summary rather than re-prefilling at the large tier. | Engineering consequence of W3-1; connects to W2-43; `(speculative — cross-model KV reuse is not generally available)` |

**The honest cost.** All five require integration inside vLLM or SGLang, not a proxy in front of them — a real engineering commitment (capability table C7) and a maintenance burden against two fast-moving upstreams [S32].

---

## B. Model introspection and self-estimated competence (4)

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-6 | **P(True) self-evaluation** | Ask the model to score the probability its own answer is correct, as a separate forward pass over the produced answer. | Kadavath et al., "models mostly know what they know" |
| W3-7 | **Verbalised confidence elicitation** | Have the model state a numeric confidence in words; cheap and black-box-compatible, but known to be poorly calibrated and to require W1-12/W1-14 on top. | Verbalised uncertainty literature |
| W3-8 | **Pre-generation competence gate** | Ask the small tier whether it *can* answer before it tries — the classifier route implemented by the small tier itself, at the cost of one short generation. | `(speculative — plausible ablation, unmeasured; and [S4]'s bottleneck argument predicts it plateaus too)` |
| W3-9 | **Introspective abstention** | Escalate on an explicit refusal or hedge token pattern rather than on a probability, catching the case where the model is confidently wrong but visibly evasive. | Abstention literature; composes with W1-22 |

---

## C. Speculative decoding as an implicit cascade (4)

The serving stack already runs a small model in front of a large one and escalates on disagreement. That is a cascade, and its telemetry is free.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-10 | **Draft-and-verify speculative decoding** | A draft model proposes tokens, the target model verifies; identical structure to a cascade, but at token granularity and with a *lossless* acceptance criterion. | Leviathan et al.; Chen et al. |
| W3-11 | **Self-speculative decoding / layer skipping** | The same model drafts with skipped layers and verifies with all of them, removing the second resident model — relevant because W2-42 charges each tier for resident VRAM. | Self-speculative decoding |
| W3-12 | **Multi-head drafting (Medusa)** | Extra decoding heads on one model produce candidate continuations, again collapsing two tiers into one set of weights. | Medusa (Cai et al.) |
| W3-13 | **Acceptance-rate telemetry as a difficulty signal** | The fraction of draft tokens the target accepts is a per-request, already-computed difficulty measurement — a free routing signal for any deployment already running speculation. | `(speculative — not published as a routing signal; testable on existing vLLM metrics)` |

**Why this cluster matters strategically.** It is the sharpest form of "why won't the serving layer just do this" [S11]: routing is being absorbed into the open serving stack, and speculative decoding is the shape it takes there.

---

## D. Adversarial robustness of deferral (5)

A cost-optimising router is a new attack surface. [S9] demonstrates **cascade deferral attacks**: semantics-preserving perturbations that suppress small-tier confidence and force escalation, inflating a victim's inference bill.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-14 | **Forced-deferral threat modelling** | Treat escalation as an attacker-controllable resource and enumerate who can reach the endpoint, at what cost per forced escalation. | [S9] |
| W3-15 | **Per-tenant escalation budgets** | Cap escalations per API key per window; converts an unbounded bill-inflation attack into a bounded degradation. | [S9]; `(assumption: policy design untested)` |
| W3-16 | **Escalation-rate anomaly detection** | Per-key change detection on escalation rate (W2-45) as the attack detector — the attack's signature is exactly the drift signal already being computed. | [S9] + W2-45 |
| W3-17 | **Randomised / smoothed thresholds** | Add calibrated noise to τ so a gradient-free attacker cannot reliably sit just below it; costs a small amount of frontier precision. | Randomised smoothing analogue; `(speculative for deferral)` |
| W3-18 | **Perturbation-invariance canaries** | Route a small sample of requests together with a semantics-preserving paraphrase and alert when only one of the pair escalates. | `(speculative — cheap detector, unmeasured)` |

---

## E. Latency as a third frontier axis (3)

A declared non-goal for year one (`../../BRIEF.md` §Wedge; survey §1). Catalogued so the axis is a deliberate omission rather than an oversight.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-19 | **Latency-aware routing under dynamic load** | Add measured latency as a third objective, so the frontier becomes a surface rather than a curve. | [S12] |
| W3-20 | **Queue-aware admission control** | Route on current queue depth per tier, not only on predicted difficulty — the large tier being busy is a routing fact. | Queueing theory; [S12] |
| W3-21 | **SLO-conditioned tier selection** | Pin the tier when the request's deadline cannot survive a cascade's two sequential generations — the cascade route's structural latency cost, which the classifier route does not pay. | [S12]; `(assumption: cascade latency penalty unmeasured on a self-hosted pool)` |

---

## F. Import — signal detection theory and detection cascades (5)

The founding literature. Cascades were solved for face detection in 2001; the threshold-selection mathematics is a century old and CAMIR should not re-derive it.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-22 | **Viola–Jones attentional cascade** | Order stages cheapest-first with high recall at each, so the overwhelming majority of instances are resolved by stage one; total cost is dominated by stage one's resolution rate, not by the last stage's accuracy. | Viola–Jones (2001); survey §2.1 |
| W3-23 | **ROC / DET curve analysis** | The escalation threshold is an operating point on a detection curve; DET's log axes make differences at the low-false-escalation end readable where ROC compresses them. | Signal detection theory; Martin et al. (DET) |
| W3-24 | **Neyman–Pearson thresholding** | Fix the false-escalation rate and maximise correct escalations — the formulation for a customer who declares a *budget* constraint rather than a quality constraint. | Neyman–Pearson lemma |
| W3-25 | **Cost curves (Drummond–Holte)** | Plot expected cost against the operating condition, so a router's dominance region is visible instead of a single ROC-AUC number that hides where it wins. | Drummond–Holte cost curves |
| W3-26 | **Sequential probability ratio test (SPRT)** | Accumulate evidence across repeated samples of the small tier and stop as soon as the likelihood ratio crosses a bound — an optimal stopping rule for W1-22's k-sample self-consistency, replacing a fixed k. | Wald's SPRT; `(speculative for cascade stopping)` |

---

## G. Import — hedging and option pricing for tolerance policy (3)

An escalation is a premium paid to avoid a tail outcome. That is an option, and options have a pricing literature.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-27 | **Real-option framing of escalation** | Value the escalation as the price of avoiding a quality loss whose cost is asymmetric and endpoint-specific; makes the τ-setting conversation about the *cost of being wrong* rather than about a percentage. | Real options; `(speculative — framing device, no pricing model claimed)` |
| W3-28 | **CVaR on quality loss** | Set tolerance on the conditional expected loss in the worst α of requests, not on the mean — because [S21]'s backlash was driven by tail failures on complex queries, which a mean tolerance hides entirely. | Rockafellar–Uryasev CVaR; [S21] |
| W3-29 | **Hedge fraction — mirrored traffic** | Send a small random share to the large tier regardless of the decision, as paid insurance that also supplies the unbiased sample W2-23's propensities need. | Composes with W2-23/W2-26; `(assumption: hedge fraction is a cost/precision trade-off, unpriced)` |

---

## H. Import — FinOps counterfactual accounting (4)

Cloud FinOps already sells savings it must prove. CAMIR's savings-attribution feature is that product, one layer up.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-30 | **Eligible-savings definition as a contract term** | Write down which cost deltas count *before* measuring. Share-of-savings vendors publish **no universal rate**, and the eligible-savings definition is where the negotiation actually happens [S38]. | [S38][S39] |
| W3-31 | **Counterfactual baseline accounting** | "What you would have spent" computed on a declared, frozen baseline policy — the operational form of W2-22, and the reason the harness must run inside the customer's perimeter [S17]. | [S38][S39]; `../../strategy/value_prop_canvas.md` PR-D1 |
| W3-32 | **Cost-per-request as the reported unit** | Report a unit rate rather than total spend so growth and efficiency are separable — the metric Dana can defend when traffic grows and the bill still rises. | `../../strategy/value_prop_canvas.md` PR-D2 |
| W3-33 | **Showback / chargeback per endpoint** | Attribute cost and escalations to the owning team, which is what converts Ravi's structural asymmetry ("he bears the risk, someone else banks the saving") into a visible, negotiable number. | `../../strategy/value_prop_canvas.md` §Canvas 3 Pn1 |

---

## I. Import — statistical process control (4)

Tolerance-breach alerting is a control-chart problem that manufacturing solved in 1924. Using an ad-hoc threshold instead produces either alert fatigue or silent drift.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-34 | **Shewhart control chart on measured quality** | Alert when quality leaves control limits set from measured process variance — not from a guessed percentage. | Shewhart; `../../strategy/value_prop_canvas.md` PR6 |
| W3-35 | **CUSUM** | Accumulate small deviations to detect a persistent shift a Shewhart chart misses; the right detector for slow tolerance erosion after a model upgrade. | Page's CUSUM |
| W3-36 | **EWMA chart on escalation rate** | Exponentially weighted moving average, catching gradual drift in the router's own behaviour before it shows up in quality. | Roberts' EWMA; composes with W2-45 |
| W3-37 | **Run rules for alert suppression** | Fire only on defined patterns (e.g. eight consecutive points on one side) so a single noisy judge verdict does not page anyone — the fix for a quality signal that flips **~30% of verdicts at temperature 1** [S34]. | Western Electric rules; [S34] |

---

## J. Import — sequential and shadow-mode experimentation (4)

Shadow mode is a **P0 feature** (`../../strategy/value_prop_canvas.md` PR4/PR-R4). These are the statistics that make its output decision-grade.

| # | Technique | Mechanism | Evidence anchor |
|---|---|---|---|
| W3-38 | **Dark-launch / shadow evaluation** | Compute and log the routing decision while all traffic still goes to the baseline; produces a full counterfactual dataset at zero quality risk, and is the only evidence that survives Ravi's veto. | `../../strategy/value_prop_canvas.md` PR4; standard practice |
| W3-39 | **Always-valid sequential testing (mSPRT)** | Let an operator watch the shadow-mode result continuously without inflating error rates — necessary because they *will* watch it continuously. | Mixture SPRT / always-valid p-values (Johari et al.) |
| W3-40 | **Group-sequential design with alpha spending** | Pre-plan interim looks and stop early for success or futility, bounding the length of a shadow run before enforcement. | O'Brien–Fleming / Lan–DeMets |
| W3-41 | **CUPED variance reduction** | Use each request's pre-period covariate (its endpoint, its historical difficulty stratum) to cut variance, shortening the shadow run needed to resolve differences near the **2.13-point** scale [S4] reports. | Deng et al., CUPED; [S4] |

---

## Ranking of the frontier bets

Because a catalog of forty-one options that does not rank them is a menu, not a plan.

| Rank | Bet | Upside | Risk | Verdict |
|---|---|---|---|---|
| **1** | **W3-38 to W3-41** — shadow mode with proper sequential statistics | Makes a P0 feature decision-grade; nothing here is research | Low — all four are settled methods | **Build this quarter** |
| **2** | **W3-34 to W3-37** — control charts for tolerance breach | Turns PR6 from a threshold into an alerting system that survives judge noise [S34] | Low | **Build this quarter** |
| **3** | **W3-30 to W3-33** — FinOps accounting | Makes share-of-savings pricing (A5) defensible | Low technically, high commercially [S38] | **Build with the control plane** |
| **4** | **W3-14 to W3-18** — deferral-attack defences | Removes a named risk (R7) at low cost, since W3-16 reuses W2-45 | Low | **Build W3-15 and W3-16 only** |
| **5** | **W3-1 to W3-5** — prefill-activation routing | The only structurally exclusive signal class [S10]; the only path that could clear the plateau | **High** — one paper, serving-engine integration, no reproduction | **Research track, explicitly labelled; never a launch claim** |
| 6 | W3-10 to W3-13 — speculative decoding | W3-13 is a free difficulty signal in any deployment already speculating | Medium — also the clearest commoditisation threat [S11] | Instrument, do not build |
| 7 | W3-19 to W3-21 — latency axis | A third axis buyers will eventually ask for | Declared non-goal year one | Defer, deliberately |

## Recommended next 3

1. **Move W3-13 to the first benchmark run.** If the pool already runs speculative decoding, draft-acceptance rate is a per-request difficulty signal that costs one exported metric — the cheapest untested routing signal available, and it is testable before any classifier is trained.
2. **Label prefill-activation routing as a research track in every artifact that mentions it.** It is one paper [S10] and a serving-engine patch. Stated as a differentiator it is the single most falsifiable claim in the pack; stated as a bet it is the most interesting one.
3. **Implement W3-15 (per-tenant escalation budgets) before any public endpoint exists.** [S9] makes bill inflation a semantics-preserving perturbation away, and a rate cap is an afternoon of work now versus an incident later.

<!-- critic: unresolved — none outstanding after round 2. -->
