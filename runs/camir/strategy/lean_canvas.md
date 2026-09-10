# CAMIR — Lean Canvas

**What this is** — Maurya's Lean Canvas for CAMIR: problem, segments, unique value proposition, solution, channels, revenue, cost, metrics, unfair advantage — each cell at most three bullets, with the riskiest cell marked ⚠.
**Why it exists** — this is the one-page version a reader can hold in their head, and its job here is to make the **weak cells visible next to the strong ones**. CAMIR's unfair advantage cell is nearly empty and its revenue cell rests on untested pricing; a canvas that padded those two would let the pack's strong research layer disguise the two places the venture actually breaks.
**How to read it** — read the ⚠ cells only if you are short of time: **Unfair Advantage** and **Revenue Streams**. Both link to the assumption that would kill them.
**Depends on / feeds** — depends on [../BRIEF.md](../BRIEF.md), [positioning.md](positioning.md), [market_sizing.md](market_sizing.md), [personas.md](personas.md); feeds [business_model_canvas.md](business_model_canvas.md) (Blank's nine blocks, which cover the three this one drops) and [../validation/riskiest_assumptions.md](../validation/riskiest_assumptions.md).

**Cross-link:** the Lean Canvas deliberately omits **Key Partners, Key Activities and Customer Relationships**. Those are in [business_model_canvas.md](business_model_canvas.md), and for CAMIR they are not incidental — the partner cell holds the vLLM/LiteLLM channel decision that determines whether anyone installs this at all. Read both.

---

## The canvas

| **1. PROBLEM** | **4. SOLUTION** | **3. UNIQUE VALUE PROPOSITION** | **9. UNFAIR ADVANTAGE** ⚠ | **5. CUSTOMER SEGMENTS** |
|---|---|---|---|---|
| • Every request goes to one model because request difficulty is unknown in advance — so frontier prices are paid for "what's the refund policy" | • **Cascade route**: small tier first, escalate on calibrated low confidence [S8] | **Set your own quality tolerance and see the measured cost-quality frontier for your own traffic — because the harness runs inside your perimeter, the savings number is yours to audit rather than your vendor's to report.** | • **Nearly empty, and stated so.** The algorithm is not defensible; 21 routing methods converge into one narrow band [S4] | • **Beachhead:** platform teams self-hosting an open-weight model pool at ≥~$50k/month inference spend (P2 Marcus) |
| • Existing fixes are static policies over dynamic traffic: one model, length/keyword rules, endpoint assignment, one stale A/B test | • **Classifier route**: predict difficulty pre-generation, dispatch once — run as the *ablation*, not the headline | | • **What is real:** benchmark-engineering capability against a measurement problem the field just documented — 65% MMLU truncation, 5–12% parse failures, ~76% inter-judge agreement [S5][S33] | • **Edge-low:** drop-in proxy teams self-hosting for data residency, not price (P1 Priya) [S28] |
| • Nobody knows where their policy sits on the frontier, because nobody has plotted one for a self-hosted pool [G2] | • **The frontier harness**: mixed-difficulty benchmark, judging protocol at temperature 0 with published inter-judge agreement, cost axis derived for self-hosted pools rather than borrowed from hosted list prices [S7][S34] | | • **What compounds:** per-deployment routing history and calibration in the control plane — real, but per-customer, not network-wide | • **Edge-high:** teams with their own tiers, judge and eval capacity (P3 Wen) |
| | • **Per-endpoint tolerance ownership + tier stamped on every trace** — the anti-Ravi feature, P0 not observability | | → assumptions **A6**, **A7** in [../ASSUMPTIONS.md](../ASSUMPTIONS.md) | • **Not a segment:** teams below ~16M tokens/day, who should not be self-hosting at all [S27] |

| **8. KEY METRICS** | | | | **2. CHANNELS** |
|---|---|---|---|---|
| • **Oracle ceiling** on the customer's own traffic — bounds everything and is measurable in week one | | | | • Open-source repository → a frontier chart in one afternoon on one GPU (P6 Sam) |
| • **Realised saving at declared tolerance**, computed against the fixed-model baseline and re-auditable by the customer | | | | • **Routing strategy shipped inside LiteLLM** rather than as a rival proxy [S22] — reach the installed base instead of fighting it |
| • **Escalation rate** on the cascade route — the number that decides whether cascade economics survive compressed tier spreads | | | | • Published frontier + methodology as the credibility artifact; the reproducibility critique [S13] is the opening |
| • *Vanity, to be ignored:* GitHub stars. TensorZero had 11,000 and returned the capital [S23] | | | | • Never: outbound sales to engineers. Dana signs, Marcus decides, and Marcus is unreachable by outbound |

| **7. COST STRUCTURE** | **6. REVENUE STREAMS** ⚠ |
|---|---|
| • GPU-hours for benchmark runs and per-deployment classifier training — the dominant variable cost, and it scales with customers, not with traffic | • **Share of measured savings**, ~28% — precedent exists in cloud FinOps, where vendors price per customer and publish no universal rate [S38][S39] |
| • Judge inference for continuous quality measurement — recurring, not one-off, because the frontier moves with every model upgrade | • **Fallback: per-request or ~5% of inference spend**, anchored to OpenRouter's revealed take rate [S17] |
| • Engineering: 3 founders, no sales headcount before evidence | • **ACV ≈ $30k/yr** at a $600k/yr-spend customer `(assumption: no pricing has been tested with any buyer)` |
| • **Not a cost:** model hosting. CAMIR routes across the customer's pool; it does not serve models | • ⚠ → assumptions **A4** (will anyone pay for routing as a line item at all, when a frontier vendor gives it away free [S20]) and **A5** (are savings attributable enough to bill on) |

---

## The two ⚠ cells, at full strength

### ⚠ Unfair Advantage — the weakest cell in the canvas

`../BRIEF.md` says it plainly: *"the moat is weak and I'd rather say so than invent one."* The canvas keeps that.

- **What a funded copycat lacks after two years:** accumulated per-customer routing history and its calibration. That is it.
- **Switching cost:** a base-URL change. Genuinely low (A6).
- **The algorithm:** published, plateaued, and reproducible by anyone [S4].
- **The one asymmetry worth defending:** prefill-activation routing is structurally unavailable to hosted competitors [S10] — not because they lack the skill, but because using it would require them to stop being hosted. It is the only cell entry that a competitor cannot copy by deciding to.
- **The honest reframe:** in a market where the mechanism commoditises into the serving engine [S11], the durable asset is the **measurement apparatus and the tolerance policy**, both of which are per-deployment. That is a services-shaped moat, not a software-shaped one, and it should be called what it is.

### ⚠ Revenue Streams — priced against a free substitute

- Routing is free for hosted-catalog teams, with no separate fee [S20]. CAMIR's price must be defended against zero for the adjacent segment.
- Share-of-savings is precedented [S38][S39], but those vendors' contracts turn on the **definition of eligible savings** — which is exactly the counterfactual a skeptical engineer will contest, and exactly why the harness has to be auditable rather than a dashboard.
- Open-core conversion is **1–5% of active users** [S40]. The revenue model needs a large free base to work at all, which makes P6 (Sam) load-bearing and makes any move to relicense fatal.

---

## What this canvas says when read as a whole

The left half (problem, solution, metrics, cost) is strong and evidence-backed. The right half (advantage, revenue) is thin and untested. **That is the accurate shape of CAMIR as of September 2026**: a well-grounded technical proposition with an unproven business layer, which is what a pre-traction capstone-origin venture should look like on paper. The failure mode to avoid is filling the right half with confident prose; the correct response is [../validation/riskiest_assumptions.md](../validation/riskiest_assumptions.md).

## Recommended next 3

1. **Test A4 before A1.** The technical assumption is measurable in two weeks with local GPUs and the founders will enjoy measuring it; the willingness-to-pay assumption is measurable in ten conversations and is the one that decides whether the technical result matters commercially.
2. **Move the Channels cell from aspiration to a decision** — specifically, commit to shipping as a LiteLLM routing strategy rather than a competing proxy, and put it in [channel_plan.md](channel_plan.md) with its economics.
3. **Re-read this canvas against [business_model_canvas.md](business_model_canvas.md)'s Key Partners block.** If the partner channel fails, the Channels cell here has nothing in it but a repository and a blog post, and the revenue cell has no path.
