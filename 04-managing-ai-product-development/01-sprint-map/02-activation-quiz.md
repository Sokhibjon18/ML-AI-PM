# Activation Quiz

> Module: Managing AI Product Development · Topic: Sprint Map · Lesson 2
> Original: «Активирующий квиз»

## Key ideas
- A warm-up quiz that previews the sprint's themes: try simple before complex, plan under uncertain outcomes, split work between data roles, and revise plans when new information changes feasibility.

## Frameworks / methods
Concepts tested and the principle behind correct answers:

- **Self-reflection (no wrong answers):** have you pushed for a simple solution first, planned work with non-guaranteed results, split tasks across specialists, changed plans after unexpected early results? Each maps to a sprint topic (heuristic vs ML, research planning, DE/DS/MLE roles, flexible planning).
- **Rule before model:** Yarmarka support staff already have intuition about what similar buyers like → formalize it as simple rules and test first. Training a model, hiring a data scientist, or running two competing approaches in parallel all add complexity before proving it's needed.
- **Rule vs model economics:** rule costs 15,000 ₽/month, +0.1 pp conversion; model costs 180,000 ₽/month, +0.3 pp. Extra 0.2 pp × 62.9M visits ≈ 125,800 orders × 2,900 ₽ ≈ 300M+ ₽ extra GMV vs 165,000 ₽ extra cost → the switch pays off by a wide margin; orders-of-magnitude difference means margin data isn't needed for the conclusion. Conversion must still be converted into money to compare with cost.
- **Data Engineer vs Data Scientist:** reliable delivery (daily dedup'd load from three systems into one table; real-time catalog updates) → data-flow role (DE); finding patterns (time-on-site vs purchase; which item features predict interest) → research role (DS).
- **Commitments for research tasks:** promise to *test the hypothesis by an agreed method and decide the next step* (or "we'll know whether to keep this feature") — not to guarantee improvement or ship regardless of results.
- **When to revise the plan:** when a check reveals insufficient data to proceed (changes feasibility of the route). A few days' delay, a more elegant alternative for the same goal, or a stakeholder status question are not reasons by themselves.

## PM takeaways
- Commit to experiments and decisions, not to unknown outcomes.
- Compare approaches in money, not just in model or conversion metrics.
- Replan on information that changes feasibility, not on noise.
