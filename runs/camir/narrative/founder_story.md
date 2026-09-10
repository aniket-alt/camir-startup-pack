# CAMIR — Founder story

**What this is** — a first-person account of why this team chose benchmark engineering and customer-owned measurement as CAMIR's starting point.
**Why it exists** — a pre-traction capstone can borrow the voice of a company before it has earned evidence; this story keeps the origin, edge and uncertainty visible so the reader does not confuse technical fluency with traction.
**How to read it** — read the lived insight, then the claimed edge and its test; attack A13, the transfer from test automation to LLM evaluation infrastructure.
**Depends on / feeds** — depends on [BRIEF.md](../BRIEF.md), [ASSUMPTIONS.md](../ASSUMPTIONS.md), [research/sources.md](../research/sources.md), [tech/whitepaper.md](../tech/whitepaper.md) and [validation/riskiest_assumptions.md](../validation/riskiest_assumptions.md); feeds [one_pager.md](one_pager.md), [vc_memo.md](vc_memo.md), [mission_vision.md](mission_vision.md) and [README.md](../README.md).

## The insight

We started with a practical irritation: a platform team can run several open-weight models, know that requests vary in difficulty, and still have no defensible answer to the question, “What did this request cost, and what quality did we give up?” The default response is to pick one tier for an endpoint. The alternative is often a routing demo that reports a percentage without exposing the harness, utilization or the engineer who carries the risk.

The research made the tempting version of the story impossible. The Routing Plateau found 21 routing methods converging in a narrow band far below the oracle [S4]. The right claim was not that our router would be smarter. It was that the customer's frontier should be measurable on the customer's pool, and that some apparent unsolvability may be evaluation artifact [S5].

## Why this team is attempting it

This is a pre-traction SJSU CMPE 295A capstone, not a story about prior customers or famous credentials. Our claimed edge is narrower: test-automation and evaluation-infrastructure background may transfer to benchmark engineering, where truncation, parse failure, judge disagreement and reproducibility are first-order engineering problems. That transfer is assumption A13. It becomes credible only if E1 and E2 reproduce a useful artifact decomposition on a self-hosted pool.

The work is attractive because a negative result is valuable. If the oracle ceiling is too low, CAMIR should issue a disqualification report. If the cost axis collapses under idle GPU utilization, the model must say so. If a consuming engineer pins large, the record should show that the safety control worked and why the route was rejected.

## What I would refuse to claim

I would not claim router superiority. I would not call a hosted benchmark a CAMIR measurement. I would not call an illustrative $30,000 ACV a contract. I would not invent traction, logos, advisors, testimonials or credentials. I would keep the core loop visible: **Classify → Dispatch → Judge → Attribute → Recalibrate**.

## The work ahead

The first proof is not a polished dashboard. It is an artifact-controlled oracle ceiling, a pool-specific GPU-hour cost derivation and a shadow-mode report that Ravi can inspect. The first commercial proof is not enthusiasm; it is a buyer who accepts the eligible-savings definition after independently reproducing the counterfactual.

That is why CAMIR starts as an open harness and a measured frontier. The router may become a serving-engine feature. The measurement record still has to be true.

## Recommended next 3

1. Run E1/E2 and publish the result, including a null result if the ceiling does not support routing.
2. Ask the capstone advisor and practitioner network for past-behavior interviews about self-hosted inference and budget ownership.
3. Test whether benchmark-engineering discipline transfers into repeatable customer-perimeter evaluation.
