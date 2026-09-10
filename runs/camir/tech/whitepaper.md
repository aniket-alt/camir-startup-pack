# CAMIR — The Mechanism Whitepaper

**What this is** — the arithmetic behind CAMIR's claim: what a mixed-difficulty request stream costs today on a self-hosted model pool, which named frictions produce that cost, what mechanism removes each one, and what multiple survives when the conservative end of every band is multiplied together.
**Why it exists** — the pack's riskiest move is quoting somebody else's headline. RouteLLM's 3.66× is an MT-Bench number that falls to 1.41× on MMLU [S2], and FrugalGPT's 98% was measured under 2023 hosted price ratios [S3]. If this file does not do the arithmetic on **self-hosted, post-cache, coverage-limited** traffic, the deck inherits a borrowed multiple, a partner falsifies it in one search, and the pack loses the benefit of having been honest everywhere else.
**How to read it** — §2.6 is the assembled arithmetic; read it first, then walk back into §1 to check the frictions. A skeptic should attack the four inputs in §2.6 — tier cost ratio, escalation rate, harness-repair delta, routed coverage — and then read §4, which already concedes the two corrections that shrink the answer most.
**Depends on / feeds** — depends on [../research/survey.md](../research/survey.md), [../research/capability_table.md](../research/capability_table.md), [../research/sources.md](../research/sources.md), [../strategy/positioning.md](../strategy/positioning.md); feeds [deep_dives.md](deep_dives.md), [architecture/00_INDEX.md](architecture/00_INDEX.md), [not_vaporware.md](not_vaporware.md), [../narrative/vc_memo.md](../narrative/vc_memo.md) and [../financials/unit_economics.md](../financials/unit_economics.md).

---

## Executive summary

A platform team serving mixed-difficulty traffic from one large tier overpays on the easy majority. That much is settled and has been since FrugalGPT [S3]. What is *not* settled — and what nobody has published — is the size of the prize on a **self-hosted open-weight pool** after semantic caching has taken the easy repeats [S36], after the tier cost spread compressed [S29], and after the product engineers who bear the quality risk have pinned their endpoints back to the large tier [S21].

This whitepaper does that arithmetic. The answer is **~1.5× on token-proportional GPU cost in the base case (≈33% reduction) at a declared quality tolerance of ≤2 percentage points, with a conservative floor of 1.08× and an optimistic ceiling of 2.84×**. Every input is a factor a reader can change and recompute (§2.6).

That is not a 10× claim and this document will not make one. Three of the four mechanisms below are already in the literature and are not CAMIR's to claim. The one that is unclaimed is the third: **a meaningful share of what a team will measure as "the small tier cannot do this" is its own harness**, and repairing the harness moves the frontier without touching the router [S5]. The routing plateau result says the routing *decision* is near its ceiling under current signals — 21 methods converging into a narrow band, best remedies worth up to 2.13 percentage points [S4]. The unsolvability-ceiling result, published a month later, says the routing *measurement* is broken in ways worth more than 2.13 points: truncation under fixed generation budgets in **65% of MMLU and 57% of MedQA cases**, **5–12% parse failures on MMLU**, judge bias toward verbosity over correctness [S5].

**The claim is therefore not "our router is smarter." It is "your frontier is measurable on your pool, and a meaningful part of the apparent ceiling is your harness."**

---

## 1. The current inefficiency, decomposed

Five named frictions. Magnitudes are sourced where a source exists and flagged where none does. The order matters: F1 is the money, F2–F5 are the reasons nobody collects it.

### F1 — Uniform-tier overpayment on a non-uniform stream

Every request in an endpoint goes to the tier chosen once for the hardest request in that endpoint. The tax is per request, invisible per request, and nothing ever fails — which is why it persists (`../BRIEF.md` §Problem).

**Magnitude.** FrugalGPT escalated only **16.6% of queries** to GPT-4 while matching its performance [S3]; the complement is the routable share in that setting. RouterBench's **405,467 inference outcomes across 14 models** formalise the same fact as a cost-quality plane in which one model is a point and a router is a curve [S7]. No equivalent measurement exists for a self-hosted open-weight pool — that is gap [G2], and it is why this whitepaper computes rather than cites.

**Carried forward:** routable share `p` = 0.40 / 0.55 / 0.70 (low/base/high) `(assumption: FrugalGPT's 83% complement [S3] discounted heavily for compressed self-hosted tier spreads and post-cache adverse selection; no measurement exists [G2])`.

