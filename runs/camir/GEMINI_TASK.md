# Task brief — finishing the CAMIR run

**Read this file first, then `AGENTS.md` at the repo root, then `runs/camir/HANDOFF.md`.** This brief is the work order; `HANDOFF.md` §2 is the binding constraint set. Nothing in this file overrides `HANDOFF.md`.

---

## 1. What this repo is

`startup-skills` is a harness-agnostic skill pack that turns a startup idea into a research-grounded founder artifact pack. `AGENTS.md` explains how to run it without native skill loading: treat each `skills/<name>/SKILL.md` as an instruction file and follow its contract literally.

The active run is **`runs/camir/`** — CAMIR, a cost-aware inference router for platform teams running LLM features on self-hosted open-weight model pools. It is a **pre-traction CMPE 295A capstone**, written as a venture pack with that origin stated plainly.

**Three files are binding and must be read before writing anything:**

| File | What it binds |
|---|---|
| `runs/camir/HANDOFF.md` §2 | The locked decisions. **Do not re-open these.** |
| `references/quality-bar.md` | Nine properties every artifact must satisfy. Property 0 is the most-skipped. |
| `references/artifact-manifest.md` | The file-by-file definition of done, plus the A50 and A55 contracts. |

---

## 2. State as of this brief

**50 of 61 required artifacts · 62 files on disk · all committed and pushed to `main`.**

Complete: phases 0 (brief), 1 (research, 40 live-searched sources), 2 (strategy, 11 files), 3 (product, 8 files), 4 (tech, 19 files), 6 (validation, 9 files), 7 (financials, 6 files).

**Verified, do not redo:** all 11 Mermaid diagrams in `tech/architecture/` and `tech/techniques/decision_tree.md` parse cleanly; all 45 `[Sn]`/`[Gn]` citation tags resolve to `research/sources.md` with none unused.

**Not started: narrative, visuals, audit, website.**

---

## 3. The work, in order

### TASK A — Critic pass on nine unreviewed drafts *(do this first)*

These nine were written by generator agents whose critic loops never ran. Everything downstream arranges what they contain, so an error here propagates into the one-pager, memo and deck where it is most expensive to find.

```
runs/camir/product/PRD.md
runs/camir/product/features_flagship.md
runs/camir/product/features_prioritized.md
runs/camir/tech/whitepaper.md
runs/camir/tech/techniques/wave1.md
runs/camir/validation/riskiest_assumptions.md
runs/camir/validation/experiment_board.md
runs/camir/financials/pricing.md
runs/camir/financials/revenue_build.md
```

Follow `skills/startup-critic/SKILL.md`. Run **three separate passes**, one per persona, each as its own pass rather than one blended review:

1. **Skeptical deep-tech VC** — unsourced numbers, mechanism-free claims, ignored risks, cross-document arithmetic that does not reconcile, anything an investor could falsify in one search.
2. **Domain PhD** (ML systems, LLM inference, evaluation methodology) — misused terminology, overclaimed evidence, `[Sn]` citations used beyond what `research/sources.md` says that source supports, technique names in `wave1.md` whose one-line mechanism misstates the method, mechanisms that would not work in a real vLLM/SGLang deployment.
3. **Elite operator-founder** (open-core dev tools, cloud FinOps) — unbuildable scope, priority tiers that are secretly chronological, dependencies pointing the wrong way, experiments a three-person pre-seed team cannot actually run, pricing that breaks on contact with a buyer.

**Verdict format, per critic per artifact:**

```
VERDICT: pass | revise
Top issues (max 5, ranked):
  1. [severity: fatal|major|minor] "<exact quote>" — <what is wrong> — <what would fix it>
Strongest element: <one line — protect this in revision>
```

Rules: **quote the artifact** — an issue that cannot point to a line is not an issue. Critics may **not** expand scope; only sections the owning skill's contract requires count as missing. Fix fatal and major issues; minor at your judgement, logging skipped ones as an HTML comment at the bottom of the artifact. Maximum three rounds, then ship with a `<!-- critic: unresolved -->` note listing what remains honestly.

