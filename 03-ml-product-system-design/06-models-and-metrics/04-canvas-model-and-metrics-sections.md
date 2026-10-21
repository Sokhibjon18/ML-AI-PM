# Filling the "Model" and "Metrics" Sections of the AI Canvas

> Module: ML Product System Design · Topic: Models and Metrics · Lesson 4
> Original: «Заполняем шаблон разделов «Модель» и «Метрики» AI Canvas»

## Key ideas
- "Model: recommender system; Metrics: recall, precision, GMV" is technically filled but tells the team nothing. These sections should be a short, testable justification of a product-technical decision.
- Causal chain to follow: **problem → model output → task type → error → model metric → product result**.
- The Canvas is a set of hypotheses, not a final spec.

## Frameworks / methods

### "Model" section — four elements
1. **Input** — what the model receives (buyer context, item attributes, interaction history, availability, features justified in "Data"). Link to Data section, don't repeat it.
2. **Output** — what it returns to the product.
3. **Task type** — derived from the output form.
4. **Rationale** — one sentence on why that type fits the output.

Consistency checks after drafting:
1. Is the input data available at decision time?
2. Does it work for new users and new items (cold start)?
3. Can the output reach the right point in the product?
4. Does the output match the value promised elsewhere in the Canvas?

### "Metrics" section — steps
1. Define the positive outcome (per stage — candidate retrieval vs final selection differ).
2. Describe FP and FN in product language, not just acronyms.
3. Choose the primary metric **with a causal "because"**.
4. Add a **guardrail/constraint metric** so optimizing one metric doesn't break the system (e.g., max recall by passing the whole catalog).
5. Don't invent target values ("recall ≥ 95%") without data — state direction of improvement + constraint; set numbers later from experiments, requirements, error costs or baseline.
6. Name the **baseline** concretely (e.g., current home page with popular items), not "conversion will grow".
7. Link model quality → user behavior change → product result → business effect.

## Case / example
Yarmarka problem restated (not "we need a recommender", which is a solution): buyers struggle to find suitable items in a huge catalog; much of the assortment is rarely shown.

Model draft: input = buyer info, interactions, available item attributes; output = items ordered by predicted relevance; type = ranking; rationale = need relative order for personal home page.

Metrics draft:
- Broad retrieval — primary: recall (missed item can't return later); constraint: control candidate-set size/quality.
- Final selection — primary: precision (irrelevant item steals a scarce slot); baseline: current popular-items home page; product metrics: conversion to purchase, GMV per user; also monitor long-tail discoverability and search→recommendation cannibalization.

Example inconsistencies:
- Value "boost new items" + Data "only per-item purchase history" → new items have no history (cold start contradiction). Quiz answer: check how the system handles items with no interaction history first — not library, language, LOC or feature count.
- Model = ranking, Metric = number of registered users → no link between model quality and that business metric.
- Quiz: "CTR is the model metric; if it rises, personalization succeeded" → fix by separating model quality from product result and verifying CTR gains via conversion/GMV.

## Practical assignment
Fill drafts for your project or Yarmarka:
- Model: product result, matching task type, why, needed data and its availability at decision time.
- Metrics: positive event, FP meaning, FN meaning, which is costlier, primary model metric, constraint metric, baseline, product/business metric that must move.

Self-check highlights: type follows output form and isn't replaced by a technology name; cold-start fallback exists; output is actually used in product; both errors' costs assessed; guardrail set; baseline defined; model vs product metrics separated; targets justified.

## PM takeaways
- Every Canvas line should answer "why", not just "what".
- If adjacent links in the chain don't connect, revisit data, cold-start plan, task framing or evaluation.
- Further reading: Google recommendation systems overview, Google ML Crash Course (classification), scikit-learn metrics; books by Huyen, Eric Siegel ("The AI Playbook"), Aggarwal, Burkov.
