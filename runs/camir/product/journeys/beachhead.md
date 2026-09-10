# Journey — Beachhead · Marcus Bell · first session to habitual use

**What this is** — the core paying journey, from Marcus's first `camir init` through the four weeks that decide whether CAMIR becomes infrastructure or a folder he deleted: first frontier, the negotiation with a product team he does not manage, the first enforced endpoint, the first upgrade that moves the curve, and the budget review where Dana signs.
**Why it exists** — the beachhead deal is not won at the frontier. It is won or lost in week two, in a conversation between Marcus and an engineer in a **different reporting line** who bears the quality risk and gets none of the saving. A journey that stops at "the curve looked good" describes the demo, not the deal, and produces a product with no answer for the person who can stop it. This narrative exists to force the organisational failure into the product spec.
**How to read it** — Week 2 is the load-bearing act; everything before it is Priya's journey with more tiers. A skeptic should attack §Week 2's claim that per-endpoint tolerance ownership is sufficient to stop an escalation, and §Week 4's counterfactual, which is the number Dana signs against.
**Depends on / feeds** — depends on [../../strategy/personas.md](../../strategy/personas.md) P2/P4/P5, [../PRD.md](../PRD.md) §5, [../features_flagship.md](../features_flagship.md), [../../strategy/sales_roadmap.md](../../strategy/sales_roadmap.md); feeds [day_in_life.md](day_in_life.md), [../ux_spec.md](../ux_spec.md), [../../financials/unit_economics.md](../../financials/unit_economics.md), [../../validation/decision_making_unit.md](../../validation/decision_making_unit.md) and [../../narrative/vc_memo.md](../../narrative/vc_memo.md).

---

## The profile

**Marcus Bell.** Staff platform engineer, ~400-person company. Owns the shared inference service six product teams call through one internal API. Self-hosted pool: 8B, 32B, 70B on a modest fleet. **~$50k/month**, the second-largest line in his budget [S25], and he is asked about it monthly.

**The trigger.** Not curiosity. A directive: *cut infrastructure cost 20% without degrading anything.* A number with no method attached.

**The people who decide.** Marcus evaluates. **Dana Okonkwo** (Head of Platform, two levels up) signs. **Ravi Menon** (senior engineer, product org, different reporting line) can stop it unilaterally by escalating a quality regression, and gains nothing if it works. See [../../validation/decision_making_unit.md](../../validation/decision_making_unit.md).

**What he is defending against.** *"Every vendor tells me they saved me money. They're also the ones doing the math."* OpenRouter takes ~5% of the bill it sits on [S17]; a frontier vendor is paid more when it routes up [S20]. This objection is not answered by a claim. It is answered by the counterfactual being computed by open-source code running on his hardware from records he holds (G3, O6).

---

## Week 0, Monday — the finance report lands

The cost-allocation report says inference grew 40% on flat headcount. Marcus's honest position: he has one A/B test from March concluding "the mid model seems fine for the FAQ path," never re-run, two model upgrades ago. He does not know what his small tier can do on his traffic. **Nobody at his company does, and no published benchmark can tell him** — RouterBench prices against hosted API list prices [S7], and no routing savings have ever been published for a self-hosted open-weight pool [G2].

He clones the repo before he talks to anyone, because the harness being open is what makes that possible without a procurement conversation. Sam Ortega's post is how he found it [S30] — the OSS adopter is the channel, not a failed lead.

---

## Week 1 — the frontier, on his traffic

**Day 1 · the replay corpus builder** draws 8,000 requests stratified across six endpoints and four length bands, from 30 days of logs. He raises the sample above the default because his traffic is genuinely heterogeneous — the support endpoint ranges from *"what's the refund policy"* to multi-step account reasoning through one API, and a corpus that under-samples the hard tail produces a ceiling that flatters the small tier. **Written:** corpus hash, pinned.

**Day 1 · the pool manifest** records three tiers with weights, hardware, resident VRAM and — the parameter he argues with himself about for ten minutes — the utilisation assumption. His cards run at 41% measured. He also sets the all-in multiplier to **4×** raw rental after reading why it is there [S27]. Marcus is exactly the customer who would have caught CAMIR quoting a saving against raw GPU cost, and the parameter being declared rather than hidden is why he keeps reading.

**Day 2 · the oracle ceiling probe** runs 8,000 requests × 3 tiers = 24,000 generations overnight on his fleet. **The artifact guard** runs the ceiling twice, guarded and unguarded. **The judge harness** scores under the forced protocol — temperature 0, fixed answer position, verbosity control, **exact-match verification wherever the task admits it** (his structured-extraction endpoint does; the summarisation one does not), and two judges with agreement published [S33][S34].

**Day 3 · the first `frontier_run`.**

