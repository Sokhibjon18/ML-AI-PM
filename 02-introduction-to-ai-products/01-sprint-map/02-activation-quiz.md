# Activation Quiz

> Module: Introduction to AI Products · Topic: Sprint Map · Lesson 2
> Original: «Активирующий квиз»

## Key ideas
- An ungraded warm-up quiz testing how much classic product intuition transfers to AI products, and where it misleads.

## Concepts tested and the principles behind correct answers
- **What makes a product ML-based:** the decision relies on a pattern **learned from data**, not one a developer coded explicitly. Personalization, complex logic or large data volumes alone don't prove ML — they can all be rule-based or plain databases.
- **Rule vs model:** use a rule when you need certainty (e.g., block transfers above a fixed amount). Recommendations, "people you may know" and fraud-pattern detection are probabilistic and trained on data — fraud patterns change too fast for a finite rule list.
- **Three criteria for choosing a model over a rule** (detailed in Topic 4): enough data; a complex, non-obvious pattern; business tolerance for some error rate. Time-to-market is a project constraint, not a criterion for whether ML is needed.
- **Model behavior in production:**
  - Strong results on historical data don't guarantee the same on live users; one backtest is not enough to ship — validate on real traffic.
  - Models can silently degrade after launch with zero errors in logs; only quality metrics reveal it.
  - Choose the quality metric based on the **business cost of each error type**, not by default.
  - Acceptance is not "works / doesn't work" but "how well, and at what cost".
- **Offline vs online gap:** live uplift being noticeably smaller than historical uplift is the rule, not the exception — historical data is skewed by the company's past decisions. Plan for online testing; this is not a reason to distrust models.

## PM takeaways
- Treat ML acceptance as a quality/cost trade-off measured continuously, not a one-time pass/fail.