### F2 — The tolerance is never declared, so it is implicitly zero at maximum price

A fixed-model policy states no quality tolerance at all. Marcus picked the 70B once and has an A/B test from March that is two model upgrades stale (`../strategy/personas.md` P2). Priya picked hers six months ago and has one rule: over 2,000 characters gets the big model (P1). Neither has a method, and neither has a number.

**Magnitude.** Not a percentage — a **duration**. This friction sets how long F1 runs uncorrected, and the observed answer in both persona cards is "indefinitely, until someone is told to cut 20%." It persists because the alternative is not available: a 2026 paper argues directly that router evaluations across papers are **not comparable** [S13], so a team cannot look the number up. They guess, and the guess is the fixed-model baseline.

### F3 — Instrumentation error inflates apparent difficulty, and the inflation is one-directional

When a team *does* measure, it measures against its own harness, and 2026 audited what those harnesses do. Truncation under fixed generation budgets affected **65% of MMLU and 57% of MedQA cases**; output-format mismatch caused **5–12% parse failures on MMLU**; judges are biased toward verbosity over correctness — across **206,000 query-model pairs** on Gemma 4 and Llama 3.1 families [S5]. Independently: judge test-retest same-verdict rates are **>95% at temperature 0 but ~70% at temperature 1**, position bias produces **~40% GPT-4 inconsistency**, verbosity bias inflates by **~15%** [S34]. Inter-judge agreement is only **~76%**, so judge *choice* may damage validity more than judge randomness [S33].

**Why it is one-directional, and therefore compounding.** Truncation, parse failure and verbosity bias all penalise the *terser, smaller* tier. A broken harness does not add noise symmetrically; it systematically understates `p`, which makes routing look less valuable than it is, which discourages the measurement that would have found the error.

**Carried forward:** harness-repair recovery `h` = 0.03 / 0.06 / 0.12 of traffic reclassified from "small fails" to "small succeeds" `(assumption: anchored on the most conservative single artifact in [S5] — the 5–12% MMLU parse-failure band — and deliberately ignoring the truncation and verbosity findings, which are larger)`.

### F4 — Utilisation waste, which can make a naive pool cost *more*

A self-hosted tier is billed by the hour, not the token. **A GPU idle at 10% utilisation costs 10× per token** [S27]. And CAMIR's tiers are **distinct base models**, not adapters: vLLM multi-LoRA reduces per-model memory overhead to megabytes only because adapters share one base model [S31], which small/mid/large tiers do not. Each tier needs separate resident VRAM (`../research/capability_table.md` C2).

**Magnitude.** Adding a small tier that receives 30% of a stream but occupies its own accelerator can raise blended cost per token rather than lower it. This is the friction that turns a routing project into a negative result, and no routing paper models it.

**Carried forward:** a **guard**, not a gain — see §2.4.

### F5 — Unattributable regression risk, which converts routable traffic into pinned traffic

The saving lands in Dana's budget; the risk lands on Ravi, who is in a different reporting line, gains nothing, and can pin his endpoint to the large tier without filing a ticket (`../strategy/personas.md` P5, `../strategy/value_prop_canvas.md` Canvas 3). This is not hypothetical: when a frontier vendor shipped routing as a default with a vendor-set tolerance, users publicly reported complex queries degraded by being sent to the smaller model, with no dial [S20][S21].

**Magnitude.** Ravi's current workaround *is* endpoint pinning — workaround #3 in `../BRIEF.md`. Pinning applies a coverage multiplier to everything above it.

**Carried forward:** routed coverage `k` = 0.60 / 0.80 / 0.90 `(assumption: no deployment data exists; the low end assumes the two highest-visibility endpoints pin out, the high end assumes shadow-mode evidence converts them)`.

### How they compound

The frictions are not additive; they are a chain of multiplications on the routable share, and three of the four terms are less than one.

```
realised harvest  =  p_true  ×  (1 − artifact discount)  ×  coverage  ×  utilisation guard
                       F1              F3                     F5             F4
```

With base-case inputs, a *broken* harness and *no* attribution surface:

`0.55 × (1 − 0.11) × 0.60 = 0.29` of traffic actually routed down — against a true routable share of 0.55. **Nearly half the prize is lost to instrumentation and organisational friction before the router's accuracy is in question.** F2 explains why this is never noticed and [S13] explains why it cannot be looked up.

