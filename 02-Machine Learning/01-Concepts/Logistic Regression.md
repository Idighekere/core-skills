---
title: "Logistic Regression"
date: 2026-08-31
type: concept
tags: [ml, concept/logistic-regression]
---

# Logistic Regression

A linear classifier. Despite the name, it predicts a **class** (0 or 1), not a regression target. The "regression" refers to the continuous probability it produces in between.

## What the model is

A linear model that turns a real-valued score into a probability using the **sigmoid** function. The score is the usual linear combination; the sigmoid squashes it into $(0, 1)$.

$$\hat{p} = \sigma(z) = \frac{1}{1 + e^{-z}}, \quad z = w \cdot x + b$$

- $w$ is a weight vector, one per feature.
- $b$ is the bias (intercept).
- $x$ is the input feature vector.
- $z$ is the linear score, before squashing.
- $\hat{p}$ is the model's estimate of $P(y = 1 \mid x)$.

The decision rule is then:

$$\hat{y} = \begin{cases} 1 & \text{if } \hat{p} \ge 0.5 \\ 0 & \text{otherwise} \end{cases}$$

The 0.5 threshold is the default, but it's not sacred — see [Classification](Classification.md) for when to move it.

## Why the sigmoid

Any linear function $w \cdot x + b$ outputs real numbers. Probabilities live in $[0, 1]$. The sigmoid is the simplest monotonic squashing that:
- maps any real number to $(0, 1)$,
- is smooth everywhere (so gradient descent works),
- has a tidy derivative $\sigma'(z) = \sigma(z)(1 - \sigma(z))$.

## What the model trains against

[Log loss](Loss%20Functions.md) — the negative log probability of the correct class. The full derivation isn't worth re-doing; what matters is:

$$J(w, b) = -\frac{1}{n} \sum_{i=1}^{n} \left[ y^{(i)} \log \hat{p}^{(i)} + (1 - y^{(i)}) \log (1 - \hat{p}^{(i)}) \right]$$

Log loss is **convex** w.r.t. $w, b$, so gradient descent is guaranteed to find the global minimum. (Try MSE on the same problem and you get a non-convex surface — it works, but it gets stuck.)

## Code

```python
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline

pipe = Pipeline([
    ("scale", StandardScaler()),
    ("clf", LogisticRegression(max_iter=1000)),
])

pipe.fit(X_train, y_train)
pipe.predict(X_test)        # class labels (0 or 1)
pipe.predict_proba(X_test)  # two columns: P(y=0), P(y=1)
```

- `predict` returns the **class label** using the default 0.5 threshold.
- `predict_proba` returns the **probability**, useful when you want to move the threshold or compute a metric that needs probabilities.
- `predict_log_proba` returns $\log \hat{p}$ — handy for sanity-checking your loss.

## Coefficients are interpretable (with care)

After fitting:
- `clf.coef_` shows how the log-odds change per unit of $x$. Positive coefficient → feature pushes towards class 1.
- `clf.intercept_` is the log-odds when every feature is zero (often meaningless without centering).

The "per unit" part is the catch: with un-scaled features, the coefficients aren't comparable. Always scale first (see [Pipelines](Pipelines.md) and [Data Preprocessing](Data%20Preprocessing.md)).

## Limitations

- **Linear decision boundary.** Splits the feature space with a single hyperplane. Add polynomial or interaction features if the boundary curves.
- **Default 0.5 threshold is wrong for imbalanced classes.** If 99% of your data is class 0, predicting 1 every time is wrong; the threshold needs to move. See [Classification](Classification.md).
- **Probabilities aren't always calibrated by default.** `predict_proba` is usually well-calibrated for `LogisticRegression`, but in general (especially for tree ensembles) use `CalibratedClassifierCV` if you need trustworthy probabilities.

## Pitfalls

- Treating the output of `predict` as a probability. It isn't — `predict` already thresholded. Use `predict_proba`.
- Forgetting to scale. Coefficients become nonsense and convergence gets slow.
- Fitting on the test set. The 0.5 threshold and the accuracy score will both look great and both be wrong.
- Reporting accuracy on a class-imbalanced dataset. The classifier can hit 99% by predicting the majority class. Use precision/recall/F1 instead.

## Related

- [Support Vector Machines](Support%20Vector%20Machines.md) — the other linear classifier; same boundary, different loss
- [Loss Functions](Loss%20Functions.md) — log loss derivation
- [Linear Classifiers](Linear%20Classifiers.md) — index
- [Classification](Classification.md) — metrics, threshold tuning, class imbalance
- [Pipelines](Pipelines.md) — why the example above wraps the scaler + classifier
- [Data Preprocessing](Data%20Preprocessing.md) — the scaling step in detail