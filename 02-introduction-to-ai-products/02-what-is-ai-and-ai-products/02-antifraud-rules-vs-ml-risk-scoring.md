# Antifraud: Rules vs ML Risk Scoring

> Module: Introduction to AI Products · Topic: What Is AI and AI Products · Lesson 2
> Original: «Антифрод: правило и ML-скоринг риска»

## Key ideas
- To the user, both approaches look the same (payment held, please confirm). The difference is internal: how the system decided the operation was suspicious.
- Business goal: cut fraud losses while not making clients constantly confirm safe payments.
- When many signals interact (amount, time, country, device, client type, behavior history, similar fraud cases), ML scoring is the stronger hypothesis; rules remain useful for obvious scenarios.

## Concepts & definitions
- **Rule set** — hand-written conditions, e.g., amount > 100,000 RUB **and** new country for this client → extra check. Transparent, fast to launch, easy to explain to support/legal/ops. Weak against adaptive fraud: splitting amounts, using familiar countries, changing devices, mimicking normal behavior.
- **ML risk scoring** — a model trained on past operations with their verified outcome (legit/fraud) outputs a risk score (0–1) for a new operation; product logic then maps it to approve / confirm / manual review. Can weigh tens or hundreds of signals at once.

## Frameworks / methods
**Comparison (condensed):**

| Question | Rules | ML scoring |
|---|---|---|
| Decision basis | Predefined conditions | Model trained on past operations |
| Data needed | Few fields to check conditions | Operation history, features, verified outcome |
| Product receives | Rule fired: hold or pass | Risk probability / score |
| Pros | Transparent, quick, explainable | Many signals, adapts to complex patterns, tunable thresholds |
| Risks | Heavy manual upkeep, slow to react to new schemes | Data/label dependence, metric & monitoring quality, poor explainability of individual scores |
| Fits when | Simple known threat, little data, need fast control | Lots of labeled history, complex patterns, high cost of error |

## Case / example — regulatory and legal layer
- In antifraud, regulation mostly sets **boundaries**: how suspicious operations are checked, decisions recorded, data stored, client rights protected. The bank designs the model itself and owns its quality.
- **Credit risk is stricter:** to use internal models for capital adequacy, the regulator reviews methodology, data and model quality — internationally Basel III; in Russia the Bank of Russia's IRB approach (incl. Regulation No. 483-P), which requires regulator approval.
- A credit model may rank borrowers well for business purposes, but for regulatory use the team must prove valid data, documented methodology, and regular quality checks.

## PM takeaways
- Legal/regulatory requirements are a source of product rules, quality criteria and constraints: the model must be useful, measurable **and** permissible — not just maximize a business metric.
- Consider explainability, decision logging, internal validation and audit from the start in regulated domains.
- Rules and ML are not mutually exclusive: use rules for clear-cut threats, ML for multi-factor patterns.
