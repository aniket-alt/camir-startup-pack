# CAMIR — Founder story

**What this is** — the first-person founder-market-fit account: the insight, the edge, the weakness stated plainly, and why this team is positioned for the next ten years of this problem. Written to be usable verbatim in applications and introductions.
**Why it exists** — a pre-traction capstone team writing about an LLM router has two tempting stories, and both fail on contact: "we built a smarter router" is falsified by one citation [S4], and "we have deep domain experience" is not true. Without an honest version, the pitch borrows a credibility the team has not earned and loses the one it has — that the hard part of CAMIR is evaluation, and evaluation is what this team has actually done before.
**How to read it** — the second section is the claim; the third is where a skeptic should push, because it states the weakness first. The edge rests on assumption A13, and E2 is the experiment that tests it.
**Depends on / feeds** — depends on [../BRIEF.md](../BRIEF.md) §Founder edge and §Wedge, [../ASSUMPTIONS.md](../ASSUMPTIONS.md) A13, [../tech/whitepaper.md](../tech/whitepaper.md), [../validation/experiment_board.md](../validation/experiment_board.md) E2; feeds [one_pager.md](one_pager.md), [vc_memo.md](vc_memo.md), [pitch_deck.md](pitch_deck.md) slide 13 and [mission_vision.md](mission_vision.md).

---

## What we noticed

Anyone who has maintained a large automated test suite learns one lesson early and relearns it constantly: **a broken harness produces failures that look exactly like product bugs.** A timeout set too tight, a parser that expects one output format, an assertion that rewards the wrong thing — each one shows up in the dashboard as "the system cannot do this," and each one is fixed in the harness, not the system.

When we started reading the LLM routing literature, we found the same failure at scale. The 2026 audit of routing evaluation found truncation under fixed generation budgets in 65% of MMLU and 57% of MedQA cases, 5–12% parse failures on MMLU, and judges that reward verbosity over correctness, across 206,000 query-model pairs [S5]. Much of what a team would measure as "the small model cannot handle this request" is the harness.

At almost the same time, the field's own benchmark of 21 routing methods found them converged in a narrow band far below the oracle, with the best remedies worth up to 2.13 percentage points [S4]. The routing *decision* is near its ceiling. The routing *measurement* is not. **That asymmetry is the company.** It is why we do not claim a better router, and why CAMIR's first output is a measured ceiling — including, sometimes, the recommendation not to deploy.

## Why us

We are Abhishek Darji, Aniket Anil Naik and Tamizh Selvan Manivannan, building CAMIR as an SJSU CMPE 295A capstone advised by Prof. Vijay Eranti.

Our edge is narrow and specific. The team brings two years of professional test automation and validation work: data-driven test harnesses running more than 1,000 test cases, CI pipelines, and systematic evaluation infrastructure. The hard part of CAMIR is not the router. It is building a mixed-difficulty benchmark, judging correctness consistently enough that two judges agree, and producing a cost-quality frontier that survives someone hostile re-running it. **That is benchmark engineering, and it is the part of this problem we have done before.**

## What we are not

We came to LLM routing recently. We have no commercial work in this space, no prior product here, no customers, no revenue and no measurement of our own. Whether our background transfers is assumption A13 in our own register, and we treat it as one: if E2 — the same generations run through a naive and an artifact-controlled harness — shows less than a two-point difference on a realistic pool, then our thesis is true only of sloppy harnesses and we are an ordinary router team in a plateaued field. We have written that down in advance so we cannot reinterpret it afterwards.

We will not claim router superiority. We will not quote a hosted benchmark as a CAMIR result. We will not call the $30,000 ACV a contract. And we will not invent traction, logos, advisors or testimonials — there are none.

## Why this team for the next ten years

The founding brief puts the ten-year view in one line: **routing becomes a default layer in the inference stack — nobody sends every request to one model, the same way nobody serves every static asset from origin.**

If that happens, the router itself is free; the serving engines are already absorbing it [S11]. What does not become free is knowing whether a routing decision was right on *your* traffic, and knowing again after every model upgrade, every cache change, every quantisation choice. The frontier is a dated measurement that decays each time the pool moves [S29]. In test engineering this is simply the regression suite — the thing that runs on every change, that nobody wants to own, and that silently rots when nobody does. **The inference stack is about to need a regression suite for cost and quality, and building regression suites is our trade.**

That is also why a negative result does not frighten us. If the oracle ceiling on real self-hosted traffic is too low, the right output is a published null result and a disqualification report. A team that already knows how to be told its tests were wrong is well suited to a company whose product sometimes tells the customer to walk away.

## Recommended next 3

1. **Run E1 and E2 before telling this story to anyone who can fund it.** The story's central claim is testable in two weeks on a public corpus; telling it with the result attached is a different conversation from telling it as a plan.
2. **Pre-register the E2 protocol publicly** — both harness arms, the judge set, and the two-point fail line. It is the single artifact that turns "we are good at evaluation" from an assertion into something a stranger can check.
3. **Use this story unchanged in the capstone report and in investor introductions.** One version of the founder narrative, with the weakness in it, is worth more than two versions tuned to different audiences.
