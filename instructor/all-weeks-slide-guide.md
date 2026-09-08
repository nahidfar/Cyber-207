# CYBER 207 — All-Weeks Slide Facilitation Guide

This guide accompanies the fourteen Marp decks in `slides/`. It keeps CYBER 207 centered on **how models learn and how evidence supports decisions**. Prompt attacks, red teaming, governance, secure deployment, and agent security remain in CYBER 290. All classroom examples use supplied synthetic/local, inert, defensive data.

## Common room and technology preparation

- Export or preview each Marp deck before class; verify KaTeX, tables, pagination, and any Mermaid rendering.
- Keep an offline PDF as fallback. No activity requires a live target, external API, or production account.
- Prepare a visible timer and activity handouts/data named in the relevant week.
- Use the forest/amber deck palette consistently; amber signals transitions/caution, red-orange signals costly errors.
- For 90-minute sessions, protect the activity and exit ticket first. If running late, shorten lecture examples rather than deleting interpretation.
- Never distribute instructor answer anchors as student-facing assessment material.

## Week 1 — ML framing, splits, and linear regression

**Preparation**
- Print the five-row containment-time table and a blank problem-formulation canvas.
- Prepare calculators or a shared arithmetic sheet; no coding prerequisite.
- Draw a time-based train/validation/test partition before class.

**Key misconceptions**
- A model output is the operational decision.
- Random row splitting is always neutral.
- A regression coefficient proves causality.
- MSE is directly measured in hours rather than squared hours.

**Activity answer anchors**
- Predictions for queue depths 4, 8, 12, 16, 20 are 2.12, 3.24, 4.36, 5.48, and 6.60 hours.
- Residuals are actual minus prediction; positive means underprediction.
- Good next features are available at incident opening, such as affected-host count or current queue age—not closure disposition.
- Reward an explicit median/mean baseline and a temporal/grouped split.

**Timing adjustments**
- If short, calculate only rows 8, 16, and 20, then discuss residual pattern.
- If ahead, compare MSE with MAE and ask which better communicates analyst workload.

## Week 2 — KNN, trees, and ensembles

**Preparation**
- Supply a 12-row scaled synthetic URL-feature table plus outputs from KNN, tree, forest, and boosting models.
- Mark campaign/group IDs so students can identify leakage.

**Key misconceptions**
- KNN learns coefficients.
- Trees do not overfit because they are interpretable.
- Feature importance proves causal relevance.
- Boosting is simply more trees in parallel.

**Activity answer anchors**
- In the neighbor example, $k=3$ predicts suspicious (2–1); $k=5$ predicts benign (3–2). Smaller $k$ is more locally sensitive.
- The worked tree split lowers weighted Gini from .50 to .32, an improvement of .18.
- Strong recommendations cite fold variability, high-cost errors, inference burden, and a simple baseline.

**Timing adjustments**
- If short, compare KNN, one tree, and forest; assign boosting as follow-up.
- If ahead, have teams propose a permutation-importance test for correlated features.

## Week 3 — Security feature engineering / Project 1

**Preparation**
- Provide the synthetic login table, feature inventory worksheet, and four ablation result sets.
- Have a minimal pipeline diagram showing transformations fitted inside each fold.
- Run `notebooks/security-feature-engineering.ipynb` and keep `notebook-walkthroughs/security-feature-engineering-walkthrough.md` available for the worked lab.

**Key misconceptions**
- Raw telemetry fields are automatically valid features.
- Missing always means zero or benign.
- A vocabulary/scaler can be fit globally because it does not use labels.
- Ablation proves causality.

**Activity answer anchors**
- Post-investigation fields, future windows, globally fitted transforms, and cross-fold entities are leakage risks.
- A valid ablation changes one feature group while holding split, model, seed policy, and metric procedure fixed.
- Prefer a minimal set when its score is stable and operationally equivalent; document correlated substitutes.

**Timing adjustments**
- If short, complete feature inventory for three fields and one ablation.
- If ahead, ask teams to write tests for unseen categories and late-arriving events.

## Week 4 — Logistic regression and probabilistic decisions

**Preparation**
- Distribute the validation threshold table and a one-page cost/capacity worksheet.
- Prepare a simple reliability diagram with one overconfident model.

**Key misconceptions**
- A score is automatically calibrated probability.
- Lower threshold improves a model rather than changing policy.
- Odds ratio is the same as probability increase.
- Accuracy is adequate under imbalance.

**Activity answer anchors**
- In the logit example, $z=-.3$, so $p<.5$; a .4 threshold likely predicts positive because $\sigma(-.3)\approx.426$.
- Threshold costs are 260, 220, and 275; .20 violates capacity. Under stated assumptions, .40 is defensible.
- $e^{w_j}$ multiplies odds per documented unit, holding modeled features fixed.

**Timing adjustments**
- If short, compute only .40 and .60 and discuss assumptions.
- If ahead, vary false-negative cost and identify when the preferred threshold changes.

