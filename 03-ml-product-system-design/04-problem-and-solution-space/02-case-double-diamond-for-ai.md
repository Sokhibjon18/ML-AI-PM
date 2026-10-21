# Case: Reasoning Through the Double Diamond for AI

> Module: ML Product System Design · Topic: Problem and Solution Space · Lesson 2
> Original: «Кейс. Ход рассуждений через Double Diamond для AI»

## Key ideas
- One business signal can have many causes; the team agreed on a rule: **don't discuss solutions until the problem is understood.**
- In AI products there's an extra bridge between Problem and Solution Space: an **AI applicability filter** — enough (quality) data? is AI the best option? can it be operated? is it worth it?
- **Determine whether a solution is possible first; pick the technology second.**
- Best AI solution ≠ highest model quality; it's the best balance of quality, speed and cost.
- A better model doesn't guarantee a better product; first launch starts a new learning cycle.
- The most important project decision, in retrospect: refusing to jump to an AI solution and first understanding the problem.

## Case / example
**Signal:** Yarmarka search-to-purchase conversion fell from **3.4% to 1.8%** in three months; search drives ~**40%** of orders. First reflex: "we need smarter AI search / LLM".

**Discover** — question: at which step do users stop moving toward purchase? Funnel: query → results → item card → cart → purchase; plus search logs, interaction history, support tickets.
- Hypothesis 1 — prices: conversion fell even where prices didn't change → rejected (refuting also reduces uncertainty).
- Hypothesis 2 — poor item cards: users often didn't even reach cards; they dropped at the results page → narrowed to query→results.
- Hypothesis 3 — search scenarios: complex intent queries ("laptop for a designer", "trail running shoes") returned gaming laptops / gym sneakers. The system matched words, not intent.

**Define** — problem statement: users don't find relevant items via search because the algorithm doesn't understand the intent of complex queries and doesn't put the best results at the top. Says *what* to change, *where*, and what success is.

**AI applicability check**
- Lots of data (queries, clicks, views, purchases, query reformulations) but weak training signal: **position bias** — clicks partly reflect position set by the old ranker; naive training would reproduce old errors.
- Uneven data across categories: electronics rich, rare items sparse. Needs log cleaning, labeling, an intent-reflecting dataset.
- Alternatives: rules/synonyms (fast, cheap, don't scale — become an exception pile; rules work only where variation is bounded); catalog quality (helps but doesn't capture query intent); LLM (handles language well but costly at millions of queries, too slow for instant results, unpredictable/unexplainable ranking → too risky as main ranker).
- Reframed question: do we have enough signals for a model to rank better than the current system? Task: estimate probability a result is useful, using graded signals: view (interest) < add-to-cart (stable interest) < purchase (intent to buy). Simple baseline comparison → consistent offline ranking gains.
- Operability: latency, running cost (storage, infra), robustness to seasonality and assortment/behavior shifts; model must be monitored and refreshed.

**Develop** — avoid hunting for "the best model" instead of the best solution. Four options:
1. Better rules — quick, explainable, but infinite phrasings make it unmaintainable. Rejected.
2. Semantic search — good for vague intent, weak on exact queries (SKU codes, specific models), needs big infra/process changes.
3. LLM as search core — strong NLU, but high opex, latency, inaccuracy, low explainability. Useful as a component, not the main ranker.
4. **Hybrid ranking** — keep the classic engine, add ML re-ranking with item attributes, seasonality, search history, user context; fast to integrate, keeps latency, improves as data accumulates. **Chosen.**
Selection lens: by tech sophistication LLM wins; by ease rules win; product decision weighs user ease, incremental profit, cost payback, scalability.

**Deliver**
- Offline: users found the right item in top results far more often.
- Prod: target 200 ms, actual 450–600 ms; model only ~40 ms, the rest was feature collection. Trade-off: precompute some features in a separate store, drop others — slight quality loss for required speed.
- A/B test: better ranking but smaller conversion lift than offline suggested. Reasons: users adapted to old search habits; found items not bought due to price/size/offer; weak categories with few training examples.
- Iteration instead of immediate scaling: reworked signals, better training data prep, selective new features, separate strategies for low-data categories, expanded monitoring. Second A/B → more stable business-metric growth.
- Real outcome: a reproducible decision process (understand → test hypotheses → check feasibility → measure business effect → scale).

**After launch** — environment keeps changing (new items, prices, query patterns, behavior); quality can drop with zero code bugs. Agreed retraining triggers in advance: shift in query structure, search quality drop, new categories, major behavior change. The managed object is the whole AI system: data, model, infra, monitoring, business result.

## PM takeaways
- Reject hypotheses with data; each rejection narrows the search.
- Watch for biased training signals (position bias from the existing system).
- Compare rules / semantic / LLM / hybrid on value, latency, cost, explainability, scalability — not hype.
- Expect offline gains to shrink online; plan for a second iteration and retraining triggers.
- PM's job: reduce uncertainty step by step, linking business problem, user value, data, AI tech and product result.
