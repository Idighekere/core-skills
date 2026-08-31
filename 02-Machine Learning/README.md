# 02 - Machine Learning

Hands-on ML from courses and notebooks. The pattern here: each **concept note** explains the *why*, and the **notebooks** show the *how* with real code you can rerun in Colab.

## 01 - Concepts

| Note | Covers |
|------|--------|
| [Data Preprocessing](01-Concepts/Data%20Preprocessing.md) | cleaning, encoding, feature scaling — the first 80% of most projects |
| [Simple Linear Regression](01-Concepts/Simple%20Linear%20Regression.md) | ordinary least squares, cost, gradient descent, evaluation |
| [Linear Classifiers](01-Concepts/Linear%20Classifiers.md) | index — what linear classifiers are, links to the two below |
| [Logistic Regression](01-Concepts/Logistic%20Regression.md) | sigmoid, log loss, decision boundary, `predict_proba` |
| [Support Vector Machines](01-Concepts/Support%20Vector%20Machines.md) | max-margin, hinge loss, kernel trick |
| [Loss Functions](01-Concepts/Loss%20Functions.md) | MSE, log loss, hinge — what they measure, when to use each |
| [Supervised Learning in scikit-learn](01-Concepts/Supervised%20Learning%20in%20scikit-learn.md) | the 5-step workflow (load → split → fit → predict → score) |
| [Classification](01-Concepts/Classification.md) | accuracy, precision/recall/F1, ROC-AUC, threshold tuning, class imbalance |
| [Regression](01-Concepts/Regression.md) | MAE, RMSE, R², residual plots |
| [Fine-Tuning Models](01-Concepts/Fine-Tuning%20Models.md) | cross-validation, `GridSearchCV` / `RandomizedSearchCV`, the leakage trap |
| [Pipelines](01-Concepts/Pipelines.md) | `Pipeline` + `ColumnTransformer`, automatic fit/transform discipline |
| [Tree Based Models](01-Concepts/Tree%20Based%20Models.md) | CART — Gini, entropy, MSE reduction, `max_depth` |
| [The Bias-Variance Tradeoff](01-Concepts/The%20Bias-Variance%20Tradeoff.md) | the decomposition, learning curves, regularisation, what knobs affect what |

## 02 - Code

Notebooks (load into Google Colab/Jupyter; the CSVs are local helper data and are git-ignored — re-download or re-upload as needed):

| Notebook | Open in Colab |
|----------|---------------|
| [data-preprocessing.ipynb](02-Code/data-preprocessing.ipynb) | <a href="https://colab.research.google.com/github/Idighekere/core-skills/blob/main/02-Machine%20Learning/02-Code/data-preprocessing.ipynb" target="_blank"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |
| [simple-linear-regression.ipynb](02-Code/simple-linear-regression.ipynb) | <a href="https://colab.research.google.com/github/Idighekere/core-skills/blob/main/02-Machine%20Learning/02-Code/simple-linear-regression.ipynb" target="_blank"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |

**Data note:** `Salary_Data.csv` (regression) and `Data.csv` (preprocessing) are kept locally but **not committed** to git. To run the notebooks, upload the CSVs in Colab or download them from the course sources.

## Priors / next steps

- Everything here was written on a first pass through ML coursework (IBM machine learning + a linear-regression intro) and the DataCamp ML Scientist in Python track. Revisit the concept notes and rewire examples into a small portfolio project.
- Next topic: ensemble methods (bagging / random forest / boosting), once `Tree Based Models.md` and `The Bias-Variance Tradeoff.md` are reviewed.