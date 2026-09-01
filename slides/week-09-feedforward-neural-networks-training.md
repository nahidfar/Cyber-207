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
  .network { display:flex; align-items:center; justify-content:center; gap:38px; margin:30px 0; } .node { width:62px; height:62px; border-radius:50%; background:var(--forest); box-shadow:0 0 0 10px var(--sage); animation:neuronFire 3s ease-in-out infinite; } .node:nth-child(3) { animation-delay:.35s; } .node:nth-child(5) { animation-delay:.7s; } .edge { width:110px; height:6px; background:var(--amber); transform-origin:left; animation:signalPass 3s ease-in-out infinite; }
  @keyframes neuronFire { 0%,100% { transform:scale(.92); background:var(--forest); } 50% { transform:scale(1.08); background:var(--amber); } }
  @keyframes signalPass { 0%,100% { transform:scaleX(.2); opacity:.35; } 50% { transform:scaleX(1); opacity:1; } }
  @media (prefers-reduced-motion:reduce) { .node,.edge { animation:none; } }
footer: "CYBER 207 · Week 09 · Feedforward networks"
---
<!-- _class: lead -->
# Feedforward neural networks & training
## Diagnosing learning on synthetic phishing features
CYBER 207 · Week 09 · 90 minutes

---
# Measurable outcomes
- Trace tensors through neurons, layers, and activations.
- explain backpropagation as chain-rule credit assignment.
- distinguish batches, epochs, optimizers, and learning rates.
- diagnose underfit/overfit from learning curves.
- report a neural experiment against a classical baseline.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Neurons, layers, activations, forward pass |
| 20–40 | Loss and backpropagation |
| 40–60 | Batches, epochs, optimizers, learning rates *(50–55 break)* |
| 60–75 | Dropout, regularization, early stopping |
| 75–90 | Diagnose phishing-model learning curves |

---
# Visual recap · evidence before architecture
<div class="network"><span class="node"></span><span class="edge"></span><span class="node"></span><span class="edge"></span><span class="node"></span></div>

`training data → fitted representation → validation choice → frozen model → untouched test`

The logistic baseline, split, metric, and threshold remain fixed while network capacity changes.

<!-- _notes: The animation shows signal flow, not intelligence or semantic understanding. The static nodes and edges retain the same meaning in PDF and PPT exports. -->

---
# Local defensive dataset
Synthetic email-feature vectors:
- URL and sender aggregates;
- TF–IDF or fixed local embeddings;
- message length/structure;
- historical sender context available at receipt.

Label: simulated analyst disposition. No live links or external API.

---
# A neuron
$$z=\mathbf{w}^{\top}\mathbf{x}+b,\qquad a=\phi(z)$$

- weights combine inputs;
- bias shifts threshold;
- activation $\phi$ introduces nonlinearity;
- output feeds the next layer.

Without nonlinear activations, stacked linear layers collapse to one linear map.

---
# Dense layer shapes
For batch $X\in\mathbb{R}^{B\times d}$:
$$Z=XW+b$$

If hidden width is $h$:
- $W\in\mathbb{R}^{d\times h}$;
- $b\in\mathbb{R}^{h}$;
- $Z,A\in\mathbb{R}^{B\times h}$.

Shape reasoning catches many implementation errors.

---
# Activations
- ReLU: $\max(0,z)$; simple, common, sparse activations.
- sigmoid: $(1+e^{-z})^{-1}$; useful binary output.
- softmax: normalized multiclass output.
- tanh: centered bounded activation, common in recurrent history.

Match output activation and loss to target semantics.

---
# Forward pass
For two layers:
$$h=\operatorname{ReLU}(W_1x+b_1)$$
$$p=\sigma(W_2h+b_2)$$

The network composes learned transformations. Hidden units are not automatically human concepts.

---
# Guided shape check
Batch size $32$, input features $100$, hidden widths $64$ and $16$, binary output.

Shape trace:
- $X:[32,100]$;
- $W_1:[100,64]$, $h_1:[32,64]$;
- $W_2:[64,16]$, $h_2:[32,16]$;
- output probabilities: $[32,1]$.

Widths are design choices; batch size is a training control.

---
# Loss measures mismatch
Binary classification commonly uses:
$$L=-\frac1B\sum_i[y_i\log p_i+(1-y_i)\log(1-p_i)]$$

The scalar loss summarizes batch mismatch; metrics evaluate decisions but often are not differentiable training objectives.

---
# Backpropagation intuition
Backprop computes how each parameter contributed to loss using the chain rule.

For composition $L(f(g(w)))$:
$$\frac{dL}{dw}=\frac{dL}{df}\frac{df}{dg}\frac{dg}{dw}$$

It reuses intermediate derivatives from output back toward input.

---
# One update step
1. forward pass produces $p$;
2. calculate batch loss;
3. backpropagate gradients;
4. optimizer updates parameters;
5. clear gradients;
6. repeat on next batch.

Evaluation mode disables training-only behavior such as dropout.

---
# Gradient meaning
Gradient $\nabla_\theta L$ points toward increasing loss.

Update:
$$\theta\leftarrow\theta-\eta\nabla_\theta L$$

Large gradients/step can destabilize; tiny values can slow learning. Monitoring training dynamics is part of evidence.