**This is the whole argument.** The field has spent three years and 21 published methods on the router's accuracy and bought up to 2.13 percentage points [S4]. The chain above says the larger losses are elsewhere.

---

## 2. Mechanisms, evidence, multipliers

Notation. Costs are normalised to the large tier: `c_l = 1`. Tier cost ratio `r = c_s / c_l`.

**Where `r` comes from.** A single H100 at high-concurrency batching serves gpt-oss-120b at **~$0.10 per million tokens raw GPU cost**, against ~$0.60 per million output tokens for the identical model hosted [S26]. Token cost on a saturated accelerator scales roughly with active parameters `(assumption: linear-in-active-parameters, standard for compute-bound batched decode; not measured here)`. An 8B-class small tier against a 120B-class large tier gives `r ≈ 0.10`; a 31B-class small tier — which is what [S29] says you need for the small tier to be non-embarrassing — gives `r ≈ 0.25`.

**Carried forward:** `r` = 0.25 / 0.15 / 0.10 (conservative / base / optimistic). **Note the direction: the conservative case is the one where the small tier is good, because a good small tier is a big small tier.**

### 2.1 Mechanism M1 — the cascade route: pay for the small attempt, escalate on calibrated low confidence

**Removes:** F1. **Primary route.**

**Mechanism.** Send every request to the small tier. Score the completion with a calibrated uncertainty measure — maximum softmax probability, margin, predictive entropy, or distance-to-uniform. Escalate only above threshold. See [deep_dives.md](deep_dives.md) §1 and [architecture/D02.md](architecture/D02.md).

**Evidence.** FrugalGPT's cascade formulation reached 50–98% savings with **16.6% escalation** [S3]. Critically, [S8] finds that **simple confidence measures route as well as trained routing models** — so the escalation signal is a by-product of a generation already paid for, and the mechanism does not depend on solving the hard prediction problem (`../research/survey.md` §2.2–2.3).

**The arithmetic, which is exact rather than estimated:**

```
cost_cascade / cost_baseline  =  r + e          (e = escalation rate)
```

Two structural consequences fall straight out:

1. **The cascade beats the baseline iff `e < 1 − r`.** At `r = 0.10` that is `e < 0.90`; at `r = 0.25` it is `e < 0.75`. Generous, but not unconditional — and the condition tightens exactly as the tier spread compresses [S29].
2. **The cascade can never beat `1/r`.** At `r = 0.25` the hard ceiling is 4×, before any escalation at all. **Any cascade claim above 4× on a modern self-hosted pool is arithmetically impossible**, which by itself disposes of quoting FrugalGPT's 98% [S3] into a 2026 deck.

| `e` | at `r`=0.25 | at `r`=0.15 | at `r`=0.10 |
|---|---|---|---|
| 0.30 | 1.82× | 2.22× | 2.50× |
| 0.45 | 1.43× | 1.67× | 1.82× |
| 0.50 | 1.33× | 1.54× | 1.67× |
| 0.65 | 1.11× | 1.25× | 1.33× |

**Multiplier M1: 1.11×–2.50×, base 1.54×** at `e = 0.50`, `r = 0.15`. `e` is set at 3× FrugalGPT's measured 16.6% [S3] to price in compressed spreads and post-cache adverse selection.

**A three-tier cascade adds less than intuition suggests.** With `c_s = 0.10`, `c_m = 0.40`, `c_l = 1.0` and `e₁ = e₂ = 0.5`: `0.10 + 0.5(0.40 + 0.5·1.0) = 0.55` versus `0.60` for the two-tier at the same `e`. **1.82× against 1.67× — a 9% improvement for a second resident model and a second escalation threshold to calibrate.** A real finding, and it argues for shipping two tiers first (D05, `not_vaporware.md`).

### 2.2 Mechanism M2 — the classifier route as ablation: dispatch once, pay no failed attempt

**Removes:** the cascade's own overhead, on the subset where difficulty *is* predictable.

**Mechanism.** Predict difficulty from prompt features before generation and dispatch once. See [deep_dives.md](deep_dives.md) §2 and [architecture/D03.md](architecture/D03.md).

**Evidence, stated against the mechanism.** This is the weaker of the two routes and the pack says so. 21 routing methods across 5 benchmarks converge into a narrow band far below the oracle router, diagnosed as a **predictability bottleneck**: routers learn globally averaged model-performance trends rather than query-specific signal. More data, stronger encoders and encoder fine-tuning bought **up to 2.13 percentage points** [S4].