## Week 5 — Multiclass classification and metrics

**Preparation**
- Supply a four-class confusion matrix, per-class supports, and six sanitized error records.
- Prepare a blank one-vs-rest count worksheet.

**Key misconceptions**
- Softmax probability guarantees correctness.
- Macro, micro, and weighted F1 are interchangeable.
- ROC-AUC determines an operating threshold.
- A high overall score guarantees rare-family quality.

**Activity answer anchors**
- For the shown ransomware row: precision $25/38\approx.658$, recall $25/40=.625$, F1 $\approx.641$.
- Macro averaging gives equal class weight; weighted/micro summaries can hide rare-class failure.
- Strong error analyses tag label ambiguity, missing evidence, new pattern, collection issue, or reject-policy problem.

**Timing adjustments**
- If short, calculate one class and interpret two off-diagonal cells.
- If ahead, add a reject class and discuss how to score abstentions.

## Week 6 — k-means and PCA / Project 2

**Preparation**
- Provide 15 synthetic campaign aggregates, several $K$ results, PCA scores/loadings, and a cluster-profile template.
- Ensure students can distinguish feature space from projected display.

**Key misconceptions**
- Cluster ID is a threat label or actor identity.
- Highest silhouette always chooses $K$.
- A principal component has an intrinsic semantic name.
- Outlier means malicious.

**Activity answer anchors**
- For the tie example, both centroid distances are $\sqrt5$; use it to discuss ambiguity and initialization.
- The example supports $K=3$: useful, stable, and nearly best silhouette; $K=5$ is unstable.
- Valid profiles describe measured differences and follow-up evidence without asserting common actor or intent.

**Timing adjustments**
- If short, profile one cluster and one boundary case.
- If ahead, compare stability across random seeds or later time slices.

## Week 7 — Classical ML integration and midterm review

**Preparation**
- Assemble the synthetic detector packet: fold metrics, capacity counts, PCA drift summary, and four error records.
- Set up five review stations with formulas and blank reasoning prompts.

**Key misconceptions**
- Best validation score is sufficient for deployment recommendation.
- Drift diagnosis starts with retraining.
- Supervised and unsupervised outputs make equivalent claims.
- Model choice can compensate for a flawed split.

**Activity answer anchors**
- A good design uses rolling/expanding time folds, groups overlapping entities, and reserves a final future month.
- June’s worse performance coincides with missing-signer growth; this is a hypothesis about collection, not proven cause.
- Reward claims that distinguish label agreement from geometric novelty.

**Timing adjustments**
- If short, run three review stations and post the remaining prompts.
- If ahead, ask teams to propose a diagnosis order before any retraining.

## Week 8 — Midterm and final-project framing

**Preparation**
- Verify assessment files, formula sheet, accommodations, submission channel, backup copies, timer announcements, and group roster.
- Keep the student-facing deck visible only on instruction/timer slides during the assessment.
- This guide intentionally contains **no exam answers, item values, or solution key**.

**Key misconceptions to review neutrally**
- Framing: algorithm before unit/action/cost.
- Partitions: future data, global preprocessing, entity bleed, test tuning.
- Metrics: denominator errors and accuracy-only reasoning.
- Interpretation: causal claims from coefficients/importance; anomaly as threat.
- Temporal evidence: drift symptom treated as root cause.

**Activity answer anchors**
- Self-check output is a misconception category, a practice action, and an office-hours question—never an exam item answer.
- A viable proposal sentence names authorized user/workflow, bounded outcome, point-in-time evidence, human decision, metric/error, baseline, and holdout.
- Reject proposals requiring live probing, sensitive/unapproved data, or CYBER 290 topics.

**Timing adjustments**
- Do not shorten the 60-minute assessment except for approved accommodations.
- If collection takes longer, compress group sharing but retain written proposal canvas and exit ticket.

## Week 9 — Feedforward neural networks and training

**Preparation**
- Provide four learning-curve panels and a synthetic phishing-feature dataset or precomputed outputs.
- Have a tensor-shape worksheet and logistic baseline result.

**Key misconceptions**
- More layers automatically improve evidence.
- Backprop “understands” semantic meaning.
- Test data may control early stopping.
- One seed is representative.

**Activity answer anchors**
- For batch 32, input 100, widths 64 and 16: $X[32,100]$, $W_1[100,64]$, $h_1[32,64]$, $W_2[64,16]$, $h_2[32,16]$, output $[32,1]$.
- The worked curve overfits after about epoch 5; recommend early stopping near the validation minimum and verify across seeds.
- Controlled interventions change one factor and preserve split, metric, and baseline.

**Timing adjustments**
- If short, diagnose two curve panels.
- If ahead, compare calibration before/after the neural model.

## Week 10 — Convolutional neural networks

**Preparation**
- Supply inert generated arrays, architecture output, confusion matrix, and a 1D/2D representation comparison.
- Print shape and parameter-count worksheet.

