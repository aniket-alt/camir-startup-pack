# CAMIR — Session Handoff

**What this is** — everything a fresh agent session needs to resume this run without re-deciding what has already been decided: the locked decisions, the exact remaining file list in draw order, and the traps this run has already hit.
**Why it exists** — the run is being handed from one model to another mid-pipeline. Most of what constrains the remaining artifacts is not written in any skill file — it was derived during phases 1 and 2 and passed to the generator agents verbally. Without this file the next session will re-open settled questions (is the claim "our router is smarter"? is positioning on the cost-quality plane?) and produce a pack that contradicts its own research layer.
**How to read it** — §2 Locked decisions is the part that cannot be reconstructed from the repo; treat it as binding. §4 is the work queue. §5 lists the mistakes already made so they are not repeated.
**Depends on / feeds** — depends on [BRIEF.md](BRIEF.md), [ASSUMPTIONS.md](ASSUMPTIONS.md), [research/](research/), [strategy/](strategy/); feeds every remaining artifact in this run.

---

## 0. Update — end of 2026-09-10: the draw order in §4 is complete

**61/61 required artifacts · 45/45 visuals rendered (HTML and Mermaid snapshots) · 0 of 68 documents unillustrated.** Tiers 1–4 of §4 are done except the website, which still needs the founder's consent. Row-by-row status: [audit/COVERAGE.md](audit/COVERAGE.md). What the review changed, including errors in this file's own §2.5 (the blended CAC was $5,400 and did not follow from its inputs — now ~$4,200): [audit/CRITIC_LOG.md](audit/CRITIC_LOG.md). The §1 table and §4 draw order below are kept as the historical record of where the session started.

**If you are resuming this run**, the open work is: (1) E1/E2 — the first measurement of CAMIR's own; (2) an independent critic pass on `narrative/` and `financials/`, since the recorded pass was not independent; (3) the website, only with consent.

## 1. State as of the start of 2026-09-10

**29 of 61 required artifacts · 30 files on disk · 0 visuals · all work committed and pushed to `main`.**

| Phase | Status | Notes |
|---|---|---|
| 0 Brief | **complete** | `BRIEF.md`, `ASSUMPTIONS.md` (14 rows, A1–A3 kill-the-pack) |
| 1 Research | **complete** | 5 files, 40 live-searched sources dated 2026-09-09, confidence-graded, 5 marked gaps G1–G5 |
| 2 Strategy | **complete** | 11 files |
| 3 Product | **partial** | `PRD.md`, `features_flagship.md` (20), `features_prioritized.md` (50) written. **4 journeys + `ux_spec.md` missing** |
| 4 Tech | **partial** | `whitepaper.md`, `techniques/wave1-3.md` written. **`deep_dives.md`, `architecture/` (11 files), `not_vaporware.md`, `techniques/decision_tree.md`, `techniques/technique_feature_matrix.md` missing** |
| 5 Narrative | **not started** | Held back deliberately — this layer *arranges*, it does not invent, so it must run after 3, 4, 6, 7 |
| 6 Validation | **partial** | `riskiest_assumptions.md`, `experiment_board.md` written. **7 files missing** |
| 7 Financials | **partial** | `pricing.md`, `revenue_build.md` written. **4 files missing** |
| 8 Visuals | **not started** | No text-to-image tool was available in the previous session; A52 (`images/*.png`) is `opt` and may be left `pending-image`. A50 HTML infographics and A52b `docimages.json` are still required |
| 9 Audit | **not started** | `audit/COVERAGE.md` + finalise `README.md` |
| 10 Website | **not started** | Needs the user's explicit consent before enabling GitHub Pages — it makes the repo contents public |

**Critic-loop status.** `techniques/wave2.md` and `wave3.md` carry `<!-- critic: unresolved -->` markers showing the adversarial pass ran. **The other nine phase 3–7 artifacts are unreviewed drafts** and need a `startup-critic` pass before their gate counts as passed.

---

## 2. Locked decisions — BINDING, and not reconstructible from the repo alone

These were settled with the founder or derived in phases 1–2. **Do not re-open them.**

### 2.1 Founder decisions (taken 2026-09-09, in session)

1. **Model pool scope: self-hosted first, hybrid later.** Beachhead is open-weight pools on the customer's own hardware; the tier abstraction admits hosted API models later as an additional top tier. (Assumption A9.)
2. **Commercial form: open-core + commercial control plane.** Open: router, tier abstraction, benchmark harness, published frontier. Paid: per-deployment classifier training, quality-tolerance policy, savings measurement and attribution, routing observability. (A7.)
3. **Framing: venture pack, capstone origin stated plainly.** Full investor-grade pack; the pre-traction CMPE 295A origin declared in the brief, the README status line and the narrative layer. Every forward number tagged. (A10.)