```
Endpoint                 Oracle ceiling    Artifact share    Baseline cost/1k req
support-answering              61%             14pp                $0.94
structured-extraction          88%             31pp   ←            $0.51
account-reasoning              22%              6pp                $2.10
doc-summarisation              57%             11pp                $1.32
classification                 91%              4pp                $0.22
internal-tools                 44%              9pp                $0.77
Inter-judge agreement: 0.79 (field baseline ~0.76 [S33])
```

Three findings land in the same minute, and only one of them is the saving.

1. **`account-reasoning` is disqualified.** A 22% ceiling on his most expensive endpoint means routing cannot help there, and the **disqualification report** says so with the histogram. CAMIR just told him not to route his biggest line item. He did not expect that, and it is the moment the tool stops reading as a vendor.
2. **`structured-extraction`'s ceiling is 31 points harness.** Nearly a third of "the 8B can't do this" was truncation under a fixed generation budget and strict-parse failure [S5] — recovered by a generation-budget change he could have made himself for free, and would never have found, because nobody measures the thing they believe is a capability limit.
3. **`classification` at 91% has been running on the 70B for eleven months.**

**The order matters.** The disqualification lands first. Everything Marcus believes about the saving afterwards is credible *because* the tool opened by removing his largest endpoint from scope.

---

## Week 2 — the conversation that decides the deal

`structured-extraction` belongs to Ravi's team. Marcus owns the service; **Ravi owns the feature's quality metric and does not report to Marcus.** In the world without CAMIR this conversation goes: *"we're turning on routing"* / *"you're not touching my endpoint"* / done. That is the GPT-5 routing backlash in miniature — mandatory routing, vendor-set tolerance, complaints of degradation on complex queries, no dial and no attribution [S21].

**What Marcus brings instead is four product features, and they are P0 for exactly this reason** (PRD G2):

| What Ravi gets | Component | Why it changes his answer |
|---|---|---|
| **He sets the tolerance, not Marcus** | tolerance policy engine — `tolerance_policy.owner` is a required, non-null field naming *him* | The asymmetry is structural: the saving lands in Dana's budget, the risk lands on Ravi. Moving the tolerance to the risk-bearer is the only fix that is not a promise |
| **Tier decision on his own traces** | trace stamper — span attributes on *his* OpenTelemetry spans | When his thumbs-down rate moves, he answers "was it the router?" from his own telemetry in minutes, without asking Marcus and without CAMIR being up |
| **Shadow mode first** | dispatcher in shadow — decisions computed and logged, all traffic still to the 70B | He gets two weeks of evidence on his own endpoint before a single user request is routed |
| **Unilateral pin-to-large** | pin registry — one flag, effective next request, no ticket | He can end it himself at 02:00. Not escalate. End it |

Ravi sets his tolerance at **0.5%**, tighter than the platform default, and that is the point: he *can*, and it is recorded against his name and the `frontier_run` it was set on. He does not agree to enforcement. He agrees to shadow, which is a much smaller thing to agree to — and is why the deal moves.

> **The honest gap.** Four features make Ravi's veto expensive to exercise rather than free. They do not make it impossible, and nothing here has been tested on a real Ravi — no discovery calls have been run. This is the single most consequential untested assumption in the journey; see [../../validation/riskiest_assumptions.md](../../validation/riskiest_assumptions.md).

---

## Week 3 — shadow, then enforcement on two endpoints

Fourteen days of shadow on `structured-extraction` and `classification`. Per request: tolerance read, pin checked, **cascade route** taken — small tier, **confidence gate**, resolve or escalate — `decision_record` written with tiers attempted, tokens, GPU-seconds per tier, escalation flag and policy version. The **classifier route** runs alongside as an ablation and is reported **against calibrated confidence, never against the fixed-model baseline** [S8], because beating a fixed model is table stakes and beating calibrated confidence is the only result that means anything (PR3).

Shadow output at day 14, on `structured-extraction`:

```
Escalation rate        23%
Cost decomposition     failed small attempt 19%  ·  gate 2%  ·  large answer 79%
Measured quality delta -0.3%  (Ravi's tolerance: 0.5%)
Classifier ablation    +0.4pp over calibrated confidence — within noise. Cascade stays primary
```

The **escalation rate is the reported metric, not a debug counter**, because cascade economics are set by how often stage one resolves, not by classifier accuracy (PR1). At 23% the cascade wins; at 70% it would have been more expensive than going straight to the 70B, and the decomposition is shown *before* enforcement so Marcus can see that himself.

**Day 21 — Ravi flips enforcement on his own endpoint.** Marcus does not do it. The **tolerance breach alert** is armed at 0.5% with auto-revert; a quality drop with no alert is a P0 defect class, not a missed feature (O4).

