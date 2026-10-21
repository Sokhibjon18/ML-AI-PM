# AI Canvas in a Complex Case

> Module: ML Product System Design · Topic: AI Canvas in a Complex Case · Lesson 1
> Original: «AI Canvas в сложном кейсе»

## Key ideas
- When one user journey sparks several AI ideas, resist bundling them into one "AI assistant": if outputs, consumers and downstream actions differ, they are **separate hypotheses with separate Canvases**.
- Find the break in the funnel, ship a simple **baseline** first, then aim ML where the baseline is weak.
- Historical user choices are not ground truth when wrong choices *are* the problem — expert labeling is needed.
- Metric choice follows error cost and the output format (a short list).

## Case / example
**"Puls" telemedicine** — video consultations. AOV 2,400 ₽, 30% commission = 720 ₽ per paid consult. 1,800 doctors, 22 specialties, 460 regularly active. 200K users/month enter booking funnel, 48K pay (24%), ~34.6M ₽ commission/month.

Funnel: complaint 200K → specialty chosen 138K (69%) → doctor card 84K (61%) → checkout started 61K (73%) → paid 48K (79%). Biggest loss: 62K (31%) drop at specialty choice. Additionally 31% of consultations start with the wrong specialist; 19% of those end in refunds; NPS of that group −22. Search by complaint doesn't help ("stomach ache" → gastroenterologist, GP, surgeon, gynecologist — user still must choose).

**Part 1 — Two separate metrics:** (1) share choosing a specialty after the complaint; (2) share of consultations starting with an unsuitable specialist. Growth of (1) is only useful if (2) holds or improves. Note: some referrals are clinically valid diagnostic paths — must be separated from genuinely wrong routing (matters for labeling and offline eval).

**Part 2 — Baseline in 3 weeks:** PM + chief physician build a question tree for frequent complaints (where, how long, accompanying symptoms) based on clinical guidelines → suggests one or several specialties; covers ~60% of typical cases. Baseline's two roles: solves part of the problem cheaply; reveals where rules break down (tree bloat, ambiguity, maintenance cost). Track coverage, quality (expert agreement), build time, maintenance cost. ML's first target = cases where the tree is weak (ambiguous complaints, varied wording). A baseline can also be a very simple ML model.

**Part 3 — Two AI tasks in one journey:** second idea — ~7 of 21 consultation minutes go to repetitive history-taking. Extended Walking Skeleton (data → model → output → consumer → action):
| Hypothesis | Chain |
|---|---|
| Specialty matching | complaint + clarifying answers + context → ranking of specialties → short prioritized list → Puls UI → user picks specialty |
| Pre-consultation history taking | complaint + answers → dialogue model → structured summary + follow-up questions → doctor → reads summary, clarifies gaps |

Canvas 1 (matching) highlights: value = faster, more accurate specialty choice; boundary = no diagnosis or treatment; stakeholders incl. chief physician, doctors, ML/data, legal, infosec, health regulators; output = short ranked list, multiple clinically acceptable specialties allowed; integration = after complaint, before doctor catalog; costs incl. de-identification, expert labeling, inference, offline eval, monitoring; effect must be proven vs baseline.
Review changes: expert labeling becomes the offline-eval gold standard; historical choices and referrals are auxiliary signals only; cost includes label review, dispute resolution and updates; experts' role clarified; evaluation must allow several valid specialties. Walking Skeleton structure unchanged.

Canvas 2 (history taking): value = save doctor time, richer consult start; customers = doctors and patients; data = dialogue answers, agreed question list, clinical guidelines; output = structured summary + items to clarify; doctor makes all medical decisions. Economics check: 21 → 17 min ≈ +23% capacity in theory, but money appears **only where schedules are already full** and consult time is the bottleneck — check load by doctor and hour (Cost ↔ Revenue link verified pre-development).

A merged "AI assistant" Canvas would mix two outputs, two customers (user vs doctor) and two revenue mechanisms (conversion vs time savings), losing the output→effect link. Separate Canvases allow launching one even if the other fails.

**Part 4 — Effect sizing and metric:**
| Source | Calculation | ≈ per month |
|---|---|---|
| Step-1 conversion 69% → 72% | 200K × 3 pp = 6K; × 34.8% (48K/138K) ≈ 2,088 payments; × 720 ₽ | 1.5M ₽ |
| Wrong-specialist share 31% → 20% | 48K × 11 pp = 5,280; × 19% ≈ 1,000 refunds avoided; × 720 ₽ | 0.7M ₽ |
| Total | | ≈ 2.2M ₽ |
Order-of-magnitude estimates with assumptions still to verify.

- **Recall** as primary: of 100 expert-labeled complaints, correct specialty in the shown list for 88 → recall 88%. A miss leaves the user without help (drop-off or wrong choice); an extra but clinically acceptable option is cheaper.
- **Precision as guardrail** + max list length in product requirements — otherwise recall is gamed by showing 8 specialties, recreating the choice problem.
- **Accuracy fits poorly**: multiple valid specialties per complaint; single-label scoring would count a useful list as wrong, and it doesn't show complete misses directly.
- Three-level chain: system output (ranked list) → output quality (recall/precision) → product effect (step conversion, wrong-route share, money). Improvement at one level doesn't guarantee the next.

## Frameworks / methods
Team path, each step removing one uncertainty: funnel locates the loss → baseline gives an honest alternative → Walking Skeleton splits the broad assistant into two hypotheses → Canvas review exposes data and economics limits → metric choice gets a concrete product rationale.

Next steps for matching: build expert gold labels, fix offline-eval protocol and baseline, agree UI requirements → build v1 → offline comparison vs baseline → if thresholds met, live experiment on two product metrics (step-pass rate, wrong-specialist share).

## PM takeaways
- Baseline first; measure ML's value relative to what already works.
- Split ideas by output, consumer and action — not by proximity in the journey.
- Check that time savings actually convert to revenue (capacity constraints).
- Pair a primary metric with a guardrail and a hard UX constraint (list length).
- In high-stakes domains, define the system's boundary explicitly (routing, not diagnosis).
