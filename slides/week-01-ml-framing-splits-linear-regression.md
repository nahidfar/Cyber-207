---
marp: true
theme: default
paginate: true
math: katex
style: |
  :root { --forest:#123c32; --moss:#2f6b55; --amber:#e0a126; --cream:#f7f1df; --ink:#17302a; --coral:#c8513a; }
  section { background:var(--cream); color:var(--ink); font-family:"Segoe UI",Arial,sans-serif; padding:46px 64px; font-size:29px; }
  section.lead { background:linear-gradient(135deg,var(--forest),#205d49); color:#fff; border-bottom:12px solid var(--amber); }
  h1,h2 { color:var(--forest); letter-spacing:-.03em; } section.lead h1,section.lead h2 { color:#fff; }
  h1 { font-size:2.15em; } h2 { font-size:1.35em; } strong { color:var(--coral); }
  code { background:#102b25; color:#fff8e8; border-radius:8px; } table { font-size:.72em; background:#fffaf0; }
  blockquote,.card { border-left:7px solid var(--amber); background:#fffaf0; padding:.45em .8em; border-radius:10px; box-shadow:0 8px 24px #123c3218; }
  .flow { display:flex; align-items:center; justify-content:center; gap:18px; margin-top:55px; }
  .node { padding:18px 24px; border-radius:16px; background:#fffaf0; border:3px solid var(--moss); font-weight:700; }
  .arrow { color:var(--amber); font-size:2em; }
  .split-token { animation:splitMove 5s ease-in-out infinite; }
  .loss-track { position:relative; height:180px; margin:35px 8%; border-bottom:8px solid var(--moss); border-radius:0 0 50% 50%; }
  .loss-dot { position:absolute; width:32px; height:32px; border-radius:50%; background:var(--coral); animation:descend 5s ease-in-out infinite; }
  @keyframes splitMove { 0%,18%{transform:translateX(-22px)} 45%,68%{transform:translateX(132px)} 82%,100%{transform:translateX(286px)} }
  @keyframes descend { 0%{left:5%;top:5px} 50%{left:44%;top:118px} 100%{left:72%;top:76px} }
  @media (prefers-reduced-motion:reduce){ .split-token,.loss-dot{animation:none!important} .split-token{transform:translateX(132px)} .loss-dot{left:44%;top:118px} }
footer: "CYBER 207 · Week 01 · Models learn; evidence decides"
---
<!-- _class: lead -->
# ML framing, splits & linear regression
## From security question to defensible numeric prediction
CYBER 207 · Week 01 · 90 minutes

---
# Measurable outcomes
By the end, you can:
- map a defensive decision to **unit, features, target, action, and cost**;
- design train/validation/test partitions that resist time and entity leakage;
- fit and interpret a linear regression baseline;
- calculate residuals and MSE and explain gradient descent;
- recommend next evidence without overstating model capability.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–15 | ML in defensive security; features, labels, targets |
| 15–30 | Train, validation, test; leakage |
| 30–50 | Linear regression, residuals, MSE, coefficients |
| 50–70 | Loss surfaces and gradient descent *(50–55 reset break)* |
| 70–90 | Containment-time regression lab and interpretation |

---
# Visual recap · decision contract
<div class="flow">
  <div class="node">observable evidence</div><div class="arrow">→</div>
  <div class="node">numeric estimate</div><div class="arrow">→</div>
  <div class="node">bounded action</div>
</div>

**Anchor:** unit · target · action · costly error · baseline

---
# The 207 modeling boundary
**This course explains:** model learning and the evidence required for defensible use.

- We study data, representations, objectives, optimization, metrics, and error analysis.
- Defensive examples are synthetic or local and bounded to authorized analysis.
- Prompt attacks, red teaming, governance, secure deployment, and agent security belong to **CYBER 290**.

---
# A model is not the decision
Telemetry → features → prediction → threshold/workflow → analyst action → outcome

A useful detector requires:
- an observable unit of prediction;
- a target tied to a later outcome;
- an action someone can actually take;
- a comparison with a simple baseline.

---
# Problem formulation canvas
| Element | Containment-time example |
|---|---|
| Unit | one newly opened incident |
| Features | severity, queue depth, affected hosts |
| Target | hours until containment |
| Action | staffing/escalation priority |
| Costly error | underpredict a long, harmful incident |

**Point-in-time test:** every feature existed when the prediction was requested.

---
# Features, labels, and targets
- **Feature $x$:** measured evidence supplied to the model.
- **Label/target $y$:** outcome the model learns to predict.
- **Prediction $\hat y$:** model output for a new example.
- **Residual $e=y-\hat y$:** signed prediction error.

For regression, $y$ is numeric: minutes, cost, count, or severity score.

---
# Guided micro-activity · formulate
Scenario: estimate analyst minutes required for a queued alert.

In pairs, write:
`unit | 3 features | target | action | expensive error`

Constraints:
- no post-investigation fields;
- no personally identifying content;
- one feature must represent workload context.

---
# Hold data out to estimate transfer
Training performance measures fit to known examples.

Held-out performance estimates transfer to unseen examples.

Using one dataset for fitting, choosing, and reporting quietly turns evaluation into memorization of analyst choices.

---
# Animated split discipline
<div class="flow">
  <div class="node">TRAIN<br>fit</div><div class="arrow split-token">◆</div>
  <div class="node">VALIDATE<br>choose</div><div class="arrow">→</div>
  <div class="node">TEST<br>report once</div>
</div>

Groups and future observations remain isolated across boundaries.

<!-- Presenter note: Narrate the amber token moving from fitting to selection to final reporting. Emphasize that it never moves backward from test to tuning. -->

---
# Security-aware splitting
Random rows may share a host, campaign, or future event across partitions.

Prefer:
- **temporal holdout:** earlier months train, later month tests;
- **group holdout:** one entity/campaign appears in one partition only;
- **both** when deployment predicts future events for evolving entities.

---
# Leakage triage · resolved examples
<div class="card">

**Leaking:** post-closure disposition · duplicated campaigns across splits · repeated test-guided tuning

**Valid when correctly timestamped:** alert count from the prior 24 hours

</div>

Availability precedes correlation as the first feature test.

---
# Linear regression intuition
A line summarizes how expected target value changes with a feature:

$$\hat y = wx+b$$

- $w$: predicted change in $y$ for one-unit increase in $x$.
- $b$: predicted value when $x=0$; it may lack operational meaning.
- Multiple features generalize to $\hat y=\mathbf{w}^\top\mathbf{x}+b$.

---
# Synthetic containment data
| Queue depth $x$ | Actual hours $y$ |
|---:|---:|
| 4 | 2.0 |
| 8 | 3.3 |
| 12 | 4.1 |
| 16 | 6.0 |
| 20 | 6.6 |

Candidate baseline: $\hat y=0.28x+1.0$.

---
# Make two predictions
Using $\hat y=0.28x+1$:
- at $x=8$, $\hat y=3.24$ hours;
- at $x=20$, $\hat y=6.60$ hours.

Interpret $w=0.28$: within this dataset, one extra queued incident is associated with **0.28 predicted hours**, holding other modeled features fixed.

Association is not a causal staffing law.

---
# Residuals show direction
$$e_i=y_i-\hat y_i$$

At $x=16$: $\hat y=5.48$, actual $y=6.0$, so $e=+0.52$.

- Positive residual: model underpredicted.
- Negative residual: model overpredicted.
- A residual plot should look patternless around zero; curves or widening spread signal mismatch.

---
# Mean squared error
$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat y_i)^2$$

Squaring:
- prevents positive and negative residuals cancelling;
- penalizes large misses strongly;
- expresses loss in squared target units.

Also report MAE in hours when interpretability matters.

---
# Coefficients need context
A coefficient is conditional on:
- feature units and scaling;
- correlated predictors included in the model;
- sampled population and period;
- linearity assumptions.

Operational statement: “For alerts like the held-out sample…”—not “queue depth causes containment delay.”

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 within the optimization segment
Stand, hydrate, and return with one question about how a line is fitted.

---
# Learning means minimizing loss
Define parameters $\theta=(w,b)$ and objective $J(\theta)=\operatorname{MSE}$.

Gradient descent repeats:
$$\theta_{t+1}=\theta_t-\eta\nabla_\theta J(\theta_t)$$

- gradient: uphill direction of fastest increase;
- minus sign: move downhill;
- $\eta$: learning rate / step size.

---
# Animated gradient descent
<div class="loss-track"><div class="loss-dot"></div></div>

$$\theta_{t+1}=\theta_t-\eta\nabla J(\theta_t)$$

The dot follows local slope toward lower loss. Large $\eta$ overshoots; small $\eta$ advances slowly.

<!-- Presenter note: Track the dot through descent and overshoot. The still frame preserves the valley and update rule in PowerPoint. -->

---
# Guided activity · 12 minutes
Using the five-row local table:
1. compute $\hat y$ and residual for each row;
2. compute MAE (calculator allowed);
3. identify the most operationally concerning miss;
4. sketch residual versus queue depth;
5. propose one pre-incident feature to test next.

Deliver one annotated table per pair.

---
# Demonstration result
Expected predictions: $2.12,3.24,4.36,5.48,6.60$.

- Average error can hide a dangerous underprediction.
- The smallest numerical error need not produce the safest workflow.
- A training-median baseline establishes minimum evidence.

Evaluation starts with comparison, not celebration.

---
# Operational tradeoffs
- Underprediction may delay escalation or staffing.
- Overprediction may over-allocate scarce responders.
- Extreme incidents can dominate MSE.
- Future workload may differ from historical workload.
- A linear baseline is valuable even when a nonlinear model later wins.

Recommendation must name the user, decision, and consequence.

---
# Common failure modes
1. Random split leaks the same incident family.
2. Post-closure fields reveal the answer.
3. Test set guides repeated tuning.
4. Coefficients are presented as causes.
5. No median/mean baseline is reported.
6. Aggregate MSE hides subgroup or high-severity errors.

---
# scikit-learn implementation
```python
from sklearn.model_selection import train_test_split, GroupShuffleSplit, TimeSeriesSplit
from sklearn.dummy import DummyRegressor
from sklearn.linear_model import LinearRegression, Ridge
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import mean_absolute_error, mean_squared_error

X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=.2, random_state=207)
model = Pipeline([("scale", StandardScaler()), ("reg", Ridge(alpha=1.0))])
model.fit(X_tr, y_tr); pred = model.predict(X_te)
mae, rmse = mean_absolute_error(y_te, pred), mean_squared_error(y_te, pred) ** .5
```

---
# scikit-learn field guide
| API / parameter | Effect of changing it |
|---|---|
| `train_test_split(test_size, random_state)` | controls holdout size and reproducibility |
| `GroupShuffleSplit(test_size, n_splits)` | isolates entities; repeats grouped holdouts |
| `TimeSeriesSplit(n_splits, gap)` | adds temporal folds; `gap` creates a safety buffer |
| `DummyRegressor(strategy)` | selects mean, median, quantile, or constant baseline |
| `LinearRegression(fit_intercept)` | includes or removes the intercept |
| `Ridge(alpha)` | larger values shrink coefficients more strongly |
| `StandardScaler(with_mean, with_std)` | controls centering and variance scaling |

---
# Continuity and takeaway
Week 2 turns numeric prediction into classification with **KNN, trees, and ensembles**.

Bring forward:
- split discipline;
- baseline thinking;
- point-in-time features;
- error examples.

The next layer replaces a straight-line response with flexible decision boundaries.

---
# References · official docs and inspiration
- https://scikit-learn.org/stable/modules/model_evaluation.html
- https://scikit-learn.org/stable/modules/cross_validation.html
- https://scikit-learn.org/stable/modules/linear_model.html
- https://scikit-learn.org/stable/modules/compose.html

Visual-explainer acknowledgments, inspiration only: **Distill**, **MLU-Explain**, **Deep Learning for Cyber**, and official scikit-learn examples. All wording, diagrams, animation, and code composition in this deck are original.
