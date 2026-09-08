# Security Feature Engineering — Instructor Walkthrough

**Companion notebook:** `../notebooks/security-feature-engineering.ipynb`  
**Suggested time:** 75–90 minutes  
**Teaching goal:** Turn raw security telemetry into point-in-time-correct, testable model evidence.

> All events are synthetic. A review score prioritizes analyst attention; it does not prove malicious behavior.

## Before class

1. Restart the kernel and run all cells in order.
2. Keep the Week 3 slides open beside the notebook.
3. Write the decision, prediction time, and unit of prediction where students can see them.
4. Ask students to predict outputs before revealing tables or plots.

## Opening and data creation · 10 minutes

Frame feature engineering as measurement design rather than column manipulation. The notebook creates 1,200 time-ordered network events with identifiers, timestamps, network fields, missing values, outliers, and an imbalanced synthetic target.

**Ask:** Which columns describe the event, which identify it, and which exist only after investigation?

**Expected observations:** `event_id` invites memorization; `investigation_result` leaks the label; bytes are long-tailed; missing duration is associated with UDP collection.

## Quality inspection · 8 minutes

Use the schema, missingness, summary statistics, and plots to separate data defects from meaningful absence.

**Misconception:** Missing duration means zero-duration traffic.

**Correction:** Missing means not observed. The cause could be protocol behavior, sensor coverage, or processing failure.

## Feature construction · 18 minutes

Walk through four families:

| Family | Teaching emphasis |
|---|---|
| Time | Cyclic encoding keeps midnight near 23:00. |
| Network | IPs and ports have structure; they are not ordinary numbers. |
| Rates | Every denominator needs an explicit zero/missing policy. |
| History | A row can use prior events, never itself or future events. |

Pause on `point_in_time_counts`. Draw the open-left window $(t-1h,t)$ and show why the current event is appended only after its feature values are recorded.

## Learned transformations · 10 minutes

Contrast `fit` with `transform`:

- imputation learns training medians;
- encoding learns category names and order;
- scaling learns center and spread;
- frozen state transforms validation, test, and production rows.

The unknown-category example should produce an all-zero one-hot row without crashing. Discuss whether an explicit unknown indicator would be preferable.

## Leakage demonstration · 8 minutes

The valid small model should have modest average precision. The deliberately invalid future-outcome model reaches 1.0.

**Ask:** Did feature engineering improve the model?

**Answer:** No. It invalidated the question. A perfect result from post-outcome information is evidence of leakage.

## Pipeline and evaluation · 12 minutes

Trace one numeric and one categorical column through the `ColumnTransformer`. Show that transformed feature names are stable and inspect precision, recall, average precision, ROC-AUC, and the thresholded confusion matrix.

**Ask:** Which error consumes analyst capacity? Which error misses a review-worthy event? Who owns the threshold decision?

## Comparison and selection · 10 minutes

Read every chronological fold, not only the mean. Treat mutual information, correlation, and permutation importance as different diagnostics.

**Misconception:** Important means causal or safe.

**Correction:** Importance measures model reliance under this dataset and scoring setup. Operational cost, drift, and collection risk remain separate decisions.

## Production surprises and exercise · 10 minutes

The unseen categories and missing duration should still produce a finite probability. Emphasize that successful execution proves compatibility, not prediction quality.

Have pairs change the burst threshold and uncommon-port frequency rule. Require one sentence explaining the operational consequence of each change.

## Validation and exit ticket · 7 minutes

Run the assertions and connect each one to a production failure it prevents. Students then write one feature contract including entity, timestamp, window, null behavior, expected range, owner, drift monitor, and parity test.

## Expected anchors

- Final output: `All feature-contract checks passed.`
- Invalid leaked model average precision: `1.000`.
- Engineered held-out average precision: approximately `0.38` with seed 207.
- Exact metrics may vary across library versions; definitions and comparisons should not.

## If time changes

**60 minutes:** Skip detailed feature-selection diagnostics and assign the student exercise after class.

**90 minutes:** Add a late-arriving-event test and ask teams to propose a feature-monitoring dashboard.