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
  strong { color:var(--signal); } table { font-size:.73em; background:var(--paper); } th { background:var(--forest); color:#fff; } blockquote,.card { border-left:6px solid var(--amber); background:var(--paper); padding:.55em .85em; border-radius:8px; }
  .rail { display:flex; gap:10px; align-items:center; margin:28px 0; } .rail span { height:18px; flex:1; background:var(--sage); border-radius:12px; } .rail .active { background:var(--amber); animation:progressPulse 2.8s ease-in-out infinite; }
  .stamp { display:inline-block; padding:10px 18px; border:3px solid var(--forest); border-radius:999px; font-weight:700; animation:stampSet 2.8s ease-in-out infinite; }
  @keyframes progressPulse { 0%,100% { transform:scaleX(.94); opacity:.72; } 50% { transform:scaleX(1); opacity:1; } }
  @keyframes stampSet { 0%,100% { transform:rotate(-1deg); } 50% { transform:rotate(1deg); } }
  @media (prefers-reduced-motion:reduce) { .rail .active,.stamp { animation:none; } }
footer: "CYBER 207 · Week 08 · Midterm & project framing"
---
<!-- _class: lead -->
# Midterm + final-project framing
## Cumulative practical assessment and evidence-based proposal
CYBER 207 · Week 08 · 90 minutes

---
# Session outcomes
After the assessment, you can:
- categorize misconceptions without exposing exam answers;
- identify which reasoning process needs repair;
- frame a bounded final-project modeling objective;
- define measurable evidence, baseline, split, and deliverables;
- distinguish CYBER 207 modeling scope from CYBER 290 security scope.

---
# Agenda — exact syllabus timing
| Time | Focus |
|---|---|
| 0–60 | Cumulative practical assessment |
| 60–75 | Neutral solution-process and misconception review |
| 75–90 | Final-project groups and proposal framing |

No break: assessment/presentation-format exception. Quiet reset occurs during collection at minute 60.

---
# Before the exam · materials
Permitted:
- instructor-provided formula sheet;
- local course environment and supplied files if stated;
- calculator;
- blank paper.

Not permitted unless accommodations specify otherwise:
- external communication;
- generative assistants;
- internet searching;
- accessing another student’s work.

# Integrity rules
- Work independently for 60 minutes.
- Use only authorized local data and tools.
- Do not photograph, copy, or redistribute prompts.
- Ask the instructor only for clarification of wording or environment issues.
- Cite any permitted reference material used.
- Stop work immediately when time is called.

---
# Assessment evidence expected
The practical may ask you to:
- frame a defensive decision;
- identify leakage or flawed evaluation;
- compute/interpret a metric;
- compare model evidence;
- inspect an error or temporal slice;
- state a bounded recommendation.

This is a scope map, **not a disclosure of assessment solutions**.

---
# Time management · 60 minutes
Suggested allocation:
- 0–5: scan all tasks and mark constraints;
- 5–40: complete core analysis;
- 40–52: interpretations and recommendations;
- 52–58: check units, denominators, and split logic;
- 58–60: save/submit exactly as instructed.

---
# During the exam
Use this private reasoning sequence for each task:

<div class="rail"><span class="active"></span><span></span><span></span><span></span><span></span></div>

`unit + decision → point-in-time evidence → legal partition → metric meaning → bounded claim`

<!-- _notes: Keep this sequence visible as process support. Do not elaborate with item-specific values or solution paths. -->

---
<!-- _class: lead -->
# Assessment in progress
## 0–60 minutes · independent work
Keep this slide displayed. Instructor announces 30-, 10-, and 2-minute checkpoints.

# Submission checklist
Before minute 60:
- all requested files use the naming convention;
- notebook/report runs or opens locally;
- tables have labels and units;
- answers distinguish evidence from speculation;
- no external links, live targets, or sensitive data;
- final upload/save confirmation is visible.

---
# Minute 60 · close and collect
1. Stop typing/writing.
2. Submit through the designated local/LMS channel.
3. Keep assessment content private.
4. Take a two-minute silent reset while collection is verified.
5. Do not discuss specific prompt values or answers.

---
# Post-exam review boundary
We will discuss **reasoning categories**, not item answers.

Allowed:
- leakage detection routines;
- macro-averaging use cases.

Deferred:
- exact prompt data, selected options, calculations, or expected outputs.

---
# Misconception category 1 · framing
Symptoms:
- model selected before action is defined;
- row/unit unclear;
- label not connected to future outcome;
- costly error unnamed.

Repair routine: `unit → observable evidence → target → action → cost`.

---
# Misconception category 2 · data partitions
Symptoms:
- fitting preprocessing globally;
- tuning on test data;
- duplicated entities across folds;
- future fields used as features.

Repair routine: draw time, groups, and fitted-state boundaries before modeling.

---
# Misconception category 3 · metrics
Symptoms:
- accuracy used under imbalance;
- wrong denominator;
- threshold-free metric treated as a policy;
- average hides high-cost slice.

Repair routine: write confusion counts first, then formula, then operational meaning.

---
# Misconception category 4 · interpretation
Symptoms:
- coefficient called causal;
- anomaly called malicious;
- cluster named as an actor;
- importance treated as proof.

Repair routine: use conditional language and identify the missing validation evidence.

---
# Misconception category 5 · temporal evidence
Symptoms:
- one random split represents future deployment;
- drift assumed to mean attack;
- sensor change ignored;
- retraining proposed before diagnosis.

Repair routine: compare time slices, collection health, labels, then model behavior.

---
# Neutral misconception map · 5 minutes
Without discussing exam items, record one category:
- framing;
- partitions;
- metrics;
- interpretation;
- temporal evidence.

Capture the reasoning step to strengthen, one practice action, and one topic for office hours.

<span class="stamp">PROCESS REVIEW ONLY</span>

---
<!-- _class: lead -->
# Final-project framing
## 75–90 minutes
A modeling study with baselines, evaluation, failure analysis, demo, and defense.

---
# CYBER 207 project boundary
In scope:
- how a model learns from local/synthetic/approved data;
- representations, baselines, metrics, errors, and drift;
- LLM method comparison in Weeks 12–13.

Out of scope here:
- prompt attacks, red teaming, governance, secure deployment, agent security;
- real-target probing or unapproved sensitive data.

---
# Problem-statement template
> For **[authorized user/workflow]**, predict/discover **[bounded outcome]** from **[point-in-time evidence]** to support **[human decision]**, evaluated by **[metrics and error costs]** against **[baseline]** on **[held-out period/groups]**.

If the sentence cannot be completed, the project is not yet scoped.

---
# Proposal canvas
Each group drafts:
- members and roles;
- unit, target, intended user/action;
- approved/local data source;
- baseline and candidate model;
- time/group split;
- primary metric plus costly error;
- two likely failure modes;
- demo artifact.

---
# Evidence ladder
Minimum final evidence:
1. data sheet and reproducible pipeline;
2. naive/rule baseline;
3. candidate method with controlled comparison;
4. held-out metrics with uncertainty;
5. concrete error cases;
6. limitation and next experiment;
7. honest demo showing input, output, and interpretation.

---
# Group formation activity · 8 minutes
Groups of 3–4:
1. share one candidate task each;
2. test it against scope and data constraints;
3. select one task;
4. complete the problem-statement template;
5. assign a data, modeling, evaluation, and presentation owner.

Instructor signs off only on scope—not promised performance.

---
# Proposal deliverable
Submit by the stated course deadline:
- 1-page problem statement;
- team/role list;
- data authorization and schema sketch;
- baseline, model, split, metric plan;
- risk/limitation register;
- milestone schedule for Weeks 9–14.

Approval is required before expanding data or scope.

---
# Project handoff card
- misconception category selected for practice;
- group problem statement recorded;
- baseline and untouched evaluation slice named;
- most expensive model error stated;
- CYBER 207 modeling scope confirmed.

---
# sklearn workflow reference · no assessment answers
```python
pipe = Pipeline([
  ("prep", StandardScaler()),
  ("model", LogisticRegression(max_iter=1000, random_state=7))
])
scores = cross_validate(pipe, X_train, y_train, cv=splitter,
            scoring=["precision", "recall", "f1"])
pipe.fit(X_train, y_train)
final_pred = pipe.predict(X_test)  # once, after choices freeze
```

Transforms fit inside folds. The untouched test set supports only the final estimate.

---
# Next week continuity
Week 9 begins feedforward neural networks and training.

Bring your approved proposal and classical baseline. Neural models do not remove the need for split discipline, calibration, uncertainty, and error analysis.

Next focus: layered nonlinear functions learn useful representations from features.

---
# References and visual-design acknowledgment
- https://scikit-learn.org/stable/modules/compose.html
- https://scikit-learn.org/stable/modules/cross_validation.html
- https://scikit-learn.org/stable/modules/model_evaluation.html
- https://scikit-learn.org/stable/common_pitfalls.html

Visual pedagogy was inspired by the general explanatory approaches of Deep Learning for Cyber, MLU-Explain, Distill, and official scikit-learn documentation. All wording, diagrams, HTML, CSS, and examples in this deck are original.
