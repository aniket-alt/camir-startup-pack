# CAMIR — Critic log

**What this is** — the verdict record for the adversarial critic pass on the nine phase 3–7 drafts that shipped without one: per artifact, per critic persona, the issues raised with the line quoted, the severity, and what was done.
**Why it exists** — a critic pass that leaves no record cannot be audited, and this pack's worst errors were not in any single file but *between* files: a blended CAC that did not follow from its own channel inputs, a pricing model that four other files described as rejected, an experiment threshold set three different ways. None of those is visible from inside one document. Without this log, the next reviewer re-derives all of them — or, worse, assumes they were checked.
**How to read it** — §1 is the summary; §3's cross-document findings are the ones that mattered most. A skeptic should attack §4, which lists what was deliberately left unresolved.
**Depends on / feeds** — depends on [../../../skills/startup-critic/SKILL.md](../../../skills/startup-critic/SKILL.md), [../../../references/quality-bar.md](../../../references/quality-bar.md), [../research/sources.md](../research/sources.md), [../HANDOFF.md](../HANDOFF.md) §2; feeds [COVERAGE.md](COVERAGE.md) and every artifact listed below.

---

## 1. How this pass was run, stated honestly

| Round | Who | What was recorded |
|---|---|---|
| 0 | A handed-off agent (GitHub Copilot), commit `712241c` | Domain-correct edits to all nine files; `critic: unresolved` markers on three. **No verdicts or issue lists were recorded**, so what it raised and skipped cannot be reconstructed |
| 1 | This session, three lenses applied **sequentially by one agent** | Every verdict below. The skill calls for separate subagents per persona; three were launched and all three died on an API session limit before reading a file, so the passes were run in-session instead. That is weaker than independent critics and is stated as such |
| 2 | Same session | Re-verification: every table recomputed by script, 0 broken links, all `[Sn]` tags resolve, 11/11 Mermaid fences parse |

**Personas.** **VC** — the skeptical deep-tech investor: unsourced numbers, arithmetic that does not reconcile across files. **PhD** — ML systems and evaluation methodology: misstated methods, over-read citations. **Operator** — has scaled open-core dev tooling: unbuildable scope, secretly chronological priorities, experiments a three-person team cannot run.

---

## 2. Verdicts, per artifact