### 2.2 The re-scoped technical claim — the most important item on this page

**The claim is NOT "our router is more accurate."** arXiv:2606.07587 *The Routing Plateau* [S4] benchmarks 21 routing methods across 5 benchmarks and finds they converge into a narrow accuracy band far below the oracle router, caused by a **predictability bottleneck**; the best remedies gained ~2.13 percentage points. A router-superiority claim is falsifiable by one citation.

**The claim IS: "your frontier is measurable on your pool, and a meaningful part of the apparent ceiling is your harness."** The counterweight is arXiv:2605.07395 *Unsolvability Ceiling* [S5] — 206,000 query-model pairs — finding that much of measured unsolvability is **evaluation artifact**: judge verbosity bias, truncation under fixed generation budgets in **65% of MMLU / 57% of MedQA** cases, and **5–12% parse failures on MMLU**. The gap to oracle therefore decomposes into genuine unpredictability (near its ceiling) and instrumentation error (large, fixable). The field has optimised the first and neglected the second. **That decomposition is CAMIR's technical thesis and the reason this team's benchmark-engineering background is the relevant edge.**

### 2.3 Derived decisions that bind every remaining artifact

4. **The cascade route is primary; the classifier route is the ablation.** The classifier must beat **calibrated confidence** [S8], not merely a fixed-model baseline — simple confidence measures route as well as trained routers.
5. **The measurement layer is the asset**, because the routing mechanism commoditises into the serving engine ([S11], the vLLM Semantic Router vision paper — the "commoditisation clock"). Measurement layer = mixed-difficulty benchmark + judging protocol + **a cost axis derived for self-hosted pools** (amortised GPU-hours per token), which no published benchmark has derived — RouterBench prices against hosted API list prices [S7]. That derivation is an unclaimed, achievable contribution.
6. **The judging protocol is forced** by the 2026 reliability results and is a first-class component: temperature 0 (same-verdict >95% at temp 0, ~70% at temp 1 [S34]); fixed answer position or averaged permutations (position bias ~40% GPT-4 inconsistency); verbosity control (~15% inflation); exact-match or programmatic verification where the task admits it; two or more judges with **published inter-judge agreement** (~80% agreement with humans but only ~76% with each other [S33]).
7. **Positioning is NOT on the cost-quality plane.** That plane is what CAMIR *plots*, not what it competes on. The two axes that divide the market are **who owns the weights** (hosted catalog ↔ self-hosted pool) and **who sets the quality tolerance** (vendor-set and opaque ↔ customer-set and measured). The upper-right quadrant is empty **because it is hard to monetise, not because nobody thought of it** — say so.
8. **P0 features, ranking above classifier accuracy:** per-endpoint **quality tolerance ownership**; per-request **tier decision stamped on every trace**; **shadow mode** (decisions computed and logged while all traffic still goes to the baseline); **unilateral pin-to-large**. These exist because of persona P5 Ravi.
9. **The disqualification behaviour:** CAMIR tells a prospect on day two if their oracle ceiling is too low for routing to help. Only possible because the harness is open and runs in the customer's perimeter; no competitor billing a share of the bill can say it.
10. **Price out of the inference bill or not at all.** `strategy/petal_diagram.md` finds only one of five adjacent petals holds real, approved, transferable budget. **The primary competitor in every deal is Petal 5 — the customer's own engineer saying "I could build this in three weeks"** — not Martian.
11. **Channel:** ship as a routing strategy inside **LiteLLM** rather than as a rival proxy. Cloud marketplaces (84.3% net), system integrators (59.8% net) and outbound (CAC ~117% of ACV) are rejected on economics in `strategy/channel_plan.md`. **This is a third-party dependency nobody has agreed to yet** and is first in the business-model kill order.

### 2.4 Vocabulary and prohibitions

**Use:** router · tier · model pool · classifier route · cascade route · cost-quality frontier · quality tolerance · fixed-model baseline · oracle ceiling · escalation rate · control plane · request.
**Banned:** "the product", "the platform", "the solution", "AI-powered", "seamless", "revolutionary", "cutting-edge", and any claim supported only by an adjective.