**The arithmetic, which separates cost from quality — the reason to keep the route at all:**

```
cost_classifier / cost_baseline  =  q·r + (1 − q)        (q = share dispatched small)
quality_loss (pp)                =  q · (a_l − a_s|sent-small)
```

**Cost depends only on `q`. Quality depends only on the classifier's precision on the sent-small set.** A perfect router and a coin flip that both send 40% small cost exactly the same; they differ entirely in what they break. So the classifier does not buy savings — **the tolerance buys savings, and classifier precision is what lets you set the tolerance without paying for it.**

Worked, at `r = 0.15`, baseline accuracy `a_l = 0.85` `(assumption: illustrative; the real value is measured per pool)`:

| `q` | cost ratio | multiple | small-tier accuracy on sent-small needed for ≤2pp loss |
|---|---|---|---|
| 0.30 | 0.745 | 1.34× | ≥ 0.783 |
| 0.40 | 0.660 | 1.52× | ≥ 0.800 |
| 0.55 | 0.533 | 1.88× | ≥ 0.814 |

**Multiplier M2: 1.34×–1.88×, base 1.52×** at `q = 0.40` — *conditional on the classifier hitting the precision in the right-hand column*, which is exactly where [S4]'s plateau bites.

**The bar this route must clear is not the fixed-model baseline.** It is **calibrated confidence** [S8], because free confidence already routes as well as trained routers. M2 is reported as an ablation against M1, per `../research/survey.md` §Recommended next 3. If it does not beat calibrated confidence, CAMIR ships the cascade and says so — which costs the pack a paragraph, not a thesis.

### 2.3 Mechanism M3 — harness repair: recover the routable share the instrumentation was hiding

**Removes:** F3. **This is the only one of the four mechanisms that is CAMIR's.**

**Mechanism.** Four instrumented fixes, each aimed at a named artifact in [S5] and [S34]: generous generation budgets with per-request truncation counters; strict output-format parsing with parse-failure counters rather than silent scoring-as-wrong; length-controlled judging; temperature-0 multi-judge scoring with position permutation and **published inter-judge agreement**. See [deep_dives.md](deep_dives.md) §3 and [architecture/D04.md](architecture/D04.md).

**Evidence.** [S5], across 206,000 query-model pairs: truncation in **65% of MMLU / 57% of MedQA** cases, **5–12% MMLU parse failures**, verbosity-biased judging. [S34]: verbosity bias ~15% inflation, position bias ~40% inconsistency, temperature-0 verdict stability >95%. [S33]: ~76% inter-judge agreement, and a 2026 RAND study finding no judge uniformly reliable across benchmarks.

**The arithmetic.** Harness repair reduces `e` (cascade) and raises the achievable `q` at fixed tolerance (classifier), by reclassifying `h` of traffic from "small fails" to "small succeeds":

`e' = e − h`, so `uplift = (r + e) / (r + e − h)`.

| `h` | at `r`=0.15, `e`=0.50 | uplift |
|---|---|---|
| 0.03 | 0.65 → 0.62 | **1.05×** |
| 0.06 | 0.65 → 0.59 | **1.10×** |
| 0.12 | 0.65 → 0.53 | **1.23×** |

**Multiplier M3: 1.05×–1.23×, base 1.10×.**

A modest multiplier, stated modestly on purpose — `h` is anchored on [S5]'s **smallest** reported artifact with truncation and verbosity ignored. Its strategic weight exceeds its size for two reasons. First, it is **additive to any router, including a competitor's**, which is what makes the harness the asset rather than the router (`../strategy/positioning.md`). Second, the same instrumentation is what makes a frontier reproducible at all: a frontier published without judge-agreement statistics is not a frontier, it is a chart.

### 2.4 Mechanism M4 — coverage: attribution, shadow mode and per-endpoint tolerance

**Removes:** F5, and guards F4.

**Mechanism.** Per-request tier decision stamped on every trace; shadow mode computing and logging decisions while all traffic still goes to the baseline; per-endpoint quality tolerance owned by the endpoint's engineer; unilateral pin-to-large with one flag and no ticket. P0 in `../strategy/value_prop_canvas.md`, ranked above classifier accuracy; [architecture/D08.md](architecture/D08.md) and [architecture/D10.md](architecture/D10.md).

