# Two Comparative Cases at "Yarmarka"

> Module: ML Product System Design · Topic: Models and Metrics · Lesson 3
> Original: «Два сравнительных кейса в «Ярмарке»»

## Key ideas
- One business goal, but a recommender has several sequential model decisions; error cost differs per stage, so metric choice is **per decision, per point in the product**, not per system.
- Broad retrieval stage → recall matters most; final selection for limited slots → precision matters more. Neither can be watched alone.
- Even if all model metrics and CTR improve, value must be proven against a **baseline** via product/business metrics.

## Concepts & definitions
- **Candidate generation (retrieval)** — shrink the full catalog to a few hundred candidates; result is a **short-list**.
- **Final ranking/selection** — pick the few items that get scarce UI slots.
- **Opportunity cost of a slot** — a wrongly chosen item both shows something irrelevant and pushes out a better item.
- **Baseline** — what users get without the new model (popular items, category bestsellers, editorial picks, city/season picks). A simple popularity list can be surprisingly strong.
- Top-K metrics exist for recommenders; not covered yet.

## Case / example
Yarmarka facts: 3.1M SKUs, 12K sellers, 4.2M MAU buyers, 62.9M visits/month, 1.76M orders, 2.8% visit→order conversion, AOV 2,900 ₽, ~5.1B ₽ GMV/month; 71% GMV from 4% of items; 18% of search sessions end with zero clicks; home page nearly identical for everyone.

| | Broad retrieval (3.1M → 500) | Final selection (500 → 20 slots) |
|---|---|---|
| Main risk | Losing a good item too early (irreversible) | Giving a scarce slot to a bad item |
| Costly error | FN (miss) | FP (false inclusion) |
| Key metric | Recall — "sufficient", not ~100% (else it passes the whole catalog) | Precision — but recall still matters, or you collapse to popular items |

- FN at stage 1 also hurts long-tail visibility, a core business problem.

Three levels of evaluation:
1. **System output** — personalized ordered list.
2. **Output quality** — e.g., recall 0.72 → 0.84; precision up at final stage.
3. **Product/business effect** — e.g., conversion 2.8% → 3.0% (+0.2 pp): 62.9M × 0.002 ≈ 125,800 extra orders → × 2,900 ₽ ≈ 365M ₽ GMV → × 12% commission ≈ 43.8M ₽ revenue/month. This is sizing, not proof of causality.

Built-in traps of the case:
1. No defined baseline.
2. Recommendations cannibalize search interactions.
3. Cold start for new users/items unsolved.
4. CTR optimized without GMV per user growth.
5. Effect not checked against scale and experiment uncertainty.

Why CTR can mislead: (a) real gain; (b) redistribution from search with flat conversion; (c) "clickbait" items opened but not bought.

Quiz principles: stage-1 dropped relevant items can't be recovered later; final-stage FP occupies a scarce slot; to claim business value you need conversion/GMV per user, baseline comparison and a search-cannibalization check — not algorithm name, feature count or library version.

## Frameworks / methods
Checklist before writing a metric into the AI Canvas:
1. What concrete decision does the model make?
2. What counts as positive?
3. What does an FP mean?
4. What does an FN mean?
5. Which is costlier?
6. Which metric controls that error?
7. Which other metric must not be sacrificed?
8. What baseline are we comparing to?
9. Which product/business metric proves the model improvement was useful?

## PM takeaways
- Specify the metric for each model decision, not "the recommender".
- Always pair a primary metric with a guardrail metric.
- Model metric ↑ and CTR ↑ are intermediate signals; require baseline comparison and end-metric movement.
- Further reading: Google recommendation systems course (overview, candidate generation, scoring), TensorFlow Recommenders retrieval/ranking tutorials; books by Huyen, Aggarwal, Jannach et al., Géron.