**What is deliberate and is NOT a finding:** zero measurements of CAMIR's own; every forward number tagged `[Sn]` or `(assumption)`; the refusal to claim router superiority; the cascade primary and the classifier as an ablation.

**Pay particular attention to cross-document arithmetic.** The $30,000 ACV derivation in `pricing.md` flows into `revenue_build.md`, `unit_economics.md` and `strategy/market_sizing.md`. If a critic breaks it, the fix ripples.

### TASK B — Narrative layer (6 files) → `runs/camir/narrative/`

Owning contract: `skills/startup-narrative/SKILL.md`. **This layer arranges; it invents nothing.** Every number must already exist in an upstream artifact.

| File | Manifest ID |
|---|---|
| `one_pager.md` | A31 |
| `vc_memo.md` | A32 |
| `pitch_deck.md` | A33 |
| `future_press.md` | A34 |
| `founder_story.md` | A35 |
| `mission_vision.md` | A64 |

**Slide titles are claims, never labels.** "Market" is a failing slide title; "Routing fees alone are not venture-scale — the control plane is" is a passing one. Every deck slide carries a `visual:` line, because Task C derives the visual manifest from them.

`founder_story.md` must state the capstone origin plainly and must not invent traction, credentials or advisors. The team's claimed edge is test-automation and evaluation-infrastructure background transferring to benchmark engineering — assumption **A13**, and it is an assumption.

### TASK C — Visuals (4 rows) → `runs/camir/visuals/`

Owning contract: `skills/startup-visuals/SKILL.md`. Order matters:

1. `visual_manifest.md` (A49) — ranked table grouped by audience: ID · Title · Audience · Source artifact · Form · Status. Declare the palette and type scale in the header.
2. `infographics/<ID>_<slug>.html` (A50) — self-contained, inline CSS, no external assets, print-clean. **Read the A50 contract in `references/artifact-manifest.md` before deciding which rows need HTML** — it is not all of them. A row needs HTML when the words are the content (dense tables and matrices, journeys, posters enumerating many named items, anything whose numbers must be transcribable). A row does **not** need HTML when the picture is the content, and specifically **not** when the source artifact already ships a Mermaid diagram — the reader renders those live, so an HTML poster duplicates them. Cite the source document instead.
3. `image_prompts.md` (A51) — one prompt per manifest visual, numbered `P<ID>`, each self-contained with the shared style block.
4. `docimages.json` (A52b) — **copy the shipped builders, do not rewrite them:**

```bash
cp templates/build_docmanifest.js runs/camir/visuals/
cp templates/build_docimages.js   runs/camir/visuals/
cd runs/camir && node visuals/build_docmanifest.js && node visuals/build_docimages.js
```

They walk the whole run tree and print what is still unillustrated. Re-run after every batch. Target: no substantive artifact over ~400 words with zero illustrations.

**There is no text-to-image tool on this machine.** A52 (`images/*.png`) is `opt` — leave those rows `pending-image`. The HTML carries the content. **Never mark a row `rendered` without opening the file.**

### TASK D — Audit and README

1. `audit/COVERAGE.md` (A54) — row-by-row against `references/artifact-manifest.md`. **Counts come from the glob at the moment of writing, never from memory.**
2. Refresh `runs/camir/README.md` (A55) against its contract in the manifest — all eight sections. It currently reads `29/61 · PARTIAL` and is stale. State the honest count; never round up. Do not reproduce the gap table or a task list in the README — a public founder pack must not open onto a TODO list. Row-by-row status lives in `COVERAGE.md`.

### TASK E — Website: **do not do this**

`index.html` (A56) and GitHub Pages (A57) are `opt` and require the founder's explicit consent, because enabling Pages makes the repository contents public. **That consent has not been given. Do not enable Pages. Do not ask for it as part of this task.** Also note `gh` is not installed on this machine.

---

## 4. Binding constraints — every artifact you write

### 4.1 Property 0 — the orientation block

Directly under the H1, before any section, four labelled lines, ~80–120 words total:

