# CAMIR — Go-To-Market

**What this is** — the channel strategy by segment, the acquisition loop that compounds, a CAC hypothesis with payback logic per channel, and the 90-day motion from first install to first ten paying customers.
**Why it exists** — CAMIR's buyer is unreachable by the methods a pre-revenue team can afford. Marcus (P2) does not answer outbound, does not attend webinars, and does not buy from a demo; he installs things his peers vouched for. Meanwhile the free substitute is already in his stack [S20][S22]. A GTM that lists channels without confronting that would produce a plan whose first line is "content marketing" and whose first result is nothing. This file exists to force one decision — **distribute through the proxy he already runs, not against it** — and to price the alternatives honestly enough that the decision is obvious.
**How to read it** — §The one decision is the file. Everything after it is the arithmetic that justifies it and the 90-day sequence that executes it. A skeptic should attack the CAC numbers in §Channel economics, all of which are hypotheses with a stated basis and none of which have been run.
**Depends on / feeds** — depends on [positioning.md](positioning.md), [personas.md](personas.md), [market_type.md](market_type.md), [market_sizing.md](market_sizing.md); feeds [channel_plan.md](channel_plan.md) (the discount/margin stack per channel), [sales_roadmap.md](sales_roadmap.md), [../validation/get_keep_grow.md](../validation/get_keep_grow.md) and [../financials/unit_economics.md](../financials/unit_economics.md).

**No customer has been acquired through any channel described here.** Every CAC figure is a hypothesis with its basis stated.

---

## The one decision

**Ship CAMIR as a routing strategy inside the proxy self-hosters already run — LiteLLM — rather than as a competing proxy.**

The reasoning is not preference, it is arithmetic. CAMIR's beachhead is ~1,370 reachable companies ([market_sizing.md](market_sizing.md)), which is far too few to support paid acquisition and far too many to reach by hand from three people. The only affordable path into that population is the software they have already installed. LiteLLM is the self-hosted OpenAI-compatible proxy that population runs [S22]; asking Marcus to replace it is asking him to take a migration risk to evaluate a cost optimisation, which is the wrong order of risk and reward. Asking him to enable a routing strategy in a proxy he already operates is a config change he can revert.

**What this costs:** dependence on another project's extension surface, and a weaker brand. **What it buys:** the entire top of funnel, at effectively zero CAC. Given a base-case TAM of ~$70M/year, there is no budget for the alternative.

---

## Channel strategy by segment

### Beachhead — P2 Marcus, platform teams self-hosting at ≥~$50k/month

| Channel | Motion | Why it fits | Status |
|---|---|---|---|
| **Proxy-native distribution (LiteLLM routing strategy)** | Enable a strategy in an installed proxy; produce a frontier from logged traffic in one afternoon | Zero migration risk; meets him where he already is [S22] | **Primary. Requires a partner conversation — see [channel_plan.md](channel_plan.md)** |
| **The published frontier as the credibility artifact** | Publish the methodology and the cost-quality frontier for a self-hosted pool — the thing nobody has published [G2] — with the judging protocol and inter-judge agreement statistics [S33][S34] | The field's own critique says router results are not comparable [S13]; being the reproducible one is the position | **Primary. Costs GPU-hours, not sales headcount** |
| **Peer proof from P3 Wen** | One technically demanding team adopting publicly shortens every subsequent evaluation | Marcus buys what his peers vouched for | Secondary, and it is a *consequence* of the two above |
| Conference talks / infra meetups | Speak on the measurement result, not the product | Reaches Marcus's peer set; slow, cheap, high trust | Secondary |
| Outbound email to platform engineers | — | **Explicitly rejected.** Marcus does not answer it, and it burns the open-source goodwill P6 supplies | Never |

### Edge-low — P1 Priya, drop-in teams

Same channels, different landing surface: a defaults-only path with shadow mode on by default, one command, and a conservative tolerance. Priya converts on **effort**, not evidence — if the first run needs a config file, she is gone.

### Edge-high — P3 Wen, teams with their own eval capacity

Reached by the **methodology**, not the product. Her adoption trigger is a judging protocol she can audit and a cost axis derived for self-hosted pools [S7]. Publish the harness as a citable artifact; she will find it. She is low-volume as a segment and disproportionately valuable as a reference.

### P6 Sam — the open-source adopter who never pays

**Not a lead. The channel itself.** Open-core conversion runs 1–5% of active users to hosted SaaS [S40], so the free base must be large for the paid layer to exist at all. Treat Sam's afternoon-to-a-chart experience as a first-class product surface, and never gate it.

---

## The acquisition loop that compounds

```
   Sam runs the harness on his own logs in an afternoon
              │  produces a frontier chart nobody else can produce for a self-hosted pool
              ▼
   He posts the chart  ──────────────────────────────►  Marcus sees a peer's number, not a vendor's
              │                                                        │
              │                                                        ▼
              │                                        Marcus enables the strategy in his existing proxy
              │                                                        │
              │                                                        ▼
              │                                    His frontier is more interesting than Sam's
              │                                    (real mixed traffic, three tiers, real volume)
              │                                                        │
              ▼                                                        ▼
   More published frontiers  ◄──────────────────────  He publishes it, or it circulates internally
              │
              ▼
   The corpus of self-hosted frontiers becomes the reference nobody else has [G2]
```

**What actually compounds:** not users, and not data in the network sense — the **corpus of published frontiers**. Each one is simultaneously marketing, evidence for the methodology, and a contribution to a gap the field has publicly identified [S13]. This is the only compounding asset CAMIR has, and it is weaker than a network effect. `../BRIEF.md` says so; this loop does not pretend otherwise.

