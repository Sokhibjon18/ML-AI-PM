# Practice: Find the AI Component and Build the Scheme

> Module: Introduction to AI Products · Topic: What Is AI and AI Products · Lesson 4
> Original: «Практика: найти ИИ-компонент и собрать схему»

## Key ideas
- Three exercises, all about looking at **how a decision is made**: is there a trained model, what data, what output, how is quality measured.

## Concepts tested and principles behind answers
**Task 1 — AI product vs ordinary logic**
- Ordinary logic: a fixed schedule (same reminder to everyone at 9:00), sorting by an explicit field (price), filtering by a filled field (discount > 0%).
- AI: ETA prediction from order history, district, time of day and load; bank risk scoring trained on past fraud/legit operations; spam folder routing **if** a trained model decides (a banned-word list would be a rule).
- Principle: a model generalizing from historical examples to new cases = AI; explicit conditions, schedules, sorts and filters = rules.

**Task 2 — data → model → metric (streaming recommendations)**
- Case: a streaming service shows everyone the same popular titles; wants personal picks based on viewing history and similar users.
- **Data:** viewing history, likes/dislikes, completions, skips, searches, genres, behavior of similar users.
- **Model:** estimates how likely a given user is to be interested in each title → personal selection.
- **Metric:** recommendation CTR, completion rate, watch time after recommendation, conversion to viewing, plus negative signals (hides, complaints).

**Task 3 — spot the data problem (courier ETA)**
- Model trained on summer orders; winter brings snowfall, heavier traffic, slower districts, and an ad campaign spikes demand.
- Correct issues: no training examples with snow/winter traffic → underestimated ETAs; no high-demand periods → courier load not captured → overly optimistic forecasts.
- Wrong answers: label errors (no evidence given); "too much data" (volume alone doesn't hurt quality).
- Principle: **non-representative / outdated data** (seasonality, demand shifts) degrades a model after conditions change.

## PM takeaways
- Classify by decision mechanism, not by how "smart" the feature looks.
- Include negative user signals in recommendation metrics, not just clicks.
- Ask whether training data covers upcoming seasons, events and load spikes before trusting forecasts.
