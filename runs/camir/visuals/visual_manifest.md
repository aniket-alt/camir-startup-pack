# CAMIR — Visual manifest

**What this is** — the ranked map of visuals that make CAMIR's claims inspectable across users, operators, investors and buyers.
**Why it exists** — a visual can silently invent a number or duplicate a Mermaid diagram while leaving the dense decision artifacts unreadable; this manifest makes source, audience, form and status auditable.
**How to read it** — start with the investor rows V03–V10, then the operator rows V06, V07 and V15–V18; attack every number against its source artifact and treat `pending-image` as honest absence, not completion.
**Depends on / feeds** — depends on [narrative/pitch_deck.md](../narrative/pitch_deck.md), [product/](../product/), [tech/](../tech/), [strategy/](../strategy/) and [financials/](../financials/); feeds [image_prompts.md](image_prompts.md), [docimages.json](docimages.json), [audit/COVERAGE.md](../audit/COVERAGE.md) and [README.md](../README.md).

## Visual language

Palette: ink `#14213D`, signal teal `#007C83`, warm amber `#F2A900`, coral `#D95D39`, mist `#F3F6F7`, white `#FFFFFF`.
Type scale: 30px headline, 18px section, 13px label, 11px annotation. Use a white background, thin ink rules, teal for measured paths, amber for assumptions, coral for vetoes or disqualification. HTML rows are print-clean A4/16:9 with inline CSS. Mermaid rows are rendered live from their source artifact. Raster rows remain `pending-image` because no image tool is available.

Target: 22 visuals; 9 exact-text HTML infographics, 2 live Mermaid sources (V06 → `tech/architecture/D02.md`, V16 → `tech/techniques/decision_tree.md`), 11 image-prompt-only schematic rows. No PNG rows are marked rendered. V08 was previously listed as a Mermaid source, but `tech/whitepaper.md` contains no diagram; it is an image-prompt row pending a renderer.

## Investors

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V01 | Mixed-difficulty requests cross a fixed-model baseline | Investors | narrative/pitch_deck.md | image-prompt | pending-image |
| V02 | Hosted claims versus self-hosted cost axis | Investors | narrative/pitch_deck.md | image-prompt | pending-image |
| V03 | Plateau evidence and the measurable-frontier thesis | Investors | narrative/pitch_deck.md | image-prompt | pending-image |
| V04 | Artifact-controlled oracle ceiling decomposition | Investors | narrative/pitch_deck.md | html | rendered |
| V05 | Qualify, measure, disqualify | Investors | narrative/pitch_deck.md | html | rendered |
| V09 | ACV derivation and open-core boundary | Investors | narrative/pitch_deck.md | html | rendered |
| V10 | Product 1 ceiling and conditional Product 2 expansion | Investors | narrative/pitch_deck.md | html | rendered |
| V11 | Channel dependency and fallback path | Investors | strategy/channel_plan.md | image-prompt | pending-image |
| V12 | Commodity mechanism versus customer-owned records | Investors | narrative/pitch_deck.md | image-prompt | pending-image |
| V13 | Evidence ladder from capstone to customer proof | Investors | narrative/pitch_deck.md | image-prompt | pending-image |
| V14 | Three-gate validation sequence | Investors | narrative/pitch_deck.md | html | rendered |

## Operators and team

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V06 | CAMIR core loop | Operators | tech/architecture/D02.md | mermaid | source-mermaid |
| V07 | Ravi's control path from shadow mode to pin | Operators | narrative/pitch_deck.md | html | rendered |
| V08 | Self-hosted cost-quality frontier | Operators | tech/whitepaper.md | image-prompt | pending-image |
| V15 | Feature priority and dependency boundary | Operators | product/features_prioritized.md | html | rendered |
| V16 | Technique decision tree | Operators | tech/techniques/decision_tree.md | mermaid | source-mermaid |
| V17 | Technique × feature matrix | Operators | tech/techniques/technique_feature_matrix.md | html | rendered |
| V18 | Experiment board gates | Operators | validation/experiment_board.md | html | rendered |

## Users and practitioners

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V19 | Edge-low to beachhead to edge-high journey spectrum | Practitioners | product/journeys/day_in_life.md | image-prompt | pending-image |
| V20 | One request, five records | Practitioners | product/PRD.md | image-prompt | pending-image |

## Buyers and partners

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V21 | Buyer decision rights and eligible-savings definition | Buyers | financials/pricing.md | image-prompt | pending-image |
| V22 | LiteLLM channel dependency and fallback | Buyers / partners | strategy/channel_plan.md | image-prompt | pending-image |

## Classification rule

Rows with dense tables, matrices, journeys or transcribable numbers use HTML. Rows whose source already contains Mermaid cite the Mermaid source instead of duplicating it. Image prompts carry only schematic labels; they never replace exact words in the source artifact.

## Recommended next 3

1. Open the six HTML infographics carrying the highest-risk numbers before any raster work.
2. Render the Mermaid sources from the architecture and technique artifacts in the reader.
3. Reconcile statuses to the filesystem after every image batch; no absent PNG is marked rendered.
