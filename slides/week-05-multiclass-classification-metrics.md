---
marp: true
theme: default
paginate: true
math: katex
style: |
  :root{--forest:#123c32;--moss:#2f6b55;--amber:#e0a126;--cream:#f7f1df;--ink:#17302a;--coral:#c8513a}
  section{background:var(--cream);color:var(--ink);font-family:"Segoe UI",Arial,sans-serif;padding:46px 64px;font-size:29px}section.lead{background:linear-gradient(135deg,var(--forest),#205d49);color:#fff;border-bottom:12px solid var(--amber)}
  h1,h2{color:var(--forest);letter-spacing:-.03em}section.lead h1,section.lead h2{color:#fff}h1{font-size:2.15em}h2{font-size:1.35em}strong{color:var(--coral)}code{background:#102b25;color:#fff8e8;border-radius:8px}table{font-size:.68em;background:#fffaf0}.card{background:#fffaf0;border-left:7px solid var(--amber);padding:20px 26px;border-radius:14px}
  .matrix{display:grid;grid-template-columns:repeat(3,120px);gap:12px;justify-content:center;margin-top:35px}.cell{height:82px;background:#eadfc5;border-radius:12px;display:flex;align-items:center;justify-content:center;font-weight:700}.cell.diag{background:var(--moss);color:white;animation:diagonalPulse 4s ease-in-out infinite}
  .curvebox{position:relative;height:250px;margin:20px 10%;border-left:5px solid var(--forest);border-bottom:5px solid var(--forest)}.roc,.pr{position:absolute;width:75%;height:150px;left:8%;bottom:12px;border-radius:70% 20% 0 0;border-top:9px solid var(--moss)}.pr{width:62%;height:110px;border-color:var(--coral)}.marker{position:absolute;width:26px;height:26px;background:var(--amber);border-radius:50%;animation:curveSweep 5s linear infinite}
  @keyframes diagonalPulse{0%,100%{transform:scale(1)}50%{transform:scale(1.08);box-shadow:0 0 0 10px #e0a12655}}
  @keyframes curveSweep{0%{left:12%;top:175px}50%{left:45%;top:75px}100%{left:72%;top:35px}}
  @media(prefers-reduced-motion:reduce){.cell.diag,.marker{animation:none!important}.marker{left:45%;top:75px}}
footer: "CYBER 207 · Week 05 · Multiclass metrics"
---
<!-- _class: lead -->
# Multiclass classification & metrics
## Malware-family evidence beyond accuracy
CYBER 207 · Week 05 · 90 minutes

---
# Measurable outcomes
- Contrast one-vs-rest with softmax multiclass classification.
- compute precision, recall, and F1 from confusion counts.
- choose ROC or precision–recall views appropriately.
- distinguish macro, micro, and weighted averaging.
- write a SOC-facing error analysis that rejects accuracy-only claims.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | One-vs-rest, softmax, multiclass labels |
| 20–40 | Confusion matrices, precision, recall, F1 |
| 40–55 | ROC versus precision–recall *(50–55 break)* |
| 55–70 | Macro, micro, weighted metrics; imbalance |
| 70–90 | Malware-family error analysis |

---
# Visual recap · score to policy
<div class="card">

Threshold encodes cost and capacity · ranking differs from calibration · validation selects policy · confusion counts expose consequences

</div>

---
# Synthetic defensive task
Classify inert, local feature vectors into:
- benign software;
- downloader;
- ransomware-like;
- credential-theft-like.

The labels are synthetic training categories—not assertions about real files. Goal: route samples for analyst review.

---
# Multiclass label design
Classes should be:
- operationally meaningful;
- consistently labeled;
- supported by enough examples;
- explicit about “unknown/other”;
- aligned with the action granularity.

Taxonomies can overlap; forced single labels may hide reality.

---
# One-vs-rest
Train one binary classifier per class $c$:
$$p_c\approx P(y=c\text{ versus not }c\mid x)$$

Choose largest score or apply per-class thresholds.

Advantages: reuse binary models. Caution: scores need not sum to one or be mutually calibrated.

---
# Softmax
For logits $z_1,\ldots,z_K$:
$$p_k=\frac{e^{z_k}}{\sum_{j=1}^{K}e^{z_j}}$$

Probabilities sum to one. Cross-entropy for true class $y$:
$$\ell=-\log p_y$$

Softmax competition assumes one class label per example.

---
# Worked softmax intuition
Logits: benign $=1$, downloader $=2$, ransomware $=0$.

Exponentials: $2.72,7.39,1$; sum $11.11$.

Probabilities: $.245,.665,.090$.

A largest class of .665 is not automatically safe enough for autonomous action.

---
# Top-$k$ and reject option
Useful outputs:
- top-1 predicted family;
- top-2 candidates for analyst triage;
- confidence/margin;
- abstain or “unknown” when evidence is weak.

Evaluation must score the actual workflow, including abstentions.

---
# Animated confusion matrix
Rows encode truth; columns encode prediction.
<div class="matrix"><div class="cell diag">80</div><div class="cell">12</div><div class="cell">8</div><div class="cell">10</div><div class="cell diag">35</div><div class="cell">5</div><div class="cell">6</div><div class="cell">9</div><div class="cell diag">25</div></div>

Diagonal cells are correct; off-diagonal cells expose routing failures.

<!-- Presenter note: Let the diagonal pulse establish orientation, then read one costly off-diagonal path. The full matrix remains static and legible in PowerPoint. -->

---
# Per-class precision and recall
Treat one class as positive versus rest:
$$\text{precision}=\frac{TP}{TP+FP}$$
$$\text{recall}=\frac{TP}{TP+FN}$$

Precision measures the confirmed share of ransomware predictions.
Recall measures the detected share of true ransomware examples.

---
# F1 score
$$F_1=2\frac{PR}{P+R}=\frac{2TP}{2TP+FP+FN}$$

F1 balances precision and recall but:
- hides true negatives;
- assumes equal importance through harmonic mean;
- does not encode analyst capacity or asymmetric harm.

Use with confusion counts.

---
# Worked class calculation
From the matrix, for ransomware:
- $TP=25$;
- $FP=8+5=13$;
- $FN=6+9=15$.

Precision $=25/38=.658$ · recall $=25/40=.625$ · F1 $=.641$. Benign predicted as ransomware creates avoidable review burden.

---
# Animated ROC and PR operating points
<div class="curvebox"><div class="roc"></div><div class="pr"></div><div class="marker"></div></div>

Across thresholds:
$$TPR=\frac{TP}{TP+FN},\qquad FPR=\frac{FP}{FP+TN}$$

ROC summarizes ranking; PR exposes positive alert quality. Every moving point is a concrete threshold with counts.

<!-- Presenter note: Narrate the marker as a threshold sweep. Pair each location with alert count and analyst capacity. -->

---
# Precision–recall curve
Across thresholds, plot precision versus recall.

PR is often more revealing for rare positives because precision directly reflects false-alert burden.

Baseline precision approximately equals positive prevalence for random ranking.

Always include actual counts at candidate thresholds.

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 at the curve transition
Return ready to explain why 1% FPR can still overwhelm a SOC.

---
# ROC versus PR · deliberate view selection
| Evidence need | Better first view |
|---|---|
| Positive-over-negative ranking | ROC + AUC |
| Alert quality with rare positives | PR + AP |
| Outcomes at staffed capacity | threshold counts |
| Probability trustworthiness | calibration |

No single curve answers every operational question.

---
# Macro averaging
Compute metric per class, then unweighted mean:
$$M_{macro}=\frac1K\sum_{k=1}^K M_k$$

Every class gets equal voice. Sensitive to poor performance on rare families.

Use when minority-class quality matters independently of prevalence.

---
# Micro and weighted averaging
- **Micro:** pool all class decisions/counts first; frequent classes dominate.
- **Weighted:** average per-class metrics weighted by class support.
- In single-label multiclass classification, micro-F1 equals accuracy.

Report per-class values so averaging does not erase failures.

---
# Imbalance changes interpretation
A 90%-benign dataset allows 90% accuracy by always predicting benign.

Compare with:
- majority baseline;
- stratified confusion matrix;
- macro metrics;
- PR curves for priority classes;
- counts at operational threshold.

---
# Metric uncertainty
Scores vary with sample composition.

Report:
- fold/time-slice spread;
- bootstrap interval when appropriate;
- denominator/support per class;
- performance on later periods;
- label-review uncertainty.

A metric with 8 rare examples is not stable evidence.

---
# Guided activity · SOC error analysis
Teams receive a four-class confusion matrix and six anonymized synthetic errors.
1. calculate one rare-class recall;
2. compare macro and weighted F1;
3. select two errors for root-cause review;
4. propose one data/feature test;
5. write a two-sentence SOC recommendation.

---
# Error-analysis categories
Tag each error:
- ambiguous/overlapping label;
- insufficient feature evidence;
- new/rare pattern;
- collection/missingness issue;
- threshold/reject-policy issue;
- possible annotation error.

Do not “fix” the metric before understanding the cases.

---
# Operational tradeoffs
- High rare-family recall may increase benign review burden.
- Coarse classes improve reliability but reduce routing specificity.
- Fine classes aid analysts but need more labels.
- Reject/unknown preserves honesty at cost of manual handling.
- Macro and weighted summaries serve different stakeholder evidence needs.

---
# Common failure modes
1. Accuracy-only claim under imbalance.
2. Ambiguous row/column orientation.
3. Macro/weighted label omitted.
4. ROC-AUC used without alert counts.
5. Test set selects class thresholds.
6. Unsupported rare-class conclusions.
7. No unknown/reject behavior.

---
# scikit-learn implementation
```python
from sklearn.metrics import (classification_report, confusion_matrix,
  ConfusionMatrixDisplay, roc_auc_score, average_precision_score, f1_score)
from sklearn.utils.class_weight import compute_class_weight

labels = ["benign", "downloader", "ransomware", "credential"]
print(classification_report(y_test, pred, labels=labels, zero_division=0))
cm = confusion_matrix(y_test, pred, labels=labels)
ConfusionMatrixDisplay(cm, display_labels=labels).plot()
macro_f1 = f1_score(y_test, pred, average="macro")
weighted_f1 = f1_score(y_test, pred, average="weighted")
ap = average_precision_score(y_onehot, proba, average="macro")
auc = roc_auc_score(y_onehot, proba, average="macro", multi_class="ovr")
weights = compute_class_weight("balanced", classes=classes, y=y_train)
```

---
# scikit-learn field guide
| API / parameter | Effect of changing it |
|---|---|
| `classification_report(labels, zero_division)` | fixes class order and undefined-metric behavior |
| `confusion_matrix(normalize)` | selects counts or row, column, or global proportions |
| `ConfusionMatrixDisplay.from_predictions` | renders the evaluated predictions directly |
| `f1_score(average)` | selects binary, macro, micro, weighted, samples, or per-class output |
| `roc_auc_score(average, multi_class)` | selects aggregation and OvR or OvO multiclass handling |
| `average_precision_score(average)` | aggregates area under stepwise PR summaries |
| `compute_class_weight(class_weight="balanced")` | weights inversely to observed class frequency |

---
# Next week continuity
Week 6 removes labels and asks what structure can still be discovered with k-means and PCA.

Carry forward:
- scaling discipline;
- class prevalence awareness;
- error inspection;
- cautious interpretation.

Next, unlabeled geometry is treated as an investigative hypothesis rather than a threat verdict.

---
# References · official docs and inspiration
- https://scikit-learn.org/stable/modules/model_evaluation.html
- https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html
- https://scikit-learn.org/stable/modules/generated/sklearn.metrics.ConfusionMatrixDisplay.html
- https://scikit-learn.org/stable/modules/generated/sklearn.utils.class_weight.compute_class_weight.html

Visual-explainer acknowledgments, inspiration only: **Distill**, **MLU-Explain**, **Deep Learning for Cyber**, and official scikit-learn examples. All wording, grids, and motion are original.
