# Problem Space vs Solution Space: How a PM Makes Decisions

> Module: ML Product System Design · Topic: Problem and Solution Space · Lesson 1
> Original: «Problem Space vs Solution Space: как ПМ принимает решения»

## Key ideas
- Most AI products fail not because of a bad model but because the team decided too early that it knew the solution.
- PM's core skill: find the cause of the business problem by **managing the order in which the team makes decisions**: symptom → cause → validated solution → implementation.
- AI adds uncertainty on top of classic product risk: even with a correctly framed problem, there's no guarantee an AI approach reaches the needed quality.
- Key question: "Which specific problem are we solving, and is AI really the best way to solve it?"

## Concepts & definitions
- **Business signal / symptom** — an observable change in user behavior, metrics or product performance that hints at a problem but doesn't explain its cause (e.g. 31% of telemedicine patients drop off; long tail gets no traffic; project review SLA met only 71% of the time). Business often brings guesses dressed as problems ("sales fell — it's the website"; "add an AI chat for negative reviews").
- **Problem Space** — *what* must change to remove the symptom. A good problem statement has three parts: **current situation, measurable pain, specific audience** (maps to the Value section of AI Canvas).
- **Solution Space** — *how* best to solve the defined problem: widen options, test hypotheses, pick the one with the best balance of value, feasibility, cost and risk.
- **Double Diamond** — a map of *questions*, not a task pipeline: Discover (widen problem understanding) → Define (narrow to one problem; checkpoint "do we really understand it?", else back to Discover) → Develop (widen solution options, avoid fixating on a favorite/hyped one) → Deliver (choose and prove the solution works). Transition to Solution Space only when the problem is stated with a measurable outcome (user, context, cause) and the team knows *what* it is changing.
- **"Triple Diamond" / Impact Space** — informal extension (no standard): after launch, ask whether the solution creates the expected effect in the real world — quality retention, behavior change, data drift, retraining, cost vs value. AI products are never "build and forget".

## Frameworks / methods
**AI-specific filters between problem and solution:**
1. Quality can't be guaranteed up front — a human expert or a simple algorithm may beat months of model work; validate quality under real feasibility, not just value.
2. Data is part of the product, not a tech detail — ask early "does the data exist and will it let this idea work?"
3. AI isn't always best — e.g. form-filling errors are better fixed by UI/steps.

**Four conditions for AI to create results:** enough quality data? no simpler solution? reliable technical solution possible? cost justified for the business?

**PM anti-patterns:**
- "Let's add AI" — solution before problem (LLM chat for falling conversion with no diagnosis).
- AI for AI's sake — measuring presence of tech, not value.
- "Accuracy went up, so the product improved" (85% → 90%, no business effect).
- "Build the model, find the data later" — skipping AI feasibility (demand forecasting with thin, low-quality history).
- "We already know the problem" — research only confirms the first idea; ignores price, assortment, UI, slow checkout.
- "More complex = better" — agents and multiple models where automation or process change suffices.
Common root: managing the technology instead of the solution.

## Case / example
- Yarmarka signal: 71% GMV from 4% SKU; 18% of search sessions end with zero clicks. Business proposes per-user recommendations. Risk: a working system that raises collection CTR but not orders, or just steals traffic from search.
- Problem statement: situation (3.1M items, 71%/4%, 18% no-click searches); pain (most items barely found → sellers lose sales, platform loses orders and commission); audience (buyers who can't find suitable items; sellers of new/niche items). AI not yet decided.
- Discover hypotheses: users can't find relevant items; search fails on some query types; hard to compare many offers; new items lack impressions; users actually expect personalized collections. Define: focus on product discovery — much of the catalog gets no traffic and the homepage doesn't help find new relevant items. Question: how to help users find relevant items while increasing long-tail coverage?
- Develop options: better search algorithm, category navigation, popular collections, personalized homepage, ML recommendations. Deliver: compare vs a quick popular-in-category baseline; judge by orders, conversion, GMV per user, extra commission — not only CTR (cannibalization risk).
- Feasibility findings: most historical signals were produced by the old recommender; new/rare items have almost no history → cold start on both user and item side; need cold-start strategies whose interactions become new data.
- Economics example: forecasting with 5% error is fine for long-shelf-life goods but critical for perishables; a solution saving 2M ₽/yr that costs 3M ₽/yr is feasible but not worthwhile.
- Quiz principles: researching behavior to find causes = Problem Space / Discover; proposing an ML model = Solution Space; having investigated and formulated a concrete problem = Define.

## PM takeaways
- Treat "we need personalization" as one hypothesis among search, navigation and simpler algorithms.
- Prove the problem, then check AI applicability, then pick technology.
- The costliest AI mistake: a technically perfect system for a problem that creates no value. The PM's job is asking the right questions in the right order.
