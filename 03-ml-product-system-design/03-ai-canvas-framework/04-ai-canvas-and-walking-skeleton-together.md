# AI Canvas and Walking Skeleton Together: Checking the Whole Concept

> Module: ML Product System Design · Topic: AI Canvas Framework · Lesson 4
> Original: «AI Canvas и Walking Skeleton: как проверить концепцию ИИ-продукта целиком»

## Key ideas
- **AI Canvas checks completeness and consistency; Walking Skeleton checks end-to-end logic.** Together they surface contradictions before detailed design.
- Canvas doesn't guarantee right answers — it makes them visible and linked. Honest open items ("need to check if 2 years of history is enough", "unknown whether real-time is needed") beat false certainty.
- WS doesn't show cost, team, legal limits, data completeness or quality requirements (Canvas covers those), but quickly catches: unclear input, undefined output, unused output, unclear next action, action not tied to value.
- Treat the first Canvas as a **rough decision map**; change it as later topics refine problem, data, model, metrics, requirements. Don't defend the original Canvas.

## Frameworks / methods
**Four-step concept check:**
1. **Completeness** — does each block have at least a working hypothesis? ("2 years of application history; need to check sufficiency and outcome completeness" beats "big customer data".)
2. **Consistency** — five links:
   - Data ↔ Output (personalize for new buyers using only past purchases = gap)
   - Output ↔ Value (more clicks on the collection, flat orders = cannibalization from search)
   - Integration ↔ Value (promise per-visit personal order but list refreshed once a day)
   - Customers ↔ Data (data matches the users and situations served?)
   - Cost ↔ Revenue (spot potential contradiction, no exact calc needed)
3. **Write WS as one sentence:** "The system receives ___, [AI action], outputs ___, the result goes to ___ and is used for ___." If you can't finish it, you found a team question. "Data → AI → recommendations" is not a WS. Prefer an explicit question ("what do we show a new buyer?") over hiding it behind "personalization".
4. **Return to the Canvas** — fix what WS revealed.

**Which tool first:**
| Unclear… | AI Canvas | WS |
|---|---|---|
| Value created | Yes | No |
| Data needed | Yes | Yes |
| Model output | Yes | Yes |
| Who uses the output | Yes | Yes |
| What happens after output | Partly (Integration) | Yes |
| Skills / cost | Yes | No |
| How output flows through the system | Partly | Yes |
| Where economic effect comes from | Yes | Only as extra check |

Quiz principle: model known but consumer unknown → WS first; flow known but cost/skills not discussed → Canvas; gaps in both product composition and flow (e.g. new-SKU data, output, consumer all unclear) → both.

**Definition-of-Done checklist**
- Canvas: value, customers, data hypothesis, system output, skills, place of use, stakeholders, cost sources, effect source.
- Consistency: data can produce output; output linked to VP; integration delivers promised value; data fits users/situations; economic effect tied to a real process change.
- WS: input, AI function, output, consumer, next action, action linked to value.

## Case / example
- Yarmarka Canvas summary: Data (catalog, impressions, clicks, cart, purchases, visit context; check completeness and cold start); Skills (product, analytics, data, ML, dev, experiments, ops); Output (ordered list per block); VP (help buyers find better items, boost long-tail discoverability); Integration (ranking computed on homepage open, used by collection UI); Stakeholders; Customers (buyers, indirectly long-tail sellers); Cost; Revenue (possible extra GMV and commission vs baseline, net of cannibalization).
- WS: buyer & item data → ranking model → ordered list → homepage → buyer sees collection.
- Test: a craftsperson's new item with no history. If the model relies only on interactions, the new SKU never surfaces — contradicts the long-tail goal. Revisit Data (signals for new items), Output (rules when history is sparse), Integration (fallback on homepage), Skills, Cost, VP.
- Quiz: VP = long-tail discoverability, Data = per-SKU interaction history → main finding: a new SKU lacks a core input signal. Not "need a more complex model", not "drop personalization", not "add the algorithm name".
- Concept-level options: identify attributes available for new items immediately, define a fallback list, revise data assumptions, check model vs VP, or admit the concept doesn't cover part of the problem.

## PM takeaways
- Practice for the Stupen project: (1) hypothesis or question in all 9 blocks; (2) check the five links, flag at least one item to verify; (3) write WS in one line, leaving "?" where unknown; (4) compare WS with Canvas and note fields to refine. It's a draft that shows what's missing — not the final answer.
- Core lessons of the topic: an AI product is wider than the model; Canvas = map of linked hypotheses judged by consistency; WS = minimal data → model → consumer, extended with output and action; both are iterative and feed each other.
- Next topic: problem space vs solution space.

## Further resources
- [AI Canvas template on Miroverse](https://miro.com/templates/ai-canvas/) — original 9-block template.
- [Start with a Walking Skeleton — 97 Things…](https://www.oreilly.com/library/view/97-things-every/9780596800611/ch60.html) — original engineering idea.
- [Rules of Machine Learning (Google)](https://developers.google.com/machine-learning/guides/rules-of-ml) — get the end-to-end chain stable before adding complexity.
- [ML Pipelines (Google)](https://developers.google.com/machine-learning/managing-ml-projects/pipelines) — overview of ML system pipelines, inputs and outputs.
- [Experiments (Google)](https://developers.google.com/machine-learning/managing-ml-projects/experiments) — early end-to-end process speeds product integration.
- [Hidden Technical Debt in ML Systems](https://research.google/pubs/hidden-technical-debt-in-machine-learning-systems/) — complexity around the model.
- Books: Ameisen *Building ML Powered Applications*; Huyen *Designing ML Systems*; Freeman & Pryce *Growing Object-Oriented Software, Guided by Tests*.
