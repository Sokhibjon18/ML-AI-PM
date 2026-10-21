# Worked AI Canvas: Yarmarka Homepage Personalization

> Module: ML Product System Design · Topic: AI Canvas Framework · Lesson 2
> Original: «Разобранный AI Canvas: персонализация главной страницы «Ярмарки»»

## Key ideas
- A full walk-through of one Canvas. "Launch recommendations and raise conversion" is a generic phrase that fits any business — it hides the customer problem, the system output, data, placement, baseline and how to tell product-wide change from a local one.
- **Canvas captures hypotheses; it must not mask unknowns with precise-looking numbers** (e.g. "+0.2 pp conversion" before a baseline exists).
- **A local decision inside the model almost always has product consequences outside it.** Document every decision in the Canvas.
- An early Canvas full of open questions is a success — it shows which decisions remain.

## Frameworks / methods
- **Course fill order** (modified from the original): Value Proposition → Customers → Stakeholders → (Data, Skills, Output) → Integration → (Cost, Revenue). Rationale: VP and customers are usually considered together (Value Proposition Canvas); stakeholders together with customers (stakeholder map); data is easier once you know whom to ask. Original order: VP → (Data, Skills, Output) → (Customers, Stakeholders, Integration) → (Cost, Revenue).
- **Cross-block review checks:**
  - Data ↔ Output: can the planned data produce a personal ranking? New SKU with zero history + history-only model = gap.
  - Output ↔ Value: did more clicks actually help buyers / business?
  - Integration ↔ Value: if the homepage refreshes too rarely or ignores the result, no value.
  - Customers ↔ Data: is there a way to serve new buyers without history?
  - Cost ↔ Revenue: does the incremental effect pay for data, compute, integration and support?
- **Cost table** (no exact budget needed): build (baseline analysis, data prep, dev & integration, experiment setup); run (compute, event storage/processing, model/data refresh, personalization services); monitor & support (quality monitoring, degradation analysis, new users/SKUs control, rule revisions).

## Case / example
- Yarmarka: 3.1M SKUs, 12k sellers, near-identical homepage, 71% turnover from 4% of assortment.
- **Value Proposition:** help buyers find suitable items faster and raise share of sessions ending in purchase — no promised % yet. Baseline candidate: top sales by category; personalization must beat that, not "nothing".
- **Customers:** buyers (primary); small sellers/craftspeople in the long tail (indirect).
- **Stakeholders:** homepage product team, recsys/ML, data engineers, product analysts, homepage devs, catalog/item card team, seller team, ops/monitoring.
- **Data:** catalog + attributes, impressions, clicks, add-to-cart, purchases, visit context. Check availability, gaps, cross-device/version consistency, usable history. Key risk: **cold start** — new buyers have no history, new items have no interactions, and new/low-visibility items are exactly the original pain.
- **Skills:** beyond "ML engineer + developer": product, buyer behavior, catalog structure, event pipelines, recsys/ranking, experimentation, UI integration, post-launch ops; possibly a merchandising expert.
- **Output:** system = ordered list of items for a given collection/block. Quality questions: are useful items at the top, stable across user groups, do new items vanish for lack of history. Business: higher collection CTR may be **cannibalization** (purchases moved from search) — need incremental, not redistributed, effect.
- **Integration (high-level flow):** buyer opens homepage → system gets context + catalog candidates → AI ranks → homepage receives list → buyer interacts → new events flow back for future updates. Not an architecture diagram.
- **Decision ripple 1:** switching from one list for all to per-user ranking touches Data, Output, Integration, Skills, Cost, Revenue (must prove vs baseline), VP. Quiz: if VP, audience and revenue mechanism stay the same, revisit **Data, Output, Integration** first.
- **Decision ripple 2:** adding item attributes so new SKUs can be ranked before history accumulates → more data (category, description, attributes), output must work with sparse history, candidate rules for new items, cold-start skills, higher data-prep cost, revenue checked via demand for new/niche items, sharper VP.
- **Revenue math:** 62.9M visits/month, 1.76M orders, AOV 2,900 ₽, 12% commission. +0.2 pp conversion ≈ 125,800 extra orders/month ≈ 365M ₽ GMV ≈ 43.8M ₽ platform revenue/month ≈ 525M ₽/year. Small relative change, big absolute money — but the experiment must be able to distinguish such a small delta from noise.
- **Final quiz:** CTR rose, total orders flat, search usage fell → the local result improved but its link to VP and economic effect is unconfirmed (not a data, output format, placement, audience or stakeholder problem).

## PM takeaways
- Write the system output precisely ("ordered list for block X"), not "better recommendations".
- Always name the baseline and test for incremental effect, watching for cannibalization.
- Explicitly record the cold-start question in the first Canvas.
- Canvas is iterative over the whole product lifecycle; a full rewrite in six months is normal.

## Further resources
- [Rules of Machine Learning (Google)](https://developers.google.com/machine-learning/guides/rules-of-ml) — start ML systems simple with measurable goals.
- [ML: The High Interest Credit Card of Technical Debt (Google Research)](https://research.google/pubs/machine-learning-the-high-interest-credit-card-of-technical-debt/) — dependencies and running costs of ML systems.
- Ameisen, *Building ML Powered Applications* — define the product goal and a simple end-to-end pipeline before complicating the model.
- Huyen, *Designing ML Systems* — model quality is only one part; data, integration, ops, feedback matter.
- Agrawal, Gans, Goldfarb, *Prediction Machines* — AI as part of decision-making; ask what happens after the prediction.
- Osterwalder & Pigneur, *Business Model Generation* — logic of linked one-page schemes.
