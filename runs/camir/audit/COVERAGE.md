# CAMIR — Coverage audit

**What this is** — the row-by-row completeness audit for the CAMIR founder pack against `references/artifact-manifest.md`.
**Why it exists** — a directory can contain plausible files while hiding a missing contract, broken link or unrendered visual; this audit makes the required pack count and the optional website/raster boundary explicit.
**How to read it** — required rows are checked first, then optional rows and quality gates; a skeptic should inspect the `blocked` and `pending-image` rows first, and should not read 60/61 as empirical validation — it counts artifacts, not evidence.
**Depends on / feeds** — depends on [../](../), [../../../references/artifact-manifest.md](../../../references/artifact-manifest.md) and [../../../references/quality-bar.md](../../../references/quality-bar.md); feeds [../README.md](../README.md) and the next run checkpoint.

## Required coverage

**60/61 required artifacts pass. One is blocked: A52b.** Architecture files D01–D10 are counted individually under A24; the visual HTML set is counted under A50. All rows were checked against the filesystem on 2026-09-10 and re-checked in a review pass the same day.

**Why A52b does not pass.** Its contract is *no substantive artifact over ~400 words left with zero illustrations*. `visuals/docimages.json` exists but is `{}`: the shipped builder places only **rendered PNGs** from `visuals/images/`, and no text-to-image tool is available on this machine, so every manifest row is skipped and the builder reports 68 documents unillustrated. The file is present; its requirement is not met. It is blocked on a renderer, not on authoring — the 22 prompts in `image_prompts.md` are ready to run. An earlier draft of this audit marked it `present`, which was wrong.

