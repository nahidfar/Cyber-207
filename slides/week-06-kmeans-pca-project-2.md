---
marp: true
theme: default
paginate: true
math: katex
style: |
  :root{--forest:#123c32;--moss:#2f6b55;--amber:#e0a126;--cream:#f7f1df;--ink:#17302a;--coral:#c8513a}
  section{background:var(--cream);color:var(--ink);font-family:"Segoe UI",Arial,sans-serif;padding:46px 64px;font-size:29px}section.lead{background:linear-gradient(135deg,var(--forest),#205d49);color:#fff;border-bottom:12px solid var(--amber)}
  h1,h2{color:var(--forest);letter-spacing:-.03em}section.lead h1,section.lead h2{color:#fff}h1{font-size:2.15em}h2{font-size:1.35em}strong{color:var(--coral)}code{background:#102b25;color:#fff8e8;border-radius:8px}table{font-size:.68em;background:#fffaf0}.card{background:#fffaf0;border-left:7px solid var(--amber);padding:20px 26px;border-radius:14px}
  .space{position:relative;height:275px;background:#fffaf0;border:3px solid var(--moss);border-radius:20px}.dot{position:absolute;width:20px;height:20px;border-radius:50%;background:var(--moss)}.dot.b{background:var(--coral)}.centroid{position:absolute;font-size:2em;color:var(--amber);font-weight:900;animation:centroidMove 5s ease-in-out infinite}
  .projection{position:relative;height:260px;margin:15px 8%;border-left:5px solid var(--forest);border-bottom:5px solid var(--forest)}.axis{position:absolute;left:12%;top:48%;width:78%;height:8px;background:var(--amber);transform:rotate(-24deg)}.project-dot{position:absolute;width:22px;height:22px;background:var(--coral);border-radius:50%;animation:projectDown 4s ease-in-out infinite}
  @keyframes centroidMove{0%{left:12%;top:25%}50%{left:34%;top:52%}100%{left:56%;top:32%}}
  @keyframes projectDown{0%{left:28%;top:20%}55%,100%{left:43%;top:52%}}
  @media(prefers-reduced-motion:reduce){.centroid,.project-dot{animation:none!important}.centroid{left:34%;top:52%}.project-dot{left:43%;top:52%}}
footer: "CYBER 207 · Week 06 · k-means, PCA · Project 2"
---
<!-- _class: lead -->
# Unsupervised learning: k-means & PCA
## Discovering synthetic phishing-campaign structure
CYBER 207 · Week 06 · 90 minutes

---
# Measurable outcomes
- Explain the k-means objective and assignment/update loop.
- choose $k$ using evidence rather than aesthetics.
- describe PCA as variance-maximizing linear projection.
- distinguish unusual points from confirmed threats.
- deliver Project 2 cluster profiles with caveats.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Clustering objectives and k-means |
| 20–40 | Choosing $k$ and cluster validation |
| 40–60 | Variance, principal components, projection *(50–55 break)* |
| 60–75 | Anomaly detection and limitations |
| 75–90 | Cluster phishing campaigns / Project 2 |

---
# Visual recap · labeled evidence
<div class="card">

Rare-class metrics · macro averaging · reference-fitted scaling · explicit unknown handling

</div>

Week 6 removes labels while retaining the same discipline around representation and claims.

---
# Unsupervised does not mean objective
Without outcome labels, the algorithm still reflects choices:
- feature representation;
- scaling and distance;
- number of clusters;
- sample period and exclusions;
- validation criteria.

Clusters are hypotheses for investigation—not ground truth.

---
# Synthetic campaign table
Each row is a sanitized email campaign aggregate:
- median URL length;
- sender-domain age bucket;
- template similarity score;
- recipient count;
- attachment indicator;
- sending-hour entropy.

No live links, message bodies, or real identities.

---
# k-means objective
Partition points into $K$ clusters minimizing within-cluster squared distance:
$$J=\sum_{i=1}^{n}\lVert\mathbf{x}_i-\boldsymbol{\mu}_{c_i}\rVert_2^2$$

$\boldsymbol{\mu}_{c_i}$ is the centroid assigned to point $i$.

---
# Animated centroid update
<div class="space"><span class="dot" style="left:18%;top:30%"></span><span class="dot" style="left:30%;top:62%"></span><span class="dot" style="left:42%;top:45%"></span><span class="dot b" style="left:68%;top:28%"></span><span class="dot b" style="left:80%;top:62%"></span><span class="centroid">✦</span></div>

Assignment maps each point to its nearest centroid; update moves each centroid to the assigned mean.

<!-- Presenter note: Trace the amber centroid toward the local mean, then explain repeated assign-update cycles. The endpoint remains readable in static export. -->

---
# Scaling defines geometry
Recipient count may range to thousands; attachment flag is 0/1.

Without scaling, large-range dimensions dominate Euclidean distance.

Use train/reference-period statistics and document transforms. Heavy skew may call for $\log(1+x)$ before standardization.

---
# Worked assignment
Centroids: $\mu_A=(0,0)$, $\mu_B=(3,3)$; campaign $x=(1,2)$.

$$d_A=\sqrt{1^2+2^2}=\sqrt5$$
$$d_B=\sqrt{(-2)^2+(-1)^2}=\sqrt5$$

Tie reveals ambiguity; initialization and tie policy can matter.

---
# k-means assumptions
Works best when clusters are:
- roughly spherical in chosen feature space;
- similar scale/density;
- summarized meaningfully by a mean;
- separated under Euclidean distance.

Categorical-heavy or irregular structure may violate these assumptions.

---
# Choosing $K$: elbow
Plot within-cluster sum of squares versus $K$.

Look for diminishing improvement—not a guaranteed elbow.

Larger $K$ always lowers training objective; the question is whether added partitions are stable, useful, and interpretable.

---
# Silhouette score
For point $i$:
- $a(i)$: average distance within its cluster;
- $b(i)$: smallest average distance to another cluster.

$$s(i)=\frac{b(i)-a(i)}{\max(a(i),b(i))}$$

Near 1 is separated; near 0 is boundary; negative suggests mismatch.

---
# Validate clusters from several angles
- internal: inertia, silhouette;
- stability: rerun seeds/bootstrap/time slices;
- external: later analyst tags if available;
- utility: profiles support a bounded investigation;
- interpretability: feature differences remain descriptive without invented labels.

---
# Worked $K$ decision
Candidate results:
| $K$ | silhouette | stability | analyst usefulness |
|---:|---:|---:|---|
| 2 | .41 | high | too broad |
| 3 | .47 | high | useful |
| 5 | .51 | low | fragmented |

$K=3$ balances separation, stability, and analyst utility. The larger $K=5$ silhouette does not offset fragile fragments.

---
# Animated PCA projection
<div class="projection"><div class="axis"></div><div class="project-dot"></div></div>

PCA rotates centered data to orthogonal axes and projects each point onto the retained subspace.

First component:
$$\mathbf{v}_1=\arg\max_{\lVert v\rVert=1}\operatorname{Var}(X\mathbf{v})$$

Projection scores: $Z=XW$ after centering (and often scaling).

<!-- Presenter note: Follow the coral observation onto the amber principal axis. The settled projection is the PowerPoint-readable state. -->

---
# What a component means
A component is a weighted combination:
$$PC_1=.6z_{url\_length}+.5z_{recipients}-.4z_{domain\_age}+\cdots$$

Interpret using loadings, not by naming it “maliciousness.”

Sign can flip without changing the represented axis.

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 within PCA
Return with one reason a two-dimensional plot can mislead.

---
# Explained variance
For eigenvalues $\lambda_j$:
$$r_j=\frac{\lambda_j}{\sum_k\lambda_k}$$

Cumulative explained variance helps choose dimensions for compression, but retained variance is not retained predictive/security value.

Low-variance signals can still matter.

---
# PCA for visualization
A 2D PCA scatterplot is a projection.

Check:
- percent variance shown;
- loadings;
- overlap hidden in omitted dimensions;
- scale and outlier sensitivity;
- whether colors are post hoc labels.

Visual separation is exploratory evidence only.

---
# Reconstruction error as unusualness
With retained components $W_q$:
$$\hat X=XW_qW_q^\top,\qquad e_i=\lVert x_i-\hat x_i\rVert$$

Large error means poorly represented by learned subspace.

It does **not** mean malicious; sensor faults and rare benign campaigns can be unusual.

---
# Anomaly detection · name the reference
“Anomalous” remains relative to a declared population, period, entity baseline, feature geometry, threshold, and review capacity.

A defensible anomaly workflow requires review and feedback labels.

---
# Guided activity · cluster campaigns
Teams receive 15 synthetic campaign rows, cluster assignments, PCA scores, and profiles.
1. name each cluster descriptively;
2. identify defining feature differences;
3. inspect one boundary/outlier case;
4. propose one follow-up query;
5. state one unsupported conclusion.

---
# Cluster profile template
**Cluster 2: high-volume, young-domain campaigns**
- support: 4 campaigns;
- above reference median: recipients, URL length;
- below reference median: domain age;
- boundary cases: C07;
- follow-up: compare later confirmed dispositions;
- caveat: no claim of common actor or maliciousness.

---
# Operational tradeoffs
- Smaller $K$: stable summaries, broad mixtures.
- Larger $K$: detailed but fragile fragments.
- PCA compresses and denoises but obscures original units.
- Outlier review consumes analyst capacity.
- Unsupervised outputs are strongest as discovery aids, not autonomous verdicts.

---
# Common failure modes
1. Skip scaling.
2. choose $K$ from a pretty plot.
3. Name clusters as threats without labels.
4. Treat PCA axes as causal factors.
5. Call every outlier malicious.
6. Ignore initialization/stability.
7. Fit reference geometry using future periods.

---
# scikit-learn implementation
```python
from sklearn.cluster import KMeans, MiniBatchKMeans
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline
from sklearn.metrics import silhouette_score, adjusted_rand_score

study = Pipeline([("scale", StandardScaler()),
                  ("cluster", KMeans(n_clusters=3, n_init="auto", random_state=207))])
labels = study.fit_predict(X_reference)
sil = silhouette_score(study["scale"].transform(X_reference), labels)
pca = PCA(n_components=2, whiten=False, random_state=207)
Z = pca.fit_transform(study["scale"].transform(X_reference))
stability = adjusted_rand_score(labels, rerun_labels)
mini = MiniBatchKMeans(n_clusters=3, batch_size=256, random_state=207)
```

---
# scikit-learn field guide
| API / parameter | Effect of changing it |
|---|---|
| `KMeans(n_clusters, init, n_init)` | changes partition count, seeding, and restart robustness |
| `KMeans(max_iter, tol)` | changes convergence budget and stopping tolerance |
| `MiniBatchKMeans(batch_size, reassignment_ratio)` | trades speed, update noise, and low-count reassignment |
| `PCA(n_components)` | selects dimensions or retained variance fraction |
| `PCA(whiten)` | normalizes component variance while discarding scale information |
| `silhouette_score(metric)` | evaluates cohesion and separation under a distance |
| `adjusted_rand_score` | compares two partitions while correcting chance agreement |

---
# Next week continuity
Week 7 integrates classical ML end to end and reviews for the midterm.

Bring:
- Project 1 pipeline discipline;
- supervised metrics;
- Project 2 unsupervised caveats;
- examples of errors and instability.

Next, method selection follows the decision and evidence constraints.

---
# References · official docs and inspiration
- https://scikit-learn.org/stable/modules/clustering.html
- https://scikit-learn.org/stable/modules/decomposition.html#pca
- https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html
- https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_rand_score.html

Visual-explainer acknowledgments, inspiration only: **Distill**, **MLU-Explain**, **Deep Learning for Cyber**, and official scikit-learn examples. All geometry, wording, and animation are original.
