---
marp: true
theme: default
paginate: true
math: katex
style: |
  :root{--forest:#123c32;--moss:#2f6b55;--amber:#e0a126;--cream:#f7f1df;--ink:#17302a;--coral:#c8513a}
  section{background:var(--cream);color:var(--ink);font-family:"Segoe UI",Arial,sans-serif;padding:46px 64px;font-size:29px}section.lead{background:linear-gradient(135deg,var(--forest),#205d49);color:#fff;border-bottom:12px solid var(--amber)}
  h1,h2{color:var(--forest);letter-spacing:-.03em}section.lead h1,section.lead h2{color:#fff}h1{font-size:2.15em}h2{font-size:1.35em}strong{color:var(--coral)}code{background:#102b25;color:#fff8e8;border-radius:8px}table{font-size:.67em;background:#fffaf0}.card{background:#fffaf0;border-left:7px solid var(--amber);padding:20px 26px;border-radius:14px}
  .pipe{display:flex;align-items:center;justify-content:center;gap:12px;margin-top:55px}.stage{padding:18px;border:3px solid var(--moss);border-radius:14px;background:#fffaf0}.signal{font-size:2em;color:var(--amber);animation:integrationFlow 5s linear infinite}
  .folds{display:grid;grid-template-columns:repeat(6,1fr);gap:10px;margin-top:65px}.fold{height:110px;border-radius:12px;background:var(--moss);color:#fff;display:flex;align-items:center;justify-content:center}.fold.valid{background:var(--amber);color:var(--forest);animation:foldSweep 6s steps(1) infinite}
  @keyframes integrationFlow{0%{transform:translateX(-25px);opacity:.3}50%{transform:translateX(12px);opacity:1}100%{transform:translateX(42px);opacity:.3}}
  @keyframes foldSweep{0%,16%{transform:translateY(-18px)}17%,100%{transform:translateY(0)}}
  @media(prefers-reduced-motion:reduce){.signal,.fold.valid{animation:none!important}.fold.valid{transform:translateY(-12px)}}
footer: "CYBER 207 · Week 07 · Classical ML integration"
---
<!-- _class: lead -->
# Classical ML integration lab
## Evidence from problem framing to temporal evaluation
CYBER 207 · Week 07 · 90 minutes

---
# Measurable outcomes
- Select methods from data/decision constraints.
- construct a leakage-resistant pipeline and tuning plan.
- combine supervised and unsupervised evidence cautiously.
- diagnose temporal drift.
- explain and defend a complete classical-ML study.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Select a model from data and decision constraints |
| 20–40 | Pipelines, cross-validation, tuning |
| 40–60 | Compare supervised and unsupervised evidence *(50–55 break)* |
| 60–75 | Temporal evaluation and drift |
| 75–90 | Midterm case review |

---
# Visual recap · evidence chain
<div class="card">

Ranking differs from calibration · macro F1 elevates rare classes · unusualness is not a threat label · future windows leak

</div>

---
# Integration case
Goal: prioritize synthetic endpoint alerts for review next hour.

Available at event time:
- process-tree aggregates;
- signer category;
- host’s trailing event counts;
- queue depth.

Later labels: analyst-confirmed priority. Constraint: 50 reviews/day.

---
# Start with the decision
Define:
- unit: one endpoint alert;
- prediction: priority probability;
- action: rank then review top 50;
- expensive error: missed high-impact alert;
- evidence horizon: next month;
- comparison: rules + simple logistic baseline.

Algorithm selection comes after this contract.

---
# Method selection matrix
| Constraint | Candidate |
|---|---|
| numeric target, transparent baseline | linear regression |
| probability, sparse features | logistic regression |
| local nonlinear pattern | KNN |
| interactions/inspectable path | tree |
| strong tabular baseline | forest/boosting |
| no labels/discovery | k-means/PCA |

Choose by evidence and workflow—not novelty.

---
# Baseline ladder
1. policy/rule baseline;
2. prevalence/median baseline;
3. simple linear/logistic model;
4. tree/ensemble;
5. unsupervised discovery if labels are absent.

A complex model earns adoption by changing an operationally important outcome.

---
# Animated pipeline blueprint
<div class="pipe"><div class="stage">events</div><span class="signal">◆</span><div class="stage">features</div><span class="signal">◆</span><div class="stage">split</div><span class="signal">◆</span><div class="stage">model</div><span class="signal">◆</span><div class="stage">evidence</div></div>

Version schema, label rule, split dates and groups, fitted settings, seed, and evaluation artifact.

<!-- Presenter note: Narrate the signal as a versioned evidence object. Pause at split to reinforce group and time isolation. -->

---
# Animated cross-validation sweep
<div class="folds"><div class="fold">M1</div><div class="fold">M2</div><div class="fold">M3</div><div class="fold valid">M4<br>validate</div><div class="fold">M5</div><div class="fold">M6</div></div>

Expanding folds train on prior months and validate on the next month. A final later period remains untouched.

Group campaigns/hosts where duplicates would leak.

<!-- Presenter note: Use the raised amber fold as the validation window, then verbally advance it through later months. The static state preserves ordering. -->

---
# Tuning is search over choices
Examples:
- KNN $k$ and distance;
- tree depth/leaf size;
- regularization strength;
- ensemble learning rate/trees;
- threshold under capacity.

Search inside validation folds; report search space and selection rule.

---
# Nested reasoning, not test peeking
Correct sequence:
1. compare settings on cross-validation;
2. select one pipeline;
3. refit using allowed pre-test data;
4. evaluate once on final test;
5. analyze errors without silently redesigning and re-reporting the same test.

---
# Worked design pattern
Given 20,000 alerts from 500 hosts across six months:
- months 1–4 train, month 5 validate, month 6 test;
- group hosts when repeated host behavior can memorize identity;
- use precision at 50 with priority-alert recall;
- compare with a rule or prevalence baseline;
- exclude post-review analyst disposition.

Time: 6 minutes.

---
# Supervised evidence
Measures agreement between predictions and available labels.

Use:
- confusion counts at capacity;
- PR/ranking metrics;
- calibration when probabilities drive decisions;
- per-slice error analysis;
- uncertainty across time folds.

Label quality limits every conclusion.

---
# Unsupervised evidence
Describes geometry and unusualness under the chosen representation.

Use to:
- discover cohorts;
- select cases for review;
- characterize drift;
- generate hypotheses.

Do not use cluster IDs as truth labels without validation.

---
# Triangulating evidence
A high supervised score plus stable clusters can support exploration, but neither proves causality.

Example:
- classifier misses a new process pattern;
- PCA flags it as outside prior subspace;
- analyst review supplies a later label;
- future training can incorporate verified evidence.

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 within evidence comparison
Return with one sentence separating prediction evidence from discovery evidence.

---
# Integration activity · detector study
Teams receive:
- synthetic fold metrics for logistic, tree, forest;
- capacity-level confusion counts;
- PCA drift summary;
- four error records.

Select one model and write the strongest defensible claim plus one unsupported claim.

---
# Temporal drift
Data distribution changes over time:
$$P_t(X),\quad P_t(Y),\quad P_t(Y\mid X)$$

- covariate drift: features change;
- prevalence drift: class rate changes;
- concept drift: relationship changes;
- label/process drift: annotation practice changes.

---
# Drift diagnostics
Compare time slices using:
- feature missingness/quantiles;
- category frequencies;
- score and calibration distributions;
- per-class metrics and capacity counts;
- PCA projection/reconstruction error;
- label delay and reviewer behavior.

Diagnostics indicate where to investigate, not root cause by themselves.

---
# Worked temporal result
| Month | prevalence | top-50 precision | recall | missing signer |
|---|---:|---:|---:|---:|
| Apr | 8% | .62 | .48 | 2% |
| May | 7% | .58 | .43 | 3% |
| Jun | 5% | .34 | .29 | 18% |

Hypothesis: collection change contributes; verify before retraining.

---
# Operational response choices
Depending on evidence:
- repair sensor/pipeline;
- recalibrate threshold;
- update reference period;
- collect labels for new cohort;
- retrain and revalidate;
- temporarily rely more on rule/baseline.

Model replacement is not the automatic first step.

---
# Midterm scope map
Apply these skills:
- frame unit/features/target/action/cost;
- find leakage;
- compute regression/classification metrics;
- interpret coefficients and thresholds;
- compare KNN/tree/ensemble behavior;
- interpret k-means/PCA cautiously;
- diagnose a temporal evaluation table.

---
# Case review · evidence chain
1. State the decision and action.
2. Establish observability at prediction time.
3. Declare baseline, groups, and temporal split.
4. Name objective and operational metric.
5. Inspect consequential errors.
6. Bound the supported claim.
7. Select the next controlled experiment.

---
# Guided midterm review
Stations (3 minutes each):
- leakage and split;
- residual/MSE;
- threshold confusion counts;
- macro versus weighted metrics;
- $K$/cluster caveats.

Record one evidence statement per station; class debrief addresses categories, not an exam answer key.

---
# Common failure modes
1. Start with an algorithm, not decision.
2. preprocess globally.
3. tune on final test.
4. compare models on different splits.
5. average away rare errors.
6. confuse drift symptom with cause.
7. convert unsupervised pattern into threat verdict.

---
# scikit-learn implementation
```python
from sklearn import set_config
from sklearn.pipeline import Pipeline
from sklearn.model_selection import (cross_validate, GroupKFold, TimeSeriesSplit,
  RandomizedSearchCV, learning_curve, validation_curve)

set_config(display="diagram")
pipe = Pipeline([("prep", preprocessor), ("clf", classifier)])
cv = GroupKFold(n_splits=5)
scores = cross_validate(pipe, X, y, groups=host_id, cv=cv,
            scoring={"ap":"average_precision", "f1":"f1"},
            return_train_score=True, return_estimator=True)
search = RandomizedSearchCV(pipe, distributions, n_iter=30, scoring="average_precision", cv=cv)
search.fit(X, y, groups=host_id)
```

---
# scikit-learn field guide
| API / parameter | Effect of changing it |
|---|---|
| `Pipeline` step names | enable nested search such as `clf__max_depth` |
| `cross_validate(scoring, return_estimator)` | adds metrics and fitted-fold inspection |
| `GroupKFold(n_splits, shuffle)` | isolates groups and optionally randomizes group order |
| `TimeSeriesSplit(n_splits, gap, test_size)` | controls temporal folds, buffer, and horizon |
| `RandomizedSearchCV(n_iter, scoring, refit)` | sets search budget, selection metric, and final refit |
| `learning_curve(train_sizes)` | varies sample size to expose bias and variance |
| `validation_curve(param_name, param_range)` | isolates one hyperparameter effect |
| `set_config(display="diagram")` | renders nested estimators as an interactive diagram |

---
# Next week continuity
Week 8 begins with the 60-minute cumulative practical assessment.

Prepare to reason from supplied evidence—no hidden production systems, live targets, or offensive tasks.

After the exam: neutral misconception review and final-project problem framing. No student-facing answer key will be shown.

---
# References · official docs and inspiration

- https://scikit-learn.org/stable/modules/cross_validation.html
- https://scikit-learn.org/stable/modules/learning_curve.html
- https://scikit-learn.org/stable/modules/compose.html
- https://scikit-learn.org/stable/modules/grid_search.html

Visual-explainer acknowledgments, inspiration only: **Distill**, **MLU-Explain**, **Deep Learning for Cyber**, and official scikit-learn examples. All text, diagrams, and motion are original.
