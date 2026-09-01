---
marp: true
theme: default
paginate: true
math: katex
style: |
  :root{--forest:#123c32;--moss:#2f6b55;--amber:#e0a126;--cream:#f7f1df;--ink:#17302a;--coral:#c8513a}
  section{background:var(--cream);color:var(--ink);font-family:"Segoe UI",Arial,sans-serif;padding:46px 64px;font-size:29px}section.lead{background:linear-gradient(135deg,var(--forest),#205d49);color:#fff;border-bottom:12px solid var(--amber)}
  h1,h2{color:var(--forest);letter-spacing:-.03em}section.lead h1,section.lead h2{color:#fff}h1{font-size:2.15em}h2{font-size:1.35em}strong{color:var(--coral)}code{background:#102b25;color:#fff8e8;border-radius:8px}table{font-size:.69em;background:#fffaf0}.card{background:#fffaf0;border-left:7px solid var(--amber);padding:20px 26px;border-radius:14px}
  .sigmoid{position:relative;height:260px;margin:15px 8%;border-left:5px solid var(--moss);border-bottom:5px solid var(--moss)}.curve{position:absolute;left:8%;top:42%;width:84%;height:70px;border-top:10px solid var(--amber);border-radius:50%;transform:rotate(-22deg)}.score-dot{position:absolute;width:30px;height:30px;background:var(--coral);border-radius:50%;animation:sigmoidTravel 5s ease-in-out infinite}
  .threshold-track{position:relative;height:90px;margin:70px 8%;background:linear-gradient(90deg,#83a98f,#e0a126,#c8513a);border-radius:45px}.sweep{position:absolute;top:-25px;width:8px;height:140px;background:var(--forest);animation:thresholdSweep 5s ease-in-out infinite}
  @keyframes sigmoidTravel{0%{left:10%;top:185px}50%{left:48%;top:105px}100%{left:83%;top:18px}}
  @keyframes thresholdSweep{0%,100%{left:18%}50%{left:78%}}
  @media(prefers-reduced-motion:reduce){.score-dot,.sweep{animation:none!important}.score-dot{left:48%;top:105px}.sweep{left:48%}}
footer: "CYBER 207 · Week 04 · Logistic regression & decisions"
---
<!-- _class: lead -->
# Logistic regression & probabilistic decisions
## From risky-login score to defensible action
CYBER 207 · Week 04 · 90 minutes

---
# Measurable outcomes
- Convert logits to probabilities with the sigmoid.
- Explain binary cross-entropy and regularization.
- assess calibration and select an operational threshold.
- interpret coefficients as changes in log-odds/odds.
- recommend a risky-login policy tied to error costs.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Logits, sigmoid, odds, boundaries |
| 20–40 | Binary cross-entropy and regularization |
| 40–60 | Probabilities, calibration, thresholds *(50–55 break)* |
| 60–75 | Coefficient interpretation |
| 75–90 | Tune a risky-login policy |

---
# Visual recap · evidence to action
<div class="card">

Point-in-time features → fold-fitted transforms → controlled ablation → validated score → capacity-aware action

</div>

---
# Synthetic defensive scenario
For each login, estimate probability of confirmed account takeover within seven days.

Features:
- failed attempts in prior hour;
- new-device indicator;
- travel-velocity anomaly score;
- account-age bucket.

Actions: allow, step-up authentication, or review. Data are synthetic/local.

---
# Linear score, nonlinear probability
First compute a logit:
$$z=\mathbf{w}^{\top}\mathbf{x}+b$$

Then map it to $(0,1)$:
$$p(y=1\mid x)=\sigma(z)=\frac{1}{1+e^{-z}}$$

A linear boundary in feature space becomes a smooth probability curve.

---
# Animated sigmoid mapping
<div class="sigmoid"><div class="curve"></div><div class="score-dot"></div></div>

$z=-2\rightarrow .119$ · $z=0\rightarrow .500$ · $z=2\rightarrow .881$

Equal logit movement changes probability most near the center.

<!-- Presenter note: Follow the coral score from low logit to high probability. The midpoint remains visible in reduced-motion and PowerPoint states. -->

---
# Odds and log-odds
If probability is $p$:
$$\text{odds}=\frac{p}{1-p},\qquad \operatorname{logit}(p)=\log\frac{p}{1-p}$$

$p=.8$ means odds $=.8/.2=4$: one positive outcome per four-to-one odds—not “80% more risky.”

Logistic coefficients add in log-odds space.

---
# Decision boundary
At threshold $\tau=.5$:
$$\sigma(z)\ge .5 \iff z\ge 0$$

Changing the operational threshold changes the decision, **not the fitted probability model**.

A threshold is a policy choice informed by costs and capacity.

---
# Worked calculation
Model: $z=-2+0.8(\text{new device})+0.3(\text{failed count})$.

For new device $=1$, failed count $=3$:
$z=-.3$, so $p\approx.426$. A threshold of $.4$ triggers action; $.5$ does not. Queue depth is one useful omitted operational context.

---
# Binary cross-entropy
For label $y\in\{0,1\}$ and probability $p$:
$$\ell(y,p)=-[y\log p+(1-y)\log(1-p)]$$

Confident wrong predictions receive large loss.

It trains probabilities by rewarding assigned probability to the observed class—not by counting thresholded accuracy.

---
# Loss intuition
If $y=1$:
- $p=.9 \Rightarrow \ell\approx .105$;
- $p=.5 \Rightarrow \ell\approx .693$;
- $p=.01 \Rightarrow \ell\approx 4.605$.

Cross-entropy distinguishes confidence even when two examples fall on the same side of a threshold.

---
# Regularization
Penalized objective:
$$J(\mathbf{w})=\frac1n\sum_i\ell_i+\lambda\lVert\mathbf{w}\rVert_2^2$$

- Larger $\lambda$: stronger shrinkage, simpler boundary.
- Helps with noisy/correlated features.
- Standardize numeric features for comparable penalty.
- Select strength on validation folds.

---
# L1 versus L2
| Penalty | Tendency | Caution |
|---|---|---|
| L1 $\lambda\sum|w_j|$ | some coefficients exactly zero | unstable choice among correlated features |
| L2 $\lambda\sum w_j^2$ | smooth shrinkage | keeps many small coefficients |

Neither turns observational coefficients into causes.

---
# Probabilities need calibration
A model is calibrated when among cases scored near $p$, roughly fraction $p$ are positive.

Example: among 100 logins scored around .20, about 20 later become confirmed takeover.

Ranking can be good while probability values are unreliable.

---
# Calibration diagnostics
- reliability diagram: observed rate versus predicted probability;
- Brier score: $\frac1n\sum_i(p_i-y_i)^2$;
- calibration intercept/slope;
- counts per probability bin;
- checks by time and operational subgroup.

Calibrate using held-out validation data, then test once.

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 within probability and threshold work
Return with one reason a well-ranked model can still have poor probabilities.

---
# Animated threshold sweep
<div class="threshold-track"><div class="sweep"></div></div>

Lower $\tau$ produces more true positives and false positives. Higher $\tau$ reduces interruptions and misses more takeovers.

Expected cost:
$$C(\tau)=c_{FP}FP(\tau)+c_{FN}FN(\tau)$$

Add review capacity and user-friction constraints explicitly.

<!-- Presenter note: Move from green through amber to coral while narrating review volume, misses, and user friction. The gradient encodes the same tradeoff without motion. -->

---
# Three-action policy
Example policy on calibrated validation scores:
- $p<.10$: allow and log;
- $.10\le p<.45$: step-up authentication;
- $p\ge.45$: step-up + prioritized review.

Policies should include fallback behavior when required features are missing.

---
# Coefficient interpretation
For coefficient $w_j$, a one-unit increase multiplies odds by:
$$\exp(w_j)$$

If $w_{new\_device}=0.69$, odds multiply by $e^{.69}\approx2$, holding modeled features fixed.

Probability change depends on the starting logit and other features.

---
# Scaling changes coefficient units
If failed-login count is standardized, its coefficient describes a **one standard deviation** increase, not one failed login.

Always report:
- transformation;
- unit/reference category;
- confidence/variability if available;
- conditional, noncausal wording.

---
# Worked validation table
| threshold | TP | FP | FN | challenges |
|---:|---:|---:|---:|---:|
| .20 | 42 | 180 | 8 | 222 |
| .40 | 35 | 70 | 15 | 105 |
| .60 | 25 | 25 | 25 | 50 |

Assume $c_{FN}=10$, $c_{FP}=1$, capacity $\le120$ challenges.

---
# Guided activity · tune policy
In teams:
1. compute $FP+10FN$ for each threshold;
2. enforce challenge capacity;
3. recommend one threshold;
4. name the stakeholders bearing each error;
5. identify one calibration check before deployment discussion.

Time: 9 minutes + 4-minute debrief.

---
# Activity interpretation anchor
Costs:
- $.20$: $180+80=260$; capacity fails.
- $.40$: $70+150=220$; capacity passes.
- $.60$: $25+250=275$; capacity passes.

Under stated assumptions, $.40$ is defensible. Change the cost ratio and the answer may change.

---
# Operational tradeoffs
- Calibration can drift as base rates change.
- Authentication friction is unevenly distributed.
- Labels arrive late and may reflect analyst selection.
- Thresholds need capacity-aware monitoring.
- A transparent logistic baseline may outperform a complex model operationally.

---
# Common failure modes
1. Call uncalibrated score a probability.
2. choose threshold on test data.
3. optimize accuracy under severe imbalance.
4. interpret odds ratio as probability increase.
5. ignore coefficient units/reference categories.
6. use future investigation fields.

---
# scikit-learn implementation
```python
from sklearn.linear_model import LogisticRegression
from sklearn.calibration import CalibratedClassifierCV, CalibrationDisplay
from sklearn.model_selection import TunedThresholdClassifierCV
from sklearn.metrics import precision_recall_curve

base = LogisticRegression(C=1.0, penalty="l2", solver="lbfgs", max_iter=1000,
                          class_weight="balanced")
calibrated = CalibratedClassifierCV(base, method="sigmoid", cv=5).fit(X_train, y_train)
tuned = TunedThresholdClassifierCV(calibrated, scoring="f1", cv=5).fit(X_train, y_train)
precision, recall, thresholds = precision_recall_curve(y_test, tuned.predict_proba(X_test)[:, 1])
```

---
# scikit-learn field guide
| API / parameter | Effect of changing it |
|---|---|
| `LogisticRegression(C)` | smaller values strengthen regularization |
| `penalty` + `solver` | select shrinkage form; compatibility must match |
| `class_weight` | increases loss contribution for selected classes |
| `max_iter` | raises the convergence budget |
| `CalibratedClassifierCV(method, cv)` | selects sigmoid or isotonic mapping and calibration folds |
| `TunedThresholdClassifierCV(scoring, thresholds)` | selects the decision cutoff for a declared objective |
| `precision_recall_curve` | returns operating points across score cutoffs |

---
# Next week continuity
Week 5 expands binary decisions to multiple classes and richer metrics.

Carry forward:
- probability versus action;
- confusion counts;
- costs and capacity;
- calibration skepticism.

Next, metrics summarize error across several malware-family routing classes.

---
# References · official docs and inspiration
- https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression
- https://scikit-learn.org/stable/modules/calibration.html
- https://scikit-learn.org/stable/modules/classification_threshold.html
- https://scikit-learn.org/stable/modules/model_evaluation.html

Visual-explainer acknowledgments, inspiration only: **Distill**, **MLU-Explain**, **Deep Learning for Cyber**, and official scikit-learn examples. All text and visual motion are original.