**Core loop, already fixed in `product/PRD.md` — reuse verbatim:** **Classify → Dispatch → Judge → Attribute → Recalibrate.**
**Named components, already fixed in the PRD — reuse, do not invent new names:** ingress proxy · difficulty classifier · tolerance policy engine · pin registry · dispatcher · confidence gate · model pool · judge harness · artifact guard · judge interface · cost meter · savings attributor · trace stamper · drift monitor · recalibration scheduler · frontier builder · oracle ceiling probe · replay corpus builder · label recycler · disqualification report · savings report · frontier diff · tolerance breach alert.
**Durable records:** `decision_record` · `judgment_record` · `savings_ledger` · `frontier_run`.

**Personas, already fixed in `strategy/personas.md` — use these names:** P1 **Priya Raghunathan** (edge-low, drop-in, self-hosts for data residency not price) · P2 **Marcus Bell** (beachhead, staff platform engineer, owns the bill, ~$50k/month) · P3 **Wen Xu** (edge-high, own judge, five tiers) · P4 **Dana Okonkwo** (Head of Platform, signs, never evaluates) · P5 **Ravi Menon** (product engineer in a *different reporting line*, bears the quality risk, gains nothing, **can veto unilaterally**) · P6 **Sam Ortega** (OSS adopter below the volume floor, never pays, **is the distribution channel**).

### 2.5 Numbers that must stay consistent across the pack

