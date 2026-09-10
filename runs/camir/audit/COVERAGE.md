# CAMIR — Coverage audit

**What this is** — the row-by-row completeness audit of the CAMIR pack against [../../../references/artifact-manifest.md](../../../references/artifact-manifest.md): every required artifact, its status from the filesystem, and the quality gates that were actually run rather than asserted.
**Why it exists** — this pack has already been declared complete once when it was not: a handed-off pass reported `61/61 COMPLETE` while `docimages.json` was `{}` and 68 documents were unillustrated. A directory of plausible files hides missing contracts, empty indexes and cross-file contradictions, and only a mechanical audit finds them. This file records what was checked, how, and what remains open.
**How to read it** — §1 is the count; §3 is the part to trust or attack, because each gate names the command or inspection behind it. A skeptic should read §4 first: 61/61 counts artifacts, not evidence, and CAMIR has no evidence of its own yet.
**Depends on / feeds** — depends on [../../../references/artifact-manifest.md](../../../references/artifact-manifest.md), [../../../references/quality-bar.md](../../../references/quality-bar.md), [CRITIC_LOG.md](CRITIC_LOG.md) and the whole run tree; feeds [../README.md](../README.md) and [../HANDOFF.md](../HANDOFF.md).

---

## 1. The count

**61/61 required artifacts pass** · **45/45 visuals rendered as PNG** (34 HTML infographics, 11 Mermaid diagrams — all browser snapshots, none from a diffusion model) · **0 of 68 documents unillustrated** · checked against the filesystem on 2026-09-10.

Counted as the manifest specifies: A24 architecture files individually (11), A50 as the HTML set.

---

## 2. Required rows