**Evidence.** The failure this prevents is documented, not hypothesised [S21]. The ranking is derived: attribution + shadow mode is the **#1 fit for Marcus**, per-endpoint tolerance the **#1 fit for Ravi**, an auditable report the **#1 fit for Dana** — three personas, three different top fits, **none of which is a cheaper bill** (`../strategy/value_prop_canvas.md` §The finding).

**The arithmetic.** Coverage `k` blends routed and pinned traffic:

`blended ratio = k · (routed ratio) + (1 − k) · 1`

At a routed ratio of 0.59 (M1 base with M3 applied): `k=0.60 → 0.754` (1.33×); `k=0.80 → 0.672` (1.49×); `k=0.90 → 0.631` (1.58×).

**Multiplier M4: it creates no saving; it decides what fraction of the saving survives contact with the organisation — a 1.33× → 1.58× swing on identical routing performance.**

**And the F4 guard.** M4's observability is what enforces the utilisation rule: a tier is admitted to the pool only if its measured batch occupancy keeps per-token cost inside the `r` assumed on the frontier. At 10% utilisation the small tier costs 10× per token [S27] and `r` inverts — the "cheap" tier becomes the expensive one. **D05 and `not_vaporware.md` treat this as an admission criterion, not a monitoring nicety; it is the single most likely way a real CAMIR deployment produces a negative result.**

### 2.5 What CAMIR is *not* claiming a multiplier for

Stated here so the assembled arithmetic cannot be read as hiding them.

- **Semantic caching.** 20–45% production hit rates [S36]; declared out of scope in `../BRIEF.md` §Wedge. Upstream, composes, and *already deducted* from the inputs below.
- **Latency.** The cascade roughly doubles latency on escalated requests. Latency-aware routing is a live axis [S12] and a declared year-one non-goal.
- **Prefill-activation routing** [S10]. The one signal class a hosted competitor structurally cannot use, and therefore the highest-upside direction available. It requires a serving-engine patch, so it is **labelled research risk and carries no multiplier here** ([deep_dives.md](deep_dives.md) §7, [architecture/D01.md](architecture/D01.md)).

### 2.6 The assembled arithmetic — multiply it back yourself

Cascade route (primary), all four mechanisms composed. The conservative column uses the conservative end of **every** input simultaneously.

| Input | Conservative | Base | Optimistic | Basis |
|---|---|---|---|---|
| `r` tier cost ratio | 0.25 | 0.15 | 0.10 | [S26] anchors + linear-in-active-params `(assumption)`; conservative = 31B small tier [S29] |
| `e` escalation rate, post-cache | 0.65 | 0.50 | 0.30 | `(assumption: FrugalGPT's 16.6% [S3] degraded 2–4× for compressed spread and cache adverse selection [S36][G4])` |
| `h` harness recovery | 0.03 | 0.06 | 0.12 | `(assumption: [S5]'s 5–12% MMLU parse-failure band, most conservative artifact only)` |
| `k` routed coverage | 0.60 | 0.80 | 0.90 | `(assumption: no deployment data; [S21] is the failure being priced)` |

```
routed ratio   = r + e − h
blended ratio  = k · (routed ratio) + (1 − k)
multiple       = 1 / blended ratio
```

| | Conservative | Base | Optimistic |
|---|---|---|---|
| routed ratio | 0.25 + 0.65 − 0.03 = **0.870** | 0.15 + 0.50 − 0.06 = **0.590** | 0.10 + 0.30 − 0.12 = **0.280** |
| blended ratio | 0.60(0.870) + 0.40 = **0.922** | 0.80(0.590) + 0.20 = **0.672** | 0.90(0.280) + 0.10 = **0.352** |
| **multiple on token-proportional GPU cost** | **1.08×** | **1.49×** | **2.84×** |
| **cost reduction** | **8%** | **33%** | **65%** |

**Headline: ~1.5× (33% reduction) on token-proportional GPU cost at a declared ≤2pp quality tolerance; corridor 1.08×–2.84×.**

Two readings a skeptic should take from the table rather than from the headline:

1. **Conservative × conservative is 1.08×, and 1.08× does not pay for a control plane.** That is the finding, not a presentation flaw. The conservative column is the world in which the small tier had to be a 31B to be usable [S29], caching already took the easy third [S36], two-fifths of traffic pinned out [S21], and harness repair recovered almost nothing. **In that world CAMIR reports a null result and says so** — the walk-away condition `../BRIEF.md` already declares, and the reason the oracle ceiling is measured in week one rather than month six.
2. **The single largest swing factor is `k`, not the router.** Holding routing constant at base, `k` alone moves the answer from 1.33× to 1.58×. **Coverage is an organisational variable, and CAMIR's P0 features are aimed at it** — which is why `../strategy/value_prop_canvas.md` ranks attribution above classifier accuracy, and why an architecture that led with the classifier would be optimising the wrong term.

---

## 3. Full-spectrum applicability — one router, three configurations

The arithmetic above is the same for all three edges. What changes is who sets the inputs and who reads the output.

**Edge-low — Priya** (60-person B2B SaaS, two open-weight models on two rented GPUs, self-hosting for a contractual data-residency reason rather than for price [S28]). She never sets `q` or a threshold. She has `k = 1.0` because there is one endpoint and she owns it, and `r ≈ 0.15` because her two models are an 8B and a 70B. Her entire interaction: point the client at a different port, run shadow mode for a day, read one number. **The mechanism she consumes is M4** — the shadow-mode report — and the saving is a consequence. Her stated objection ("if this silently makes answers worse, I find out from a support ticket") is answered by M4, not by M1. If she has to tune anything, CAMIR has failed her.

**Beachhead — Marcus** (~400 people, six product teams on a shared inference service, ~$50k/month). The only one of the three for whom the full chain runs. `k < 1` because Ravi exists; genuine mixed difficulty *inside* a single endpoint, which is what defeats endpoint-level assignment; and the volume for 33% to be a number his director notices. **He consumes M1 + M3 + M4 and treats M2 as an experiment.** His stated demand is exactly §2.6: *"show me, on my traffic, what it costs at 99% of current quality and at 95%. Then I'll pick, and I'll be the one who picked."*

**Edge-high — Wen** (five tiers including a fine-tuned specialist, an internal judge, already tried and abandoned RouteLLM because two tiers were not enough and the MT-Bench numbers did not reproduce [S1][S2]). She replaces every component: her tiers, her judge, her cost model. **What she consumes is M3 alone — the harness and the inter-judge agreement statistics** — and her stated bar is that a frontier published without judge agreement is noise. Her other objection is that she could build this in three weeks, which is true; the answer is that her last internal router died of maintenance, not of capability.

**And the two non-users who decide.** Dana consumes M4's savings attribution because she is buying verification, not routing. Ravi consumes M4's per-endpoint tolerance and pin-to-large because he is buying control. **Neither is buying a lower bill.** One system serves all five through configuration surface, not through separate products — the position `../BRIEF.md` §Users takes.

---

## 4. What this is not

### 4.1 The multiple is on token-proportional GPU cost, not on the all-in inference line

The largest single correction in this document, and it belongs first. **Realistic all-in self-hosted cost is 3–5× raw GPU rental** once engineering time is counted [S27]. Engineering time does not scale with tokens routed. If raw rental is 20–33% of all-in, a 33% cut on the raw line is **7–11% of the all-in line — an all-in multiple of roughly 1.08–1.12×**. And CAMIR *adds* to the non-token side: a proxy to operate, thresholds to recalibrate, a benchmark to re-run on every model upgrade.

Two things keep this from being fatal. First, the token-proportional share rises with volume, which is precisely the definition of the beachhead — Marcus at $50k/month has a very different ratio from Priya. Second, the honest unit is **cost per request**, not total spend, which is what Dana needs anyway to separate growth from efficiency (`../strategy/value_prop_canvas.md` Canvas 2, PR-D2). `../financials/unit_economics.md` inherits this correction and must not quote §2.6 against an all-in denominator.

### 4.2 The tier cost spread is compressing, and it compresses against us

Gemma 4 31B-thinking sits within **~10 LMArena Text Elo points of 600B–1000B+ open-weight frontier models at ~10× fewer parameters** (April 2026) [S29]. This is cited everywhere as CAMIR's "why now." **It is equally an argument against CAMIR**, and §2.6 encodes that honestly by making the conservative `r` the case where the small tier is *good*. If the small tier is nearly as good as the large one, two things happen at once: `r` rises toward 0.25, and the correct policy converges on "just use the 31B for everything" — a fixed-model baseline, not a router. **Routing was most valuable when the spread was widest, which was 2023, when FrugalGPT was measured** [S3]. A structural risk that worsens over time (`../research/survey.md` R4).