---

## Week 4 — the upgrade, and the review

**Day 26 — the pool changes.** Marcus swaps the 8B for a newer small model. The **drift monitor** flags the `pool_manifest` change; the **recalibration scheduler** re-runs the pinned corpus and produces a new dated `frontier_run`. The **frontier diff** shows the old curve, the new curve, and **Ravi's chosen point projected onto both** — so the question "did my tolerance point move?" has a picture, and the answer is that his point now sits at 19% escalation instead of 23%. He does nothing. That is the correct outcome and it required the diff to exist.

This beat is why CAMIR is a subscription rather than a consulting engagement: the frontier is not a fact, it is a dated measurement that decays every time the pool or the traffic moves [S29].

**Day 28 — Dana's budget review.** She receives one page from the **savings report**: baseline cost, cost at declared tolerance, measured quality delta, inter-judge agreement, and the *who checked* field — which names Ravi, not Marcus and not CAMIR. Her question has always been *"what did it cost us before, what does it cost now, and who checked that nothing got worse — I need the third one in writing."* The third one is a schema constraint (O3), which is why it is in writing.

The number on the page is **17% of the pool's monthly cost**, not 85%. Two of six endpoints are enforced, one is disqualified, `account-reasoning` still runs entirely on the 70B and always will. `(assumption: illustrative; no CAMIR measurement exists — [G2] records that no routing savings have been published for a self-hosted open-weight pool, and RouteLLM's headline 3.66× is MT-Bench, collapsing to 1.41× on MMLU [S2])`

**Dana signs a $30,000/yr control-plane contract** [../../financials/pricing.md](../../financials/pricing.md), against a measured saving on her own traffic, computed by her own engineer, using open code she could hand to an auditor. The open half stays free forever and no feature ever moves open → paid [S23].

---

## What habitual use looks like — month 3

Marcus opens CAMIR **twice a month**, which is the goal, not a disappointment.

- The **savings ledger** feeds his monthly cost-allocation answer without him assembling it.
- The **drift monitor** pages him only on escalation-rate anomaly — which is also the detector for cascade deferral attacks, where semantics-preserving perturbations suppress small-tier confidence to inflate the bill [S9] (O5).
- The **label recycler** has promoted ~2.1M production `judgment_record` rows into the gate's calibration set and the classifier's training set. This compounds **per-deployment, never network-wide** — CAMIR cannot pool labels across customers without moving prompts out of the perimeter, which N4 forbids. `../../ASSUMPTIONS.md` A6 records that the resulting switching cost is untested, and this journey does not upgrade it.
- Two more endpoints have moved from shadow to enforcement, each by their own owner. **The `pin-to-large` rate is 0.4% and visible on the front page — rising is a product failure and must be legible as one** (M11).

**The thing that never happens:** Marcus never has to defend CAMIR's arithmetic, because he is not the one doing it and neither is CAMIR. He re-runs it.

---

## Components that fired, in order

`replay corpus builder` → `pool manifest / tier registry` → `oracle ceiling probe` → `artifact guard` → `judge harness` → `frontier builder` → `disqualification report` → `non-nested tier report` → `tolerance policy engine` → `trace stamper` → `dispatcher` (shadow) → `confidence gate` → `classifier route` (ablation) → `difficulty classifier` → `escalation-rate accounting` → `cost meter` → `savings attributor` → `pin registry` → `tolerance breach alert` → `drift monitor` → `recalibration scheduler` → `frontier diff` → `label recycler` → `savings report`.

**Every named component fires except `judge interface`** — Marcus uses the shipped judges. Wen replaces them; see [edge_high.md](edge_high.md).

---

## Recommended next 3

1. **Run the Week 2 conversation as a discovery script before building anything in Week 3.** The four Ravi features are P0 on the strength of one inference from [S21] and zero interviews. If a real Ravi says "a tolerance I own doesn't help, I still don't want variance on my endpoint," the beachhead journey has an organisational blocker no feature closes and the wedge is wrong. [../../validation/discovery_guide.md](../../validation/discovery_guide.md) carries the past-behaviour form of the question.
2. **Ship the disqualification report in the same release as the first frontier, not later.** In this journey it lands before the saving and is what makes the saving believable. A version of CAMIR that finds Marcus's ceiling and stays quiet about `account-reasoning` is indistinguishable from every vendor doing his math for him, and it forfeits the one claim no share-of-savings competitor can make.
3. **Instrument time-to-first-frontier and escalation rate as the two product health metrics from day one.** M7 decides whether Week 1 fits in a week, and M6 decides whether the cascade's economics hold — and PR1 says that variable, not classifier accuracy, is where a deployment silently turns into a more expensive way to serve the same traffic.
