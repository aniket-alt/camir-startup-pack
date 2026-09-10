# CAMIR — MVP definition: the problem MVP, the solution MVP, and the earlyvangelist test

**What this is** — two deliberately different minimum products kept apart: a low-fidelity artifact that tests whether the *problem* is real, and a high-fidelity one that tests whether the *solution* is bought. Each states what it does, what it omits, the question it answers, and the result that would falsify it. Plus the five things a person must already have done to count as an earlyvangelist.
**Why it exists** — CAMIR's obvious MVP is "a router that works", and that is the wrong first build for a specific reason: [S4] shows 21 routing methods converged in a narrow band far below the oracle, with remedies worth up to 2.13 percentage points. **A working router is both the hardest thing to differentiate and the least informative thing to ship.** The low-fidelity MVP here is therefore not a prototype of the product — it is a measurement anyone can run, and it can return a result that ends the company cheaply. A pack that merges the two MVPs spends a year building the routing half before learning whether the ceiling exists.
**How to read it** — §1 and §2 are two separate builds with two separate kill criteria; the ordering between them is the whole argument. A skeptic should attack §1's claim that a measurement tool with no routing in it is a product people will run.
**Depends on / feeds** — depends on [riskiest_assumptions.md](riskiest_assumptions.md), [experiment_board.md](experiment_board.md) E1/E2/E7/E8, [../product/features_prioritized.md](../product/features_prioritized.md), [../tech/not_vaporware.md](../tech/not_vaporware.md); feeds [stage_gate.md](stage_gate.md), [get_keep_grow.md](get_keep_grow.md), [../financials/use_of_funds.md](../financials/use_of_funds.md) and [../narrative/pitch_deck.md](../narrative/pitch_deck.md).

**Status: neither MVP is built.** Both are specifications.

---

## 1. The low-fidelity MVP — "the ceiling probe", and it is not a router

**What it is.** A command-line tool a stranger installs, points at their own request logs, and runs on their own GPUs. It draws a stratified corpus, runs every request through every tier, judges the answers under the forced protocol, and prints an oracle ceiling — **twice**, with and without the artifact guard — plus a cost axis derived for their pool.

**It routes nothing.** No proxy, no gate, no classifier, no request path, no production traffic touched.

**What it deliberately omits**

| Omitted | Why |
|---|---|
| The router entirely | Routing is what [S4] says is near its ceiling. Measuring is what nobody has done on a self-hosted pool [G2] |
| Any control plane, account or network call | An account is a conversion step in front of the only thing that produces the finding |
| Tolerance policies, ownership, pins | Nothing is being enforced, so nobody's risk is being spent |
| A UI | A terminal and a printed table |
| A saving | It reports a **ceiling**, not a saving. The saving requires a tolerance somebody chose |

**The question it answers.** *Is there enough headroom on real self-hosted traffic for routing to matter — and how much of the apparent ceiling is instrumentation?* That is A2 and the founder's own walk-away condition, plus the M3 contribution.

**What would falsify it.** Median oracle ceiling across the first ten qualified deployments **below the threshold at which routing pays for its own operating cost** — the disqualification condition. If most real traffic is bimodal such that nearly everything needs the large tier, no router saves enough to sell, and CAMIR is a null result that cost one quarter rather than one year.

**The second falsification, which is the one to watch.** If the guarded-minus-unguarded artifact share comes back **near zero across well-instrumented pools**, the technical thesis in [../tech/whitepaper.md](../tech/whitepaper.md) is true only of sloppy harnesses. Note the expected shape: the share should *shrink* as harness quality rises — 19pp for Priya, 14 for Marcus, 6 for Wen ([../product/journeys/](../product/journeys/)). A share that did not shrink would be more suspicious than one that did.

**Why anyone runs it.** It is free, open, runs entirely inside their perimeter, needs no account, and answers a question they have never been able to answer — [discovery_guide.md](discovery_guide.md) Q6 predicts **≥12 of 15** qualified teams have never measured what their small tier can do. It also produces a chart, which is what makes P6 Sam post about it [S30].

**Cost to build:** the left column of [../tech/not_vaporware.md](../tech/not_vaporware.md) §4, minus the routing rows. Every component is 2026 open-source engineering with no unpublished result required.

---

## 2. The high-fidelity MVP — "shadow to first enforcement"

**What it is.** The ceiling probe plus: the OpenAI-compatible ingress proxy, the cascade route with a conformal-thresholded confidence gate, per-endpoint tolerance policies with a named owner, shadow mode on by default, pin-to-large, the trace stamper, the cost meter and the counterfactual savings ledger.

**Its target is not a saving. Its target is a signature** — a named consuming engineer, in a different reporting line from the buyer, enabling enforcement on his own endpoint after reading his own shadow report.

**What it deliberately omits**

