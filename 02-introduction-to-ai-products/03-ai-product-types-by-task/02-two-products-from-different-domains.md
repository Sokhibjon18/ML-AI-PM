# Two Products from Different Domains

> Module: Introduction to AI Products · Topic: AI Product Types by Task · Lesson 2
> Original: «Два продукта из разных доменов»

## Key ideas
- Before shipping a model, answer: which product decision it supports, where data comes from, how to get correct answers (labels), which metrics measure quality, and how to safely test it on live traffic.
- A shared business goal ("grow sales") is not a model task: each component (search, recs, forecasting) owns a specific product decision.
- The domain dictates model requirements, labeling, evaluation and rollout plan.

## Concepts & definitions
- **Assessors** — human raters who grade query–item relevance on a predefined scale (full / partial / not relevant).
- **Offline eval** — evaluating a model on historical/labeled data before it touches users.
- **Shadow mode** — new model runs on real requests in parallel; users still see the old system.
- **Guardrail metrics** — metrics that must not get worse while optimizing the main one.
- **Cold start** — no history for new users (or items), so personalization lacks signal.
- **Backtesting** — forecasting past periods using only data available at that time.
- **Position bias** — items get clicks because they're shown high, not because they're best.

## Case / example — E-commerce marketplace (three problems)
Symptoms: search → add-to-cart conversion fell 18% → 14%; homepage recommendation blocks rarely opened; simultaneous stockouts of popular items and overstock of slow movers.

**1. Search & ranking**
- Losses: relevant items exist but rank too low; system matches queries too literally (no synonyms, attributes, typos — e.g., "white running shoes" misses "color: milky, purpose: running"). Diagnose via logs + manual review of low-conversion queries.
- Input: query, available catalog, constraints (region, price, delivery). Output: ranked list.
- Data: logs (queries, impressions, clicks, add-to-cart, purchases) — plentiful but biased by position and attractive photos. Add assessor labels, focusing on high-frequency queries, new items, and click-vs-expert disagreements.
- Offline metric: **NDCG@10**, plus empty-result rate and latency (high NDCG is useless if results are slow or exclude regionally available items).
- Rollout: shadow mode (stability, latency, empty results, big diffs vs old) → A/B test. Main metric: search → cart conversion; guardrails: query reformulations, response time.
- Risks: popular items get even more exposure, new items never surface (handle position bias, new items, stock, diversity); attractive-but-irrelevant items raise clicks yet lower purchases.

**2. Recommendations (no query yet)**
- Input: view/purchase history, add-to-cart, item properties, session context. Output: personal list.
- Behavioral labels with weights: purchase > view; fast bounce = negative. Decide how to treat returns, gifts, seasonality, repeat purchases.
- Cold start: fall back to overall popularity and current-session actions.
- Risks: narrowing to familiar categories, boosting a few brands or high-margin items over user interest. Balance relevance, diversity and business constraints; split metrics for new vs returning users so averages don't hide cold-start problems.

**3. Demand forecasting (weekly purchase volume per SKU per warehouse)**
- Input: daily sales, stock, prices, promo/holiday calendar, lead times, seasonality. Output: forecast + suggested order a manager can override.
- Trap: **sales ≠ demand** during stockouts (sales drop to zero while interest remains). Identify stockout periods and reconstruct lost demand using proxies like "notify me when available" clicks.
- Evaluate via backtesting; **MAE** in units plus **bias** (under → stockouts, over → overstock). Main challenge: incorporating future campaigns, seasonality, trends.

## Case / example — Medtech (clinic network)
Problems: growing X-ray volume, long waits for reports at peak; doctors manually read dozens of documents per patient history.

**CV component — flag suspicious area**
- If risk above threshold, scan is moved up the radiologist queue and the area is shown as a hint; the specialist makes the final call.
- Needs **expert labeling** (radiologists drawing boxes/contours) — archive text reports aren't enough. Expensive; experts disagree on boundaries → have 3–5 independent annotators per image and agreed merge rules. Budget for cost, expert availability, conflict rules, dataset prep time.
- Split train/test **by patient** (otherwise the model memorizes individuals). Separate eval on other clinics / other equipment vendors to test generalization.
- Risks: missed pathology, too many false alarms, doctor over-reliance; even scanner model or brightness settings matter. PM defines usage boundaries, mandatory specialist confirmation, quality monitoring. Metrics: sensitivity and specificity.

**NLP component — key problems from patient history**
- Not diagnosing — structuring: extract key facts, show dynamics with links to source documents; separate active vs resolved issues, chronic vs one-off, tie each claim to a source.
- Main offline metric: **recall on key problems** (experts mark 5, system finds 4 → 80%). Missing a critical fact is dangerous.
- Also guard precision: flagging resolved or minor issues as critical makes doctors stop trusting the tool.

## Frameworks / methods
**Comparison template for a new AI solution:**

| Business goal | Model input | Output → action | Type | Offline eval | Main risk |
|---|---|---|---|---|---|
| Lift search conversion | Query, catalog, filters | Ranked list → show results | Search & ranking | NDCG@10 | Position bias, popularity amplification |
| Lift homepage engagement | Action history, items, context | Personal list → show block | Recommendations | NDCG@20 | Monotony, cold start |
| Cut stockouts & overstock | Sales, stock, promo, seasonality | Forecast → suggest order | Forecasting | MAE + bias | Sales ≠ demand during stockouts |
| Flag urgent scan | Medical image | Area/risk → reprioritize, show doctor | Detection | Sensitivity, specificity | Missed pathology, over-trust |
| Extract key issues from reports | Report texts | Issues, dynamics, source links | NLP / classification | Recall | Missing a critical fact or noise overload |

## PM takeaways
- Sequence: understand input data → agree on labeling (who defines truth, disputed cases, freshness) → choose an offline metric that reflects the costly error, plus constraints on latency, manual-review load and per-segment quality → link offline eval to the product metric.
- Rollout depends on the cost of error: shadow mode fits almost everything; search/recs → A/B test; medical → limited pilot, mandatory human review, gradual expansion.
- A complete PM brief to the team covers: decision, data, labeling, model output, product action, eval, launch method, risks.
