# CAMIR — Technique × feature matrix

**What this is** — the mapping from the 139 techniques catalogued in waves 1–3 to the 20 flagship features, at cluster granularity, with two lists that matter more than the grid: the technique clusters that power nothing (**orphans**) and the features that rest on no technique at all (**unsupported**).
**Why it exists** — a 139-technique arsenal invites two opposite self-deceptions. The first is that breadth is depth: every cluster is claimed to power everything, the matrix fills up, and it says nothing. The second is that the features which decide whether CAMIR is adopted must be the algorithmically interesting ones. **The second turns out to be false here, and that is this document's main finding**: the four features that most determine whether a deployment survives contact with a real organisation have essentially no algorithmic content, and a roadmap ranked by technical interest would build them last.
**How to read it** — skip the grid and read §3 (orphans) and §4 (unsupported) first; the grid is evidence for them. A skeptic should attack §4's claim that a feature with no technique behind it is nonetheless P0.
**Depends on / feeds** — depends on [wave1.md](wave1.md), [wave2.md](wave2.md), [wave3.md](wave3.md), [decision_tree.md](decision_tree.md), [../../product/features_flagship.md](../../product/features_flagship.md); feeds [../not_vaporware.md](../not_vaporware.md), [../../product/features_prioritized.md](../../product/features_prioritized.md) and [../../validation/mvp_definition.md](../../validation/mvp_definition.md).

**Counting rule.** ● = the cluster is load-bearing; the feature does not work without it. ○ = contributing but replaceable. blank = no relationship. **A cluster marked ● on more than four features has almost certainly been over-claimed** and each one below is checked against that.

---

## 1. The grid — wave 1 (48 techniques, 10 clusters)

| Cluster | F1 ceiling | F2 disqual | F3 cost axis | F4 artifact | F5 corpus | F6 non-nested | F7 tolerance | F9 shadow | F12 ledger | F13 cascade | F14 gate | F15 escalation | F16 classifier | F18 judge | F19 drift | F20 diff |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **A** Cascade construction (6) | | | | | | | | | | ● | ○ | ● | | | | |
| **B** Learning to defer (5) | | | | | | | | | | ○ | ● | | | | | |
| **C** Calibration (5) | | | | | | | | | | | ● | | ○ | | ○ | |
| **D** Confidence measures (7) | | | | | | | | | | | ● | | ● | | | |
| **E** Preference-model routing (5) | | | | | | | | | | | | | ● | | | |
| **F** Clustering / instance (3) | | | | | | | | | | | | | ○ | | | |
| **G** Surface heuristics — *documented failures* (4) | | | | | | | | | ○ | | | | | | | |
| **H** Budgeted / anytime (3) | | | | | | | ● | | | | ○ | | | | | |
| **I** Verification without a judge (4) | ○ | | | ● | | | | | | | | | | ● | | |
| **J** Benchmark construction (6) | ● | ● | | | ● | ● | | ○ | | | | | | | | ○ |

## 2. The grid — waves 2 and 3 (91 techniques, 20 clusters)

| Cluster | F1 | F2 | F3 | F4 | F6 | F7 | F9 | F11 | F12 | F13 | F14 | F15 | F16 | F18 | F19 | F20 |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **W2-A** Conformal (6) | | | | | | ● | | ○ | | | ● | | | | ○ | |
| **W2-B** Latent-ability / IRT (5) | ○ | | | | ● | | | | | | | | ○ | | | |
| **W2-C** Bandits / online cost-aware (5) | | | | | | | | | | ○ | | | ○ | | | |
| **W2-D** Multi-objective frontier (5) | ● | ○ | ○ | | | ○ | | | | | | | | | | ● |
| **W2-E** Off-policy / counterfactual (5) | | | | | | | ● | | ● | | | | | | | |
| **W2-F** Judge-protocol engineering (7) | ● | | | ○ | | | | | | | | | | ● | ○ | |
| **W2-G** Artifact instrumentation (5) | ● | ○ | | ● | | | | | | | | | | ○ | | |
| **W2-H** Cost-axis derivation (5) | | ● | ● | | | | | ● | | | | ○ | | | | ○ |
| **W2-I** Drift detection (4) | | | | | | | | ○ | | | | ○ | | | ● | ● |
| **W2-J** Adverse selection post-cache (3) | ○ | | | | | | | | ○ | | | | | | | |
| **W3-A** Internal-state signals (5) | | | | | | | | | | | ○ | | ● | | | |
| **W3-B** Model introspection (4) | | | | | | | | | | | ○ | | ○ | | | |
| **W3-C** Speculative decoding (4) | | | | | | | | | | | ○ | | | | | |
| **W3-D** Adversarial robustness (5) | | | | | | | | ● | | | ● | ● | | | ○ | |
| **W3-E** Latency third axis (3) | | | | | | | | | | | | | | | | |
| **W3-F** Signal detection theory (5) | | | | | | | | | | ● | ● | ○ | | | | |
| **W3-G** Hedging / option pricing (3) | | | | | | ○ | | | | | | | | | | |
| **W3-H** FinOps counterfactual (4) | | ○ | ● | | | | | | ● | | | | | | | |
| **W3-I** Statistical process control (4) | | | | | | | | ● | | | | ○ | | | ● | |
| **W3-J** Sequential / shadow experimentation (4) | | | | | | | ● | | ○ | | | | | | | |

---

## 3. Orphan clusters — techniques powering nothing shipped

Six clusters, ~24 techniques, carry no ● anywhere. Each is a finding rather than an oversight, and each has a different reason.

