# Three Lifecycle Stages

> Module: Managing AI Product Development · Topic: AI Product Lifecycle: PoC to Production · Lesson 1
> Original: «Три стадии жизненного цикла»

## Key ideas
- AI product development is a sequence of decisions that each reduce uncertainty, not one big build: **PoC → baseline → production readiness**. Each stage answers a different question and ends in a go / change / stop decision.
- A model result means nothing without comparison: 84% correct routing vs a keyword baseline at 82% (cheaper, maybe partial coverage) tells a very different story.
- Stopping a weak hypothesis or keeping the baseline as the permanent solution is a legitimate, resource-saving product outcome.

## Concepts & definitions
| Stage | Main question | Output | PM decision |
|---|---|---|---|
| PoC | Is it technically feasible on available data? | Prototype + binary "works / doesn't" | Continue, change approach, or stop |
| Baseline | How well does a simple approach already solve it (metrics), and what improvement must a model add? | Simple reproducible solution + its metrics | Keep baseline or invest in complexity |
| Production | Can the system run stably, safely and efficiently in the real product? | Integrated system with monitoring and ops plan | Launch, limit scope, or send back |
- Stages can overlap in time (baseline can be prepared alongside PoC), but measurable comparison starts after feasibility is confirmed. A complex prototype can still be "just a PoC"; a simple solution can become permanent.
- **Proof of Concept (PoC)** — limited experiment (historical data, small sample, outside the product; notebooks, manual prep and temporary code are fine) proving the input→output chain works. Quality, latency and comparisons are *not* judged here.
- **Baseline** — simple rules, current manual process, or a simple ML model; evaluated on quality, coverage, build time, maintenance cost. In the lifecycle it also sets the bar to beat, stays as a fallback, and can serve simple cases while the model handles hard ones.
- **Shadow mode** — new model receives real requests in parallel with the current system; its outputs are logged, not used. Checks technical readiness (data arrives, latency, timeouts, segment errors) — cannot measure product impact; that needs real traffic, e.g., an A/B test.
- **SLO (Service Level Objective)** — agreed latency, availability, coverage and failure behavior.

## Frameworks / methods

### PM on PoC
- Formulate a testable hypothesis (e.g., "a prototype can take ticket text and return one of the defined topics").
- With the Data Scientist, scope: inputs and formats, history period, excluded rare scenarios, allowed simplifications.
- Fix up front: success criterion (end-to-end scenario works on test examples), time and budget, and what to do under each outcome — prevents endless "just a bit more tuning".
- Don't choose model architecture for the DS.
- Risks: a badly framed experiment (hand-picked examples, hidden manual steps) mistaken for feasibility; skipping PoC pushes technical uncertainty into the most expensive stage; treating PoC as a product turns shortcuts into failures; drawing quality or business conclusions from a PoC.

### PM on baseline
- Fix the baseline version and comparison conditions: sample, period, classes, metric calculation; define the **minimum improvement** worth a more complex model; check key segments (averages hide failures on rare topics).
- Decide baseline's future role: main solution for simple cases, or fallback when the model is down / low-confidence.
- Four metric groups, measured on the same sample and conditions as the future model:
| Group | Measures | PM question |
|---|---|---|
| Quality | Correct decisions, error types, per-segment quality | How good, which errors are critical? |
| Coverage | Share handled without manual help | How much is already automated? |
| Product | Response time, redirect rate, queue load | Effect on user path and ops? |
| Economics & ops | Build/support cost, latency, availability | Does the model's extra effect justify the extra system cost? |
- Example: keyword routing handles 65% of tickets, 86% of those routed correctly, 12% redirected. Model requirements set in advance: coverage ≥80%, quality ≥86%, redirects ≤8%, plus latency/cost limits (real thresholds come from error costs, economics, tech constraints).
- Move to production prep when results reproduce on held-out data, thresholds vs baseline are met, key segments hold, and cost/speed limits are met.
- Main risk: too weak a baseline (beating random proves nothing) or an outdated one; version it and recompute on new data.

### Production readiness
| Area | Experiment | Before production |
|---|---|---|
| Data | One-off historical sample | Regular inflow with format/quality checks |
| Model | Offline metric | Versioning, reproducibility, latency requirements |
| Integration | Computed outside product | API or batch pipeline, fallback, error handling |
| Evaluation | Technical quality criterion | Product experiment + guardrail metrics |
| Operations | Manual result checks | Monitoring, alerts, incident owner, rollback |
- Support example: model gets messages via API, returns topic within allowed time to the queue system; low confidence or outage → general queue or baseline rules.
- PM: fix the user scenario and where the output is used; agree SLOs with engineers; set product and guardrail metrics (time to first response, redirect share, queue load, misroutes); assign owners for data, model, integration, incidents.
- Gradual rollout: internal testing → shadow mode → small % of traffic → A/B test → scale; rollback conditions and guardrail stop-thresholds defined before real traffic.
- Readiness is judged on the whole set: great offline metrics don't save an unstable/expensive service without fallback; a stable system doesn't justify launch if the experiment shows no effect.

### Stage transition criteria
| Transition | Go when… | Stop when… |
|---|---|---|
| Idea → PoC | Data and means to test exist; scope, time, budget, works/doesn't criterion defined | Can't test on available data, or test costs more than expected value |
| PoC + baseline → comparison/decision | Reproducible; model meets thresholds on quality, coverage, segments, speed, cost; baseline's ceiling understood | Baseline is good enough, or model fails a critical constraint |
| Prep → launch | Quality and reliability requirements met; monitoring, fallback, rollback in place | Critical risks remain or launch results can't be safely assessed |

## PM takeaways
- Common mistakes: building the full system immediately and reading a good PoC as a promise of business impact; not fixing baseline metrics or moving the comparison goalposts after results; launching without monitoring, fallback and an incident owner.
- PM's job across stages: tie technical experiments to product value and pre-define go/stop criteria, so AI work doesn't become an endless chase for model quality.
- Next lesson applies this lifecycle to the Yarmarka case.
