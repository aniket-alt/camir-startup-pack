# CAMIR — Petal Diagram

**What this is** — Blank's petal diagram, not a 2×2: CAMIR at the centre, with the five **adjacent markets** it draws customers from, the incumbents holding each one, and what those customers currently spend there.
**Why it exists** — [positioning.md](positioning.md) answers *where rivals sit*. This answers the harder and more useful question: **whose budget does CAMIR actually take money out of, and what habit does it displace?** For CAMIR the answer is unsettling and worth finding now rather than in a pricing conversation — four of the five petals hold budgets that are either zero, already spoken for by a free product, or belong to a *different person* than the one CAMIR sells to. A pack that skipped this would price against a budget that does not exist.
**How to read it** — the **Displaced budget** column is the point. Then read §The finding, which states what the five petals collectively say about where CAMIR's first dollar comes from.
**Depends on / feeds** — depends on [../research/competitors.md](../research/competitors.md), [../research/landscape.md](../research/landscape.md), [market_sizing.md](market_sizing.md), [personas.md](personas.md); feeds [../financials/pricing.md](../financials/pricing.md), [sales_roadmap.md](sales_roadmap.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

---

## The petals

```
                    ┌───────────────────────────────┐
                    │  PETAL 1                      │
                    │  Raw inference spend          │
                    │  (the GPU/API bill itself)    │
                    └───────────────┬───────────────┘
                                    │
  ┌──────────────────────┐          │          ┌──────────────────────┐
  │  PETAL 5             │          │          │  PETAL 2             │
  │  Internal engineering│          │          │  LLM gateway &       │
  │  time on bespoke     │          │          │  observability       │
  │  routers & evals     │          │          │  tooling             │
  └──────────┬───────────┘          │          └───────────┬──────────┘
             │                      │                      │
             └──────────────┬───────┴───────┬──────────────┘
                            │    CAMIR      │
                            │  self-hosted  │
                            │   routing +   │
                            │  measurement  │
             ┌──────────────┴───────┬───────┴──────────────┐
             │                      │                      │
  ┌──────────┴───────────┐          │          ┌───────────┴──────────┐
  │  PETAL 4             │          │          │  PETAL 3             │
  │  Cloud/AI cost       │          │          │  Inference           │
  │  optimisation        │          │          │  efficiency tooling  │
  │  (FinOps)            │          │          │  (caching, quant,    │
  │                      │          │          │   serving)           │
  └──────────────────────┘          │          └──────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │  Adjacent, not a petal:       │
                    │  hosted-catalog routing —     │
                    │  free [S20], different segment│
                    └───────────────────────────────┘
```

---

## Petal-by-petal

### Petal 1 — Raw inference spend (the GPU or API bill itself)

**Incumbents.** Nobody sells this as a product; it is the cost of the compute. The "incumbent" is the customer's own default policy — one model, chosen once.
**What those customers spend.** The beachhead profile: **~$600k/yr per company** ($50k/month), against a total self-hosted inference spend of roughly **$2.9B/yr** globally (base case, [market_sizing.md](market_sizing.md)). Inference is the second-largest line item in enterprise AI budgets in 2026 [S25].
**What CAMIR displaces.** A fraction of the bill — not a competitor's revenue. **This is the only petal with a large, real, already-approved budget**, and it is the one the share-of-savings pricing model reaches into.
**Why they would move.** The money is already being spent and nobody has to approve a new line item; the saving pays for the fee out of the same budget. Easiest sale in the diagram.
**Why they would not.** Nothing breaks if they do nothing. `../BRIEF.md` is explicit: the pain persists precisely because it fails silently.

### Petal 2 — LLM gateway and observability tooling

**Incumbents.** OpenRouter (**$160M annualised revenue, ~5% of inference spend**, reported Stripe acquisition >$7B) [S17]; Portkey; LiteLLM's enterprise tier [S22]; Langfuse, now inside ClickHouse after a January 2026 acquisition [S23].
**What those customers spend.** ~5% of inference spend where they use an aggregator [S17]; enterprise gateway/observability contracts otherwise.
**What CAMIR displaces.** **Almost nothing, and this is the important finding.** The beachhead self-hosts *specifically to avoid* an aggregator, so they are not paying OpenRouter's 5%. They may pay for observability — but that budget belongs to the platform team as a whole, not to the inference bill.
**Why they would move.** They would not, mostly. **This petal is better read as a channel than a source of budget**: LiteLLM is where CAMIR distributes ([gtm.md](gtm.md)), not who CAMIR bills against.

### Petal 3 — Inference efficiency tooling (caching, quantisation, serving optimisation)

**Incumbents.** GPTCache and successors [S37]; vLLM and SGLang [S31][S32]; quantisation and speculative-decoding tooling — **all free and open source.**
**What those customers spend.** Effectively zero in licence terms, and substantial in engineering time. Semantic caching alone removes 20–45% of production traffic before it reaches any router [S36].
**What CAMIR displaces.** **Nothing monetary.** It composes with all of it, and caching actively erodes CAMIR's addressable volume by taking the easy traffic first [G4].
**Why they would move.** They do not move; they add. The realistic framing: CAMIR is the **next tool a team reaches for after caching and batching are exhausted**, which makes this petal a *sequencing* signal — a team that has already deployed a semantic cache is qualified, because they have proven they will do work to cut inference cost.
**This is the most useful qualification signal in the diagram.**

### Petal 4 — Cloud and AI cost optimisation (FinOps)

**Incumbents.** ProsperOps, nOps, Usage.ai and the cloud FinOps category, several of which price on **share of savings with per-customer rates that are never published** [S38][S39].
**What those customers spend.** A percentage of realised savings, negotiated per contract.
**What CAMIR displaces.** **Not a budget — a pricing precedent.** FinOps vendors do not touch inference routing, so there is no revenue to take. What this petal supplies is the *legitimacy* of share-of-savings pricing at an enterprise, and the warning that comes with it: in those contracts, the definition of **eligible savings** is what actually gets negotiated [S38].
**Why they would move.** They would not. But the buyer's organisation may already have a FinOps function that has normalised this pricing shape, which shortens the pricing conversation considerably.
**Watch this petal.** A FinOps vendor extending into inference cost is the most plausible acquirer profile and the most plausible fast follower.

### Petal 5 — Internal engineering time on bespoke routers and evaluations

**Incumbents.** The customer's own team. Wen (P3) has already built and abandoned an internal router; Marcus (P2) has an A/B test from March that is two model upgrades stale.
**What those customers spend.** `(assumption: 2–8 engineer-weeks to build a first internal router, plus recurring maintenance that in practice does not happen — which is why these systems go stale)`. At loaded engineering cost that is **roughly $20k–$80k of one-time spend**, and the recurring maintenance is real but unbudgeted.
**What CAMIR displaces.** The build-versus-buy decision, and more importantly the **maintenance that nobody is doing**. Wen's objection — *"I could build this in three weeks"* — is true, and the honest counter is that she then owns it, and the thing she lacks is maintenance capacity, not capability.
**Why they would move.** Because the frontier moves with every model release and a stale router silently reverts to overpaying, or worse, to under-serving. Continuous re-measurement is the part nobody does themselves.
**Why they would not.** Engineering time is rarely a budget anyone protects, and "we'll build it" is the default answer from exactly the people CAMIR sells to.

---

## The finding

**Only one petal holds real, approved, transferable budget: Petal 1, the inference bill itself.**

- Petal 2's budget exists but belongs to teams who chose *not* to use an aggregator — it is a channel, not a wallet.
- Petal 3 is free software; there is no money in it, and it shrinks CAMIR's volume.
- Petal 4 is a pricing precedent and an acquirer profile, not a source of customers.
- Petal 5 is unbudgeted engineering time, which converts into willingness to pay only when the maintenance cost becomes visible — and it usually does not.

Three consequences that change the plan:

1. **Price out of Petal 1 or not at all.** Share-of-savings is not merely elegant, it is the only pricing that reaches a budget that already exists. Every alternative requires creating a new line item, and creating one is a much harder sale than shrinking an existing one. This is the strongest argument for share-of-savings in the whole pack, and it is stronger than the incentive-alignment argument in `../BRIEF.md`.
2. **Qualify on Petal 3 behaviour.** A team that has already deployed semantic caching or done serving optimisation has *demonstrated past behaviour* of spending effort to cut inference cost — the Mom-Test-clean qualifier. A team that has not is a team that does not care yet.
3. **The real competitor in every deal is Petal 5.** Not Martian, not the vendor's free router — the customer's own engineer saying "I could build this." That objection is answered with maintenance and measurement, never with algorithm quality, which is fortunate because the algorithm has plateaued [S4].

## Recommended next 3

1. **Rewrite the qualification criteria in [../validation/discovery_guide.md](../validation/discovery_guide.md) around Petal 3 behaviour** — has this team already done work to reduce inference cost, and what happened.
2. **Make "we'd build it ourselves" the primary objection the sales roadmap answers** ([sales_roadmap.md](sales_roadmap.md)), with maintenance-and-drift as the counter, not features.
3. **Track a FinOps vendor entering inference optimisation as a leading indicator** in [../financials/risk_matrix.md](../financials/risk_matrix.md). Petal 4 holds both the most likely acquirer and the most likely fast follower, and the two arrive by the same door.