| ID | Path | Status | Check |
|---|---|---|---|
| A00 | BRIEF.md | present | orientation and source brief |
| A01 | ASSUMPTIONS.md | present | assumption ledger |
| A02 | research/landscape.md | present | research layer |
| A03 | research/competitors.md | present | teardown |
| A04 | research/capability_table.md | present | capability table |
| A05 | research/survey.md | present | survey |
| A06 | research/sources.md | present | 40-source register |
| A07 | strategy/market_type.md | present | strategy |
| A08 | strategy/positioning.md | present | strategy |
| A09 | strategy/market_sizing.md | present | TAM/SAM/SOM |
| A10 | strategy/personas.md | present | P1–P6 |
| A11 | strategy/lean_canvas.md | present | canvas |
| A12 | strategy/value_prop_canvas.md | present | canvas |
| A13 | strategy/gtm.md | present | GTM |
| A14 | product/PRD.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md) |
| A15 | product/features_flagship.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md) |
| A16 | product/features_prioritized.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md) |
| A17 | product/journeys/edge_low.md | present | journey |
| A18 | product/journeys/beachhead.md | present | journey |
| A19 | product/journeys/edge_high.md | present | journey |
| A20 | product/journeys/day_in_life.md | present | journey |
| A21 | product/ux_spec.md | present | UX spec |
| A22 | tech/whitepaper.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md) |
| A23 | tech/deep_dives.md | present | technical layer |
| A24 | tech/architecture/00_INDEX.md, D01–D10.md | present | 11 files; Mermaid set pre-verified |
| A25 | tech/techniques/wave1.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md); `critic: unresolved` marker retained |
| A26 | tech/techniques/wave2.md | present | critic marker retained |
| A28 | tech/techniques/decision_tree.md | present | Mermaid pre-verified |
| A29 | tech/techniques/technique_feature_matrix.md | present | matrix |
| A30 | tech/not_vaporware.md | present | stack and risk |
| A31 | narrative/one_pager.md | present | Task B |
| A32 | narrative/vc_memo.md | present | Task B |
| A33 | narrative/pitch_deck.md | present | 14 claim-led slides |
| A34 | narrative/future_press.md | present | future-only framing |
| A35 | narrative/founder_story.md | present | capstone origin stated |
| A36 | validation/riskiest_assumptions.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md) |
| A37 | validation/experiment_board.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md) |
| A38 | validation/discovery_guide.md | present | validation |
| A39 | validation/get_keep_grow.md | present | validation |
| A40 | validation/stage_gate.md | present | validation |
| A41 | validation/metrics_by_stage.md | present | validation |
| A42 | validation/pivot_log.md | present | validation |
| A43 | financials/pricing.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md); `critic: unresolved` marker retained |
| A44 | financials/revenue_build.md | present | critic pass recorded in [CRITIC_LOG.md](CRITIC_LOG.md); `critic: unresolved` marker retained |
| A45 | financials/unit_economics.md | present | unit economics |
| A46 | financials/use_of_funds.md | present | funds |
| A47 | financials/risk_matrix.md | present | risks |
| A48 | financials/comps_exits.md | present | comps |
| A49 | visuals/visual_manifest.md | present | 22 rows; declared palette |
| A50 | visuals/infographics/*.html | present | 9 HTML infographics; exact-text rows |
| A51 | visuals/image_prompts.md | present | P01–P22 |
| A52b | visuals/docimages.json | **blocked** | file present but `{}` — builder places PNGs only; 0 placements, 68 docs unillustrated. Needs a text-to-image tool |
| A54 | audit/COVERAGE.md | present | this file |
| A55 | README.md | present | refreshed from glob |
| A58 | strategy/business_model_canvas.md | present | strategy |
| A59 | strategy/petal_diagram.md | present | strategy |
| A60 | strategy/channel_plan.md | present | strategy |
| A61 | strategy/sales_roadmap.md | present | strategy |
| A62 | validation/mvp_definition.md | present | validation |
| A63 | validation/decision_making_unit.md | present | validation |
| A64 | narrative/mission_vision.md | present | Task B |

## Optional rows

| ID | Path | Status | Reason |
|---|---|---|---|
| A27 | tech/techniques/wave3.md | present, optional | retained as existing artifact |
| A52 | visuals/images/*.png | pending-image | no text-to-image tool is available; no row is marked rendered |
| A53 | ingest/SOURCE_<n>.md | not started | no ingested-source requirement in this run |
| A56 | index.html | not started — awaiting founder consent | website is optional and would expose repository contents |
| A57 | GitHub Pages URL | not started — awaiting founder consent | Pages deliberately not enabled; `gh` unavailable |

## Quality gates

- **Property 0:** all newly written narrative, visual and audit artifacts open with the four labelled orientation lines. Existing upstream artifacts were already generated under the same pack contract.
- **Numbers:** Task A removed the highest-risk unsupported claims; narrative and visuals use source tags or explicit assumptions. No CAMIR traction or benchmark result is presented as observed.
- **Mermaid:** all 11 fences (D01–D10 and the decision tree) re-parsed with mermaid 11 under jsdom in the review pass — 0 failures. Two manifest rows had cited files containing no Mermaid (V06 → `00_INDEX.md`, V08 → `whitepaper.md`); V06 now cites `D02.md` and V08 is an image-prompt row.
- **HTML:** 9 self-contained files were opened as text and checked for inline CSS, source lines and bound labels. They carry exact content where a raster would be unreliable.
- **Links and citations:** every relative link in every `.md` file under the run resolves; every `[Sn]`/`[Gn]` tag resolves to `research/sources.md` (S1–S40, G1–G5).
- **Critic process:** the nine drafts received domain-correct edits (Chow's rule qualified, grammar-constrained decoding scoped to schema tasks, self-consistency flagged as conflicting with temperature-0 judging, the artifact-direction claim downgraded to a hypothesis). The first round left no verdict record; a second, recorded round is in [CRITIC_LOG.md](CRITIC_LOG.md) — 1 fatal, 12 major and 17 minor issues across the nine, plus three cross-document errors (the blended CAC, the status of share-of-savings pricing, and experiment thresholds set three ways) that no single-file review would catch. That round was run by one agent applying three lenses in sequence, not by independent critics, and says so.

## Gaps and residual risk

1. **Raster images and A52b:** A52 (optional) and A52b (required) are both blocked on a text-to-image tool. HTML and live Mermaid carry the exact-text content, but the per-document illustration requirement is unmet until the 22 prompts are rendered and the builder is re-run.
2. **Unvalidated venture claims:** oracle ceiling, customer segment, price, channel acceptance, Product 2 demand and switching cost remain planned assumptions. The pack is complete as an artifact set, not empirically validated.
3. **Website:** A56/A57 remain intentionally absent. GitHub Pages was not enabled and must stay disabled without founder consent.

## Recommended next 3

1. Run the relative-link and source-tag checks from the repository root after any future artifact edit.
2. Execute E1/E2 before promoting any scenario multiple or ACV into evidence.
3. Keep A52 pending and Pages disabled unless the founder changes the explicit consent decision.