| Omitted | Why |
|---|---|
| The difficulty classifier | The ablation arm. It is expected to be null against calibrated confidence [S8] and it is not what the MVP tests |
| Multi-tier beyond three | Wen's five-tier case is edge-high; the beachhead has three |
| Drift monitor, recalibration, frontier diff | Month-4 features. The MVP has to survive week 4 first |
| The signed savings report | Dana's artifact. She is last in the order ([decision_making_unit.md](decision_making_unit.md) §5) |
| Any hosted component | N4 |

**The question it answers.** *Does per-endpoint tolerance ownership convert a free veto into an accepted risk?* Not *does routing save money* — the ceiling probe already bounded that.

**What would falsify it.** Across five design-partner deployments: **fewer than two consuming engineers enable enforcement on their own endpoint within six weeks of a shadow report.** If risk-bearers will not accept a measured, owned, revocable tolerance on their own feature, then no product feature closes the veto, the four P0 features are wrong, and the organisational thesis in [decision_making_unit.md](decision_making_unit.md) needs replacing — not the router.

**The second falsification.** Escalation rate above the break-even line ([../tech/architecture/D05.md](../tech/architecture/D05.md)) on the majority of endpoints — the cascade costing more than the baseline, which PR1 says is set by first-stage resolution and which no amount of classifier work fixes.

---

## 3. Why in this order, and what merging them would cost

| | Low-fidelity first | If they were merged |
|---|---|---|
| Time to a kill signal | One quarter | One year |
| Risk taken from a customer | **None** — no production traffic touched | Production quality, before the ceiling is known |
| Security review needed | Minimal | Full, before any evidence exists |
| What a null result costs | A quarter, and a publishable negative finding | The company |
| Distribution | An open tool a stranger runs and posts about | A product requiring a sales conversation |

**The low-fidelity MVP is also the wedge, the disqualification path, and the public artifact that makes the technical claim checkable by strangers.** It is the same build serving four purposes, which is why [../tech/architecture/D07.md](../tech/architecture/D07.md) recommends shipping it as *the product*, not the trial.

---

## 4. The earlyvangelist definition — five things already done

A person counts as an earlyvangelist for CAMIR only if **all five** are already true. Not aspirations: past actions.

1. **They have already put weights on hardware they control**, at production volume — not a laptop, not an evaluation. The floor is real: self-hosting begins to pay around 16M+ tokens/day on H100 [S27].
2. **They have already been asked, by a named person, to reduce inference cost** — a directive with a number, and no method attached ([discovery_guide.md](discovery_guide.md) Q1).
3. **They have already improvised a workaround** — one model chosen once, endpoint-level assignment, a hand-written length rule, or a one-off A/B nobody re-ran. **They have a problem badly enough to have solved it badly.**
4. **They have already tried and failed to measure it** — or know they have never been able to. Q6's "no" is the qualifying answer.
5. **They have already bought infrastructure tooling before**, and can name who signed and what evidence that person required (Q11–Q12).

**What is not on the list, deliberately:** enthusiasm about routing, a belief that AI costs are too high, or an expressed intention to try CAMIR. All three are available from anyone and predict nothing.

**A sixth, for the beachhead specifically:** their shared inference service is called by **teams they do not manage**. Without that, there is no Ravi, no veto, and the deployment tests a much easier organisation than the one CAMIR is designed for.

---

## 5. What each MVP must instrument about itself

| MVP | Instrument | Because |
|---|---|---|
| Low-fidelity | Time from install to first frontier (M7) | G1 promises under a week; if the median is three weeks the wedge is a project, not an afternoon |
| Low-fidelity | Runs that terminate in a disqualification (M16) | Should be **non-zero**. A zero rate means the ceiling probe is not believed |
| Low-fidelity | Share of installs that reach a frontier at all | The drop-off point is the product's real onboarding problem, and it is probably log parsing |
| High-fidelity | Shadow-to-enforcement conversion, **by the endpoint's own owner** (M10) | The MVP's actual pass/fail |
| High-fidelity | Pin-to-large rate (M11) | Rising is a product failure and must be legible as one |
| High-fidelity | Escalation rate against break-even (M6) | The variable that decides whether the cascade saves anything (PR1) |

---

## Recommended next 3

1. **Build and publish the low-fidelity MVP with a reference `frontier_run` on public open-weight models.** It is the wedge, the experiment for A2, the disqualification mechanism and the public artifact simultaneously — and it is the only version of this company where a null result costs a quarter instead of a year.
2. **Do not build the classifier for the high-fidelity MVP.** It is the interesting engineering, it is expected to be null against calibrated confidence [S8], and every week spent on it is a week not spent on the four features that decide whether anyone is allowed to route at all.
3. **Screen every design partner against all six earlyvangelist criteria before the high-fidelity build, and record who fails which.** A partner missing criterion 6 — teams they do not manage — will validate a product for an organisation CAMIR was not designed for, and that false positive is more expensive than no partner at all.
