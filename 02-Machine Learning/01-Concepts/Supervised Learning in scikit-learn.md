---
title: "Supervised Learning in scikit-learn"
date: 2026-08-31
type: concept
tags: [ml, concept/supervised-sklearn]
---

# Supervised Learning in scikit-learn

The pattern every supervised-learning project follows. Five steps, repeated until the held-out score stops improving.

## The shape of the data

You have a labelled dataset $(X, y)$:
- $X$ — the features, a matrix of shape `(n_samples, n_features)`. Each row is one observation; each column is one attribute.
- $y$ — the target, a vector of shape `(n_samples,)`. Numbers for regression, class labels for classification.

"Supervised" because the labels are there — the model sees both the inputs and the correct answer during training, and learns to map inputs to outputs.

The two task types:
- **Classification** — $y$ is a category. Binary (2 classes) or multi-class (3+). See [Classification](Classification.md).
- **Regression** — $y$ is a continuous number. See [Regression](Regression.md).

## The 5-step workflow

1. **Load and inspect.** Read the data; check dtypes, missingness, class balance, scale of features. See [Data Preprocessing](Data%20Preprocessing.md).
2. **Split.** Hold out a test set *once, now*, and don't touch it until the final evaluation. Use `train_test_split` with a fixed `random_state`.
3. **Fit.** Train the model on the training set. Wrap preprocessing and the model in a [Pipeline](Pipelines.md) so fit/transform discipline is automatic.
4. **Predict.** Apply the model to the held-out test set. Score with the [classification](Classification.md) or [regression](Regression.md) metric that matches the cost of mistakes.
5. **Score.** Once. On the held-out test set. This is the number you report.

Steps 3-4 are repeated inside [cross-validation](Fine-Tuning%20Models.md) to pick hyperparameters without leaking the test set. The single test-set score at step 5 is the honest number.

## The minimal example

```python
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=0, stratify=y
)

pipe = Pipeline([
    ("scale", StandardScaler()),
    ("clf", LogisticRegression(max_iter=1000)),
])

pipe.fit(X_train, y_train)
y_pred = pipe.predict(X_test)
print(accuracy_score(y_test, y_pred))
```

- `stratify=y` keeps the class proportions the same in train and test — important for imbalanced data.
- `random_state=0` makes the split reproducible. Pick any number, just pick one and stick with it.
- `accuracy_score` is the right metric only when the classes are roughly balanced. For imbalanced data, use F1 or ROC-AUC.

## What's in each link

- **[Classification](Classification.md)** — accuracy vs. precision/recall/F1, ROC-AUC, threshold tuning, class imbalance.
- **[Regression](Regression.md)** — MAE, RMSE, R², residual plots, what to look for when a regression model is wrong.
- **[Fine-Tuning Models](Fine-Tuning%20Models.md)** — cross-validation, `GridSearchCV`, `RandomizedSearchCV`, the leakage trap.
- **[Pipelines](Pipelines.md)** — `Pipeline` and `ColumnTransformer`, why every project should use them.
- **[Data Preprocessing](Data%20Preprocessing.md)** — the leakage rule, scaling, encoding, imputation, the foundational step.

## Pitfalls

- **Skipping step 2 (the hold-out).** If you fit and score on the same data, the score is meaningless.
- **Using `accuracy` on imbalanced data.** Hit 99% by predicting the majority class. Use F1 or ROC-AUC.
- **Tuning on the test set "just to see".** It isn't "just to see" once you act on what you see. Use cross-validation on the training set for hyperparameter search.
- **Reporting training score.** Always overfits. Report held-out score.
- **Not using a pipeline.** Forces you to manually track which preprocessing was fit where. Skip the bookkeeping; wrap it in a pipeline.

## Related

- [Classification](Classification.md)
- [Regression](Regression.md)
- [Fine-Tuning Models](Fine-Tuning%20Models.md)
- [Pipelines](Pipelines.md)
- [Data Preprocessing](Data%20Preprocessing.md)
- [Linear Classifiers](Linear%20Classifiers.md) — a starting point for the classification step