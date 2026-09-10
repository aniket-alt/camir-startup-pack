# CAMIR — Mission, vision and values

**What this is** — CAMIR's durable mission, future vision, operating values and reason for existing, independent of any named competitor.
**Why it exists** — a company can turn a routing mechanism into a mission and lose the customer-facing failure that matters: an unmeasured quality trade-off assigned to an engineer who did not choose it.
**How to read it** — read the mission for today's obligation, the vision for the future state, and the values as trade-offs; attack whether each survives routing becoming free inside a serving engine.
**Depends on / feeds** — depends on [BRIEF.md](../BRIEF.md), [strategy/positioning.md](../strategy/positioning.md), [product/PRD.md](../product/PRD.md), [tech/whitepaper.md](../tech/whitepaper.md) and [founder_story.md](founder_story.md); feeds [one_pager.md](one_pager.md), [pitch_deck.md](pitch_deck.md), [future_press.md](future_press.md) and [README.md](../README.md).

## Mission

Make every self-hosted model pool's cost-quality frontier measurable and auditable, so platform teams can set quality tolerance deliberately rather than making a consuming engineer absorb an invisible regression.

## Vision

Inference teams treat each model change, cache policy, quantization choice and routing decision as a reproducible frontier run. The frontier records not only cost and quality, but judge agreement, artifact flags, utilization, endpoint ownership and the decision to stop routing when the ceiling is too low.

## Values as trade-offs

1. **Disqualify before optimizing.** We will publish a do-not-deploy result when the oracle ceiling cannot support the route, even when a positive demo would be easier to sell.
2. **Customer-owned tolerance.** We will give the consuming engineer a pin and a traceable policy instead of maximizing aggregate savings under an opaque vendor tolerance.
3. **Measurement before novelty.** We will ship calibrated confidence and artifact controls before a trained classifier, because the classifier must beat the real baseline [S8].
4. **Open the counterfactual.** We will keep the router, harness and savings computation inspectable inside the customer's perimeter, even when a private control plane would be simpler to monetize.
5. **Name the uncertainty.** We will label assumptions, benchmark transfers and future scenarios rather than turning them into traction or company results.

## Why we exist

The quality risk of routing is distributed badly: the budget owner wants a lower bill, the platform engineer wants an auditable number, and the consuming product engineer can veto a route that makes their endpoint worse. CAMIR exists to put the tolerance, tier decision, judge record and counterfactual in the same accountable loop: **Classify → Dispatch → Judge → Attribute → Recalibrate**.

## Recommended next 3

1. Turn the mission into the acceptance criteria for the first frontier run.
2. Test the values in shadow mode with a consuming engineer who can pin unilaterally.
3. Revisit the vision after E1/E2 and remove any part the measured evidence does not support.