---
# Batch and epoch
- **Batch:** subset used for one gradient estimate/update.
- **Epoch:** one pass through training examples.
- Smaller batches: noisier updates, lower memory.
- Larger batches: smoother estimates, more memory.

Shuffle training rows unless sequence/time structure requires otherwise.

---
# Optimizers
- SGD: direct noisy gradient descent.
- Momentum: accumulates direction.
- Adam: adaptive per-parameter scaling and momentum-like estimates.

Optimizer choice changes dynamics, not the evaluation contract. Learning rate usually matters most.

---
# Learning-rate symptoms
Too high:
- loss oscillates/diverges;
- NaNs or unstable validation.

Too low:
- loss barely changes;
- training budget ends before convergence.

Compare curves under controlled seeds and settings.

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 within training dynamics
Return with one observable symptom of a learning rate that is too high.

---
# Learning curves
Plot training and validation loss/metric by epoch.

Patterns:
- both poor/flat: underfit, optimization, or weak features;
- train improves, validation worsens: overfit;
- both improve then plateau: convergence candidate;
- noisy validation: small sample or unstable process.

---
# Regularization tools
- weight decay / L2 penalty;
- smaller architecture;
- dropout;
- early stopping;
- more representative data;
- feature simplification.

Apply one controlled change at a time and compare against baseline.

---
# Dropout
During training, randomly zero activations with probability $p$ and rescale survivors.

Intuition: discourage fragile co-adaptation.

At evaluation, dropout is disabled. High dropout can underfit; it does not replace held-out evidence.

---
# Early stopping
Monitor a validation quantity and retain best checkpoint.

Document:
- monitored metric;
- direction (min/max);
- patience and minimum change;
- maximum epochs;
- restored checkpoint.

The validation set influences training duration; test remains untouched.

---
# Worked learning curves
| epoch | train loss | val loss | val recall |
|---:|---:|---:|---:|
| 1 | .62 | .64 | .61 |
| 5 | .31 | .39 | .78 |
| 10 | .15 | .47 | .74 |
| 20 | .05 | .72 | .66 |

Best evidence suggests stopping near epoch 5, then verifying across seeds/folds.

---
# Guided activity · diagnose curves
Teams receive four curve panels:
1. label underfit, overfit, unstable, or healthy;
2. cite two observations;
3. propose one controlled intervention;
4. state fixed comparison controls;
5. compare the candidate against the logistic baseline.

Time: 10 minutes + 3-minute share-out.

---
# Ablation plan
Compare:
- logistic baseline;
- one hidden layer;
- two hidden layers;
- no dropout versus dropout;
- TF–IDF versus fixed local embeddings.

Keep split, primary metric, threshold policy, and training budget documented.

---
# Operational tradeoffs
- More parameters increase capacity and compute.
- Better average score may not improve high-cost errors.
- Neural probabilities may need calibration.
- Training variability requires multiple seeds.
- Representation and data quality often dominate architecture tweaks.

---
# Common failure modes
1. Test set used for early stopping.
2. Train/eval mode confused.
3. Output activation mismatched with loss.
4. Accuracy hides imbalance.
5. One lucky seed reported.
6. No classical baseline.
7. Curves omitted, preventing diagnosis.

---
# Deliverable · training report
Include:
- local data/split and baseline;
- architecture with tensor shapes;
- loss, optimizer, learning rate, batch size;
- learning curves and checkpoint rule;
- at least one ablation and multiple seeds;
- held-out metrics, calibration/threshold notes;
- concrete errors and limitations.

---
# sklearn field guide · safe pipeline
```python
pipe = Pipeline([
  ("scale", StandardScaler()),
  ("mlp", MLPClassifier(hidden_layer_sizes=(64, 16),
      activation="relu", solver="adam", alpha=1e-4,
      batch_size=32, learning_rate_init=1e-3, max_iter=300,
      early_stopping=True, validation_fraction=.15,
      n_iter_no_change=12, random_state=7))
])
pipe.fit(X_train, y_train)
```

Scaling is learned inside the pipeline; validation stopping never uses the test set.

---
# MLPClassifier parameter effects
| Control | Operational effect |
|---|---|
| `hidden_layer_sizes`, `activation` | capacity and nonlinear representation |
| `solver`, `learning_rate_init` | update dynamics and stability |
| `alpha` | L2 pressure against large weights |
| `batch_size`, `max_iter` | gradient noise and training budget |
| `early_stopping`, `validation_fraction` | internal validation checkpointing |
| `n_iter_no_change` | patience before stopping |

Inspect `loss_curve_`, `validation_scores_`, and `n_iter_`. Use `learning_curve` for sample-size diagnosis.

---
# Next week continuity
Week 10 uses shared local filters to learn spatial/ordered patterns with CNNs.

Carry forward:
- shape tracing;
- training curves;
- regularization;
- controlled ablations;
- baseline comparison.

Next focus: one learned pattern detector reused across positions.

---
# References and visual-design acknowledgment
- https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html
- https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html
- https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html
- https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.learning_curve.html

Visual pedagogy was inspired by the general explanatory approaches of Deep Learning for Cyber, MLU-Explain, Distill, and official scikit-learn documentation. All wording, diagrams, HTML, CSS, and examples are original.
