# CAMIR — Positioning

**What this is** — the two axes that actually divide the LLM inference-cost market, where every competitor sits on them, the open quadrant, and CAMIR's one-sentence positioning statement.
**Why it exists** — the instinctive positioning for a router is "cheaper inference, same quality", and that sentence is dead on arrival in 2026: a frontier vendor gives routing away with no separate fee [S20], and the field's own benchmark of 21 routing methods shows they all land in the same narrow accuracy band [S4]. Positioning on the cost-quality plane puts CAMIR into a price war against free, on an axis where it cannot differentiate. This file exists to stop that sentence being written into the deck.
**How to read it** — §Why the obvious axes are wrong is the load-bearing section; a skeptic should attack there, because if cost-versus-quality *is* the right map then CAMIR has no position. Then check the statement in §Positioning statement against the mechanism in [../research/landscape.md](../research/landscape.md) §7.
**Depends on / feeds** — depends on [../research/competitors.md](../research/competitors.md) §Two axes, [market_type.md](market_type.md); feeds [value_prop_canvas.md](value_prop_canvas.md), [gtm.md](gtm.md), [../narrative/one_pager.md](../narrative/one_pager.md), [../narrative/pitch_deck.md](../narrative/pitch_deck.md) and [../narrative/mission_vision.md](../narrative/mission_vision.md).

---

## Why the obvious axes are wrong

The default map for anything cost-related is **cost reduction × quality retained**. For CAMIR it fails three separate tests, and it fails them in a way worth being precise about, because the axes are not unimportant — they are simply the *output* of every product here, not the dimension that separates them.

1. **Everyone converges on the quality axis.** 21 routing methods across five benchmarks — classifier, retrieval, ranking, latent-factor, contrastive, cascade and bandit families — collapse into a narrow accuracy band far below the oracle router, because they share a predictability bottleneck [S4]. An axis on which the entire field scores within a few points does not divide a market.
2. **Nobody's numbers are comparable on the cost axis.** A 2026 paper argues directly that router evaluations across papers cannot be compared [S13], and the same router shows **3.66× on MT-Bench and 1.41× on MMLU** [S2]. Placing competitors on an axis requires commensurable measurements that do not exist.
3. **The cost axis has a free point on it.** Vendor-native routing charges no routing fee [S20]. Competing on cost reduction means competing against zero.

**The cost-quality frontier is what CAMIR *plots*. It is not what CAMIR *competes on*.** That distinction is the whole positioning.

---

## The two axes that do divide this market

### Axis X — Who owns the weights: hosted catalog ←→ self-hosted pool

A hard structural boundary rather than a preference. It determines four things at once:

- **What signals the router can see.** Prefill activations are available only to whoever runs the weights [S10]; a hosted router cannot reach them without ceasing to be hosted.
- **Whether the saving is auditable.** On the hosted side, the party computing "what you would have spent" is the party billing the difference.
- **Whether data leaves the perimeter.** The reason a large share of self-hosters self-host at all — and note that self-hosting **rarely wins on cost alone** against budget open-weight APIs [S28], so price is not their motive.
- **Whether incentives align.** OpenRouter takes ~5% of inference spend [S17]; its revenue rises with the bill. A frontier vendor is paid more when it routes up [S20].

**No commercial incumbent can cross this axis**, because on the hosted side their product *is* the catalog and the intermediary position.

### Axis Y — Who sets the quality tolerance: vendor-set and opaque ←→ customer-set and measured

The axis the market discovered the hard way in 2026. When routing shipped as a default with a tolerance the vendor chose, users immediately reported complex queries degraded by being sent to the smaller model — and had no dial [S20][S21]. Fixed-model policies state no tolerance at all. Commercial routers state a saving they compute themselves.

**Nobody hands the customer a plotted frontier for their own traffic and lets them choose the point on it.** That is what "customer-set and measured" means, and it is empty.

### The map

```
                customer-set tolerance · frontier measured on your traffic
                                       ▲
                                       │
                                       │        ◆ CAMIR
                        (empty)        │          ── the open quadrant ──
                                       │
   hosted catalog ────────────────────┼────────────────────► self-hosted pool
                                       │
   ◆ Martian          ◆ Not Diamond    │   ◆ LiteLLM     ◆ vLLM Semantic Router
   ◆ OpenRouter       ◆ GPT-5 router   │   ◆ RouteLLM    ◆ hand-written rules
   ◆ fixed frontier model              │   ◆ fixed cheap model
                                       │
                                       ▼
                    vendor-set tolerance · opaque, self-reported, or absent
```

### Placement, with the reason for each