**What does not compound:** the routing algorithm (published and plateaued [S4]), and per-customer routing history (real, but private to each deployment — A6).

---

## Channel economics — CAC hypotheses

Full discount/margin stack in [channel_plan.md](channel_plan.md). Here: acquisition cost and payback, against an ACV of **$30k/yr** ([market_sizing.md](market_sizing.md)).

| Channel | CAC hypothesis | Basis | Payback | Verdict |
|---|---|---|---|---|
| **Proxy-native distribution** | **$0 marginal**, ~$40k one-off integration engineering | `(assumption: 4–6 engineer-weeks to build and upstream a routing strategy plus its config surface)` | Amortised across all customers; ~2 customers to repay the build | **Viable at any volume. The only channel that is.** |
| **Published frontier / methodology** | ~$8k per customer acquired | `(assumption: ~$25k of GPU-hours and engineering per major publication, converting ~3 evaluations over 12 months)` | ~3.2 months | **Viable.** Also produces the research contribution, so it is dual-purpose spend |
| **Peer proof / reference customers** | ~$3k per customer | `(assumption: cost of supporting a reference deployment beyond normal onboarding)` | ~1.2 months | **Best ratio, lowest volume.** Cannot be scaled deliberately |
| **Conference talks** | ~$6k per customer | `(assumption: ~$12k travel and preparation per talk, ~2 evaluations each)` | ~2.4 months | Viable, slow, and it compounds into peer proof |
| **Paid developer advertising** | ~$25k+ per customer | `(assumption: developer-infrastructure CPMs against a ~1,370-company population; the audience is too small for targeting to work)` | ~10 months | **Rejected.** The population is too small for paid to find |
| **Outbound sales** | ~$35k+ per customer | `(assumption: SDR-driven outbound at infrastructure ACVs)` | >12 months, exceeds ACV in year one | **Rejected.** Exceeds ACV and antagonises the open-source base |

**The pattern is not subtle.** Every viable channel is a by-product of doing the technical work well, and every rejected one requires a budget CAMIR's TAM does not justify. That is a constraint, and it is also a reason the plan is cheap to run.

---

## The 90-day motion

### Days 1–30 — establish the evidence, not the product

1. Run the **oracle-ceiling experiment** on a public mixed-difficulty benchmark with a three-tier self-hosted pool ([../validation/experiment_board.md](../validation/experiment_board.md)). Instrument against the three known artifacts from day one — generous generation budgets with truncation logging, strict format parsing with failure counts, verbosity-controlled judging [S5].
2. Publish the **self-hosted cost axis** — amortised GPU-hours per token under a stated utilisation assumption — which no published benchmark has derived [S7][S26][S27].
3. Ship the **harness** as an open repository that runs on one GPU and produces a frontier chart in an afternoon. This is P6 Sam's entire experience and the top of the whole funnel.
4. Run **ten discovery calls** against the P2 profile, whose primary question is the sizing question that collapses factor f2 ([market_sizing.md](market_sizing.md)), not a product pitch.

**Gate:** if the oracle ceiling is low, stop and publish the null result. It is a real contribution and it saves everything downstream.

### Days 31–60 — get into the stack

5. Build and upstream the **LiteLLM routing strategy**, with per-endpoint tolerance and tier-stamped traces from the first version — the P0 features from [value_prop_canvas.md](value_prop_canvas.md), which cannot be retrofitted after a deployment has already scared a Ravi.
6. Publish the **frontier + methodology** write-up, with inter-judge agreement reported alongside every curve [S33][S34].
7. Convert the ten discovery calls into **three shadow-mode installs** — no charge, no enforcement, tier decisions logged against real traffic.

**Gate:** three shadow installs producing frontiers on real traffic. If nobody will run shadow mode for free, willingness to pay (A4) is already answered.

### Days 61–90 — the first paid conversations

8. Take the three shadow installs to **enforcement at a declared tolerance**, each with its own before-and-after report — the artifact Dana signs on.
9. Run the **first pricing conversations** against A4 and A5 with those three, testing share-of-savings against per-request as alternatives rather than presenting one.
10. Recruit the **first reference deployment** from whichever of the three has the most demanding evaluation practice — a Wen, if one is available.

**Gate for the first 10 customers:** not a revenue number. It is **three enforced deployments with published before-and-afters and a signed savings definition**. Without an agreed definition of eligible savings, share-of-savings pricing is unbillable — the lesson from the FinOps precedent, where the eligible-savings definition is exactly what the contract negotiates [S38][S39].

### First 10 → first 100

The first ten come from discovery and the published methodology, one at a time, hand-run. The first hundred come from proxy-native distribution: the routing strategy is in the proxy, a fraction of installs enable it, and a fraction of those cross the volume floor where the control plane is worth paying for. **That transition is the whole company risk** — if proxy-native distribution does not convert to paid at open-core rates [S40], CAMIR is a widely-used free tool with no revenue, which is the shape TensorZero had when it archived its repository with 11,000 stars [S23].

---

## Recommended next 3 moves

1. **Open the LiteLLM partner conversation this month.** It is the primary channel, it is the only one whose CAC survives contact with the TAM, and it depends on a third party who has not yet agreed. Until it is agreed, the GTM has no primary channel — that is a dependency, not a plan, and it belongs in [../financials/risk_matrix.md](../financials/risk_matrix.md).
2. **Publish the null result if the oracle ceiling is low.** A credible negative on a question the field is actively arguing [S4][S5] builds more standing than a quiet pivot, and it is the honest use of a capstone.
3. **Instrument the free-to-paid ratio from the first install.** Open-core conversion at 1–5% [S40] is the assumption the entire revenue build rests on, and it is measurable long before anyone is asked to pay.
