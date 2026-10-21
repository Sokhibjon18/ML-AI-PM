# Guide: Writing the Explanatory Note

> Module: ML Product System Design · Topic: Requirements and Final Synthesis · Lesson 3
> Original: «Гайд по написанию пояснительной записки»

## Key ideas
- Produce your own note: a coherent document with **five justifications** that complements the AI Canvas and Walking Skeleton.
- Work step by step in the template; only revisit earlier data/model/metric decisions if you find a contradiction or missing grounds.
- Don't copy Yarmarka's rules/thresholds without justification; if data is missing, record the question and how you'll find the answer, and keep going.

## Frameworks / methods
Inputs: case description, filled AI Canvas and Walking Skeleton, conclusions on data/model/metrics, requirements from lesson 1, the template, the Yarmarka example.

12 steps:
1. **Transfer context** into sections 1 (problem: concrete issue, current limits, 1–2 sourced facts), 2 (user, situation, system output, end consumer), 3 (value mechanism, major costs, metric with baseline/target/period if known), 6 (v1 scope, exclusions, constraints). Separate facts from assumptions; no generic "AI is relevant" text.
2. **Justify using AI** (section 4, 2–3 sentences): what part ML improves, vs what non-ML alternative, why gain covers extra cost; fallback to simpler option if not.
3. **Justify data and labeling** (section 4): sources, purpose, availability, how labels are obtained; distinguish training data, runtime inputs and evaluation data; don't present wished-for data as available.
4. **Justify model type**: chain *decision → basis → alternative/trade-off → limitation and check*; name an algorithm only if its choice is already justified.
5. **Describe use of model output**: inputs → output → consumer system; consistent with section 2 scenario; no detailed tech stack.
6. **Transfer requirements** (section 5) with IDs, conditions, checks; add new edge cases. "Eligible for selection" must not drift into "guaranteed exposure"; NFR numbers justified or marked preliminary with a refinement path.
7. **Justify metrics** (start of section 8): via error consequences; reference NFR IDs instead of repeating thresholds; compare with alternatives (e.g., vs CTR); describe how PM/data science/engineering agree acceptable quality; state trade-offs and applicability limits.
8. **Risk mitigation** (section 7): for each key risk — cause, consequence, mitigation, verification, residual uncertainty; for assumptions — who validates and what outcome triggers a rethink. Vague "improve the data" must be made specific.
9. **Verification plan** (end of section 8): (a) requirements testing — test data/scenarios with requirement IDs; (b) business-goal testing — baseline, effect metrics, conditions to revisit. Keep model-quality and business-effect checks separate; lack of significance ≠ proof of no effect.
10. **Resolve contradictions** across note, Canvas, Skeleton and requirements by tracing one main and one edge scenario (e.g., "personalization on first visit" vs schema using only purchase history → fix logic or justify another data source; update all docs).
11. **Attach artifacts** (section 9): Canvas and Skeleton with version and date (page/tab if same file); links readable; links don't replace textual justification.
12. **Prepare for hand-off**: remove template hints; 8 content sections + section 9; ≤1,000 words of content (metadata, links, attachments excluded). Cut repetition, not alternatives or open questions.

Five justifications map:
| Justification | Section |
|---|---|
| Why AI | 4 |
| Why these data and labeling | 4 |
| Why this model type | 4 |
| Why these metrics / trade-off | 8 (refs 5) |
| Key risks and mitigation | 7 |

Final self-check: problem, audience, goals, scope described; all five justifications present; facts/hypotheses/assumptions separated with sources; FRs cover inputs/outputs/edge cases and NFRs are measurable; metric choice tied to product consequences with limits; each risk has mitigation and check, open questions have a path; all documents consistent; artifacts attached with versions/dates; ≤1,000 words, hints removed.

## Case / example
Yarmarka sample phrasings used as hints: v1 covers home page only but search order shifts are counted in overall effect; bestsellers ignore individual interests — revert to simpler option if gain doesn't cover costs; impressions distinguish "not shown" from "no reaction"; FR-03 eligibility for items without history doesn't guarantee impressions; continue the experiment if data is insufficient.

## PM takeaways
- Every justification should name an alternative and a check that could overturn it.
- Consistency across artifacts is part of the deliverable, not an afterthought.
- Note which justification was hardest — a good topic for your mentor.
