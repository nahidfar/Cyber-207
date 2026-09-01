---
marp: true
html: true
theme: default
paginate: true
math: katex
style: |
  :root{--w2-forest:#123c32;--w2-moss:#2f6b55;--w2-amber:#e0a126;--w2-cream:#f7f1df;--w2-paper:#fffaf0;--w2-ink:#17302a;--w2-coral:#c8513a}
  section{background:var(--w2-cream);color:var(--w2-ink);font-family:"Segoe UI",Arial,sans-serif;padding:42px 60px;font-size:27px} section.lead{background:linear-gradient(135deg,var(--w2-forest),#205d49);color:#fff;border-bottom:12px solid var(--w2-amber)}
  h1,h2{color:var(--w2-forest);letter-spacing:-.03em} section.lead h1,section.lead h2{color:#fff} h1{font-size:2.05em} h2{font-size:1.32em} strong{color:var(--w2-coral)} code{background:#102b25;color:#fff8e8;border-radius:7px} pre code{font-size:.73em;line-height:1.25} table{width:100%;font-size:.69em;background:var(--w2-paper)} th{background:var(--w2-forest);color:#fff} td,th{padding:7px 10px}
  .w2-card{background:var(--w2-paper);border-left:7px solid var(--w2-amber);padding:16px 22px;border-radius:14px;box-shadow:0 8px 22px #123c3218}.w2-grid2{display:grid;grid-template-columns:1fr 1fr;gap:20px}.w2-small{font-size:.72em}
  .w2-tree-stage{position:relative;height:390px;max-width:900px;margin:6px auto 0}.w2-node{position:absolute;width:230px;padding:12px 8px;text-align:center;border:4px solid var(--w2-moss);border-radius:16px;background-color:var(--w2-paper);font-weight:700;opacity:1}.w2-root{left:335px;top:0}.w2-left{left:105px;top:155px}.w2-right{right:105px;top:155px}.w2-leaf-a{left:0;top:315px}.w2-leaf-b{left:255px;top:315px}.w2-leaf-c{right:0;top:315px}.w2-edge{position:absolute;height:8px;width:215px;border-radius:8px;background-color:var(--w2-amber);transform-origin:left center;opacity:1}.w2-edge-a{left:420px;top:90px;transform:rotate(145deg)}.w2-edge-b{left:475px;top:90px;transform:rotate(35deg)}.w2-edge-c{left:210px;top:245px;transform:rotate(143deg)}.w2-edge-d{left:240px;top:245px;transform:rotate(37deg)}.w2-edge-e{right:205px;top:245px;transform:rotate(37deg)}
  .w2-tree-1{animation:w2-tree-show 7s ease-in-out 0s infinite;animation-fill-mode:both}.w2-tree-2{animation:w2-tree-show 7s ease-in-out .8s infinite;animation-fill-mode:both}.w2-tree-3{animation:w2-tree-show 7s ease-in-out 1.6s infinite;animation-fill-mode:both}.w2-tree-4{animation:w2-tree-show 7s ease-in-out 2.4s infinite;animation-fill-mode:both}
  .w2-bag-stage{display:grid;grid-template-columns:220px 70px 220px 70px 190px;grid-template-rows:repeat(3,92px);gap:10px;align-items:center;justify-content:center}.w2-sample,.w2-mini-tree,.w2-vote{border:3px solid var(--w2-moss);border-radius:14px;padding:12px;text-align:center;background-color:var(--w2-paper);font-weight:700;opacity:1}.w2-flow{color:var(--w2-amber);font-size:2em;text-align:center;opacity:1}.w2-bag-a{animation:w2-bag-flow 6.6s ease-in-out 0s infinite;animation-fill-mode:both}.w2-bag-b{animation:w2-bag-flow 6.6s ease-in-out .7s infinite;animation-fill-mode:both}.w2-bag-c{animation:w2-bag-flow 6.6s ease-in-out 1.4s infinite;animation-fill-mode:both}.w2-vote{grid-column:5;grid-row:1/4;background-color:#f3d98f;font-size:1.15em;animation:w2-vote-converge 6.6s ease-in-out 2.2s infinite;animation-fill-mode:both}
  .w2-boost-stage{display:flex;align-items:center;justify-content:center;gap:22px;margin:22px 0 14px}.w2-learner{width:210px;min-height:115px;border:4px solid var(--w2-moss);border-radius:16px;padding:14px;text-align:center;background-color:var(--w2-paper);font-weight:700;opacity:1}.w2-boost-arrow{color:var(--w2-amber);font-size:2em}.w2-boost-1{animation:w2-learner-arrive 7.2s ease-in-out 0s infinite;animation-fill-mode:both}.w2-boost-2{animation:w2-learner-arrive 7.2s ease-in-out 1.1s infinite;animation-fill-mode:both}.w2-boost-3{animation:w2-learner-arrive 7.2s ease-in-out 2.2s infinite;animation-fill-mode:both}.w2-points{display:flex;justify-content:center;gap:28px}.w2-point{width:40px;height:40px;border-radius:50%;border:4px solid var(--w2-forest);background-color:var(--w2-coral);opacity:1}.w2-fix-1{animation:w2-point-correct 7.2s ease-in-out 1.1s infinite;animation-fill-mode:both}.w2-fix-2{animation:w2-point-correct 7.2s ease-in-out 2.2s infinite;animation-fill-mode:both}.w2-fix-3{animation:w2-point-correct 7.2s ease-in-out 3.3s infinite;animation-fill-mode:both}
  @keyframes w2-tree-show{0%,10%{opacity:.18;transform:translateY(-16px);background-color:#fffaf0}28%,82%{opacity:1;transform:translateY(0);background-color:#f3d98f}100%{opacity:1;transform:translateY(0);background-color:#fffaf0}}
  @keyframes w2-bag-flow{0%,12%{opacity:.28;transform:translateX(-18px);background-color:#fffaf0}32%,82%{opacity:1;transform:translateX(0);background-color:#e5f0e9}100%{opacity:1;transform:translateX(0);background-color:#fffaf0}}
  @keyframes w2-vote-converge{0%,30%{opacity:.4;transform:scale(.92);background-color:#fffaf0}50%,88%{opacity:1;transform:scale(1.05);background-color:#f3d98f}100%{opacity:1;transform:scale(1);background-color:#fffaf0}}
  @keyframes w2-learner-arrive{0%,15%{opacity:.2;transform:translateX(-20px);background-color:#fffaf0}35%,85%{opacity:1;transform:translateX(0);background-color:#f3d98f}100%{opacity:1;transform:translateX(0);background-color:#fffaf0}}
  @keyframes w2-point-correct{0%,30%{opacity:1;transform:scale(1);background-color:#c8513a}52%,88%{opacity:1;transform:scale(1.22);background-color:#2f6b55}100%{opacity:1;transform:scale(1);background-color:#c8513a}}
  @media(prefers-reduced-motion:reduce){.w2-tree-1,.w2-tree-2,.w2-tree-3,.w2-tree-4,.w2-bag-a,.w2-bag-b,.w2-bag-c,.w2-vote,.w2-boost-1,.w2-boost-2,.w2-boost-3,.w2-fix-1,.w2-fix-2,.w2-fix-3{animation:none!important;opacity:1!important;transform:none!important}}
footer: "CYBER 207 · Week 02 · Trees, ensembles, neighbors & text"
---
<!-- _class: lead -->
# Trees → forests → boosting
## Then neighbors and a text baseline
CYBER 207 · Combined Week 2 + Week 4 live session · 90 minutes

---
# Learning outcomes
- Maintain train, validation, and test boundaries.
- Control one decision tree and interpret its local path.
- Distinguish parallel bagging from sequential boosting.
- Compare trees with scaled, weighted KNN.
- Build a sparse text baseline with Multinomial Naive Bayes.
- Report class-specific evidence, not accuracy alone.

---
# Agenda · 90 minutes
| Time | Focus |
|---:|---|
| 0–8 | Evaluation setup |
| 8–30 | One decision tree |
| 30–48 | Bagging and random forest |
| 48–53 | Break |
| 53–68 | Boosting |
| 68–78 | Concise KNN comparison |
| 78–86 | Naive Bayes for text |
| 86–90 | Implementation recap |

---
# Evaluation starts before modeling
<div class="w2-grid2">
<div class="w2-card"><strong>Train</strong><br>Fit transforms and model parameters.</div>
<div class="w2-card"><strong>Validation</strong><br>Select depth, tree count, learning rate, and neighborhood size.</div>
<div class="w2-card"><strong>Test</strong><br>Estimate final generalization once.</div>
<div class="w2-card"><strong>Groups or time</strong><br>Keep campaigns, senders, hosts, and future periods isolated.</div>
</div>

A stratified split preserves class ratios. A group-aware split prevents related records from crossing partitions.

---
# Security metrics expose different failures
| Evidence | Operational reading |
|---|---|
| Confusion matrix | Counts of each error type |
| Precision | Fraction of alerts that are truly suspicious |
| Recall | Fraction of suspicious records detected |
| F1 | Precision–recall balance |
| Balanced accuracy | Equal class weighting under imbalance |
| ROC AUC | Ranking quality across thresholds |

Always compare with `DummyClassifier` and include `classification_report`.

---
<!-- _class: lead -->
# One decision tree
## 8–30 minutes · learn a controlled sequence of tests

---
# A tree partitions feature space
At each node, the learner selects a feature and threshold that most reduces class mixture.

$$Gini=1-\sum_c p_c^2$$

Entropy provides another mixture score:

$$H=-\sum_c p_c\log_2 p_c$$

Both produce greedy local choices. Neither guarantees a globally optimal tree.

---
# Animated tree · splits build top-down
<div class="w2-tree-stage">
  <div class="w2-node w2-root w2-tree-1">domain age &lt; 30 days</div>
  <div class="w2-edge w2-edge-a w2-tree-2"></div><div class="w2-edge w2-edge-b w2-tree-2"></div>
  <div class="w2-node w2-left w2-tree-2">digit ratio &gt; .28</div><div class="w2-node w2-right w2-tree-2">token count &gt; 2</div>
  <div class="w2-edge w2-edge-c w2-tree-3"></div><div class="w2-edge w2-edge-d w2-tree-3"></div><div class="w2-edge w2-edge-e w2-tree-3"></div>
  <div class="w2-node w2-leaf-a w2-tree-4">suspicious leaf</div><div class="w2-node w2-leaf-b w2-tree-4">benign leaf</div><div class="w2-node w2-leaf-c w2-tree-4">review leaf</div>
</div>

<!-- Presenter note: Let two cycles run. Narrate the root, second-level tests, and leaves in order. All nodes remain visible without animation and in reduced-motion mode. -->

---
# Static tree reading · PowerPoint-safe
<div class="w2-card"><strong>Root population</strong> → domain-age split → branch-specific test → leaf class distribution</div>

| Element | Interpretation |
|---|---|
| Path | Local sequence of tests for one record |
| Leaf support | Training records reaching the leaf |
| Leaf proportion | Training class mix, not a causal probability |
| Early split | Strong greedy reduction in this sample |

A readable path can still be unstable under small data changes.

---
# Classification and regression trees
- `DecisionTreeClassifier` predicts class labels and class proportions.
- `DecisionTreeRegressor` predicts a numeric leaf average.
- Both create axis-aligned recursive partitions.
- Both can memorize small leaves when complexity is unconstrained.

Defensive uses include alert classification, score approximation, and triage-volume estimation.

---
# Tree overfitting controls
| Control | Effect |
|---|---|
| `max_depth` | Caps rule-chain length |
| `min_samples_split` | Requires support before another split |
| `min_samples_leaf` | Stabilizes terminal estimates |
| `max_features` | Restricts candidates per split |
| `ccp_alpha` | Prunes weak branches using complexity cost |
| `class_weight` | Changes relative class error cost |

Track training and validation curves. A widening gap indicates variance.

---
# `DecisionTreeClassifier` field guide
| Parameter | Practical use |
|---|---|
| `criterion` | `gini`, `entropy`, or `log_loss` scoring |
| `splitter` | `best` searches; `random` samples candidates |
| `max_depth` | hierarchy depth |
| `min_samples_split` | support required before splitting |
| `min_samples_leaf` | terminal support |
| `max_features` | features inspected per split |
| `class_weight` | class-sensitive fitting |
| `ccp_alpha` | pruning strength |
| `random_state` | reproducible randomized choices |

---
# Fit and evaluate one tree
```python
tree = DecisionTreeClassifier(
    criterion="gini", splitter="best", max_depth=4,
    min_samples_split=24, min_samples_leaf=12,
    max_features=None, class_weight="balanced",
    ccp_alpha=.001, random_state=207,
).fit(X_train, y_train)

pred = tree.predict(X_test)
print(classification_report(y_test, pred, zero_division=0))
ConfusionMatrixDisplay.from_predictions(y_test, pred)
```

---
<!-- _class: lead -->
# Bagging and random forest
## 30–48 minutes · stabilize trees through diversity

---
# Animated bagging · independent paths converge
<div class="w2-bag-stage">
  <div class="w2-sample w2-bag-a">Bootstrap A<br><small>rows resampled</small></div><div class="w2-flow w2-bag-a">→</div><div class="w2-mini-tree w2-bag-a">Tree A<br>vote 1</div><div class="w2-flow w2-bag-a">→</div>
  <div class="w2-sample w2-bag-b">Bootstrap B<br><small>rows resampled</small></div><div class="w2-flow w2-bag-b">→</div><div class="w2-mini-tree w2-bag-b">Tree B<br>vote 0</div><div class="w2-flow w2-bag-b">→</div>
  <div class="w2-sample w2-bag-c">Bootstrap C<br><small>rows resampled</small></div><div class="w2-flow w2-bag-c">→</div><div class="w2-mini-tree w2-bag-c">Tree C<br>vote 1</div><div class="w2-flow w2-bag-c">→</div>
  <div class="w2-vote">Majority<br>1 · suspicious</div>
</div>

<!-- Presenter note: Follow each row independently, then focus on the converging vote. Emphasize parallel fitting and error averaging. -->

---
# Static bagging recipe · PowerPoint-safe
| Stage | Operation | Main effect |
|---:|---|---|
| 1 | Draw bootstrap samples | Vary evidence per learner |
| 2 | Fit trees independently | Produce diverse errors |
| 3 | Vote or average | Cancel unstable variance |
| 4 | Evaluate OOB rows | Fast internal diagnostic |

Bagging helps when base learners are useful and not perfectly correlated.

---
# `BaggingClassifier` field guide
| Parameter | Practical use |
|---|---|
| `estimator` | base learner using current API naming |
| `n_estimators` | independent learner count |
| `max_samples` | rows drawn per learner |
| `max_features` | features drawn per learner |
| `bootstrap` | sample with replacement |
| `oob_score` | score omitted rows |
| `n_jobs` | parallel workers |
| `random_state` | reproducible samples |

```python
bag = BaggingClassifier(
  estimator=DecisionTreeClassifier(max_depth=5),
  n_estimators=120, max_samples=.8, max_features=.9,
  bootstrap=True, oob_score=True, n_jobs=-1, random_state=207)
```

---
# Random forest adds split-level diversity
<div class="w2-grid2">
<div class="w2-card"><strong>Bagging</strong><br>Different bootstrap rows for each tree.</div>
<div class="w2-card"><strong>Random forest</strong><br>Different rows plus a random feature subset at each split.</div>
</div>

Feature randomness prevents one dominant indicator from creating nearly identical trees.

---
# `RandomForestClassifier` field guide
| Parameter | Role | Parameter | Role |
|---|---|---|---|
| `n_estimators` | tree count | `criterion` | split score |
| `max_depth` | tree depth | `min_samples_leaf` | leaf support |
| `max_features` | split diversity | `class_weight` | class costs |
| `bootstrap` | row resampling | `oob_score` | omitted-row score |
| `n_jobs` | parallel work | `random_state` | reproducibility |

```python
forest = RandomForestClassifier(
  n_estimators=250, criterion="gini", max_depth=8,
  min_samples_leaf=5, max_features="sqrt",
  class_weight="balanced_subsample", bootstrap=True,
  oob_score=True, n_jobs=-1, random_state=207)
```

---
# OOB evidence and feature selection
A bootstrap sample omits some training rows. `oob_score_` summarizes predictions made only when a row was omitted.

Held-out permutation importance shuffles one feature and measures performance loss. `SelectFromModel` can retain important malware features, but selection must occur inside each training fold.

Importance is not causality. Correlated indicators can share or steal credit.

---
<!-- _class: lead -->
# Five-minute break
## 48–53 minutes · resume with sequential ensembles

---
<!-- _class: lead -->
# Boosting
## 53–68 minutes · add focused corrections in sequence

---
# Animated boosting · errors change state
<div class="w2-boost-stage">
  <div class="w2-learner w2-boost-1">Stump 1<br><small>coarse boundary</small></div><div class="w2-boost-arrow">→</div>
  <div class="w2-learner w2-boost-2">Stump 2<br><small>focus errors</small></div><div class="w2-boost-arrow">→</div>
  <div class="w2-learner w2-boost-3">Stump 3<br><small>refine score</small></div>
</div>
<div class="w2-points"><div class="w2-point w2-fix-1"></div><div class="w2-point w2-fix-2"></div><div class="w2-point w2-fix-3"></div><div class="w2-point"></div></div>

<!-- Presenter note: Read left to right. Coral points mark current mistakes; each delayed green state represents a corrected case. The static layout preserves all learners and points in PowerPoint. -->

---
# Static boosting sequence · PowerPoint-safe
| Round | Learner focus | Ensemble update |
|---:|---|---|
| 1 | Broad structure | Establish initial score |
| 2 | Current mistakes or residual loss | Add weighted correction |
| 3 | Remaining hard regions | Refine without replacement |

$$F_m(x)=F_{m-1}(x)+\eta h_m(x)$$

A smaller learning rate usually needs more stages. Excess capacity can chase noise.

---
# AdaBoost field guide
| Parameter | Practical use |
|---|---|
| `estimator` | shallow tree or stump |
| `n_estimators` | maximum sequential learners |
| `learning_rate` | contribution size per learner |
| `algorithm` | version-dependent and omitted when unavailable |
| `random_state` | reproducible learner behavior |

```python
ada = AdaBoostClassifier(
  estimator=DecisionTreeClassifier(max_depth=1, random_state=207),
  n_estimators=120, learning_rate=.55, random_state=207)
```

---
# Histogram gradient boosting field guide
| Parameter | Effect | Parameter | Effect |
|---|---|---|---|
| `learning_rate` | correction size | `max_iter` | stage count |
| `max_leaf_nodes` | learner complexity | `max_depth` | depth cap |
| `min_samples_leaf` | terminal support | `l2_regularization` | penalty |
| `early_stopping` | stalled-validation stop | `validation_fraction` | held-back share |
| `random_state` | reproducibility |  |  |

`HistGradientBoostingClassifier` bins numeric values for efficient tabular fitting.

---
# Parallel versus sequential ensembles
| | Bagging / forest | Boosting |
|---|---|---|
| Build order | independent, parallel | dependent, sequential |
| Diversity | resampled rows and features | remaining loss |
| Combination | vote or average | additive corrections |
| Typical benefit | variance reduction | bias reduction |
| Main risk | correlated trees | noise chasing |

---
<!-- _class: lead -->
# KNN comparison
## 68–78 minutes · local voting after global tree models

---
# KNN is a stored-neighborhood model
For a query vector, KNN measures distance, selects $k$ nearby training records, and votes.

$$d_p(x,z)=\left(\sum_j |x_j-z_j|^p\right)^{1/p}$$

- Standardize numeric features using training statistics only.
- `weights="distance"` gives nearer records more influence.
- Imbalance can dominate neighborhoods.
- High dimensions make distances less discriminative.
- Exact inference grows costly; approximate search trades exactness for latency.

---
# `KNeighborsClassifier` field guide
| Parameter | Role |
|---|---|
| `n_neighbors` | neighborhood size and smoothness |
| `weights` | uniform or distance-weighted vote |
| `metric` | distance function |
| `p` | Minkowski order; 1 Manhattan, 2 Euclidean |
| `algorithm` | search strategy |
| `leaf_size` | tree-search memory and speed |
| `n_jobs` | parallel neighbor lookup |

```python
knn = Pipeline([
 ("scale", StandardScaler()),
 ("model", KNeighborsClassifier(
   n_neighbors=15, weights="distance", metric="minkowski",
   p=2, algorithm="auto", leaf_size=30, n_jobs=-1))])
```

---
# KNN and trees make different commitments
| Dimension | KNN | Trees and ensembles |
|---|---|---|
| Representation | stored examples | learned split structure |
| Scaling | essential | usually unnecessary |
| Prediction cost | neighbor search | tree traversal |
| Boundary | local, distance-shaped | axis-aligned partitions |
| High dimensions | curse of dimensionality | feature selection at splits |
| Explanation | nearby cases | paths, votes, importance |

---
<!-- _class: lead -->
# Naive Bayes for text
## 78–86 minutes · sparse counts and a probabilistic baseline

---
# From messages to nonnegative features
1. Tokenize original synthetic message strings.
2. `CountVectorizer` builds sparse token or n-gram counts.
3. `TfidfVectorizer` downweights terms common across documents.
4. `MultinomialNB` estimates class-conditional token evidence.

The model assumes conditional feature independence given the class. A chi-square test can measure token–label dependence for screening, not causal meaning.

---
# Multinomial text pipeline
```python
text_model = Pipeline([
 ("vectorizer", CountVectorizer(
   lowercase=True, ngram_range=(1, 2), min_df=1)),
 ("model", MultinomialNB(
   alpha=1.0, force_alpha=True,
   fit_prior=True, class_prior=None))
])
text_model.fit(message_train, label_train)
```

| Parameter | Role |
|---|---|
| `alpha` | additive smoothing strength |
| `force_alpha` | preserve requested smoothing |
| `fit_prior` | learn class prevalence |
| `class_prior` | supply explicit class probabilities |

---
<!-- _class: lead -->
# Implementation recap
## 86–90 minutes · one workflow, several model families

---
# End-to-end sklearn pattern
```python
X_train, X_test, y_train, y_test = train_test_split(
  X, y, stratify=y, test_size=.25, random_state=207)
model.fit(X_train, y_train)
pred = model.predict(X_test)
print(classification_report(y_test, pred, zero_division=0))
ConfusionMatrixDisplay.from_predictions(y_test, pred)
params = model.get_params(deep=True)
```

For related records, replace stratification with group-aware splitting. Keep preprocessing, feature selection, and fitting inside the fold.

---
# Model selection summary
- Start with a dummy baseline and class-specific metrics.
- Control one tree before adding an ensemble.
- Use bagging and forests for diverse parallel trees.
- Use boosting for deliberate sequential correction.
- Scale KNN inside a pipeline and monitor dimensionality and latency.
- Use count-based Naive Bayes as a compact sparse-text baseline.
- Treat synthetic results and importance scores as teaching evidence only.

---
# Companion notebook
The offline lab reproduces the lesson with synthetic defensive features:

`notebooks/week-02-trees-bagging-boosting.ipynb`

It includes tree visualization, depth curves, OOB scoring, permutation importance, ensemble comparisons, scaled KNN, and synthetic-message Naive Bayes.

---
# References · primary APIs
- scikit-learn guides for model selection, trees, ensembles, neighbors, text feature extraction, and Naive Bayes
- scikit-learn API references for the estimator parameters shown in this deck

All language, diagrams, examples, and animations are original. No live URLs, external datasets, downloaded assets, or gradient-descent optimizer variants are used.