| ID | Path | Status | Evidence |
|---|---|---|---|
| A00 | BRIEF.md | pass | Own contract (grill-me); founder vocabulary fixed |
| A01 | ASSUMPTIONS.md | pass | 14 rows; A1–A3 kill-the-pack |
| A02–A05 | research/landscape, competitors, capability_table, survey | pass | Phase 1; capability_table at 39 substantive lines, 2,360 words — see §3.6 |
| A06 | research/sources.md | pass | 40 sources: 21 primary, 17 secondary, 2 weak; gaps G1–G5 |
| A07–A13 | strategy/ (7 files) | pass | CAC in `channel_plan.md` corrected 2026-09-10 — [CRITIC_LOG.md](CRITIC_LOG.md) §3.1 |
| A14–A16 | product/PRD, features_flagship, features_prioritized | pass | Critic round 1 recorded; 50-row dependency audit, 0 violations |
| A17–A20 | product/journeys/ (4) | pass | Every beat names the component and the record written |
| A21 | product/ux_spec.md | pass | 11 surfaces ranked by consequence |
| A22 | tech/whitepaper.md | pass | Critic round 1: 4 major, 4 minor fixed; every §2.6 cell re-derived |
| A23 | tech/deep_dives.md | pass | 8 deep dives |
| A24 | tech/architecture/00_INDEX.md + D01–D10 | pass (11/11) | Every fence parses; every diagram rendered and inspected; D02 restructured for legibility |
| A25–A26 | tech/techniques/wave1, wave2 | pass | wave1 critic round 1: 1 major, 4 minor fixed |
| A28 | tech/techniques/decision_tree.md | pass | Mermaid parses; rendered as V16 |
| A29 | tech/techniques/technique_feature_matrix.md | pass | Orphans and unsupported features both reported |
| A30 | tech/not_vaporware.md | pass | Buildable vs research split |
| A31 | narrative/one_pager.md | pass | Market and ask added 2026-09-10 (contract requires both); 748 words — §3.6 |
| A32 | narrative/vc_memo.md | pass | All six contract sections; named competitors; 2,350 words |
| A33 | narrative/pitch_deck.md | pass | 14 claim titles; every `visual:` line resolves to a manifest row |
| A34 | narrative/future_press.md | pass | Dated 2031; quotes labelled illustrative; 1,168 words — §3.6 |
| A35 | narrative/founder_story.md | pass | First person; grounded only in BRIEF.md; 1,012 words — §3.6 |
| A36–A37 | validation/riskiest_assumptions, experiment_board | pass | Harness thesis added as rank 1b; E13 hypothesis reconciled |
| A38–A42 | validation/discovery_guide, get_keep_grow, stage_gate, metrics_by_stage, pivot_log | pass | Thresholds aligned to the experiment board |
| A43–A48 | financials/ (6) | pass | CAC, churn, Y4 ACV bend and contribution rows corrected |
| A49 | visuals/visual_manifest.md | pass | 45 rows; statuses written from `ls`; form counts reconcile |
| A50 | visuals/infographics/*.html | pass (34) | Self-contained, inline CSS, no external assets |
| A51 | visuals/image_prompts.md | pass | 37 entries covering 45 rows; Mermaid rows need none (A50) |
| A52b | visuals/docimages.json | **pass** | Template builder: 68 documents, 86 placements, **0 unillustrated** — was `{}` earlier the same day |
| A54 | audit/COVERAGE.md | pass | This file |
| A55 | README.md | pass | All eight A55 sections; images embedded; every link resolves |
| A58–A61 | strategy/business_model_canvas, petal_diagram, channel_plan, sales_roadmap | pass | Blank deliverables |
| A62–A63 | validation/mvp_definition, decision_making_unit | pass | Two MVPs with separate kill criteria; saboteur mapped |
| A64 | narrative/mission_vision.md | pass | Mission, vision (2036), five trade-off values, why-we-exist; 480 words — §3.6 |

## Optional rows

| ID | Path | Status | Reason |
|---|---|---|---|
| A27 | tech/techniques/wave3.md | present | Carries its own `critic: unresolved` marker |
| A52 | visuals/images/*.png | **present as snapshots** | 45 PNGs exist, rendered from HTML and Mermaid. **Diffusion renders of `image_prompts.md` have not been made** — no text-to-image tool was available — and no row claims otherwise |
| A53 | ingest/SOURCE_<n>.md | not applicable | No sources were ingested for this run |
| A56 | index.html | **not started — awaiting founder consent** | A public site exposes the repository |
| A57 | GitHub Pages URL | **not started — awaiting founder consent** | Pages deliberately not enabled; `gh` is not installed |

---

## 3. Quality gates — what was actually run

| # | Gate | Method | Result |
|---|---|---|---|
| 3.1 | Relative links | Every `[..](..)` in every `.md` under the run resolved against the filesystem by script | **0 broken** |
| 3.2 | Citations | Every `[Sn]`/`[Gn]` checked against S1–S40, G1–G5 | **All valid, none unused** |
| 3.3 | Mermaid | All fences parsed with mermaid 11 under jsdom, then rendered in headless Edge | **11/11 parse; 11/11 render** |
| 3.4 | Images, stage 1 | Size > 10 KB and PNG header `89504e47`, for every file | **45/45** |
| 3.5 | Images, stage 2 | Every PNG opened and inspected in contact sheets for clipping, overflow and label binding | 4 defects found and fixed (a clipped bar label, duplicated step numbers, a cramped zone, a mislabelled title); D02's tangled layout restructured |
| 3.6 | Stub threshold | Substantive lines counted per required file by script | Five files sit under ~40 lines: `founder_story` (23), `mission_vision` (21), `one_pager` (30), `future_press` (36), `capability_table` (39). **Judged not stubs**: each has every section its contract names, and 480–2,360 words — the count treats a prose paragraph as one line. Recorded here so a reader can disagree; not padded, because quality-bar property 7 penalises length |
| 3.7 | Property 0 | Four-line orientation block checked in the first 4,000 characters of every artifact | Present everywhere except the three files with their own contracts (BRIEF, ASSUMPTIONS, sources) and README (A55) |
| 3.8 | Critic loop | Three personas over the nine phase 3–7 drafts, verdicts recorded | [CRITIC_LOG.md](CRITIC_LOG.md): 1 fatal, 12 major, 17 minor, plus 3 cross-document errors — all fixed or deliberately logged |
| 3.9 | Banned vocabulary | "the product/platform/solution", "AI-powered", "seamless", "revolutionary", "cutting-edge" grepped across narrative, visuals, audit, README | 0 uses referring to CAMIR (two hits are "the platform engineer/team", a persona role) |
| 3.10 | Locked numbers | Pack-wide grep for each figure in [../HANDOFF.md](../HANDOFF.md) §2.5 | Consistent after the CAC correction (~$4,200, from $5,400) |

---

## 4. What 61/61 does not mean

1. **Nothing in the pack is measured.** No interview, no line of code, no benchmark run. Every CAMIR figure is `(assumption)` or borrowed from [../research/sources.md](../research/sources.md). The first number that will be CAMIR's own is the E1/E2 oracle ceiling.
2. **The critic pass was not independent.** Round 0 left no record; round 1 was one agent applying three lenses in sequence after three independent critic agents failed on an API session limit. An independent pass on `narrative/` and `financials/` is the most valuable review not yet done.
3. **The images are snapshots, not illustrations.** They are exact, legible and deliberately plain. Stylised renders from `image_prompts.md` would need a text-to-image tool and both verification stages.
4. **The website is off by decision.** A56/A57 wait on the founder, because publishing makes the repository public.

---

## Recommended next 3

1. **Run E1 and E2.** They are the only way any figure in this pack stops being an assumption, and the audit cannot improve on that.
2. **Commission an independent critic pass on the narrative and financial layers** — a separate session or a different model — before either is shown to an investor (§4.2).
3. **Script gates 3.1–3.4 and 3.10 as one check and run it on every commit.** Every error this audit found was found by a script or by opening a file; none by re-reading prose.
