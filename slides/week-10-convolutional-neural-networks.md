---
marp: true
theme: default
paginate: true
math: katex
style: |
  :root { --forest:#174c3c; --amber:#e4a11b; --cream:#f4f1e8; --ink:#17201d; --paper:#fffdf7; --sage:#dce7e1; --signal:#e5482f; }
  section { background:var(--cream); color:var(--ink); font-family:Arial,sans-serif; padding:52px 68px; font-size:27px; }
  section.lead { background:#174c3c; color:#fff; border-bottom:12px solid #e4a11b; }
  h1,h2 { color:var(--forest); letter-spacing:-.025em; } h1 { font-size:1.9em; } section.lead h1,section.lead h2 { color:#fff; }
  strong { color:var(--signal); } table { font-size:.72em; background:var(--paper); } th { background:var(--forest); color:#fff; } blockquote,.card { border-left:6px solid var(--amber); background:var(--paper); padding:.55em .85em; border-radius:8px; }
  .scan { position:relative; display:grid; grid-template-columns:repeat(8,48px); gap:8px; width:max-content; margin:38px auto; } .scan i { width:48px; height:48px; background:var(--sage); border:2px solid var(--forest); } .kernel { position:absolute; left:0; top:-8px; width:160px; height:64px; border:6px solid var(--amber); animation:kernelSlide 4s ease-in-out infinite; } .map { height:22px; width:70%; margin:auto; background:linear-gradient(90deg,var(--forest),var(--amber)); animation:mapGlow 4s ease-in-out infinite; }
  @keyframes kernelSlide { 0%,100% { transform:translateX(0); } 50% { transform:translateX(280px); } }
  @keyframes mapGlow { 0%,100% { opacity:.45; transform:scaleX(.75); } 50% { opacity:1; transform:scaleX(1); } }
  @media (prefers-reduced-motion:reduce) { .kernel,.map { animation:none; } }
footer: "CYBER 207 · Week 10 · CNNs"
---
<!-- _class: lead -->
# Convolutional neural networks
## Local patterns across bytes, traffic, and images
CYBER 207 · Week 10 · 90 minutes

---
# Measurable outcomes
- Compute a simple convolution output.
- explain filters, weight sharing, receptive fields, and pooling.
- choose 1D versus 2D CNN representation.
- trace tensor shapes through a CNN.
- defend architecture and error-analysis choices.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Convolution, filters, receptive fields |
| 20–40 | Pooling, invariance, feature maps |
| 40–60 | 1D versus 2D CNNs for bytes, traffic, images *(50–55 break)* |
| 60–75 | Architecture and tensor-shape design |
| 75–90 | Malware CNN error-analysis studio |

---
# Visual recap · shared local filters
<div class="scan"><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><b class="kernel"></b></div>
<div class="map"></div>

A compact kernel scans ordered values and emits a feature map. Static export preserves the kernel at its initial position.

<!-- _notes: Emphasize that weight sharing is useful only when the same local relation can recur across positions. -->

---
# Convolution intuition
A small learned filter slides across an ordered input.

The same weights detect a local pattern wherever it occurs.

Benefits:
- parameter sharing;
- local connectivity;
- hierarchical feature composition;
- efficient pattern reuse.

---
# 1D discrete convolution
For input $x$ and kernel $w$ of width $K$:
$$y_t=b+\sum_{k=0}^{K-1}w_kx_{t+k}$$

Deep-learning libraries often implement cross-correlation (no kernel reversal) but call it convolution.

---
# Worked 1D filter
Input $x=[1,2,0,3]$, kernel $w=[1,-1]$, bias $0$, stride $1$.

Outputs:
- $1-2=-1$;
- $2-0=2$;
- $0-3=-3$.

Feature map: $[-1,2,-3]$ before activation.

---
# Channels and filters
Input can have $C_{in}$ channels; each filter spans all input channels and emits one output channel.

For $C_{out}$ filters:
$$W\in\mathbb{R}^{C_{out}\times C_{in}\times K}$$

Each feature map responds to a learned local pattern.

---
# Receptive field
The receptive field is the input region that can influence one activation.

Stacking convolutions grows context:
- two width-3, stride-1 layers → receptive field 5;
- pooling/stride grows it faster.

Architecture must match pattern length relevant to the task.

---
# Padding and stride
Output length for 1D convolution:
$$L_{out}=\left\lfloor\frac{L_{in}+2P-D(K-1)-1}{S}+1\right\rfloor$$

$P$: padding, $S$: stride, $D$: dilation.

Trace dimensions before coding.

---
# Guided calculation
For $L_{in}=20$, $K=5$, $P=2$, $S=1$, $D=1$:
- output length is $20$;
- changing stride to $2$ produces length $10$;
- downsampling removes positional resolution;
- edge padding is valid only when the representation gives it a defensible meaning.

---
# Pooling
Max pooling summarizes strongest local activation; average pooling summarizes local mean.

Effects:
- reduces resolution and compute;
- provides limited shift tolerance;
- enlarges effective receptive field;
- can discard precise position/timing.

Pooling is an information tradeoff, not automatic improvement.

---
# Invariance versus equivariance
Convolution is approximately translation **equivariant**: shift input → shift feature map.

Pooling/global aggregation can make final prediction more **invariant** to small shifts.

Ask whether position should matter: protocol-field location may carry meaning.

---
# Feature-map interpretation
A high activation means the learned filter matched some local pattern.

It does not by itself prove:
- a known signature;
- malicious intent;
- causal importance.

Use controlled probes, ablations, and error cases.

---
# 1D CNN representations
Suitable for ordered sequences:
- byte windows;
- packet-size/direction sequences;
- token embeddings;
- API-call sequences;
- time-binned event channels.

Preserve ordering and define truncation/padding policy.

---
# 2D CNN representations
Suitable when both axes have meaningful local adjacency:
- actual images;
- time × feature matrices;
- carefully justified byte layouts.

Turning bytes into a square image invents vertical neighbors at row boundaries; document that artifact.

---
# Representation decision
| Data | Candidate | Decision cue |
|---|---|---|
| packet sequence | 1D CNN | local order is meaningful |
| spectrogram | 2D CNN | time and frequency neighborhoods matter |
| tabular aggregates | MLP/tree | spatial locality is absent |
| byte image | 1D/2D compare | artificial adjacency must be measured |

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 within representation comparison
Return ready to defend one axis of locality in your data.

---
# Synthetic malware-image study
Provided local grayscale arrays are generated patterns labeled into benign-like and three simulated families.

Purpose: compare representations and error behavior.

They are not executable files, real malware, or actor attribution evidence.

---
# Architecture sketch
Example:
`[B,1,64,64] → Conv(16,3) → ReLU → Pool(2) → Conv(32,3) → ReLU → GlobalAvg → Linear(4)`

For each stage, record:
- tensor shape;
- receptive field;
- parameter count;
- reason for downsampling.

---
# Parameter count
Conv2D parameters:
$$C_{out}(C_{in}K_hK_w+1)$$

For 16 filters, 1 input channel, $3\times3$ kernel:
$$16(1\cdot3\cdot3+1)=160$$

Weight sharing makes this independent of image width/height.

---
# Guided shape activity
Input `[32,1,64,64]`; conv $3\times3$, padding 1, 16 filters; max-pool 2; second conv 32 filters.

Write each output shape and compute first-layer parameters.

Then identify where most information is discarded.

---
# Architecture tradeoffs
- More filters: more patterns, compute, overfit risk.
- Larger kernels: broader local context, more parameters.
- Deeper stack: hierarchical features, harder optimization.
- Global pooling: fewer parameters, less location detail.
- Transfer learning: useful only if source representation transfers.

---
# Error-analysis studio
Teams receive confusion matrix, curves, and six synthetic samples.
1. select two costly confusions;
2. compare CNN with MLP baseline;
3. inspect whether errors cluster by size/padding;
4. propose one representation ablation;
5. write a bounded claim.

---
# Useful ablations
- shuffle sequence order;
- 1D versus 2D layout;
- remove pooling;
- vary truncation window;
- zero a channel;
- compare global-average with dense head.

Each ablation tests a representation/architecture claim.

---
# Operational tradeoffs
- CNNs are efficient when local stationarity is real.
- Truncation may omit decisive late evidence.
- Padding can become a shortcut.
- Image conversion may introduce artificial geometry.
- Better average score may hide family-specific regressions.

---
# Common failure modes
1. Wrong tensor channel order.
2. Shape mismatch after pooling.
3. Train/eval mode confusion.
4. Random split leaks related samples.
5. CNN used for unordered tabular columns.
6. Visualization mistaken for explanation.
7. No representation baseline.

---
# Deliverable · CNN experiment
Include:
- representation rationale and local-data statement;
- architecture with shape/receptive-field table;
- training curves and regularization;
- MLP/classical baseline;
- at least one representation ablation;
- held-out metrics and error examples;
- limitations and next experiment.

---
# Framework boundary · sklearn and CNNs
**scikit-learn does not provide CNN estimators.** Use it for preprocessing, splitting, baselines, and evaluation.

```python
baseline = Pipeline([
  ("scale", StandardScaler()),
  ("clf", LogisticRegression(max_iter=1000, random_state=7))
])
baseline.fit(X_train_flat, y_train)
print(classification_report(y_test, baseline.predict(X_test_flat)))
```

CNN layers require a deep-learning dependency such as Keras or PyTorch.

---
# Keras-style CNN pseudocode · separate dependency
```python
model = Sequential([
  Conv1D(filters=32, kernel_size=5, strides=1,
         padding="same", activation="relu"),
  MaxPooling1D(pool_size=2),
  Conv1D(filters=64, kernel_size=3, activation="relu"),
  GlobalAveragePooling1D(), Dense(n_classes, activation="softmax")
])
```

Parameter effects: more `filters` add channels; `kernel_size` expands local context; `stride` downsamples; `padding` controls edges; `pooling` compresses position.

---
# Next week continuity
Week 11 models sequences with recurrent state, LSTM gates, and embeddings.

Carry forward:
- local order;
- shape tracing;
- receptive field;
- controlled representation comparisons.

Next focus: carrying information across widely separated sequence steps.

---
# References and visual-design acknowledgment
- https://scikit-learn.org/stable/modules/preprocessing.html
- https://scikit-learn.org/stable/modules/model_evaluation.html
- https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression
- https://keras.io/api/layers/convolution_layers/convolution1d/
- https://pytorch.org/docs/stable/generated/torch.nn.Conv1d.html

Visual pedagogy was inspired by the general explanatory approaches of Deep Learning for Cyber, MLU-Explain, Distill, and official scikit-learn documentation. All wording, visualizations, HTML, CSS, and examples are original.