**Key misconceptions**
- Convolution is automatically invariant.
- Any matrix-like input has meaningful 2D locality.
- Feature maps are direct explanations.
- Malware-image layout preserves natural adjacency everywhere.

**Activity answer anchors**
- Width-2 filter on $[1,2,0,3]$ gives $[-1,2,-3]$.
- For input length 20, kernel 5, padding 2, stride 1, output remains 20; stride 2 yields 10.
- First 2D conv with 16 filters, one channel, $3\times3$ kernel has 160 parameters.
- Strong ablations compare 1D/2D layout, pooling, truncation, or channel removal.

**Timing adjustments**
- If short, trace one convolution/pooling block and inspect two errors.
- If ahead, compute receptive field across two layers.

## Week 11 — RNNs, LSTMs, sequence embeddings / Project 3

**Preparation**
- Provide generated event sequences, masks/lengths, model result table, and temporal error records.
- Include bag-of-events baseline and identical splits for RNN/LSTM/CNN.

**Key misconceptions**
- Final hidden state preserves every detail.
- LSTM gates have fixed human semantic roles.
- Padding is harmless without a mask.
- Bidirectional models are always better for online prediction.

**Activity answer anchors**
- Order-distinct sequences defeat bag-of-token counts but can differ under recurrent/CNN representation.
- The sample table supports LSTM if long-sequence recall justifies latency; CNN is defensible when latency dominates.
- A valid comparison groups overlapping windows/entities and fits vocabulary on training data only.

**Timing adjustments**
- If short, compare LSTM and CNN against bag baseline.
- If ahead, slice errors by informative-event position and unseen-token rate.

## Week 12 — Transformers and LLM foundations

**Preparation**
- Prepare a four-token causal-mask worksheet, supplied sanitized alert summaries, two prompt variants, and schema-validation results.
- Keep discussion inside model mechanics and evaluation.

**Key misconceptions**
- Attention weight is causal explanation.
- Longer context always improves answers.
- Instruction tuning guarantees factuality.
- Prompt quality can be judged from one fluent output.

**Activity answer anchors**
- Softmax of scores $[2,1,0]$ is approximately $[.665,.245,.090]$.
- A four-token causal mask is lower triangular including the diagonal; padding mask excludes padding positions for all relevant queries.
- Strong structured-output checks cover parseability, required keys/types, enums, exact evidence spans, and extra-key policy.

**Timing adjustments**
- If short, omit hand calculation after showing the result and preserve schema activity.
- If ahead, compare low- versus higher-temperature consistency on extraction.

## Week 13 — RAG, fine-tuning, LLM evaluation / Project 4

**Preparation**
- Build a six-document local sanitized corpus, relevance judgments, eight Q&A cases, retrieved rankings, and method outputs.
- Verify citation spans against stable document IDs.

**Key misconceptions**
- Embedding similarity is truth.
- RAG and fine-tuning solve the same knowledge problem.
- Final answer score identifies retrieval failure.
- Valid citation syntax proves support.

**Activity answer anchors**
- Lexical retrieval is strong for exact rare terms; vector retrieval handles paraphrase; hybrid can combine both but requires validation.
- Use RAG for changing document facts/provenance; fine-tuning is a candidate for stable repeated behavior/format gaps.
- Tag failures as retrieval miss/ranking/truncation, evidence ignored, unsupported addition, wrong citation, incorrect abstention, or schema failure.
- Method recommendation must preserve same prompt/decoding where comparison requires it.

**Timing adjustments**
- If short, score four Q&A cases and one unanswerable case.
- If ahead, calculate Recall@5 and reciprocal rank manually for one query.

## Week 14 — Final project presentations

**Preparation**
- Publish order and exact slot length based on team count.
- Test display, audio, offline demo fallback, timer, rubric collection, and transition procedure.
- Prepare project-method matrix for synthesis.

**Key misconceptions**
- A demo is evaluation evidence.
- Architecture complexity is the main contribution.
- “Future work” should broaden scope rather than reduce uncertainty.
- Limitations can be generic rather than connected to observed errors.

**Activity answer anchors**
- High-quality peer feedback cites one supported result, one evidence gap, one controlled next test, and one scope correction.
- Synthesis should map each project through decision, data, representation, model/objective, validation choice, test evidence, errors, and bounded action.
- Reward teams that show a failure honestly and explain what it changes.

**Timing adjustments**
- Use the deck’s team-count variants. Never let presentations consume the individual exit ticket and course close.
- If a demo fails, switch immediately to recording/screenshots and reserve Q&A time.

## Course-wide assessment language

Use these phrases consistently:
- “The evidence supports…”
- “For examples like the held-out period…”
- “This pattern motivates the next test…”
- “The result does not establish causality/actor identity/maliciousness…”
- “The operational choice depends on error cost and capacity…”

Avoid unbounded language such as “proves,” “solves,” “always,” or “production-ready” unless the stated evidence truly supports it.
