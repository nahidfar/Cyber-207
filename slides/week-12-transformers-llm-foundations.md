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
  .attention { display:grid; grid-template-columns:repeat(4,90px); gap:22px; justify-content:center; margin:42px 0; } .token { padding:20px 8px; text-align:center; color:#fff; background:var(--forest); border-radius:12px; animation:attend 4s ease-in-out infinite; } .token:nth-child(2) { animation-delay:.3s; } .token:nth-child(3) { animation-delay:.6s; } .token:nth-child(4) { animation-delay:.9s; } .beam { height:8px; background:linear-gradient(90deg,var(--amber),var(--forest),var(--amber)); animation:route 4s ease-in-out infinite; }
  @keyframes attend { 0%,100% { transform:scale(.94); } 45% { transform:scale(1.08); background:var(--amber); } }
  @keyframes route { 0%,100% { opacity:.35; transform:scaleX(.25); } 50% { opacity:1; transform:scaleX(1); } }
  @media (prefers-reduced-motion:reduce) { .token,.beam { animation:none; } }
footer: "CYBER 207 · Week 12 · Transformers & LLM foundations"
---
<!-- _class: lead -->
# Transformers & LLM foundations
## Attention, pretraining, decoding, structured output
CYBER 207 · Week 12 · 90 minutes

---
# Measurable outcomes
- Explain self-attention with queries, keys, and values.
- trace a transformer block and causal mask.
- distinguish next-token pretraining from instruction tuning.
- reason about context windows and decoding controls.
- evaluate structured output on a bounded defensive task.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–20 | Sequence limits and self-attention |
| 20–40 | Transformer blocks, position, causal masking |
| 40–55 | Next-token pretraining and instruction tuning *(50–55 break)* |
| 55–70 | Context windows and decoding |
| 70–90 | Prompting and structured output |

---
# Visual recap · direct token interaction
<div class="attention"><span class="token">LOGIN</span><span class="token">FAIL</span><span class="token">HOST</span><span class="token">TIME</span></div>
<div class="beam"></div>

Self-attention creates content-dependent routes among positions. Static export preserves all tokens and the route summary.

<!-- _notes: Animation indicates changing routing strength, not a causal explanation. Keep the CYBER 207 boundary focused on mechanics and evaluation. -->

---
# CYBER 207 boundary
Focus today:
- how transformer models represent and generate text;
- how prompting changes task specification;
- how outputs are measured.

Prompt attacks, red teaming, governance, secure deployment, and agent security are **CYBER 290** topics and are intentionally not duplicated.

---
# From recurrence to direct interaction
RNNs process sequentially and compress history into state.

Self-attention:
- creates direct content-dependent interactions between positions;
- parallelizes training across tokens;
- still has context/computation limits;
- requires positional information.

---
# Queries, keys, values
For token representations $X$:
$$Q=XW_Q,\quad K=XW_K,\quad V=XW_V$$

Each position asks with a query, matches keys, and combines corresponding values.

Weights are learned transformations—not database fields.

---
# Scaled dot-product attention
$$\operatorname{Attention}(Q,K,V)=\operatorname{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}+M\right)V$$

- dot product measures compatibility;
- scaling stabilizes logits;
- mask $M$ forbids selected connections;
- softmax creates row-wise weights.

---
# Tiny attention calculation
Query–key scores for one token: $[2,1,0]$.

Softmax weights are approximately $[.665,.245,.090]$.

Output is weighted value mixture:
$$o=.665v_1+.245v_2+.090v_3$$

Attention weight shows routing strength, not causal explanation.

---
# Multi-head attention
Multiple heads use separate projections.

Potential benefit: represent different interaction patterns/subspaces.

Head outputs concatenate and project. More heads do not guarantee interpretable linguistic roles.

---
# Attention cost
For sequence length $n$, full attention matrix is $n\times n$:
- memory/compute roughly quadratic in $n$;
- longer context increases latency and cost;
- context window is finite;
- retrieval and chunking remain useful.

---
# Transformer block
Typical decoder block:
1. masked multi-head self-attention;
2. residual connection + normalization;
3. position-wise feedforward network;
4. residual connection + normalization.

Blocks stack to refine contextual token representations.

---
# Residual pathways
Residual form:
$$y=x+F(x)$$

Provides a direct information/gradient path and lets the block learn a correction.

Normalization stabilizes activation scale. Exact pre-norm/post-norm design varies by architecture.

---
# Feedforward sublayer
Applied independently to each position:
$$\operatorname{FFN}(x)=W_2\phi(W_1x+b_1)+b_2$$

Attention mixes across positions; FFN transforms each position’s features.

Both contribute substantial parameters and computation.

---
# Position matters
Self-attention alone is permutation-equivariant.

Position enters through:
- learned positional embeddings;
- sinusoidal encodings;
- relative/rotary schemes.

Model behavior beyond trained length depends on positional design and evidence.

---
# Causal masking
For next-token generation, position $t$ cannot attend to future positions $>t$.

Mask matrix assigns $-\infty$ to forbidden scores before softmax.

This preserves the autoregressive objective and prevents future-token leakage during training.

---
# Guided mask activity
For four tokens, the allowed-attention matrix is lower triangular, including the diagonal.

