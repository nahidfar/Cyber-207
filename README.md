# CYBER 207 — Machine Learning & Deep Learning for Cybersecurity

Student-facing presentation and notebook set for the redesigned 14-week course.

## Weekly presentations

The rendered HTML decks are available in `slides/` and from the course landing page.

## Student notebooks

- `notebooks/linear-regression-gradient-descent.ipynb`: from-scratch gradient descent, learning-rate behavior, leakage-safe regression pipelines, baselines, and residual analysis using synthetic alert-triage data.
- `notebooks/knn.ipynb`: manual distance calculations, neighbor voting, scaling, validation-based selection of `k`, imbalanced metrics, and decision boundaries using synthetic authentication data.
- `notebooks/week-02-trees-bagging-boosting.ipynb`: offline synthetic-data lab for evaluation discipline, decision trees, bagging, random forests, boosting, scaled KNN, and Multinomial Naive Bayes.
- `notebooks/security-feature-engineering.ipynb`: point-in-time feature construction, missingness, categorical encoding, robust scaling, leakage detection, ablation, drift checks, and feature-contract tests using synthetic network telemetry.

## Standalone topic lessons

- `slides/week-01-linear-regression-gradient-descent.html`: original 30-slide lesson covering regression framing, predictions, residuals, MAE/MSE/RMSE, baselines, multiple regression, gradient derivation, learning rates, scaling, regularization, and security evaluation. Includes interactive slope/intercept and parameter-space descent demonstrations and pairs with `notebooks/linear-regression-gradient-descent.ipynb`.
- `slides/week-02-knn.html`: original 21-slide KNN lesson with an interactive neighborhood-size visual, distance metrics, scaling, model selection, imbalanced evaluation, complexity, and security failure modes. Paired with `notebooks/knn.ipynb`.
- `slides/week-02-decision-trees-bagging-boosting.html`: original 27-slide lesson covering greedy tree construction, impurity, pruning, bootstrap aggregation, random forests, OOB evidence, AdaBoost, gradient boosting, and security evaluation. Includes interactive split, bootstrap, and boosting-stage demonstrations and pairs with `notebooks/week-02-trees-bagging-boosting.ipynb`.
- `slides/week-03-security-feature-engineering-project-1.html`: worked feature contracts, numeric and categorical transformations, temporal windows, leakage audits, train-only preprocessing, ablation, and offline/online parity. Pairs with `notebooks/security-feature-engineering.ipynb`.

## Structure

- `slides/week-*.html`: browser presentations with CSS algorithm animations
- `notebooks/*.ipynb`: executable student labs
- `course-syllabus.html`: detailed 14-week course roadmap

The decks use visual, intuition-first explanations followed by compact mathematics, defensive cybersecurity examples, scikit-learn implementations, important parameter guidance, and operational interpretation. HTML exports animate core algorithm concepts and respect reduced-motion preferences.

The course maintains the intended boundary with CYBER 290: CYBER 207 focuses on how models learn, how to evaluate them, and how to select methods for defensive cybersecurity applications.