Note also that Elo is a *preference* metric, not a correctness metric. It does not say which requests the small tier gets wrong, which is the only thing a router needs to know.

### 4.3 Semantic caching gets there first and takes the easy traffic

Production semantic-cache hit rates run **20–45%** [S36], and hits are disproportionately the repetitive easy requests routing would have sent small. Post-cache traffic is **adversely selected toward the hard end**, so `e` is higher on cache-miss traffic than on raw traffic. §2.6's `e` inputs are already stated as post-cache — but the residual saving available to routing after aggressive caching **has never been measured by anyone**: gap [G4], assumption A14. If caching has already harvested most of it, §2.6's base case is optimistic and the conservative column is the real one. **This is cheap to measure and should be measured early** (`not_vaporware.md`).

### 4.4 The routing plateau bounds M2, permanently, on other people's evidence

21 methods, 5 benchmarks, converging into a narrow band far below oracle; best remedies worth **up to 2.13 percentage points** [S4]. Add the co-failure ceiling: across 67 frontier models failures overlap, so the oracle ceiling is itself lower than an independence assumption predicts [S15]. **CAMIR will not beat the plateau and does not claim to.** §2.2 is written so M2's savings come from `q` and the tolerance, not from classifier cleverness — the only form of the claim that survives [S4].

### 4.5 The mechanism commoditises into the serving engine

The vLLM Semantic Router vision paper describes exactly the Workload–Router–Pool decomposition CAMIR's architecture parallels [S11]. On an 18-month horizon the routing *mechanism* is plausibly free inside the serving layer, and routing is already free inside a hosted catalog with no separate fee [S20]. **The measurement layer is the asset** — mixed-difficulty benchmark, judging protocol, and a **cost axis derived for self-hosted pools, which no published benchmark has derived** (RouterBench prices against hosted API list prices [S7]; `../research/capability_table.md` C10). That derivation is unclaimed, achievable, and a named deliverable ([deep_dives.md](deep_dives.md) §4). It is not a moat; it is a head start, and `../BRIEF.md` §Moat already grades the moat as weak.

### 4.6 Numbers this document refuses to quote

- **"Up to 85% cheaper."** RouteLLM's, on MT-Bench; the same router yields **1.41× on MMLU** [S1][S2].
- **"98% cost reduction."** FrugalGPT's, under 2023 hosted price ratios [S3] — and, per §2.1, above the arithmetic ceiling `1/r` for any modern self-hosted pool.
- **Any CAMIR measurement.** There is none. No benchmark run has been completed; the origin is an SJSU CMPE 295A capstone (September 2026). The first number that will be CAMIR's own evidence is the oracle ceiling from the two-week experiment in `../BRIEF.md` §Riskiest assumption.

### 4.7 A cost-optimising router is a new attack surface

**Cascade deferral attacks use semantics-preserving perturbations to suppress small-tier confidence and force escalation** [S9] — an attacker inflates a victim's inference bill without breaking anything. `e` becomes an adversarially controlled variable, and every multiple in §2.6 is a function of `e`. Treated in [architecture/D06.md](architecture/D06.md) and `not_vaporware.md`.

---

## Recommended next 3

1. **Run the oracle-ceiling experiment and publish `p`, `e` and `h` for one pool before writing another artifact that depends on §2.6.** Every input is a factor; three of the four are `(assumption)`. Two weeks of local GPU time replaces them with measurements and simultaneously tests A1 and A2 — the two assumptions that decide whether CAMIR is a company or a null result. **If the conservative column is the true one, stop.**
2. **Measure `h` explicitly as its own result — the artifact decomposition — not as a side effect of the frontier.** Run the same benchmark twice: once with a fixed generation budget, silent parse failure and an uncontrolled judge; once instrumented per §2.3. The delta is [S5]'s finding reproduced on a self-hosted pool, publishable independently of whether CAMIR's router wins, and the only mechanism in §2 that is CAMIR's own.
3. **Measure `k` in shadow mode before quoting any saving to a buyer.** Coverage swings the answer more than the router does, and it is the cheapest input to observe: run decisions in shadow across every endpoint, count how many endpoint owners would have pinned, and report the *blended* multiple rather than the routed one. A saving quoted at `k = 1` is a saving that will not appear on Dana's bill.