- position 3 uses three prior or current positions;
- future visibility would leak the training target;
- a padding mask removes non-content positions, while a causal mask removes future positions.

---
# Next-token pretraining
Objective:
$$L=-\sum_{t}\log P(x_t\mid x_{<t})$$

The model learns statistical regularities by predicting each next token across large corpora.

Fluent completion is not guaranteed factual task success.

---
# Instruction tuning
Further train on prompt–response examples so behavior better follows task instructions.

Changes interaction format and task performance; it does not make every answer correct.

Evaluate on the intended task, format, source, and error costs.

---
<!-- _class: lead -->
# 5-minute reset break
## 50–55 at the training-to-inference transition
Return with one difference between next-token pretraining and instruction tuning.

---
# Context window
The model conditions on tokens supplied within its limit.

Design choices:
- what evidence to include;
- ordering and delimiters;
- truncation strategy;
- redundant/irrelevant context;
- expected output budget.

Longer is not always better; relevance and placement matter.

---
# Decoding
At each step, model provides next-token distribution.

- greedy: choose highest probability;
- temperature: rescale logits;
- top-$k$: restrict to $k$ candidates;
- top-$p$: restrict to cumulative mass;
- sampling: draw from resulting distribution.

Task type determines acceptable variability.

---
# Temperature
For logits $z_i$:
$$p_i(T)=\frac{e^{z_i/T}}{\sum_j e^{z_j/T}}$$

Lower $T$: sharper/more repeatable. Higher $T$: flatter/more varied.

For structured extraction, low variability plus schema validation is often preferable.

---
# Bounded defensive task
Given a supplied, sanitized alert summary, return:
- `category` from fixed labels;
- `evidence_spans` copied from input;
- `confidence` from fixed buckets;
- `needs_review` boolean.

No actions, live queries, or external tools.

---
# Prompt as task specification
Include:
- role/context without theatrical excess;
- task and allowed labels;
- input delimiters;
- output schema;
- abstention/unknown behavior;
- one or two representative examples;
- evaluation-relevant constraints.

Prompt quality is tested, not assumed.

---
# Structured output schema
Example contract:
`{"category":"...","evidence_spans":["..."],"confidence":"low|medium|high","needs_review":true}`

Validate:
- parsable JSON;
- required keys/types;
- allowed enums;
- evidence substrings exist in source;
- no extra keys if prohibited.

---
# Guided activity · prompt + evaluate
Teams receive five sanitized alert summaries.
1. draft a compact schema-bound prompt;
2. define pass/fail checks;
3. predict two likely errors;
4. compare outputs from two supplied prompt variants;
5. select a variant using measured results.

---
# Evaluation measures
- schema validity rate;
- category accuracy/macro F1;
- evidence-span precision/recall;
- abstention appropriateness;
- consistency across repeated runs;
- latency/token use if relevant.

Human preference alone is insufficient evidence.

---
# Common failure modes
1. Attention treated as explanation.
2. Causal/padding masks confused.
3. Context truncated silently.
4. High temperature used for deterministic extraction.
5. Output not schema-validated.
6. Fluent answer accepted without source evidence.
7. LLM used without simple rule/classifier baseline.

---
# Deliverable · structured-output brief
Submit:
- bounded task and local dataset;
- prompt variants and schema;
- deterministic validation code/results;
- task metrics and error examples;
- baseline comparison;
- context/decoding settings;
- limitations and next evaluation.

---
# sklearn field guide · measurable baseline
```python
baseline = Pipeline([
  ("tfidf", TfidfVectorizer(ngram_range=(1, 2), min_df=2)),
  ("clf", LogisticRegression(max_iter=1000, class_weight="balanced"))
])
baseline.fit(train_text, train_label)
pred = baseline.predict(test_text)
print(classification_report(test_label, pred))
```

Schema evaluation separately records parse rate, required fields, enum validity, span support, and extra-field violations.

---
# Hugging Face-style conceptual code · offline contract
```python
# tokenizer and model are assumed to be supplied locally and preloaded
batch = tokenizer(local_text, max_length=512,
                  truncation=True, return_tensors="pt")
output = model.generate(**batch, temperature=.2, top_k=40,
                        top_p=.90, max_new_tokens=120,
                        do_sample=False)
```

`max_length` and `truncation` bound input. `temperature`, `top_k`, `top_p`, and `do_sample` shape decoding. `max_new_tokens` bounds output. No network or model download is required by this example.

---
# Next week continuity
Week 13 compares prompting, retrieval-augmented generation, and fine-tuning using measured retrieval and generation outcomes.

Carry forward:
- finite context;
- embeddings and similarity;
- schema validation;
- decoding controls;
- task-level evaluation.

Next focus: placing knowledge in supplied context, a retrieval index, or adapted parameters.

---
# References and visual-design acknowledgment
- https://scikit-learn.org/stable/modules/feature_extraction.html#text-feature-extraction
- https://scikit-learn.org/stable/modules/model_evaluation.html
- https://huggingface.co/docs/transformers/main_classes/text_generation
- https://pytorch.org/docs/stable/generated/torch.nn.MultiheadAttention.html

Visual pedagogy was inspired by the general explanatory approaches of Deep Learning for Cyber, MLU-Explain, Distill, and official scikit-learn documentation. All wording, visualizations, HTML, CSS, and examples are original.
