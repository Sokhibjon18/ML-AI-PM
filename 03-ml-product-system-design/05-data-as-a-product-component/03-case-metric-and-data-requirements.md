# Case: Choosing a Metric and Data Requirements

> Module: ML Product System Design · Topic: Data as a Product Component · Lesson 3
> Original: «Кейс. Выбор метрики и требования к данным»

## Key ideas
- There's no universal technical metric: the same error can have very different costs. Ask which error happened, what the product does next, and who bears the consequences.
- **Choose the metric from the business cost of errors, then check the data can measure it.**
- In another product the costs of the same errors can be reversed — learn the reasoning path, not "task X = recall".

## Concepts & definitions
- **Confusion matrix** (positive class = the thing being detected): TP (said yes, is yes), FP (said yes, is no), FN (said no, is yes), TN (said no, is no).
- **Recall** — share of truly relevant/positive cases the model found. Prioritize when missing something (FN) is costly.
- **Precision** — share of model's positive calls that are correct. Prioritize when false alarms (FP) are costly.
- **Threshold** — model outputs a confidence 0–1; cases ≥ threshold (e.g. 0.7) are flagged. Lower threshold → catch more positives but more false alarms; higher → more cautious, fewer catches and fewer false alarms.
- **Three evaluation levels:** model metrics (precision, recall) → product metrics (share of wrongly removed reviews, appeals, support load) → business metrics (active assortment, sales, seller retention). Transitions are hypotheses to test, not automatic.

## Case / example
**Photo search (find a lamp from a photo)**
- FP = an irrelevant item shown (e.g. a floor lamp of similar color) — minor; user scrolls past.
- FN = a relevant catalog item not shown at all — user never learns it exists; bad for long-tail discovery.
- Priority: **recall** (if 10 relevant items exist and 9 are found, recall is high even with some extras). Precision still monitored as a secondary metric — a flood of junk destroys trust — but high precision says nothing about items not shown.

**Fake review detector** (positive = fake)
- FP = a genuine review flagged/removed → sellers and buyers lose real feedback, trust drops. Deemed most critical.
- FN = a fake review missed → risk, but less critical; mitigations like seller-initiated manual check with support.
- Priority: **precision** — when the model says "fake", it must be right.
- Product-process fix instead of chasing a perfect threshold: auto-remove only very high scores, send borderline cases to manual review.

**Data requirements driven by the metric (fake reviews)**
- A reference answer: reliably labeled subset (moderation outcomes, confirmed violations, expert-labeled sample) and an agreed definition of "fake" — if moderators disagree, the label itself is not a valid benchmark.
- Account for rarity of fakes in the test set and interpretation, or overall accuracy hides poor performance on the target class.
- Test data separated from training data.
- Test data similar to reality: training only on already-removed reviews (mostly simple rating-boosting) may miss complex patterns like fakes aimed at lowering a competitor's rating.
- Photo search: test data needs a fairly complete set of truly relevant items per image, otherwise you can't count what was missed. Fake reviews: test set needs enough genuine reviews incl. hard/borderline ones to measure false removals.

**After launch**
- Raising the threshold improved the priority metric and cut FPs on test; in production wrongly blocked reviews and seller appeals dropped (product-level confirmation), but seller retention didn't change noticeably — not proof the model is bad; each link in the metric chain needs separate testing.

## Frameworks / methods
Order: define critical model behavior → pick metric that measures it → check data allows reliable evaluation → define how prediction becomes product action → verify with product and business metrics.

Checklist:
- Model task defined.
- Which error is more critical for the product defined.
- Metric matches the cost of error.
- Threshold and acceptable error balance set.
- Data verified to measure the metric correctly.
- How the prediction turns into a product action defined.
- Product and business metrics for validation defined.

## PM takeaways
- The PM doesn't build the dataset or labeling procedure but must ensure critical business cases are represented, correct answers can be reliably defined, and the test set matches post-launch conditions.
- Data requirements come from which errors you need to see before launch, not "more is better".