| Cluster | Why it is orphaned | Is that correct? |
|---|---|---|
| **W1-G** Surface heuristics (4) — length thresholds, keyword rules, static endpoint assignment | **Orphaned by design.** These are the four incumbent workarounds CAMIR exists to replace. They appear only as the fixed-model baseline the frontier is measured against | **Yes.** An arsenal that catalogues only what it uses cannot describe what it beats |
| **W3-E** Latency as a third axis (3) | Non-goal N6. A third axis triples the frontier's dimensionality before the two-dimensional one has been measured once [S12] | **Yes, for year one.** [S12] is 2026 work and this will not stay orphaned |
| **W2-C** Bandits and online cost-aware routing (5) | CAMIR v1 fits thresholds offline on held-out labels. Online exploration means deliberately routing some requests badly to learn — a cost paid in *someone else's quality*, which is precisely what Ravi's veto exists to prevent | **Yes, and it is the most interesting orphan.** Exploration is technically superior and organisationally forbidden |
| **W3-C** Speculative decoding (4) | An implicit cascade *inside* the serving engine. It changes what a tier costs, not which tier is chosen. Only W3-13 (acceptance-rate telemetry) crosses into CAMIR, as a difficulty signal | **Yes.** Owning it would put CAMIR inside the serving layer, which N2 forbids |
| **W3-G** Hedging / option pricing (3) | A tolerance is economically an option on quality, and mirrored-traffic hedging is a real design. Nothing in v1 uses it beyond framing | **Provisionally.** This is the one orphan that looks like an unexploited idea rather than a deliberate exclusion |
| **W2-J** Adverse selection post-cache (3) | The corrections exist; nothing validates them. Gap [G4]: nobody has measured routing savings on post-cache traffic [S36] | **No — this is a gap, not a decision.** It is the cluster most likely to be needed and least likely to work as written |

---

## 4. Unsupported features — the finding that matters

Four of the twenty flagship features have **no technique behind them at all**, which is why they do not appear as columns in the grids above.

| Feature | Techniques | What it actually is |
|---|---|---|
| **F8** Tier decision stamped on every trace | **none** | Six OpenTelemetry span attributes and a stable naming contract |
| **F10** Unilateral pin-to-large | **none** | A lookup read before tier selection, and a policy decision that nobody may override it |
| **F17** Ingress proxy | **none** | An OpenAI-compatible HTTP endpoint |
| **F7** Per-endpoint tolerance ownership | W1-38, W2-A only *partly* | A `NOT NULL` column, an append-only table, and a refusal to enforce without a name. Conformal risk control turns the declared number into a guarantee, but the *ownership* has no algorithmic content whatever |

**These four are the P0 set.** [../../product/PRD.md](../../product/PRD.md) G2 ranks them above classifier accuracy; [../../strategy/value_prop_canvas.md](../../strategy/value_prop_canvas.md) ranks them above it for all three deciding personas; and [../../product/journeys/day_in_life.md](../../product/journeys/day_in_life.md) turns entirely on F8, a feature that fires 180,000 times a day and is read once a quarter.

**The finding, stated plainly.** The features that decide whether CAMIR survives contact with a real organisation contain almost no algorithm, and the cluster with the deepest literature behind it — W1-D/W1-E, seventeen techniques of confidence estimation and preference-model routing — powers **F16, the feature CAMIR explicitly declines to claim superiority on** (N7, because [S4] would falsify it in one citation). A roadmap ranked by technical interest builds exactly the wrong four things first, and it would look rigorous doing it.

---

## 5. Concentration check — which features rest on the most machinery

| Feature | Load-bearing clusters | Reading |
|---|---|---|
| **F14** Confidence gate | W1-B, W1-C, W1-D, W2-A, W3-D, W3-F | **Six.** The most technically dense feature in the product, and the one whose failure mode is quiet — a miscalibrated gate escalates 70% of traffic and never errors |
| **F1** Oracle ceiling probe | W1-J, W2-D, W2-F, W2-G | Four, from four different disciplines. This is the contribution |
| **F18** Judge harness | W1-I, W2-F | Two, but W2-F alone is seven techniques and is a *protocol*, not a choice |
| **F16** Classifier route | W1-D, W1-E, W3-A | Three, and expected to produce a null result [S4][S8] |
| **F13** Cascade route | W1-A, W3-F | Two. The primary route is the simpler one, which is the point |
| **F8, F10, F17** | **zero** | See §4 |

**F14 is the concentration risk.** Six clusters converge on one component that sits on the critical path of every request meant to be made cheaper, and whose degradation is invisible without escalation-rate monitoring. That is the single strongest argument for shipping the forced-deferral detector and the escalation-rate charts *with* the gate rather than after it.

---

## Recommended next 3

1. **Ship the four unsupported features first.** They have no research risk, no literature dependency and no failure mode that needs a benchmark — and they are the ones that decide whether the algorithmically interesting half ever gets used. Building them last is the default outcome of every technically led roadmap, and it is the mistake this matrix exists to prevent.
2. **Resolve W2-J's orphan status with a measurement, not a decision.** It is the only cluster orphaned by a gap rather than a choice: routing prices post-cache traffic, that traffic is adversely selected toward the hard end [S36], and nobody has measured the residual [G4]. One design partner running a cache answers it, and the answer moves the whole savings estimate in [../../financials/unit_economics.md](../../financials/unit_economics.md).
3. **Revisit W2-C bandits once tolerance ownership is proven in the field.** Online exploration is the strongest technical upgrade available to the gate and it is currently blocked by an organisational constraint. If per-endpoint tolerance ownership genuinely holds, a *bounded* exploration budget an owner consents to becomes negotiable — which turns the most interesting orphan into a roadmap item rather than a permanent exclusion.
