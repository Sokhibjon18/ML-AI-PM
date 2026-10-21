# Classifier Quality Metrics

> Module: ML Product System Design · Topic: Models and Metrics · Lesson 2
> Original: «Метрики качества классификатора»

## Key ideas
- A single "% correct" number hides *where* the model errs; errors differ in product cost.
- Pick the metric from the **consequences of errors** (FP vs FN), not by habit.
- A better model metric does not prove a better product — the link must be tested.

## Concepts & definitions
- **Positive class** — the event of interest (fraud, purchase, churn). "Positive" ≠ "good".
- **Confusion matrix** — TP (correct "yes"), FP (false alarm), FN (miss), TN (correct "no"). FP and FN are where losses come from.
- **Accuracy** = (TP+TN)/all. Fine with balanced classes and similar error costs; misleading under class imbalance. Example: 950 irrelevant / 50 relevant items, an "always no" model scores 95% yet finds nothing.
- **Precision** = TP/(TP+FP) — "when the model says yes, how often is it right?" Sensitive to FP. E.g., 80 of 100 flagged are relevant → 0.8.
- **Recall** = TP/(TP+FN) — "what share of real positives did we find?" Sensitive to FN. E.g., 80 of 100 relevant found → 0.8.
- **F1** = 2·P·R/(P+R) — harmonic mean; high only if both are high. Assumes symmetric importance; knows nothing about money/user cost.
- **Threshold** — classifiers usually output a score; you choose the cut-off (0.5 is not sacred). Lower threshold → more "yes" → recall ↑, FN ↓, FP ↑, precision ↓. Higher threshold → the reverse.
- **ROC-AUC** — area under ROC curve; measures how well the model separates classes across all thresholds. Good for comparing models, but doesn't choose the threshold or price FP/FN.
- **Operating point** — chosen threshold plus the resulting P/R/FP/FN mix.
- Other terms: class imbalance, model score, baseline (current rule/process/version to beat), model vs product vs business metric.

## Frameworks / methods
Metric selection algorithm (for AI Canvas):
1. Define the positive class.
2. Describe FP: "model said yes, but actually…" → consequence.
3. Describe FN: "model said no, but actually…" → consequence.
4. Compare costs (lost user, extra operation, missed sale, delay, manual review, worse UX) — money not required at first.
5. Choose main metric: FP costly → precision; FN costly → recall; balanced costs → F1; accuracy only if classes/costs allow; ROC-AUC for separation quality, not operating mode.
6. Name the product/business metric that should move if the model truly improved.

## Case / example
- Yarmarka pre-filter before recommendations: an irrelevant item that passes can be removed later; a relevant item dropped early is lost forever → FN is costlier → recall matters here.
- Opposite case: if a positive decision auto-blocks a user, FP is very expensive.
- Medicine: false positives tolerated (extra tests beat a missed diagnosis) — but don't turn it into a lead generator for tests.
- Trap: higher recall or CTR on recommendations ≠ more GMV; recommendations may just cannibalize search.

Quiz principles: 99% accuracy with 1% positives and "always negative" model is useless; cheap manual review + costly missed critical ticket → prioritize recall; F1 doesn't price errors; ROC-AUC doesn't pick the threshold; sample size isn't a quality metric.

## PM takeaways
- Never accept "95% accurate" without the class distribution and confusion matrix.
- No universal rule like "FN is always worse" — decide per product context.
- The threshold is a product decision about the FP/FN trade-off.
- Further reading: Google ML Crash Course (classification metrics), scikit-learn metrics docs, Chip Huyen, Burkov, Géron "Hands-On ML".