### tech/whitepaper.md — round 1: VC revise · PhD revise · Operator pass

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | PhD | major | *"F3 — …and the inflation is one-directional"* | Heading contradicted the round-0 body, which correctly downgraded direction to a hypothesis | **Fixed** — heading now says direction must be measured per tier |
| 2 | PhD | major | *"= c_small_attempt + c_gate + c_large_escalation"* followed by *"Two structural consequences fall straight out: … iff e < 1 − r"* | Round 0 replaced `r + e` with a decomposition from which the stated consequences and the table do not follow | **Fixed** — `r + e` restored, with the GPU-second decomposition shown as its derivation |
| 3 | PhD | major | *"The unsolvability-ceiling result, published a month later"* | arXiv:2605.07395 [S5] precedes arXiv:2606.07587 [S4]; the ordering is backwards | **Fixed** — temporal claim removed |
| 4 | VC | major | *"Headline: ~1.5× (33% reduction)"* | Counts harness repair `h` as a production saving; §2.3 (round 0) says it changes measured labels first | **Fixed** — `h = 0` case stated: 1.39× (28%); headline now 1.4–1.5× |
| 5 | PhD | minor | *"Any cascade claim above 4× on a modern self-hosted pool is arithmetically impossible"* | True only at `r = 0.25`; at `r = 0.10` the ceiling is 10× | **Fixed** |
| 6 | PhD | minor | *"CAMIR's tiers are distinct base models"* | A LoRA specialist on an existing base (Wen's pool) is the exception the manifest records | **Fixed** |
| 7 | VC | minor | *"deep_dives.md §1 … §7"*, *"D06"* for deferral attacks | Six cross-references predated the DD and D numbering | **Fixed** — all six repointed |
| 8 | PhD | minor | *"1.08–1.12×"* | 0.33 × 20% = 6.6% → 1.07× | **Fixed** |

Strongest element, protected: §2.6's assembled table — every cell re-derived and correct.

### financials/pricing.md — VC revise · PhD pass · Operator revise

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | Operator | minor | *"~$8–15k/mo of GPU"* (Priya) | Two rented cards at [S26] rates is ~$4–6k; the persona and journey say two GPUs | **Fixed** |
| 2 | VC | minor | *"counterfactual is expensive to compute independently … friction in CAMIR's favour"* | Sits uneasily with G3's "auditable by a party that is not CAMIR" | **Logged, not changed** — it is an honest observation, not an error |

Arithmetic verified: floor, cap (~$2.4M spend → $117.6k), repricing trigger (15% → $17,640; 2.5% × $600k = $15,000). Strongest element: the two-anchor convergence check and its admission that the anchors agree at exactly one savings rate.

### financials/revenue_build.md — VC revise · PhD pass · Operator revise

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | VC | major | *"Product 2 attaches to ~35% of the base at ~$40k incremental"* beside *"$35k × 1.06 + 0.35 × $23k ≈ $45k"* | At $40k the bend is $51k, not $45k | **Fixed** — $23k |
| 2 | VC | major | *"R2 | Gross logo churn, Y5+ | 12%/yr"* | Table applies 15% every year (Y5: 19 of 128) | **Fixed** — R2 states the table holds 15%; 12% is unbanked upside (~1,040 vs 1,001 customers at Y8, computed) |
| 3 | VC | major | *"R7 | Blended CAC, Y1–Y3 | ~$5,400"* | See §3.1 | **Fixed** — ~$4,200 |
| 4 | Operator | minor | *"see risk_matrix.md R5"* | The LiteLLM risk is R7 | **Fixed** |

All 32 cells of the eight-year table recomputed: logos, churn, ending customers and ARR reconcile.

### product/features_flagship.md — VC revise · PhD pass · Operator revise

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | PhD | major | *"Break-even escalation rate 38%"* | The whitepaper's condition `e < 1 − r` puts break-even at 75–90% for this pool | **Fixed** — 80%, with the saving at 23% escalation computed (58%) |
| 2 | VC | major | *"baseline $50,200/month, actual $31,400 … signed by Marcus"* | 37% is double the pack's own ~17.5%-of-spend derivation, and the signers must be the tolerance owners (O3) | **Fixed** — matches `day_in_life.md` 16:20 |
| 3 | Operator | minor | *"44% … tolerance of −2.0"* (Ravi) | Contradicts Ravi's 0.5 tolerance in `beachhead.md` | **Fixed** |
| 4 | Operator | minor | *"reverts … if configured"* | Auto-revert is on by default everywhere else (O4, D10) | **Fixed** |
| 5 | Operator | minor | *"12,400 requests"* | Marcus's corpus is 8,000 in his journey | **Fixed** |

### product/features_prioritized.md — Operator revise · VC pass · PhD pass

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | Operator | minor | *"12 | Cascade route … Depends on 11,13"* | Ranked above its own dependency, the confidence gate (#13), inside the same tier | **Fixed** — swapped to #12 gate, #13 cascade; all dependency columns and five prose references renumbered; 0 violations on re-check |

Mechanical audit of all 50 rows: no Now item depends on Next or Later, no dangling references.

### product/PRD.md — Operator revise · VC pass · PhD pass

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | Operator | minor | *"with an auto-revert option"* | Conflicts with O4's own "silent degradation is a defect class" and the default-on behaviour everywhere else | **Fixed** |

Round-0 edits (G1 and the loop-closure paragraph) reviewed and retained.

### tech/techniques/wave1.md — PhD revise · VC pass · Operator pass

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | PhD | major | *"preserves the argmax so it can never change a routing decision, only its confidence"* | A routing decision is a threshold on confidence; rescaling moves requests across a fixed τ, and multi-class MSP can reorder | **Fixed** — "τ must be refit after calibration" |
| 2 | PhD | minor | *"most accurate of the four"* | [S1] is licensed for serving costs only | **Fixed** |
| 3 | PhD | minor | *"Bradley–Terry win-probability router"* as a fifth router | It is the preference model inside RouteLLM's similarity-weighted router | **Fixed** |
| 4 | PhD | minor | *"the cheap baseline CAMIR must beat"* (kNN) | The bar is calibrated confidence (PR3) | **Fixed** |
| 5 | PhD | minor | *"Margin sampling"* | An active-learning query strategy; the score is the top-2 margin | **Fixed** |

Every attribution checked and correct: Chow 1970, Guo 2017, Platt 1999, Zadrozny–Elkan, Naeini (ECE), Murphy, SelectiveNet, Madras / Mozannar–Sontag, Wang (self-consistency), Kuhn / Farquhar (semantic entropy), Dean–Boddy.

### validation/riskiest_assumptions.md — VC revise · PhD pass · Operator pass

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | VC | **fatal** | *"14 | A13 — the team's test-automation background transfers"* as the only row testing E2 | The pack's central technical claim (HANDOFF §2.2) had no row; by the board's own severity × decidability × sequence rule it cannot rank below 1 | **Fixed** — row **1b** added, co-ranked with A2 because the same generations decide it |

### validation/experiment_board.md — Operator revise · VC revise · PhD pass

| # | Critic | Sev | Quote | Problem | Resolution |
|---|---|---|---|---|---|
| 1 | Operator | major | E13 *"Hypothesis: zero consuming teams escalate"* beside the round-0 threshold *"a veto itself is a promised safety control"* | Hypothesis and threshold contradicted each other | **Fixed** — hypothesis now tests attribution-within-an-hour, matching the threshold |
| 2 | VC | major | *"roughly $300"* (twice) vs `use_of_funds.md`'s $45,000 GPU line | Two unexplained figures for the same track | **Fixed** — ~120 metered GPU-hours (≈ $300–450) run on a dedicated node budgeted at $45k; both files now say so |
| 3 | VC | minor | *"E2 … rank 14"* | See riskiest_assumptions #1 | **Fixed** — rank 1b |

Strongest element, protected: the capacity check, which names interviews — not GPUs — as the binding constraint and forbids silently shrinking the denominator.

---

## 3. Cross-document findings — the ones no single-file review would catch

### 3.1 The blended CAC did not follow from its own inputs

`strategy/channel_plan.md` stated a mix of 40% A / 30% B / 20% C / 10% D at ~$0 / $8,300 / $3,000 / $6,000, and a blended **$5,400**. The marginal blend is **$3,690**; with Stack A's ~$40,000 build amortised over its ~29 Y1–Y3 customers it is **~$4,240**. No amortisation produces $5,400. **Corrected to ~$4,200 (14% of ACV)** in `channel_plan`, `revenue_build`, `unit_economics`, `use_of_funds`, `metrics_by_stage`, `HANDOFF` and `GEMINI_TASK`. Year one alone is ~$11,700. The ~2.2-month payback survives, now computed at 75% margin; `unit_economics.md` had previously labelled a 100%-margin figure as 75%.

### 3.2 Share-of-savings was described as rejected in four files; it is the value metric

`financials/pricing.md` derives the $30k ACV from a 28% share of measured savings, and `petal_diagram.md`, `sales_roadmap.md` and `gtm.md` all build on it. `pivot_log.md` K4, `deep_dives.md` DD6, `get_keep_grow.md` and `metrics_by_stage.md` said it was rejected. **What is actually killed is the vendor computing its own invoice**; share-of-savings is kept because the counterfactual runs as open code in the customer's perimeter. All four corrected. `metrics_by_stage.md` also said ACV is "fixed by design at $30,000" while the revenue build grows it to ~$95k — corrected.

### 3.3 Experiment thresholds set three ways

E4 (does the segment exist): the board says PASS ≥ 6 of 20; `stage_gate`, `discovery_guide`, `metrics_by_stage` and `use_of_funds` said ≥ 8; `pivot_log` triggered at ≤ 3. E2: board FAIL < 2pp; pivot log fired at < 5pp. **All aligned to the board**, which is where each experiment is defined.

### 3.4 Smaller cross-file drifts

`deep_dives.md` asserted a directional verbosity bias the whitepaper had correctly downgraded, and claimed temperature scaling "does not distort ranking" (same error as wave1 #1). `unit_economics.md` anchored its 25% savings rate on RouteLLM's hosted multiples, which round 0 had correctly removed from `pricing.md` — now anchored on the whitepaper's own 28–33%. Its probe cost (11 GPU-hours for 24,000 generations) and E1's (120 for 12,000) differed ~20× without either stating its batching assumption — now a stated range.

---

## 4. Unresolved, deliberately

1. **Product 2 demand** (`revenue_build.md`) — carries most terminal ARR and rests on no observation. Resolving it needs discovery, not editing. Marker retained.
2. **The 28% share rate** (`pricing.md`) — no published comparable at any specificity [S38][S39]. First thing a pricing conversation tests. Marker retained.
3. **Whether tolerance ownership stops a veto** — the four P0 features rest on one public incident [S21] and zero interviews. It is E13, and no document can close it.
4. **These critics were not independent.** One agent ran three lenses in sequence. An independent pass — separate sessions, or a different model — would likely find issues this one normalised.

---

## Recommended next 3

1. **Run an independent critic pass on `narrative/` and `financials/` before either is shown to an investor.** They are where this log's cross-document errors would have surfaced most expensively, and this pass was not independent (§4.4).
2. **Add a scripted reconciliation check to the audit**: every locked number in `HANDOFF.md` §2.5, grepped pack-wide. §3.1 and §3.3 were both caught by re-deriving arithmetic by hand, which is the slowest possible detector for the most damaging class of error.
3. **Treat `HANDOFF.md` §2.5 as derived, not binding.** The blended CAC was locked there and was wrong at source. Locked *decisions* (§2.1–2.3) should stay locked; locked *numbers* should carry the derivation that produced them, so the next correction does not need an archaeology pass.
