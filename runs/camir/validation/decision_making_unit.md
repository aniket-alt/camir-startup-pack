# CAMIR — The decision-making unit: who signs, who consents, and who can say no

**What this is** — the buying unit mapped by role rather than by benefit: user, payer, champion, saboteur, technical gatekeeper and the recommender who never buys. Per role — what they want, what they fear, what evidence moves them, what kills the deal, and whether they can stop it alone.
**Why it exists** — [../strategy/personas.md](../strategy/personas.md) covers who benefits from CAMIR. Nobody in that document loses anything, which is exactly why a persona set is not a buying map: **the person most able to stop a CAMIR deployment gains nothing from it, is not in the sales conversation, does not report to the buyer, and never appears in a CRM.** A pitch built from personas alone is optimised for the two people who want to say yes and silent on the one who can say no without a meeting. This document exists to make that asymmetry a design input rather than a post-mortem.
**How to read it** — §2 (the saboteur) is the document. §4's kill order is the operational summary. A skeptic should attack the central claim that four product features convert a free veto into an expensive one — it rests on one public incident [S21] and zero interviews.
**Depends on / feeds** — depends on [../strategy/personas.md](../strategy/personas.md), [../strategy/sales_roadmap.md](../strategy/sales_roadmap.md), [../product/journeys/beachhead.md](../product/journeys/beachhead.md), [discovery_guide.md](discovery_guide.md); feeds [../strategy/gtm.md](../strategy/gtm.md), [stage_gate.md](stage_gate.md), [../financials/risk_matrix.md](../financials/risk_matrix.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

**Status: constructed, not interviewed.** Every role below is inferred from the research layer and from the founder's stated view (A8). [discovery_guide.md](discovery_guide.md) §8 carries the thresholds that would confirm or break it.

---

## 1. The unit at a glance

| Role | Who | Reports into | Gains | Loses | Can stop it alone? |
|---|---|---|---|---|---|
| **Champion / user** | **Marcus Bell**, staff platform engineer | Dana's org | Meets a cost directive with a method | Credibility, if it degrades something | No — but without him nothing starts |
| **Payer / economic buyer** | **Dana Okonkwo**, Head of Platform | — | The saving; a defensible line in a budget review | Reputation on an unsigned claim | Yes, by not signing |
| **Saboteur / risk-bearer** | **Ravi Menon**, product engineer | **A different VP** | **Nothing** | His feature's quality metric | **Yes, unilaterally and without a meeting** |
| **Technical gatekeeper** | Security / platform review | CISO or Dana | Nothing | An exposure they approved | Yes, by refusing the perimeter |
| **Recommender** | **Wen Xu**, ML infra lead elsewhere | — | A methodology she can audit | Nothing | No — but her endorsement halves the evaluation |
| **Non-participant** | **Sam Ortega**, OSS adopter | — | A chart and a post | Nothing | No — he is the channel, not a lead |

**The structural fact.** The saving lands in Dana's budget. The risk lands on Ravi. They are in different reporting lines and have never met. **Every product decision in [../product/PRD.md](../product/PRD.md) G2 exists because of that sentence.**

---

## 2. The saboteur — the role most packs omit

**Ravi is not hostile.** He is rational. He was not consulted, gains nothing, bears the entire downside of a decision made in someone else's budget, and has a cheap, legitimate, unappealable way out: insist his endpoint stays on the large model. That is not sabotage; it is the correct response to an asymmetric risk, and calling it sabotage is how vendors lose to it.

**What he fears, precisely:** a quality regression he finds out about from a support ticket, cannot attribute within the hour, and cannot stop himself. All three clauses matter. Remove any one and his answer changes.

**What CAMIR does about it — the four P0 features, ranked above classifier accuracy:**

| His fear | The product answer | Why a promise would not do |
|---|---|---|
| *"Someone else set the acceptable quality drop on my feature"* | `tolerance_policy.owner` is a **non-null column** naming him; enforcement is impossible without it (O3) | A policy document is revocable by a busy platform team; a schema constraint is not |
| *"I can't tell whether it was the router"* | Tier decision stamped on **his own** OpenTelemetry spans — his tool, his query, survives CAMIR's removal (O2) | A CAMIR dashboard is a surface he will never open under time pressure |
| *"I'm being asked to accept this on faith"* | Shadow mode on by default: two weeks of evidence on his endpoint before one user request is routed | He is agreeing to *shadow*, which is a much smaller thing to agree to — and that is why the deal moves |
| *"If it goes wrong at 2 a.m. I have to find Marcus"* | Pin-to-large, one flag, effective next request, no ticket, no approval | An off switch that requires someone else is an escalation, not a switch |

**What these four achieve, stated honestly:** they make the veto **expensive to exercise rather than free**. They do not make it impossible, and nothing here has been tested on a real Ravi. That is the single most consequential untested claim in this pack after A1–A3, and [experiment_board.md](experiment_board.md) E13 is the experiment.

**The counter-evidence that exists.** [S21] documents the public GPT-5 routing backlash: mandatory routing, vendor-set tolerance, complaints of complex queries degraded by being sent to a smaller model, no dial and no attribution. That is the failure state, publicly, at a company with far more resources than CAMIR — which is why this is a design input and not a support policy.

---

## 3. The other five, on one page each line

**Dana — the payer.** Wants a defensible line in a budget review; fears signing a number that unravels. Moved by: a before-and-after on **her own** traffic, produced by her own engineer, with a named human in the *who checked* field. Killed by: a savings number CAMIR computed. *"If it's open source, why am I paying?"* has to be answerable in one sentence — per-deployment fitting, policy management, attribution she can defend, someone to call when the frontier moves — or the open-core model has the TensorZero shape [S23]. **The line she rereads is "1 endpoint disqualified by CAMIR in week 1."**

**Marcus — the champion.** Wants a method attached to a directive that arrived with only a number. Fears being the person who broke a product team's feature. Moved by: a harness that runs inside his perimeter and that he can re-run. Killed by: being asked to trust the vendor's arithmetic — OpenRouter takes ~5% of the bill it sits on [S17], a frontier vendor is paid more when it routes up [S20]. **He is not the buyer and cannot be sold to as one**; his output is an evaluation Dana signs.

**The technical gatekeeper.** Wants the perimeter question answered before it is asked. Fears approving an exposure. Moved by [../tech/architecture/D06.md](../tech/architecture/D06.md): two arrows leave the perimeter, both metadata, telemetry off by default, prompt text nullable and default-off. Killed by: any control plane that reads prompts — which disqualifies P1 outright, since she self-hosts for residency rather than price [S28]. **This role is the reason multi-tenant SaaS is non-goal N4** and not a later phase.

**Wen — the recommender.** Wants a methodology she can audit; fears staking her credibility on someone else's benchmark [S13]. Moved by: published inter-judge agreement and a cost axis derived for self-hosted pools [S7]. Killed by: a configurable judge temperature, a closed savings computation, or any claim of router superiority — she has read [S4]. **She may never buy, and adopting her as a reference is still the highest-leverage thing in the funnel**: her conference sentence shortens Marcus's evaluation by two weeks.

**Sam — the non-participant.** Below the volume floor [S27] and will never pay. Open-core converts 1–5% of active users to hosted SaaS [S40]; **Sam is the 95–99% and treating him as a lead is how open-core projects lose the community that is their distribution.** He is how Marcus finds CAMIR.

**The absent role: finance.** A8 assumes finance is never in the room. If it is, the cycle lengthens, the self-serve motion in [../strategy/gtm.md](../strategy/gtm.md) is wrong, and share-of-savings pricing becomes a procurement negotiation about *eligible savings* rather than a rate [S38][S39]. Untested.

---

## 4. The kill order — how the deal actually dies

Ranked by likelihood, from the sequence in [../product/journeys/beachhead.md](../product/journeys/beachhead.md).

| # | Who kills it | When | The tell | Countermeasure |
|---|---|---|---|---|
| 1 | **Ravi** | Month 4, on an ordinary Tuesday, without a meeting | Pin-to-large rate rising — **which is why it is on the front page as a failure metric** (M11) | The four P0 features; the fast attribution path in [../tech/architecture/D08.md](../tech/architecture/D08.md) |
| 2 | **CAMIR itself** | Week 1 | Ceiling-to-baseline gap below operating cost | None — the disqualification report is the correct outcome, and M16 requires this rate to be non-zero |
| 3 | **The gatekeeper** | Week 2, security review | A question about where prompts go | D06, unprompted, in the first review |
| 4 | **Dana** | Week 4 | *"Why am I paying for open source?"* answered vaguely | A crisp open/paid line, committed publicly before release [S23] |
| 5 | **Marcus's own build instinct** — Petal 5 | Any time | *"I could do this in three weeks"* | True. The answer is maintenance capacity, not capability ([../product/journeys/edge_high.md](../product/journeys/edge_high.md) §Act IV) |
| 6 | Procurement / finance | Month 2+ | An unbudgeted line item | Price out of the inference bill or not at all ([../financials/pricing.md](../financials/pricing.md)) |

**Rows 1 and 5 are the two real ones.** Row 1 is an organisational failure a product feature can address; row 5 is a competitor with no sales team, no price and full access to the account.

---

## 5. What each role must see, in order

1. **Wen or Sam** publishes something. Marcus reads it. *(No sales contact has occurred.)*
2. **Marcus** runs the open harness on his own logs. Gets a ceiling, an artifact decomposition, and possibly a disqualification.
3. **The gatekeeper** sees D06 before being asked for anything.
4. **Ravi** is offered shadow mode and a tolerance with his name on it. **He is not asked to approve enforcement.**
5. **Ravi** enables enforcement himself, on his own endpoint, after his own shadow report.
6. **Dana** receives one page with a named signer who is not Marcus and not CAMIR.

**The ordering is the strategy.** Dana is last and Ravi is before her. Every conventional enterprise motion inverts this — sell to the budget holder, roll out downward — and that inversion is exactly what produced [S21].

---

## Recommended next 3

1. **Recruit a real Ravi through a real Marcus and run E13 before building the control plane UI.** The four P0 features are the pack's largest non-technical bet, they rest on one public incident, and one conversation with a consuming engineer in a different reporting line is worth more than any amount of design.
2. **Write Dana's one-sentence answer to "why am I paying for open source" and test it verbatim in five calls.** It is the question that decides whether open-core is a distribution strategy or the TensorZero failure shape [S23], and it must survive being said out loud rather than read.
3. **Put the gatekeeper's diagram into the first conversation, unprompted.** The perimeter answer gates whether the beachhead can adopt CAMIR at all [S28]; discovering a flaw in it during a design partner's security review costs the partner, not just the review.
