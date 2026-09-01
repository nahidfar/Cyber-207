# CYBER 207 — Machine Learning & Deep Learning for Cybersecurity

Complete presentation set for the redesigned 14-week course. Each week is a separate 90-minute Marp deck with animated HTML and PowerPoint exports.

## Weekly presentations

| Week | Topic | PowerPoint |
|---:|---|---|
| 1 | ML framing, data splits, and linear regression | `slides/CYBER207-Week-01-ML-Framing-Splits-Linear-Regression.pptx` |
| 2 | KNN, decision trees, and ensembles | `slides/CYBER207-Week-02-KNN-Trees-Ensembles.pptx` |
| 3 | Security feature engineering | `slides/CYBER207-Week-03-Security-Feature-Engineering.pptx` |
| 4 | Logistic regression and probabilistic decisions | `slides/CYBER207-Week-04-Logistic-Regression.pptx` |
| 5 | Multiclass classification and metrics | `slides/CYBER207-Week-05-Multiclass-and-Metrics.pptx` |
| 6 | K-means and PCA | `slides/CYBER207-Week-06-KMeans-and-PCA.pptx` |
| 7 | Classical ML integration and midterm review | `slides/CYBER207-Week-07-Classical-ML-Integration.pptx` |
| 8 | Midterm and final-project framing | `slides/CYBER207-Week-08-Midterm-and-Project-Framing.pptx` |
| 9 | Feedforward neural networks and training | `slides/CYBER207-Week-09-Feedforward-Neural-Networks.pptx` |
| 10 | Convolutional neural networks | `slides/CYBER207-Week-10-Convolutional-Neural-Networks.pptx` |
| 11 | RNNs, LSTMs, and sequence embeddings | `slides/CYBER207-Week-11-RNNs-LSTMs-Sequence-Embeddings.pptx` |
| 12 | Transformers and LLM foundations | `slides/CYBER207-Week-12-Transformers-and-LLM-Foundations.pptx` |
| 13 | RAG, fine-tuning, and LLM evaluation | `slides/CYBER207-Week-13-RAG-Fine-Tuning-LLM-Evaluation.pptx` |
| 14 | Final project presentations | `slides/CYBER207-Week-14-Final-Project-Presentations.pptx` |

## Instructor-guided notebooks

- `notebooks/linear-regression-gradient-descent.ipynb`: from-scratch gradient descent, learning-rate behavior, leakage-safe regression pipelines, baselines, and residual analysis using synthetic alert-triage data.
- `notebooks/knn.ipynb`: manual distance calculations, neighbor voting, scaling, validation-based selection of `k`, imbalanced metrics, and decision boundaries using synthetic authentication data.
- `notebooks/week-02-trees-bagging-boosting.ipynb`: offline synthetic-data lab for evaluation discipline, decision trees, bagging, random forests, boosting, scaled KNN, and Multinomial Naive Bayes.

## Notebook walkthroughs

- `walkthroughs/linear-regression-gradient-descent.html`: 60–80 minute cell-by-cell teaching guide with expected metrics, visual prompts, troubleshooting, and extensions.
- `walkthroughs/knn.html`: 60–75 minute guide to manual distance, scaling, model selection, imbalanced evaluation, and operational interpretation.
- `walkthroughs/trees-bagging-boosting.html`: 90–120 minute guide covering baselines, controlled trees, ensembles, boosting, model comparison, KNN, and text Naive Bayes.
- `walkthroughs/index.html`: deployable walkthrough collection linked from the course landing page.

## Standalone topic lessons

- `slides/week-01-linear-regression-gradient-descent.html`: original 30-slide lesson covering regression framing, predictions, residuals, MAE/MSE/RMSE, baselines, multiple regression, gradient derivation, learning rates, scaling, regularization, and security evaluation. Includes interactive slope/intercept and parameter-space descent demonstrations and pairs with `notebooks/linear-regression-gradient-descent.ipynb`.
- `slides/week-02-knn.html`: original 21-slide KNN lesson with an interactive neighborhood-size visual, distance metrics, scaling, model selection, imbalanced evaluation, complexity, and security failure modes. Paired with `notebooks/knn.ipynb`.
- `slides/week-02-decision-trees-bagging-boosting.html`: original 27-slide lesson covering greedy tree construction, impurity, pruning, bootstrap aggregation, random forests, OOB evidence, AdaBoost, gradient boosting, and security evaluation. Includes interactive split, bootstrap, and boosting-stage demonstrations and pairs with `notebooks/week-02-trees-bagging-boosting.ipynb`.

## Structure

- `slides/week-*.md`: editable Marp presentation sources with original concept animations
- `slides/week-*.html`: browser presentations with CSS algorithm animations
- `slides/CYBER207-Week-*.pptx`: separate PowerPoint presentations
- `instructor/all-weeks-slide-guide.md`: preparation, misconceptions, activity anchors, and timing adjustments
- `course-syllabus.html`: detailed 14-week course roadmap

The decks use visual, intuition-first explanations followed by compact mathematics, defensive cybersecurity examples, scikit-learn implementations, important parameter guidance, and operational interpretation. Quiz and question slides are intentionally omitted. HTML exports animate core algorithm concepts and respect reduced-motion preferences; PowerPoint exports retain readable static versions.

The course maintains the intended boundary with CYBER 290: CYBER 207 focuses on how models learn, how to evaluate them, and how to select methods for defensive cybersecurity applications.