```
**What this is** — one sentence naming the artifact's job in plain words.
**Why it exists** — the decision this informs and what goes wrong without it. Name the failure, not the benefit.
**How to read it** — where to look first, and what a skeptic should attack.
**Depends on / feeds** — upstream and downstream artifacts, as working relative links.
```

**The test: the "why it exists" line must name a decision or failure specific to CAMIR, not to documents of its type.** "This document describes the go-to-market strategy" fails.

### 4.2 Every number carries `[Sn]` resolving to `research/sources.md`, or an explicit `(assumption: <basis>)` tag

No exceptions. `research/sources.md` has a **Confidence** column — `primary` / `secondary` / `weak`. Honour it. `[S18]` (Martian's reported ~$1.3B valuation) is **weak**, a single secondary blog restating an unnamed report; it must always carry its hedge and must never appear in a deck as fact.

### 4.3 Every artifact closes with a "Recommended next 3" decision-forcing section

### 4.4 Vocabulary — use these words

router · tier · model pool · classifier route · cascade route · cost-quality frontier · quality tolerance · fixed-model baseline · oracle ceiling · escalation rate · control plane · request

**Banned:** "the product", "the platform", "the solution", "AI-powered", "seamless", "revolutionary", "cutting-edge", and any claim supported only by an adjective.

### 4.5 The core loop — reuse verbatim

**Classify → Dispatch → Judge → Attribute → Recalibrate**

### 4.6 Component names — reuse, do not invent new ones

ingress proxy · difficulty classifier · tolerance policy engine · pin registry · dispatcher · confidence gate · model pool · judge harness · artifact guard · judge interface · cost meter · savings attributor · trace stamper · drift monitor · recalibration scheduler · frontier builder · oracle ceiling probe · replay corpus builder · label recycler · disqualification report · savings report · frontier diff · tolerance breach alert

**Durable records:** `decision_record` · `judgment_record` · `savings_ledger` · `frontier_run` · `tolerance_policy` · `pool_manifest`

### 4.7 Persona names — fixed

P1 **Priya Raghunathan** (edge-low, drop-in, self-hosts for data residency not price) · P2 **Marcus Bell** (beachhead, staff platform engineer, owns the bill, ~$50k/month) · P3 **Wen Xu** (edge-high, own judge, five tiers) · P4 **Dana Okonkwo** (Head of Platform, signs, never evaluates) · P5 **Ravi Menon** (product engineer in a *different reporting line*, bears the quality risk, gains nothing, **can veto unilaterally**) · P6 **Sam Ortega** (OSS adopter below the volume floor, never pays, **is the distribution channel**)

### 4.8 Numbers that must stay consistent pack-wide

- ACV **$30,000/yr** · TAM base **~$70M/yr** (corridor $10–360M) · SAM **~$20M/yr** · **~1,370 reachable companies** · SOM Y1 ~5 / Y2 ~27 / Y3 ~68 customers, **$2.4M ARR at Y3**
- Blended Y1–Y3 CAC **~$4,200 (14% of ACV)**, payback ~2.2 months at steady-state margin *(corrected from $5,400 after this brief was issued)*
- **Y1 gross margin is 42%, not 75%** — 75% is the Y3 steady state. `financials/unit_economics.md` derives both; quoting only 75% is wrong.
- **Never quote "85% cost reduction" without naming MT-Bench.** RouteLLM's CPT is **3.66× MT-Bench / 1.41× MMLU / 1.49× GSM8K** [S2]. FrugalGPT: up to 98%, only 16.6% escalating, under **2023-era hosted price ratios** [S3] — and the tier spread is now **compressing** [S29].
- Self-hosted cost: ~$0.10/M tokens raw on a batched H100 vs ~$0.60/M hosted [S26]; **idle GPU at 10% utilisation costs 10× per token, and realistic all-in is 3–5× raw GPU rental** [S27]. Never quote a saving against raw GPU cost without this correction.
- Semantic caching removes **20–45% of production traffic upstream** [S36], gap [G4]
- Open-core conversion: hosted SaaS **1–5% of active users** [S40]

### 4.9 The technical claim — state it this way and no other way

**NOT** "our router is more accurate." arXiv:2606.07587 *The Routing Plateau* [S4] benchmarks 21 routing methods across 5 benchmarks and finds them converged in a narrow band far below the oracle, caused by a **predictability bottleneck**; the best remedies gained ~2.13 percentage points. A router-superiority claim is falsifiable by one citation and is **non-goal N7**.

**The claim IS:** *"your frontier is measurable on your pool, and a meaningful part of the apparent ceiling is your harness."* The counterweight is arXiv:2605.07395 *Unsolvability Ceiling* [S5] — 206,000 query-model pairs — finding much of measured unsolvability is **evaluation artifact**: judge verbosity bias, truncation under fixed generation budgets in **65% of MMLU / 57% of MedQA** cases, **5–12% parse failures on MMLU**.

### 4.10 No fabricated traction

No invented logos, testimonials, quotes, advisors, pilot customers or metrics, in any artifact. There are none.

---

## 5. Traps on this machine — already hit, do not repeat

1. **Parallel subagents die on API session limits.** Four died mid-write in an earlier session; three more died in the session before this brief. If you parallelise, commit at the swarm boundary and verify each file is complete before trusting it. Sequential is slower and loses nothing.
2. **`runs/*` is gitignored by default.** `runs/camir/` is whitelisted in `.gitignore`. A new run must be whitelisted or the work silently never commits.
3. **`git add -A` while agents are still writing** snapshots a half-written file and aborts with "short read while indexing". Wait for every agent to report first.
4. **Long heredocs through a Bash tool exceed the command length limit here.** Use a file-writing tool for artifact content; reserve heredocs for commit messages.
5. **`gh` is not installed.** Remote is `https://github.com/aniket-alt/camir-startup-pack`.
6. **No text-to-image tool.** Leave A52 rows `pending-image`.
7. **Commit and push after every phase**, unasked. Long runs get cut off; uncommitted work is indistinguishable from work never done.

---

## 6. Verification before you declare anything done

Not optional, and not by claim:

1. **Every Mermaid fence must parse.** A diagram that does not parse is invisible in every reader that matters. There is no mermaid CLI installed; this recipe works — install `mermaid` and `jsdom` in a scratch directory, then:

```js
import { JSDOM } from 'jsdom';
const dom = new JSDOM('<!doctype html><html><body></body></html>');
global.window = dom.window; global.document = dom.window.document;
global.DOMParser = dom.window.DOMParser; global.Element = dom.window.Element;
global.Node = dom.window.Node; global.HTMLElement = dom.window.HTMLElement;
global.SVGElement = dom.window.SVGElement;
// do NOT assign global.navigator on Node 24 — it is getter-only and throws
const mermaid = (await import('mermaid')).default;
mermaid.initialize({ startOnLoad: false, securityLevel: 'loose' });
await mermaid.parse(diagramSource);   // throws on a syntax error
```

Without the jsdom globals, flowcharts fail with `DOMPurify.addHook is not a function`, which is an environment error and **not** a syntax error — do not "fix" a valid diagram in response to it.

2. **Every relative link must resolve.** A README with one broken relative link is a `fix` row, not a pass. There are currently ~35 broken links across the pack and **every one is a forward reference to `narrative/` or `visuals/`** — they should all resolve once Tasks B and C land. If any remain afterwards, they are real.
3. **Every `[Sn]`/`[Gn]` tag must resolve** to `research/sources.md`. Valid range: S1–S40, G1–G5.
4. **Report counts from the glob at the moment of reporting**, never from memory of what you generated.
5. **Never present pre-existing artifacts as newly produced.** Check `git log --diff-filter=A -- <path>` before claiming a file is new.
6. **Verify by looking.** A file of the right size in the right place can still be the wrong content.

---

## 7. Definition of done

`audit/COVERAGE.md` reports **61/61 required artifacts** against `references/artifact-manifest.md`, with A52 rows honestly marked `pending-image` and A56/A57 marked `not started — awaiting founder consent`, and `README.md` refreshed from the glob to the A55 contract with every relative link resolving.
