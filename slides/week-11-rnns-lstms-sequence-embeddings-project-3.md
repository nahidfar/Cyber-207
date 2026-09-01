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
  strong { color:var(--signal); } table { font-size:.71em; background:var(--paper); } th { background:var(--forest); color:#fff; } blockquote,.card { border-left:6px solid var(--amber); background:var(--paper); padding:.55em .85em; border-radius:8px; }
  .sequence { display:flex; justify-content:center; gap:18px; margin:38px 0; } .step { width:120px; padding:18px 8px; text-align:center; color:#fff; background:var(--forest); border-radius:12px; animation:stateCarry 4s ease-in-out infinite; } .step:nth-child(2) { animation-delay:.35s; } .step:nth-child(3) { animation-delay:.7s; } .step:nth-child(4) { animation-delay:1.05s; } .memory { height:10px; background:var(--amber); animation:memoryFlow 4s ease-in-out infinite; }
  @keyframes stateCarry { 0%,100% { transform:translateY(0); } 35% { transform:translateY(-12px); background:var(--amber); } }
  @keyframes memoryFlow { 0%,100% { transform:scaleX(.15); opacity:.4; } 55% { transform:scaleX(1); opacity:1; } }
  @media (prefers-reduced-motion:reduce) { .step,.memory { animation:none; } }
footer: "CYBER 207 · Week 11 · Sequence models · Project 3"
---
<!-- _class: lead -->
# RNNs, LSTMs & sequence embeddings
## Modeling synthetic logs and API-event sequences
CYBER 207 · Week 11 · 90 minutes

---
# Measurable outcomes
- Trace recurrent hidden state across time.
- explain vanishing gradients and LSTM gates.
- create token embeddings and sequence representations.
- design time-respecting evaluation for event sequences.
- compare RNN, LSTM, and CNN evidence for Project 3.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Sequence modeling and recurrent state |
| 20–40 | Vanishing gradients and LSTM gates |
| 40–55 | Token embeddings and sequence representations *(50–55 break)* |
| 55–70 | Logs, API calls, network-event sequences |
| 70–90 | Compare RNN, LSTM, CNN / Project 3 |

---
# Visual recap · state travels through order
<div class="sequence"><span class="step">FAIL</span><span class="step">FAIL</span><span class="step">OK</span><span class="step">CHANGE</span></div>
<div class="memory"></div>

The same transition updates state at each step. A static export retains the complete ordered chain.

<!-- _notes: The rising cells represent sequential state updates. They do not imply that each hidden dimension has a fixed human-readable meaning. -->

---
# Sequential evidence
Order and timing carry information:
- authentication event chains;
- process/API call sequences;
- packet direction/size sequences;
- log token streams.

If order is shuffled and meaning does not change, a sequence model may be unnecessary.

---
# Synthetic local sequence
Example tokens:
`LOGIN_FAIL → LOGIN_FAIL → LOGIN_OK → PRIV_CHANGE → LOGOUT`

Task: classify generated sessions into routine, recovery-like, or review-priority patterns.

The data are inert and simulated; no operational instructions or real accounts.

---
# Vanilla RNN state
$$h_t=\phi(W_xx_t+W_hh_{t-1}+b)$$
$$\hat y=g(W_yh_T+b_y)$$

The same transition parameters are reused at every step. $h_t$ compresses prior context.

---
# Unrolling through time
At each position:
- current token representation $x_t$;
- previous state $h_{t-1}$;
- updated state $h_t$;
- optional output.

Training backpropagates through the unrolled computation graph.

---
# Many-to-one and many-to-many
- many-to-one: classify a complete session;
- many-to-many aligned: label each event;
- sequence-to-sequence: transform one sequence to another;
- next-step prediction: predict following event/token.

Choose output shape from the decision.

---
# Padding and masks
Batches need common length.
- pad shorter sequences;
- truncate longer sequences by documented policy;
- mask padding so it does not affect state/loss;
- record original lengths.

Padding tokens can become a shortcut if mishandled.

---
# Guided state reasoning
Two sequences have same token counts but different order:
- A: `FAIL FAIL OK`;
- B: `OK FAIL FAIL`.

A bag-of-tokens baseline maps both to the same counts. Recurrent and CNN representations preserve order and can separate them.

---
# Vanishing/exploding gradients
Across many steps, gradients multiply Jacobians.

Repeated factors below 1 shrink signal; above 1 amplify it.

Symptoms:
- long-range context not learned;
- unstable loss;
- gradient norms near zero or huge.

Clipping helps explosion; gating helps preserve paths.

---
# LSTM state
An LSTM maintains cell state $c_t$ and hidden state $h_t$.

Core gates:
- forget $f_t$;
- input $i_t$;
- candidate $\tilde c_t$;
- output $o_t$.

Gates are learned soft controls in $[0,1]$.

---
# LSTM equations
$$f_t=\sigma(W_f[x_t,h_{t-1}]+b_f)$$
$$i_t=\sigma(W_i[x_t,h_{t-1}]+b_i)$$
$$c_t=f_t\odot c_{t-1}+i_t\odot\tilde c_t$$
$$h_t=o_t\odot\tanh(c_t)$$

Additive cell update supports longer gradient flow.

---
# Gate intuition—not literal semantics
- forget: retain/discard old cell dimensions;
- input: write candidate information;
- output: expose cell information.

A gate dimension is not automatically “the privilege-change detector.” Interpret through probes and ablations.

---
# RNN versus LSTM
| | Vanilla RNN | LSTM |
|---|---|---|
| State | hidden only | hidden + gated cell |
| Parameters | fewer | more |
| Long context | difficult | improved, not guaranteed |
| Training | simpler | slower/heavier |

A 1D CNN may still win for bounded local patterns.

---
# Embeddings
Map token ID $j$ to learned vector:
$$e_j=E[j],\qquad E\in\mathbb{R}^{V\times d}$$

Embeddings allow similarity and dense representation. Padding gets a reserved ID/mask.

Unknown-token strategy must be documented.

---
# Sequence representation choices
- final hidden state;
- mean/max pooling over states;
- concatenated forward/backward state;
- attention-weighted summary;
- CNN global pooling.

Final-state summaries can overemphasize late positions or lose long details.

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 at the representation transition
Return with one failure caused by truncation or padding.

---
# Logs and API-call sequences
Design choices:
- token vocabulary and rare-event handling;
- sessionization boundaries;
- event order and timestamps;
- repeated-event compression;
- maximum length;
- label horizon after sequence end.

Session construction is feature engineering.

---
# Network-event sequences
Possible channels per step:
- packet direction;
- binned size;
- inter-arrival time;
- protocol category.

Avoid payload or sensitive content when metadata suffices. Define sensor vantage and missing packets.

---
# Temporal split for sequences
Prevent leakage by grouping:
- all windows from one session/host/campaign;
- overlapping windows;
- near-duplicate generated patterns.

Train earlier periods; test later. Fit vocabulary/normalization on train only.

---
# Comparison design
Hold constant:
- sequence construction;
- train/validation/test groups and dates;
- embedding dimension where applicable;
- training budget/selection rule;
- primary metric and threshold policy.

Compare RNN, LSTM, 1D CNN, and simple bag-of-events baseline.

---
# Worked result table
| model | macro F1 | long-seq recall | ms/sequence |
|---|---:|---:|---:|
| bag baseline | .68 | .42 | .2 |
| RNN | .72 | .51 | 2.1 |
| LSTM | .76 | .64 | 3.8 |
| 1D CNN | .75 | .58 | 1.4 |

Selection depends on long-sequence value and latency.

---
# Guided comparison activity
Teams:
1. choose a candidate from the table;
2. inspect three temporal errors;
3. propose one truncation/padding ablation;
4. explain likely latency tradeoff;
5. write a claim that does not exceed evidence.

Time: 10 minutes + 4-minute debrief.

---
# Temporal error analysis
Slice by:
- sequence length;
- position of informative event;
- unseen token rate;
- time gap/duration;
- missing-event fraction;
- later test period.

Inspect examples without exposing sensitive real logs.

---
# Operational tradeoffs
- LSTM may preserve longer context but costs more.
- CNN parallelizes and captures local motifs efficiently.
- RNN is compact but can forget distant evidence.
- Sessionization/truncation can dominate results.
- Deep gains must exceed a bag-of-events baseline meaningfully.

---
# Common failure modes
1. Overlapping windows cross splits.
2. Vocabulary sees test data.
3. Padding contributes to pooling/loss.
4. Sequence order accidentally shuffled.
5. Bidirectional model uses future events for online decision.
6. One seed/aggregate metric reported.
7. No simple sequence baseline.

---
# Project 3 · deliverable
Train/evaluate a deep model for text, bytes, images, or event sequences against a classical baseline.

Include:
- representation and split rationale;
- architecture/shapes/training curves;
- controlled baseline and ablation;
- multiple-seed held-out metrics;
- temporal/representation error analysis;
- compute/latency and limitations.

---
# Project 3 · acceptance checks
- local/synthetic/approved data only;
- no related entity/window leakage;
- masks and lengths tested;
- checkpoint selected on validation;
- model comparison uses same evidence contract;
- README reproduces the experiment;
- claim remains about learning/evaluation, not secure deployment.

---
# sklearn field guide · text and sequence baselines
```python
text_base = Pipeline([
  ("tfidf", TfidfVectorizer(ngram_range=(1, 2), min_df=2)),
  ("clf", LogisticRegression(max_iter=1000, class_weight="balanced"))
])

tabular_base = HistGradientBoostingClassifier(max_iter=200,
                                               learning_rate=.05)
```

`MLPClassifier` is another fixed-vector baseline. scikit-learn has no RNN or LSTM estimator; sequence layers require Keras or PyTorch.

---
# PyTorch-style LSTM pseudocode · separate dependency
```python
embed = nn.Embedding(vocab_size, 64, padding_idx=0)
lstm = nn.LSTM(input_size=64, hidden_size=128,
               num_layers=2, bidirectional=False,
               dropout=.20, batch_first=True)
x = embed(token_ids[:, :sequence_length])
states, _ = lstm(x)
logits = head(states[:, -1])
```

`hidden_size` controls state width; layers add depth; `bidirectional` uses both directions and may violate online timing; `dropout` regularizes stacked layers; sequence length sets context and cost.

---
# Next week continuity
Week 12 replaces recurrent compression with self-attention and builds transformer/LLM foundations.

Carry forward:
- embeddings;
- masks;
- sequence length;
- next-step prediction;
- representation evaluation.

Next focus: every token directly combining evidence from other positions.

---
# References and visual-design acknowledgment
- https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html
- https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
- https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html
- https://keras.io/api/layers/recurrent_layers/lstm/
- https://pytorch.org/docs/stable/generated/torch.nn.LSTM.html

Visual pedagogy was inspired by the general explanatory approaches of Deep Learning for Cyber, MLU-Explain, Distill, and official scikit-learn documentation. All wording, diagrams, HTML, CSS, and examples are original.