| Competitor | X (weights) | Y (tolerance) | Why there |
|---|---|---|---|
| Fixed frontier model | hosted | none | No decision; no tolerance stated. The tolerance is implicitly zero at maximum price |
| Fixed cheap model | either | none, and unmeasured | Accepts unbounded quality loss on the hard subset without measuring it |
| Hand-written rules | self-hosted | none | A routing policy nobody has ever plotted against a frontier |
| GPT-5-style vendor routing | hosted, single catalog | **vendor-set, no dial** | Real-time router across the vendor's own tiers, no separate fee [S20]; the backlash is the evidence for the placement [S21] |
| Martian | hosted | vendor-set, self-reported | Routing proxy with per-request attribution; the savings counterfactual is computed by the biller [S19] |
| Not Diamond | hosted | vendor-set | Recommender over the vendor's catalog, ~$0.05 per million tokens routed [S19] |
| OpenRouter | hosted, maximal catalog | absent | Gateway with an Auto Router; monetises 5% of spend [S17], so its incentive runs opposite to shrinking the bill |
| LiteLLM | **self-hosted** | absent | Normalises interfaces; it is plumbing, not policy [S22] |
| RouteLLM | self-hosted-capable | absent | Reference implementation of a strong/weak classifier; no per-deployment tolerance surface [S1] |
| vLLM Semantic Router | **self-hosted, native** | absent *today* | Workload–Router–Pool inside the serving engine [S11]. **The one competitor that could move up the Y axis**, which is the commoditisation clock |
| **CAMIR** | **self-hosted pool** | **customer-set, measured on your own traffic** | The declared tolerance plus an auditable frontier is the entire product |

---

## The open quadrant, and the honest reason it is open

The upper-right quadrant is empty. That is a fact, not a compliment, and the reason matters:

- The **commercial incumbents cannot enter** without abandoning the catalog their revenue depends on.
- The **self-hosted incumbents are free open-source projects** with no commercial motive to build a measurement and policy layer.

So the quadrant is empty because **it is hard to monetise, not because nobody thought of it**. CAMIR's position is defensible against copying and exposed to the question "can this be a business at all" — which is exactly the risk profile [market_type.md](market_type.md) identifies and [market_sizing.md](market_sizing.md) prices.

---

## Positioning statement

> **For platform teams running LLM features at volume on their own model pool, CAMIR is the only inference router that lets you set the quality tolerance yourself and shows you the measured cost-quality frontier for your own traffic — because the router, the mixed-difficulty benchmark and the judging protocol are one open harness you run inside your perimeter, so the savings number is yours to audit rather than your vendor's to report.**

### The three words doing the work

- **"your own model pool"** — the X-axis boundary. It excludes the hosted-catalog majority on purpose, and it is what makes prefill-activation signal [S10] and in-perimeter measurement possible.
- **"set the quality tolerance yourself"** — the Y-axis boundary, validated by the public failure of the alternative [S21].
- **"yours to audit"** — the incentive claim. Every incumbent's savings number is computed by the party being paid for it.

### What the statement deliberately does not say

- **Not "up to 85% cheaper."** That number is RouteLLM's, on MT-Bench, and falls to 1.41× on MMLU [S1][S2]. CAMIR has measured nothing yet, and a borrowed multiple would be the fastest way to fail the "says who" test.
- **Not "smarter routing."** [S4] falsifies it in one citation.
- **Not "the first LLM router."** It is not, and the field has a curated tracker of prior work [S16].

---

## Anti-positioning — three sentences that must never appear in this pack

1. *"CAMIR uses AI to intelligently route your requests."* — mechanism-free, and the banned-adjective test in the quality bar catches it.
2. *"CAMIR cuts inference costs by up to 85% with no quality loss."* — borrowed number, wrong benchmark, and "no quality loss" contradicts the entire tolerance concept, which exists because there *is* loss and the customer should choose how much.
3. *"CAMIR is a better router than Martian."* — wrong axis, unfalsifiable claim, and it re-frames a segment play as a feature war against a better-funded incumbent.

---

## The three objections this position must survive

| Objection | Answer | Residual weakness |
|---|---|---|
| *"Routing is free — my vendor does it."* | Only inside one catalog, with a tolerance you cannot set, on weights you do not run. If your models are your own, that router does not exist for you [S20] | None, for the beachhead. Fatal for anyone on a single hosted catalog — which is why they are not the beachhead |
| *"vLLM will ship this natively."* | Probably, for the mechanism [S11]. The measurement layer — mixed-difficulty benchmark, judging protocol that survives the 2026 reliability results [S33][S34], per-deployment savings attribution — is not on that path today | **Real and unmitigated.** This is the commoditisation clock, and CAMIR's answer is to make measurement the asset, not to deny the risk |
| *"You have no traction."* | True. Origin is a capstone; nothing is measured yet | **Real.** The counter is that the first deliverable is a *published, reproducible* frontier — evidence anyone can re-run, which is a stronger opening asset than a logo |

---

## Recommended next 3

1. **Freeze the positioning statement verbatim** and require every narrative artifact to derive from it rather than paraphrase it — paraphrase is how "measured tolerance" degrades back into "cheaper inference".
2. **Test the tolerance dial as the wedge in discovery, not the savings.** The hypothesis to falsify is that engineers care about *setting* the tolerance, not merely about the bill; the GPT-5 backlash [S21] is suggestive evidence and not proof. See [../validation/discovery_guide.md](../validation/discovery_guide.md).
3. **Instrument the commoditisation clock.** Watch vLLM Semantic Router [S11] for any measurement or evaluation feature; its arrival is the leading indicator that CAMIR's open half is losing its reason to exist, and it belongs in [../financials/risk_matrix.md](../financials/risk_matrix.md) as a tracked indicator rather than a background worry.
