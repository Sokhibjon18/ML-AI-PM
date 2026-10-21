# AI Canvas: Structure and Purpose

> Module: ML Product System Design · Topic: AI Canvas Framework · Lesson 1
> Original: «AI Canvas: структура и назначение»

## Key ideas
- A business problem ("we need recommendations") is too vague to hand to engineering: it doesn't say what data is needed, what the system outputs, how it's used, who benefits, who builds it, cost, or how it makes/saves money. These answers are interdependent.
- The course's AI Canvas (9 blocks, same as the "AI Project Canvas") puts these **assumptions** on one page so the team can see whether they form a coherent concept. It is not proof the project will work.
- **A Canvas's quality = consistency between blocks, not the number of filled fields.** Ask for every change: "if this block changes, what else must be revisited?"
- Sticky-note color code tip: yellow = assumption, green = confirmed, red = refuted. Gaps become the next Discovery iteration.

## Concepts & definitions
Nine blocks grouped into four questions:
1. **What value, for whom**
   - **Value Proposition** — the useful change, not the technology. Bad: "implement a recommender on the homepage". Good: "help buyers find suitable items faster and raise the share of search sessions ending in purchase".
   - **Customers** — who gets the final value (not just who sees the model output). Yarmarka: buyers first, sellers indirectly (long-tail items become visible).
2. **What's needed and what result**
   - **Data** — working hypothesis of required data (catalog, impressions, card clicks, add-to-cart, purchases, visit context). Must still verify collection, completeness, cold start for new users/items. A concept that doesn't explain its data has a serious gap.
   - **Skills** — competencies to build and run it (PM, recsys/ranking, data prep, analytics & experiments, app dev, domain knowledge of catalog/homepage, post-launch model support). Value: spot missing competencies that change cost, timeline or approach.
   - **Output** — three levels, keep them separate:
     1. what the system emits (risk score; ordered item list; detected empty shelf spots) — not a metric;
     2. how good that output is (quality criterion);
     3. what changes in product/business (fewer defaults, more orders, faster shelf restock).
     Chain: output → quality of output → product/business metric.
   - No separate field for model type — record and justify it in the memo.
3. **How it becomes part of the product**
   - **Integration** — where the output lands and who/what acts on it. Same model, different products: human-curated vs fully automatic homepage. Integration drives latency needs, human-in-the-loop, error tolerance, process design, dev/ops cost. PM doesn't design APIs but must own the AI component's place in the process.
   - **Stakeholders** — those who influence decisions, constraints, resources (product, homepage team, DS/ML, analysts, devs, catalog/data quality, seller team, infosec, legal for personal data). Different from Customers, who receive value.
4. **Does it make economic sense**
   - **Cost** — data collection/prep, labeling, team, compute, storage, external models/APIs, rollout, QA, post-launch support. Identify main cost sources; e.g. saving 5M/yr while costing 10M/yr kills the concept regardless of model quality.
   - **Revenue** — broader than direct revenue: more sales, lower costs, prevented losses, freed staff time, fewer manual operations. Describe the mechanism now; calculate later.

## Frameworks / methods
- **Fill order used in the course:** Value Proposition → Customers; Stakeholders; Data → Skills → Output; Integration; Cost → Revenue.
- **Typical inconsistencies to hunt for:**
  - Value vs Integration: promise 1-minute replies but require manual operator approval of every AI answer.
  - Data vs Customers: multi-country users, Russian-only training data.
  - Revenue vs Cost: "reduce manual work" but need a large team to check every model output.
- **Tool comparison:** Lean Canvas = is the business idea viable (problem, segments, channels, revenue); AI Canvas = does it form a coherent AI product concept (data, output, skills, integration, stakeholders); PRD = detailed functions, behavior, constraints, success criteria of the product being built. Complementary, typically in that order.
- Blocks unique to other canvases (not in AI Canvas): Customer Segments, Channels, Key Partners, Problem.

## Case / example
- Yarmarka: 3.1M SKUs, 71% of turnover from 4% of the assortment; homepage almost identical for everyone (only city banners differ). Team considers homepage/collection personalization.
- Trap: "launch recommendations → conversion grows" skips the system output. Correct levels: ordered item list → its quality → change in buyer behavior and economics. Higher clicks on the collection don't prove more orders (users may just move from search to the collection and buy the same things). All links are hypotheses to test experimentally.
- Always define the **baseline** (e.g. current popularity-in-category ranking); personalization must show value on top of it.
- Quiz principle: "raise search-to-purchase conversion with ML" or "raise share of orders after homepage view" in Output → first clarify what the system itself outputs.
- Shelf-CV integration questions: where staff receive the alert, what they do next, how completion feeds back. (Training photos and labeling cost belong to Data/Cost; process ownership to Stakeholders.)

## PM takeaways
- Leaving open questions after a first pass is a useful outcome — it shows what to find out.
- Never write the solution in Value Proposition or a business KPI in Output.
- Revisit the Canvas after each later topic (problem framing, data, ML task type, metrics, requirements).

## Further resources
- [AI Canvas template (Holst)](https://app.holst.so/board/025508b4-9331-4856-9e94-c38014a5dd0b) — the 9-block template used in the course.
- [Machine Learning Canvas, Louis Dorard](https://github.com/louisdorard/machine-learning-canvas) — alternative, more data/prediction/evaluation-focused.
- [Canvanizer](https://canvanizer.com/) — tool for building canvases (AI Canvas must be created manually).
- [Lean Canvas, LEANSTACK](https://www.leanstack.com/lean-canvas) — primary source on Ash Maurya's Lean Canvas.
- [Atlassian: product requirements](https://www.atlassian.com/agile/product-management/requirements) — purpose and contents of a PRD.
- [Business Model Canvas, Strategyzer](https://www.strategyzer.com/library/the-business-model-canvas) — official BMC material.
- [Introducing the AI Project Canvas](https://medium.com/data-science/introducing-the-ai-project-canvas-e88e29eb7024) — explains virtually the same 9-block structure; most useful extra read.
- Books: Maurya *Running Lean* (3rd ed.) — canvas as a working hypothesis tool; Cagan *INSPIRED* (2nd ed.) — discovery over premature specs; Haines *The Product Manager's Desk Reference* — deep dive on PRDs; Ameisen *Building ML Powered Applications* — idea to product; Huyen *Designing ML Systems* — technical system design; Osterwalder & Pigneur *Business Model Generation* — origin of one-page linked canvases; Agrawal, Gans, Goldfarb *Prediction Machines* — AI as decision economics, their own AI canvas.