- ACV **$30,000/yr** · TAM base **~$70M/yr** (corridor $10–360M) · SAM **~$20M/yr** · **~1,370 reachable companies** (corridor ~470–3,800) · SOM Y1 ~5 / Y2 ~27 / Y3 ~68 customers, $2.4M ARR at Y3.
- Blended Y1–Y3 CAC **~$4,200 (14% of ACV)**, payback ~2.2 months at 75% gross margin; year one alone ~$11,700 (Stack A's $40k build lands on ~2 customers). *Corrected 2026-09-10 from $5,400, which did not follow from `strategy/channel_plan.md`'s own inputs.*
- **Never quote "85% cost reduction" without naming MT-Bench.** RouteLLM's CPT is **3.66× MT-Bench / 1.41× MMLU / 1.49× GSM8K** [S2]. FrugalGPT: up to 98%, only 16.6% escalating, **under 2023-era hosted price ratios** [S3] — and the tier spread is now **compressing** [S29].
- Self-hosted cost: ~$0.10/M tokens raw on a batched H100 vs ~$0.60/M hosted [S26]; **idle GPU at 10% utilisation costs 10× per token and realistic all-in is 3–5× raw GPU rental** [S27]. Never quote a saving against raw GPU cost without this correction.
- Semantic caching removes **20–45% of production traffic upstream** and adversely selects the remainder toward the hard end [S36], gap [G4].
- Open-core conversion: hosted SaaS **1–5% of active users**, enterprise licences 0.01–0.1% [S40].
- **The finding `strategy/market_sizing.md` states first:** routing fees alone are not a venture-scale business at 2026 denominators; expansion to the inference-efficiency control plane **is** the venture case.

---

## 3. How to resume (harness-agnostic)

1. Read `AGENTS.md` at the repo root — it explains how to run this pack without native skill loading.
2. Read `references/quality-bar.md` (binding, nine properties, **property 0 is the most-skipped**) and `references/artifact-manifest.md` (the definition of done, including the A50 HTML-versus-image contract and the A55 README contract).
3. Read this file's §2 before writing anything.
4. Read `BRIEF.md`, `ASSUMPTIONS.md`, all of `research/`, all of `strategy/`, and `product/PRD.md`.
5. Work the draw order in §4. **Commit and push after every phase.**

**Every artifact needs:** a property-0 orientation block directly under the H1 (four labelled lines, ~80–120 words: *What this is* / *Why it exists* — naming a decision and a failure **specific to CAMIR**, not to documents of its type / *How to read it* / *Depends on / feeds* with working relative links); every number carrying an `[Sn]` tag resolving to `research/sources.md` or an explicit `(assumption: <basis>)`; and a closing **"Recommended next 3"** decision-forcing section.

---

## 4. Draw order — the exact remaining work

**Tier 1 — finish the generator phases (24 files)**

| # | File | Owner skill | Notes |
|---|---|---|---|
| 1 | `product/journeys/edge_low.md` | startup-product | Priya. Every beat names the component acting and what is written to the durable record |
| 2 | `product/journeys/beachhead.md` | startup-product | Marcus, first session to habitual use |
| 3 | `product/journeys/edge_high.md` | startup-product | Wen, genuinely stretched — her own judge and tiers plugged in |
| 4 | `product/journeys/day_in_life.md` | startup-product | One ordinary day across Marcus + Ravi + Dana (payer ≠ user) |
| 5 | `product/ux_spec.md` | startup-product | 8–12 screens, text spec only; collages belong to phase 8 |
| 6 | `tech/deep_dives.md` | startup-tech | 5–8 tier-1 items, real method names |
| 7–17 | `tech/architecture/00_INDEX.md` + `D01.md`–`D10.md` | startup-tech | Each a **valid Mermaid diagram** + a caption saying what a reviewer should notice |
| 18 | `tech/not_vaporware.md` | startup-tech | Stack, evaluation loop, cost model at [S26][S27] prices, this-quarter vs research risk |
| 19 | `tech/techniques/decision_tree.md` | startup-tech | Mermaid flowchart + logic table |
| 20 | `tech/techniques/technique_feature_matrix.md` | startup-tech | Flag orphan techniques and unsupported features — both are findings |
| 21–27 | `validation/`: `discovery_guide.md`, `get_keep_grow.md`, `stage_gate.md`, `metrics_by_stage.md`, `pivot_log.md`, `mvp_definition.md`, `decision_making_unit.md` | startup-validation | Discovery guide must be **Mom-Test clean** — past behaviour only, never hypotheticals |
| 28–31 | `financials/`: `unit_economics.md`, `use_of_funds.md`, `risk_matrix.md`, `comps_exits.md` | startup-financials | Unit economics **must** carry the compute-cost-per-unit line |

**Tier 2 — narrative (6 files), only after Tier 1**
`narrative/one_pager.md`, `vc_memo.md`, `pitch_deck.md`, `future_press.md`, `founder_story.md`, `mission_vision.md`. This layer arranges; it invents nothing. Slide titles are claims, never labels ("Market" is a failing slide title).

**Tier 3 — critic pass on the nine unreviewed drafts**
`PRD.md`, `features_flagship.md`, `features_prioritized.md`, `whitepaper.md`, `wave1.md`, `riskiest_assumptions.md`, `experiment_board.md`, `pricing.md`, `revenue_build.md`.

**Tier 4 — visuals, audit, website**
`visuals/visual_manifest.md` → `visuals/infographics/*.html` (A50) → `visuals/image_prompts.md` → `visuals/docimages.json` (A52b — **no substantive artifact over ~400 words left with zero illustrations**). Copy the builders from `templates/`, do not rewrite them. Then `audit/COVERAGE.md`, then finalise `README.md` against the A55 contract. Website last.

---

## 5. Traps this run has already hit — do not repeat

1. **Parallel subagents all died on an API session limit**, four of them after writing only part of their output. If you parallelise, **commit at the swarm boundary and verify each file is complete** (property-0 block present, closes with a decision section, no mid-sentence truncation) before trusting it. Sequential generation is slower and did not lose work.
2. **`runs/*` is gitignored by default.** `runs/camir/` was whitelisted in `.gitignore` in commit `1f9d7a3`. If you add another run, whitelist it or the work will silently never commit.
3. **`git add -A` while agents are still writing** snapshots a half-written file and aborts with "short read while indexing". Wait for every agent to report first.
4. **Long heredocs through the Bash tool exceed the command length limit** on this machine — use the file-writing tool for artifact content, and reserve heredocs for commit messages.
5. **`gh` is not installed here**, so the GitHub Pages publish steps in `startup-website` cannot run as written. The repo remote is `https://github.com/aniket-alt/camir-startup-pack`.
6. **Publishing needs the founder's explicit consent** — enabling Pages makes the repository's contents public. It has not been asked yet.
7. **No text-to-image tool was available.** A52 rendered images may be left `pending-image`; the HTML infographics carry the content. Do not block the pack on rasters, and never mark a row `rendered` without opening the file.

---

## 6. Recommended next 3

1. **Write the four journeys and `ux_spec.md` first.** They are the highest-value missing artifacts — the VC memo's operating examples are drawn from them, so the narrative layer is blocked until they exist.
2. **Do the architecture set (D01–D10) in one sitting** and validate every Mermaid fence by eye before committing; a diagram that does not parse is invisible in the pack reader, and those diagrams are the tech layer's entire argument.
3. **Run the Tier 3 critic pass before the narrative layer**, not after. The narrative arranges whatever the generators produced, so an unreviewed error in `whitepaper.md` or `pricing.md` propagates into the one-pager, the memo and the deck, where it is most expensive to find.
