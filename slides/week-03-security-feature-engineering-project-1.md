---
marp: true
theme: default
paginate: true
math: katex
style: |
  :root{--forest:#123c32;--moss:#2f6b55;--amber:#e0a126;--cream:#f7f1df;--ink:#17302a;--coral:#c8513a}
  section{background:var(--cream);color:var(--ink);font-family:"Segoe UI",Arial,sans-serif;padding:46px 64px;font-size:29px}
  section.lead{background:linear-gradient(135deg,var(--forest),#205d49);color:#fff;border-bottom:12px solid var(--amber)}
  h1,h2{color:var(--forest);letter-spacing:-.03em}section.lead h1,section.lead h2{color:#fff}h1{font-size:2.15em}h2{font-size:1.35em}strong{color:var(--coral)}
  code{background:#102b25;color:#fff8e8;border-radius:8px}table{font-size:.68em;background:#fffaf0}.card{background:#fffaf0;border-left:7px solid var(--amber);padding:20px 26px;border-radius:14px}
  .pipe{display:flex;align-items:center;justify-content:center;gap:14px;margin-top:60px}.stage{padding:18px;border:3px solid var(--moss);border-radius:14px;background:#fffaf0}.packet{color:var(--amber);font-size:2em;animation:packetFlow 5s linear infinite}
  .ablate{display:flex;align-items:flex-end;justify-content:center;gap:35px;height:260px}.bar{width:120px;background:var(--moss);color:white;text-align:center;padding-top:10px;border-radius:12px 12px 0 0;animation:barCompare 4s ease-in-out infinite}.bar.cut{background:var(--coral);animation-delay:.6s}
  @keyframes packetFlow{0%{transform:translateX(-30px);opacity:.35}50%{transform:translateX(20px);opacity:1}100%{transform:translateX(55px);opacity:.35}}
  @keyframes barCompare{0%,100%{height:110px}50%{height:210px}}
  @media(prefers-reduced-motion:reduce){.packet,.bar{animation:none!important}.bar{height:180px}.bar.cut{height:120px}}
footer: "CYBER 207 · Week 03 · Feature engineering · Project 1"
---
<!-- _class: lead -->
# Security feature engineering
## Point-in-time pipelines and Project 1
CYBER 207 · Week 03 · 90 minutes

---
# Measurable outcomes
- Identify numeric, categorical, temporal, missing, text, URL, process, flow, and behavioral features.
- Build train-fitted transformations in a reproducible pipeline.
- detect leakage with point-in-time reasoning.
- run a feature ablation and interpret changes.
- produce Project 1’s feature pipeline and data sheet.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Numeric, categorical, temporal, missing data |
| 20–40 | Text, URL, process, flow, behavioral features |
| 40–55 | Normalization, encoding, pipelines *(50–55 break)* |
| 55–70 | Leakage and point-in-time correctness |
| 70–90 | Feature ablation workshop / Project 1 |

---
# Visual recap · reproducible evidence
<div class="card">

Train-only statistics · entity-isolated folds · held-out importance · prediction-time availability

</div>

Every engineered value carries a timestamp, owner, unit, and transformation.

---
# Features are measurement choices
Raw telemetry is not automatically model-ready.

A feature pipeline claims:
- what is measured;
- over what window;
- relative to which entity;
- using which historical state;
- how missingness and categories are represented.

Those claims must be replayable at prediction time.

---
# Synthetic login table
| event_time | country | failed_1h | new_device | user_age_days | label_7d |
|---|---|---:|---|---:|---|
| 09:12 | US | 1 | no | 450 | benign |
| 02:03 | CA | 8 | yes | 12 | takeover |
| 18:44 | missing | 3 | yes | 900 | benign |

Local and synthetic; no real identities.

---
# Numeric features
Examples:
- failed attempts in trailing hour;
- unique source networks in 24 hours;
- URL length;
- bytes sent/received ratio;
- account age.

Check units, skew, outliers, impossible values, and whether aggregation uses only past events.

---
# Categorical features
Examples: device type, protocol, parent process, country bucket.

Encoding options:
- one-hot for bounded categories;
- frequency/count encoding fit on train;
- grouped “other/unknown” bucket;
- learned embeddings later.

Never assign arbitrary numeric order unless order is meaningful.

---
# Temporal features
From a timestamp, derive only defensible context:
- hour-of-day, day-of-week;
- time since prior event;
- trailing-window counts;
- deviation from entity’s past pattern.

Cyclic encoding:
$$h_{sin}=\sin(2\pi h/24),\quad h_{cos}=\cos(2\pi h/24)$$

---
# Missingness is information—and risk
Options:
- impute median/mode fit on training data;
- add a missing indicator;
- preserve an explicit unknown category;
- drop only with documented rationale.

Missingness may reflect sensor outages, product tiers, or collection changes—not attacker behavior.

---
# Guided inventory · 5 minutes
For each field in the synthetic login table label:
- raw type;
- proposed representation;
- prediction-time availability;
- likely failure mode;
- owner of the upstream sensor.

Circle any feature whose meaning could drift.

---
# Text and URL features
Defensive examples:
- character or token $n$-grams;
- length, entropy, digit fraction, path depth;
- vocabulary counts/TF–IDF;
- embedding vector from local text;
- structured parser outputs.

Avoid live navigation; supplied strings are inert samples.

---
# TF–IDF intuition
For term $t$ in document $d$:
$$\operatorname{tfidf}(t,d)=\operatorname{tf}(t,d)\log\frac{N}{\operatorname{df}(t)}$$

Frequent in this document, rare across documents → larger weight.

Fit vocabulary and document frequencies on training text only.

---
# Process-tree features
Examples:
- parent/child executable category;
- depth and branching count;
- signer/trust bucket;
- command-line token patterns;
- temporal sequence around process start.

Prefer behaviorally meaningful abstractions over exact hashes that merely memorize samples.

---
# Network-flow features
Examples:
- duration and byte/packet counts;
- inbound/outbound ratio;
- inter-arrival statistics;
- unique destinations in trailing window;
- protocol/port category.

Collection position and sampling rate define what these numbers mean.

---
# Behavioral aggregation
For entity $u$ and time $t$:
$$x_{u,t}=\sum_{i}\mathbf{1}[u_i=u\land t-W<t_i\le t]$$

This trailing-window count excludes the future.

Document inclusivity at boundaries and late-arriving events.

---
# Animated pipeline discipline
<div class="pipe"><div class="stage">raw</div><span class="packet">◆</span><div class="stage">split</div><span class="packet">◆</span><div class="stage">fit transforms</div><span class="packet">◆</span><div class="stage">model</div></div>

Transforms learn state only from each training fold. Validation and test data move through frozen state.

<!-- Presenter note: Follow the amber packet left to right. Pause at split to state that learned preprocessing begins only inside training folds. -->

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 at the pipeline transition
On return: identify one statistic that must never be learned from the full dataset.

---
# Normalization and encoding
Standardization:
$$z=(x-\mu_{train})/\sigma_{train}$$

One-hot encoding:
- train learns allowed categories;
- unseen category maps to `unknown`/ignored according to documented policy;
- feature order remains stable.

A pipeline is code **plus fitted state**.

---
# Point-in-time correctness
Apply this test to every row-feature pair:
> This exact value can be computed at prediction timestamp $t$ using only data available by $t$.

If no, the offline model is solving a different, easier problem.

---
# Four leakage patterns
1. **Target leakage:** `case_closed_reason` predicts disposition.
2. **Future window:** “next 24h failures” predicts current risk.
3. **Global fit:** vocabulary/scaler sees test period.
4. **Entity bleed:** same campaign variants across folds.

Leakage can produce plausible—not obviously perfect—scores.

---
# Data sheet essentials
Record:
- source and collection interval;
- row/unit and population;
- label definition and delay;
- feature windows and availability;
- missingness and exclusions;
- split/group rules;
- known blind spots and intended use.

---
# Ablation isolates evidence contribution
Train/evaluate controlled variants:
- full feature set;
- minus temporal context;
- minus entity-history features;
- URL lexical only;
- workload context only.

Keep split, seed, model family, and threshold-selection procedure fixed.

---
# Animated ablation comparison
<div class="ablate"><div class="bar">full</div><div class="bar cut">minus history</div><div class="bar">lexical</div></div>

If removing feature group $g$ changes metric $M$:
$$\Delta M=M_{full}-M_{-g}$$

The moving bars emphasize sensitivity, not causality. Correlated substitutes and interactions can redistribute contribution.

Report uncertainty across folds and inspect which errors changed—not just one number.

<!-- Presenter note: Describe one controlled removal at a time. The static reduced-motion state keeps all three variants comparable. -->

---
# Guided activity · feature ablation workshop
Teams receive fold results for four feature sets.
1. Rank by validation PR-AUC and variability.
2. Inspect two changed false negatives.
3. identify likely leakage/proxy risks.
4. recommend a minimal feature set.
5. draft a one-sentence intended-use statement.

Time: 12 minutes + 5-minute debrief.

---
# Operational tradeoffs
- Rich history may improve scores but increase latency and missingness.
- Text features can be sparse and vocabulary-dependent.
- Behavioral features generalize better but cost more to compute.
- Minimal pipelines are easier to audit and reproduce.
- Feature freshness and sensor ownership matter as much as model choice.

---
# scikit-learn implementation
```python
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder, StandardScaler, FunctionTransformer
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.inspection import permutation_importance

num = Pipeline([("impute", SimpleImputer(strategy="median")), ("scale", StandardScaler())])
cat = Pipeline([("impute", SimpleImputer(strategy="most_frequent")), ("onehot", OneHotEncoder(handle_unknown="ignore"))])
prep = ColumnTransformer([("num", num, numeric_cols), ("cat", cat, categorical_cols), ("text", TfidfVectorizer(min_df=2), "message")])
model = Pipeline([("prep", prep), ("clf", classifier)])
```

---
# scikit-learn field guide
| API / parameter | Effect of changing it |
|---|---|
| `SimpleImputer(strategy, add_indicator)` | changes fill rule and exposes missingness |
| `OneHotEncoder(handle_unknown, min_frequency)` | controls unseen and rare categories |
| `StandardScaler(with_mean)` | controls centering; keep false for sparse matrices |
| `TfidfVectorizer(ngram_range, min_df, max_features)` | changes phrase scope, rarity floor, and width |
| `FunctionTransformer(func, validate)` | wraps deterministic custom transforms |
| `ColumnTransformer(transformers, remainder)` | routes columns and keeps or drops leftovers |
| `Pipeline` parameter names | use `prep__num__impute__strategy` and `clf__C` for leakage-safe tuning |
| `permutation_importance(n_repeats, scoring)` | changes shuffle repetitions and evaluation objective |

---
# Project 1 · acceptance checklist
- Same input schema yields same ordered features.
- Unknown/missing values follow documented behavior.
- All fitted transforms use training folds only.
- Results include variability, not one lucky split.
- Recommendation names an operational decision.
- README states how to reproduce locally.

---
# Takeaway · Project 1 evidence bundle
1. Freeze point-in-time feature definitions.
2. Fit vocabulary, imputation, encoding, and scaling inside folds.
3. Keep split, seed, model, and metric fixed during ablation.
4. Preserve feature names and fitted versions.
5. Test missing and unseen-category behavior locally.

---
# Next week continuity
Week 4 turns engineered evidence into calibrated probabilistic decisions with logistic regression.

Bring:
- fixed feature schema;
- split discipline;
- baseline results;
- data sheet.

Next, calibrated scores connect engineered evidence to bounded actions.

---
# References · official docs and inspiration
- https://scikit-learn.org/stable/modules/compose.html
- https://scikit-learn.org/stable/modules/preprocessing.html
- https://scikit-learn.org/stable/modules/feature_extraction.html
- https://scikit-learn.org/stable/modules/permutation_importance.html

Visual-explainer acknowledgments, inspiration only: **Distill**, **MLU-Explain**, **Deep Learning for Cyber**, and official scikit-learn examples. All text, diagrams, and motion are original.
