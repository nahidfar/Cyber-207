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
  strong { color:var(--signal); } table { font-size:.70em; background:var(--paper); } th { background:var(--forest); color:#fff; } blockquote,.card { border-left:6px solid var(--amber); background:var(--paper); padding:.55em .85em; border-radius:8px; }
  .clock { width:180px; height:180px; margin:24px auto; border:18px solid var(--forest); border-top-color:var(--amber); border-radius:50%; animation:clockSweep 12s linear infinite; } .evidence { display:flex; gap:12px; } .evidence span { flex:1; height:30px; background:var(--sage); border-radius:8px; animation:evidenceRise 3s ease-in-out infinite; } .evidence span:nth-child(2) { animation-delay:.3s; } .evidence span:nth-child(3) { animation-delay:.6s; }
  @keyframes clockSweep { to { transform:rotate(360deg); } }
  @keyframes evidenceRise { 0%,100% { transform:scaleY(.55); } 50% { transform:scaleY(1); background:var(--amber); } }
  @media (prefers-reduced-motion:reduce) { .clock,.evidence span { animation:none; } }
footer: "CYBER 207 · Week 14 · Final project presentations"
---
<!-- _class: lead -->
# Final project presentations
## Demo, evidence defense, peer review, and course synthesis
CYBER 207 · Week 14 · 90 minutes

---
# Session outcomes
By the close, you can:
- communicate a modeling study as claim–evidence–limitation;
- defend baseline, split, metric, and error-analysis choices;
- review peers using a consistent rubric;
- synthesize classical ML, deep learning, and LLM method selection;
- name a responsible next experiment without overstating results.

---
# Agenda · 90 minutes
| Time | Focus |
|---|---|
| 0–5 | Opening, order, evidence lens |
| 5–65 | Presentations: five 12-minute slots |
| 65–75 | Peer-review completion and gallery synthesis |
| 75–84 | Course synthesis activity |
| 84–88 | Individual evidence reflection |
| 88–90 | Course close |

No formal break: presentation-week exception; quiet transitions provide brief resets.

---
# Evidence lens · 3 minutes
Record one durable evaluation principle, one exposed failure mode, one insufficient standalone metric, and one baseline that changed the conclusion.

Use this as your listening lens.

---
# Presentation order and timing
Each team: **12 minutes total**
- 0:00–1:00 problem and decision;
- 1:00–3:00 data, representation, split;
- 3:00–6:00 baseline and model;
- 6:00–8:30 held-out evidence + errors;
- 8:30–10:00 live/recorded reproducible demo;
- 10:00–12:00 evidence defense.

Timekeeper gives 3-, 1-, and stop signals.

<div class="clock"></div>

<!-- _notes: Reset the visible timer at each team transition. The rotating ring is decorative progress; exact minute calls remain authoritative. -->

# If class size differs
Instructor adjustment without changing evidence requirements:
- 4 teams: 14 minutes each + longer synthesis;
- 5 teams: default 12 minutes;
- 6 teams: 10 minutes each;
- more teams: parallel sections or pre-recorded 6-minute demo + live defense.

Publish the final order before class.

---
# Presentation structure
Every talk follows this evidence spine:
1. decision and user;
2. point-in-time observables;
3. baseline and candidate method;
4. split protecting the claim;
5. operational metric and costly error;
6. concrete failure;
7. next bounded experiment.

---
# Demo rules
- Use only approved local/synthetic data.
- Show a reproducible input → output → interpretation path.
- Have a recording/screenshots as fallback.
- Do not access live targets, external accounts, or sensitive systems.
- A failed demo is acceptable if the team diagnoses it transparently.

---
# Peer-review rubric · 20 points
| Criterion | 0–4 evidence |
|---|---|
| Problem framing | unit, user, action, costly error |
| Data/evaluation | point-in-time split, reproducibility |
| Modeling | baseline, justified method, controlled comparison |
| Evidence | suitable metrics, uncertainty, error analysis |
| Communication | clear claim, limitation, demo, defense |

Use comments to justify scores.

# Feedback quality rubric
Write:
- **one supported strength:** cite a slide/result;
- **one evidence gap:** identify missing or uncertain evidence;
- **one bounded next test:** change one factor;
- **one scope check:** confirm claim does not exceed data.

Avoid style-only praise or proposing a wholly different project.

---
# Evidence defense · framing and data
Rubric anchors:
- named user, decision, unit, and expensive error;
- target observed consistently after prediction time;
- point-in-time features with leakage risks disclosed;
- related entities and windows grouped;
- holdout aligned with intended future;
- transforms fit on training data only;
- collection and label drift documented.

---
# Evidence defense · metrics and models
Rubric anchors:
- primary metric aligned to action and capacity;
- raw confusion or retrieval counts shown;
- variability reported across folds, seeds, or time;
- weakest class or slice identified;
- threshold selected on validation only;
- complex model earns its place over baseline;
- representation choice supported by ablation;
- probability calibration distinguished from ranking.

---
# Evidence defense · LLM method studies
Rubric anchors:
- retrieval evaluated separately from generation;
- citations verified against exact source spans;
- unsupported cases included in abstention scoring;
- prompt, RAG, or fine-tuning tied to the knowledge need;
- stochastic behavior documented under fixed settings.

Focus on learning and evaluation, not CYBER 290 security topics.

---
<!-- _class: lead -->
# Presentation block
## 5–65 minutes
Audience: listen for claim, evidence, limitation, and next experiment. Complete one rubric per team.

---
# Between-team transition · 60 seconds
- submit/save the completed rubric;
- reset demo station;
- next team connects;
- audience records one evidence gap;
- timekeeper announces start.

Transitions are not extra presentation time.

---
# Gallery synthesis · 10 minutes
Place each project on a matrix:
- supervised ↔ unsupervised;
- fixed features ↔ learned representation;
- static data ↔ sequence/text;
- prediction ↔ retrieval/generation.

Then mark its baseline, primary evidence, and principal limitation.

---
# Synthesis: one evidence spine
Across methods:
`decision → data → representation → model/objective → validation choice → untouched test → errors → bounded action`

Changing the model never removes the need for the other links.

---
# Method-selection recap
| Need | Strong first candidate |
|---|---|
| transparent numeric baseline | linear regression |
| calibrated binary decision | logistic regression |
| nonlinear tabular baseline | tree ensemble |
| unlabeled structure | k-means/PCA |
| local ordered pattern | CNN |
| sequence state | LSTM/RNN/CNN comparison |
| document-grounded answers | retrieval + measured generation |

Start simple; earn complexity.

---
# Guided synthesis activity · 9 minutes
Each group selects another team’s project and proposes:
1. one simpler baseline;
2. one stronger split or slice;
3. one controlled ablation;
4. one operationally meaningful metric;
5. one claim that should be narrowed.

Share the most useful proposal in 30 seconds.

---
# Common presentation failure modes
1. Architecture tour without decision.
2. Best score without baseline.
3. Random split despite entities/time.
4. Aggregate metric without counts/errors.
5. Demo presented as evaluation.
6. Limitation slide with generic caveats.
7. Future work expands scope instead of testing uncertainty.

---
# Final deliverable checklist
Submit the final package:
- report and presentation;
- reproducible code/notebook;
- approved local/synthetic data or generation script;
- environment/dependency record;
- data sheet and split manifest;
- metrics/error artifacts;
- demo fallback;
- team contribution statement.

---
# Individual evidence reflection
- strongest supported claim;
- result with greatest uncertainty;
- lesson supplied by the baseline;
- next experiment with highest information value;
- evaluation habit carried into future defensive work.

---
# Course synthesis
You can now:
- frame defensive needs as measurable learning tasks;
- protect evidence with point-in-time splits;
- compare classical and deep representations;
- interpret metrics through operational costs;
- evaluate retrieval and generation separately;
- communicate errors and limits honestly.

---
# Course boundary revisited
**CYBER 207:** how models learn, represent, predict, retrieve, generate, and are evaluated.

**CYBER 290:** prompt attacks, red teaming, governance, secure deployment, agent security, and adversarial system concerns.

Knowing the boundary keeps both courses deep rather than repetitive.

---
# Model card and project closeout
- intended user, bounded use, and excluded use;
- data provenance, time range, and representation;
- baseline, model, version, and decision threshold;
- held-out metrics with uncertainty and slices;
- concrete limitations and failure cases;
- human review path and monitoring signals;
- owner, artifact location, and next review date.

---
# sklearn reproducibility checklist
- fixed and recorded `random_state` values;
- `Pipeline` contains every learned transform;
- split indices or group/time rules are versioned;
- estimator parameters captured with `get_params()`;
- environment and scikit-learn version recorded;
- final metrics regenerated from frozen artifacts;
- `check_is_fitted` or equivalent precondition verified;
- model persistence used only across compatible environments.

---
# Career and next steps
Translate the evidence spine into portfolio language:
- **problem framing:** connect telemetry to a human decision;
- **modeling:** earn complexity against a baseline;
- **evaluation:** protect time, groups, and costly slices;
- **communication:** pair every claim with evidence and limits;
- **growth:** deepen data engineering, ML systems, detection science, or applied AI evaluation.

<div class="evidence"><span></span><span></span><span></span></div>

<!-- _notes: Close with concrete role families and evidence habits rather than promises about job outcomes. -->

---
<!-- _class: lead -->
# Course close
## Evidence before confidence
Thank your teammates and peer reviewers. Preserve reproducibility artifacts, keep claims bounded, and let the next experiment target the largest uncertainty.

---
# References and visual-design acknowledgment
- https://scikit-learn.org/stable/common_pitfalls.html
- https://scikit-learn.org/stable/model_persistence.html
- https://scikit-learn.org/stable/modules/compose.html
- https://scikit-learn.org/stable/modules/model_evaluation.html

Visual pedagogy was inspired by the general explanatory approaches of Deep Learning for Cyber, MLU-Explain, Distill, and official scikit-learn documentation. All wording, diagrams, HTML, CSS, and examples are original.
