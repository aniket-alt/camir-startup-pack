# CAMIR — Risk matrix

**What this is** — the eight risks that decide whether CAMIR exists, each with likelihood, impact, the **leading indicator that fires before the risk lands**, the mitigation, and an honest residual after mitigation.
**Why it exists** — CAMIR's most dangerous risks are not the ones a reader would guess, and two of them are unusual enough that a generic matrix would miss both. **R1 is that the technical premise is simply false** — [S4] finds 21 routing methods converged in a narrow band far below the oracle, and if the ceiling on real traffic is low, no router saves enough to sell. **R2 is that the mechanism commoditises into the serving engine** [S11], which is not a competitor risk but a clock: it arrives on someone else's schedule regardless of what CAMIR does. A matrix that resolved either to "low" would be fiction, and neither does.
**How to read it** — read the **Residual** column first and stop at the two that stay High. §3's tripwires are the operational content. A skeptic should attack R6's residual, where the mitigation is a product feature justified by one public incident and zero interviews.
**Depends on / feeds** — depends on [../ASSUMPTIONS.md](../ASSUMPTIONS.md), [../validation/riskiest_assumptions.md](../validation/riskiest_assumptions.md), [../validation/pivot_log.md](../validation/pivot_log.md), [../research/sources.md](../research/sources.md); feeds [use_of_funds.md](use_of_funds.md), [comps_exits.md](comps_exits.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

---

## 1. The matrix

| # | Risk | Type | Likelihood | Impact | Leading indicator | Mitigation | **Residual** |
|---|---|---|---|---|---|---|---|
| **R1** | **The oracle ceiling on real mixed traffic is too low for routing to pay** | Technology | **Medium** | **Fatal** | Ceiling below the disqualification threshold on the first 2 of 3 corpora | None available — it is a fact about the world. **Structural response: measure it first, at 13% of the raise** ([use_of_funds.md](use_of_funds.md) Block 1), and publish the null [G2] | **High** |
| **R2** | **The serving layer absorbs routing** — vLLM Semantic Router and successors [S11] | Competition / platform | **High** | High | Engine release notes moving from dispatch into measurement: ceiling probes, judge harnesses, cost derivation | Own the **measurement** half, which no engine will run: a customer's oracle probe, published judge agreement, an amortised cost axis, a disqualification. Ship above the engine, never inside it ([../tech/architecture/D07.md](../tech/architecture/D07.md)) | **High** |
| **R3** | **Tier spread compresses** — a 31B-class model within ~10 Elo of far larger open-weight models [S29] | Cost curve | **High** | High | Frontier of open-weight Elo-per-parameter flattening; realised savings rate falling run over run | Product 2: the four efficiency levers unaffected by compression — cache policy, quantisation tier, batch/utilisation, upgrade regression ([revenue_build.md](revenue_build.md)) | **Medium** |
| **R4** | **Nobody pays for routing as a line item** (A4) | Market | Medium | High | Free installs reaching frontiers with zero paid conversions from ≥ 20 active OSS deployments | Price out of the inference bill, never a new line ([pricing.md](pricing.md)); floor at $12,000/yr; declared fallback to ~5% of spend under management [S17] | **Medium** |
| **R5** | **Open-core fails to convert** — the TensorZero shape [S23] | Business model | Medium | High | OSS installs growing while control-plane conversion stays under 1% against the 1–5% band [S40] | The open/paid line published **before** first release; no feature ever moves open → paid; the paid half is organisational work, not withheld features ([../tech/architecture/D06.md](../tech/architecture/D06.md)) | **Medium** |
| **R6** | **The consuming engineer vetoes and never un-vetoes** | Retention / organisational | Medium | High | **Pin-to-large rate rising** (M11), on the front page as a failure metric | The four P0 features: owned tolerance, per-request attribution on his own spans, shadow by default, unilateral pin. Fast attribution path ([../tech/architecture/D08.md](../tech/architecture/D08.md)) | **Medium** |
| **R7** | **The LiteLLM channel closes** — an unagreed third-party surface | Platform / channel | Medium | Medium | Any upstream signal: a competing native strategy, a plugin-API change, maintainer non-response | Standalone proxy stays a first-class deployment shape; three shapes, not one ([../tech/architecture/D07.md](../tech/architecture/D07.md)) | **Low-Medium** |
| **R8** | **Per-deployment fitting is a service, not software** | Margin | Medium | Medium | Gross margin under 60% across ≥ 5 accounts; onboarding above 3 engineer-days at customer 10 | Onboarding automation ring-fenced at $120k in Block 2 — the whole 42%→75% bend ([unit_economics.md](unit_economics.md) §6) | **Medium** |

**Two residuals stay High and neither is mitigable by effort.** R1 is a fact about the world; the only response is to learn it cheaply and early. R2 is a clock set by other people; the only response is to be somewhere the clock does not reach.

---

## 2. The four risks a generic matrix would list, and why they rank lower here

| Risk | Why it is not top-eight for CAMIR |
|---|---|
| **A frontier vendor ships routing** | **Already happened.** [S20]: a unified system with a real-time router, no separate routing fee. It is the premise of the pack, not a future event — and [S21] documents what vendor-set tolerance produced |
| **Regulatory / compliance** | CAMIR reads metadata, never prompt text (O1), and runs entirely inside the customer's perimeter (N4). The residency question is a *feature* here: it is why the beachhead self-hosts [S28]. The genuine residual is customer-side, and D06 is the answer |
| **Key person** | Real for a three-person team, and not differentiating. The specific version worth naming: the founding team's edge is benchmark engineering (A13), so the evaluation engineer is the irreplaceable role, not the founder |
| **Adversarial abuse** | Forced deferral inflates a victim's bill without degrading any answer [S9]. It is a **product requirement** (O5) rather than a company risk — the detector ships with the gate ([../tech/architecture/D08.md](../tech/architecture/D08.md)) |

---

## 3. Tripwires — the indicator, the threshold, and who watches it

A leading indicator nobody watches on a schedule is a paragraph.

| Risk | Tripwire | Threshold | Cadence | Consequence |
|---|---|---|---|---|
| R1 | Median oracle ceiling across corpora | Below disqualification threshold | Every `frontier_run` in Block 1 | **Publish and stop** ([../validation/pivot_log.md](../validation/pivot_log.md) P1) |
| R2 | vLLM Semantic Router scope | Any measurement capability shipped | **Quarterly, written, one line** | Re-open P7; the wedge may be gone |
| R3 | Realised savings rate, trended | Below 15% across ≥ 3 accounts | Per recalibration | ACV assumption in [pricing.md](pricing.md) breaks; the two price anchors diverge 41% |
| R4 | Paid conversions from active OSS deployments | Zero at ≥ 20 deployments, 12 months | Monthly | P6: move the paid boundary, or stop |
| R5 | Conversion rate vs the 1–5% band [S40] | Under 1% at month 12 | Quarterly | Same as R4 |
| R6 | **Pin-to-large rate** (M11) | Above 2%, or any upward trend | **Weekly, per deployment** | Fast-path attribution is failing; escalate before the account does |
| R7 | Upstream LiteLLM signal | Any | On notice | Standalone becomes primary; re-price for a longer cycle |
| R8 | Onboarding engineer-days | Above 3 at customer 10 | Per onboarding | The margin story is wrong; reprice as services and say so |

**R6's tripwire is weekly and the others are not**, because it is the only one that lands as a silent, individual, unappealable decision made on an ordinary Tuesday by someone who is not in any conversation with CAMIR.

---

## 4. Interactions — where two risks compound

1. **R3 × R1.** Tier compression [S29] does not only shrink the fee; it lowers the oracle ceiling itself, because routing between two nearly-equal tiers has less to find. **These are the same risk observed at two time horizons**, which is why R1's mitigation is speed and R3's is a different product.
2. **R2 × R5.** If the serving engine ships free dispatch *and* open-core conversion is weak, CAMIR is giving away the half that is commoditising while failing to monetise the half that is not. **The compound outcome is the TensorZero shape exactly** [S23]: a well-regarded open project with no commercial layer.
3. **R6 × R8.** A deployment where pins are rising demands more support, which raises cost to serve, which breaks the margin story — a retention failure that arrives disguised as a cost problem.
4. **R4 × R7.** If the LiteLLM channel closes, the sales cycle lengthens and CAC rises against a $30,000 ACV that cannot fund it, converting a channel risk into a pricing crisis.

---

## 5. The honest summary

**CAMIR carries two unmitigable High residuals, and they are different in kind.** R1 is *epistemic* — the answer exists in the world today and the company can buy it for $280k in four months. R2 is *temporal* — the answer does not exist yet and arrives on someone else's schedule.

The plan's whole structure is a response to that pair: **resolve R1 before spending the raise, and position against R2 by owning the half of the problem an engine has no reason to build** — a customer's own ceiling, published judge agreement, an amortised self-hosted cost axis, and a report that tells a customer not to deploy.

**If both High residuals land, this is not a company.** That is a real possibility and it is the reason the negative result is pre-committed as a publishable outcome rather than a failure.

---

## Recommended next 3

1. **Put R1 on a calendar with a date, not a milestone.** It is the only risk in this matrix that can be fully resolved for a known price on a known schedule, and every week it stays open is a week the other seven are being managed against a premise nobody has checked.
2. **Start the R2 quarterly review now, before there is a product to protect.** One written line per quarter on whether the serving engines have moved from dispatch into measurement. The failure mode is not being surprised — it is noticing eighteen months late because nobody owned the note.
3. **Watch M11 weekly from the first enforced endpoint.** R6 is the only risk that lands silently, individually and unappealably, and the pin-to-large rate is the sole signal that precedes it. It is already on the product's front page as a failure metric; the discipline is that someone reads it every week.
