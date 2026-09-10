# CAMIR — Use of funds

**What this is** — the raise: how much, over what period, with each dollar block tied to a named de-risking milestone rather than a function; the hires in order with the reason each one is next; the capital-efficiency metric to report; and what the next round's story requires this round to prove.
**Why it exists** — CAMIR's cheapest possible outcome is a **negative result that costs one quarter**: if the oracle ceiling on realistic mixed traffic is too low, no router — not even a perfect one — saves enough to sell, and the honest response is to publish and stop ([../validation/pivot_log.md](../validation/pivot_log.md) P1). A conventional seed plan spends eighteen months building the product and discovers this in month fourteen, having burned the whole raise to learn something a quarter of GPU time would have shown. **This plan is therefore sequenced so the kill signal arrives before the majority of the money is committed**, and the first block is deliberately small enough that stopping after it is not a failure anyone has to be talked into.
**How to read it** — §2's block table is the document; the gates between blocks matter more than the amounts. A skeptic should attack Block 1's size — arguing it is too small to attract a team is the strongest objection to this structure.
**Depends on / feeds** — depends on [../validation/stage_gate.md](../validation/stage_gate.md), [../validation/mvp_definition.md](../validation/mvp_definition.md), [unit_economics.md](unit_economics.md), [revenue_build.md](revenue_build.md), [../tech/not_vaporware.md](../tech/not_vaporware.md); feeds [risk_matrix.md](risk_matrix.md), [comps_exits.md](comps_exits.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

**Status: no raise has occurred, no term sheet exists, no capital has been committed.** This is a plan. `(assumption)` on every figure; costs are US-market engineering rates.

---

## 1. The ask

**$2.2M, 21 months, staged in three blocks with a gate between each.**

| | |
|---|---|
| Instrument | Pre-seed / seed `(assumption: no conversations have occurred)` |
| Runway | 21 months to Gate 2 ([../validation/stage_gate.md](../validation/stage_gate.md)) |
| Team at close | 3 → 6 |
| **Deliberate structure** | Block 1 is **$280k of the $2.2M**. If Gate 1.1 fails, ~87% of the raise is unspent |

**Why $2.2M and not $5M.** The revenue build reaches **$0.86M ARR in Y2 and $2.38M in Y3** at a $30,000 ACV. A larger raise buys a sales team the ACV cannot support — outbound CAC runs ~117% of ACV [../strategy/channel_plan.md](../strategy/channel_plan.md) — and the two channels that actually work (published frontier, peer proof) are bought with engineering and credibility, not headcount. **The binding constraint on this company is evidence, not capital.**

---

## 2. The three blocks

### Block 1 — $280,000 · months 1–4 · *"Does the ceiling exist?"*

The cheapest information available anywhere in the plan.

| Spend | Amount | Buys |
|---|---|---|
| Two engineers, 4 months | $180,000 | The low-fidelity MVP: corpus builder, oracle ceiling probe, artifact guard, judge harness, frontier builder, disqualification report |
| **GPU capacity for the measurement track** | **$45,000** | A dedicated multi-GPU node for ~2 months, so a 70B-class tier can stay resident alongside the small and mid tiers. E1 itself meters only ~120 GPU-hours (≈ $300–450 at [S26] rates); the node also carries E2, E3, E12, re-runs on ≥ 3 corpora, and the published reference `frontier_run`. Renting capacity rather than metered hours is the choice that keeps the track on schedule `(assumption: ~$2.50–3.75/GPU-hr [S26], 8 GPUs)` |
| Founder time on 20 discovery screens | $40,000 | f2, the never-measured rate, veto incidence |
| Legal, entity, licence commitment | $15,000 | The open-core boundary published **before** first release [S23] |

**Milestone: Gate 1** — ceiling above the disqualification threshold on ≥ 3 corpora, artifact share ≥ 5pp on ≥ 2, ≥ 6 of 20 screened qualify on E4's definition, ≥ 7 of 15 report a real veto.

**Kill condition:** ceiling below threshold → **publish the negative result and stop.** It is genuinely publishable — [G2] records that no routing savings have ever been published for a self-hosted open-weight pool, so the null is a contribution. That is what makes stopping a defensible professional outcome rather than a failure, and it is the reason this block is first.

**What is deliberately not funded here:** the router, the classifier, any control plane, any UI, any sales motion.

### Block 2 — $820,000 · months 5–12 · *"Will a risk-bearer accept it?"*

| Spend | Amount | Buys |
|---|---|---|
| Team of 4 (2 eng + founding eng + founder), 8 months | $520,000 | The high-fidelity MVP: ingress proxy, cascade route, conformal-thresholded gate, tolerance policies with named owners, shadow mode, pin registry, trace stamper, cost meter, counterfactual ledger |
| **Onboarding automation** | **$120,000** | Log-format coverage, pre-flight estimation, corpus defaults. **The 42%→75% gross-margin bend is entirely this** ([unit_economics.md](unit_economics.md) §6) |
| LiteLLM strategy integration | $60,000 | Stack A, ~40% of Y1–Y3 logos: the ~$40,000 build in [../strategy/channel_plan.md](../strategy/channel_plan.md) plus upstream maintenance — **and the dependency is unagreed** |
| 5 design partners: GPU, support, travel | $80,000 | E13, the veto experiment |
| Published reference frontier + cost-axis derivation | $40,000 | The Stack B artifact (~$8,300 CAC, half of it fairly chargeable to R&D), and the public credibility [S7] leaves unclaimed |

**Milestone: Gate 2.1** — **≥ 2 of 5 design partners' consuming engineers enable enforcement on their own endpoint**, within 6 weeks of their own shadow report, unprompted. Plus: escalation rate below break-even on ≥ 60% of enforced endpoints, disqualification rate non-zero.

**Kill condition:** ≤ 2 of 5 → pivot P4: sell the **measurement layer alone**, with no request-path component. That is a smaller company and it is a real one.

**The line item that will be cut first and must not be:** onboarding automation. It buys no feature, demos to nobody, and is the entire difference between a 42% and a 75% gross margin.

### Block 3 — $1,100,000 · months 13–21 · *"Does it repeat, and will they pay?"*

| Spend | Amount | Buys |
|---|---|---|
| Team of 6, 9 months | $720,000 | Control plane: policy management across owners, savings attribution and the signed report, drift monitor, recalibration scheduler, frontier diff, routing observability |
| First commercial hire (technical, not a closer) | $180,000 | Runs evaluations, not a pipeline. At a $30,000 ACV the sale **is** the evaluation |
| Design-partner conversion + 3 paid pilots | $110,000 | Gate 2.2 |
| Product 2 spike — one lever, measured | $60,000 | Cache-policy quality cost [G4] or quantisation-tier measurement: the first evidence for the expansion thesis |
| Reserve | $30,000 | |

**Milestone:** ≥ 3 paid pilots at ≥ $30,000 ACV; counterfactual survives a customer's own re-derivation in ≥ 2 accounts with no dispute on method; **one account survives a pool change and recalibrates.**

---

## 3. Hires, in order, with the reason each is next

| # | Hire | When | Why this one now | Why not earlier |
|---|---|---|---|---|
| 1 | **Evaluation / benchmark engineer** | Block 1 | The whole company is a measurement claim. This is the founding team's stated edge (A13) and the first hire has to be the thing being sold | — |
| 2 | **Systems engineer** (serving, GPU, throughput) | Block 1 | The ceiling probe is O(corpus × tiers) full generations; it either finishes in a customer's afternoon or the wedge is a project | — |
| 3 | **Founding product engineer** | Block 2 | The four P0 features are pure product engineering with **no technique behind them** ([../tech/techniques/technique_feature_matrix.md](../tech/techniques/technique_feature_matrix.md) §4), and a technically led team builds them last | Nothing to build until the ceiling exists |
| 4 | **Developer-relations / community** | Block 2, late | Stacks B and C are 50% of Y1–Y3 logos, C is the lowest-CAC live channel, and P6 Sam is the channel | Community before an artifact is marketing without a product |
| 5 | **Technical account / solutions** | Block 3 | The sale is an evaluation; onboarding cost is the margin story | — |
| 6 | **Second systems engineer** | Block 3 | Multi-endpoint scale and recalibration load | — |

**Not hired in 21 months, deliberately:** a VP Sales (outbound CAC ~117% of ACV), a designer as a full-time role (two of the three deciding people never open the product — [../product/ux_spec.md](../product/ux_spec.md)), and an ML researcher. **The last is the counterintuitive one:** the classifier is an ablation expected to be null [S4][S8], and hiring a researcher creates an organisational incentive to make it the roadmap.

---

## 4. Capital efficiency — the metric to report

**Not burn multiple. Not ARR per employee.** Both are meaningless at 5 customers.

**Report: dollars spent per assumption killed.**

| Block | Spend | Assumptions resolved | $/assumption |
|---|---|---|---|
| 1 | $280,000 | A2 (ceiling), A13 (harness edge), A3 (segment), veto incidence | **$70,000** |
| 2 | $820,000 | The veto's closability, cascade economics at compressed spreads, channel viability | $273,000 |
| 3 | $1,100,000 | A4 (will anyone pay), A5 (is the saving billable), A6 (does switching cost rise) | $367,000 |

**Block 1 kills the three that can end the company, at 13% of the raise.** That ratio is the plan's actual argument, and it is the one to defend in a partner meeting rather than the revenue build.

Secondary metrics: **time to first frontier** on a stranger's hardware (M7, target < 5 days), and **disqualification rate** (M16, target non-zero — a company reporting zero has stopped believing its own instrument).

---

## 5. What the next round's story requires this one to prove

The Series A story is **not** "routing saves money." At 2026 denominators [../strategy/market_sizing.md](../strategy/market_sizing.md) states first that routing fees alone are not a venture-scale business — a $70M/yr TAM base case does not carry a venture return. The A story is **the inference-efficiency control plane**: CAMIR measured the first lever, and there are four more nobody measures at all ([revenue_build.md](revenue_build.md) Product 2, TAM $460–760M by 2031).

**For that story to be sayable, this round must produce four things:**

1. **A published, reproducible frontier methodology** — cost axis derived for self-hosted pools, judging protocol, inter-judge agreement — that someone outside CAMIR has re-run. It is the standard-setting claim, and it is what [S7] and [S13] leave unclaimed.
2. **Two consuming engineers who enabled enforcement themselves.** The organisational proof. Without it, CAMIR is a tool for teams with no internal politics, which is a much smaller market than the one being sized.
3. **One account that survived a pool change and recalibrated.** This is what distinguishes a subscription from a consulting engagement, and [S29] guarantees the event arrives.
4. **One Product 2 lever measured on real traffic.** One number nobody else has — cache-policy quality cost [G4] is the best candidate — is what converts the expansion thesis from a slide into evidence.

**What this round must not produce:** a classifier accuracy result. It is the most legible engineering progress available and the least relevant [S4], and a deck led by it invites the one citation that ends the meeting.

---

## Recommended next 3

1. **Raise Block 1 alone if the full round is hard, and say why.** $280k against a quarter that can end the company is an easier conversation than $2.2M against an eighteen-month build, and the structure itself is the strongest signal about how this team makes decisions.
2. **Put the P1 kill condition in the deck, not the data room.** A team that has pre-committed to publishing a negative result and stopping is making a statement about integrity that no traction slide substitutes for — and it is far easier to hold before there is revenue to lose by holding it.
3. **Ring-fence the $120k onboarding-automation line in Block 2.** It is invisible on any customer-facing roadmap, it will be reallocated to a feature under partner pressure, and it is the whole 42%→75% gross-margin bend that the Series A margin story depends on ([unit_economics.md](unit_economics.md) §6).
