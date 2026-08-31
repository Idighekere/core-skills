---
title: "Fine-Tuning Models"
date: 2026-08-31
type: concept
tags: [ml, concept/fine-tuning]
---

# Fine-Tuning Models

Training a model once on a train/test split tells you how that one model does on that one split. It doesn't tell you how the model will do on data it hasn't seen, and it doesn't tell you which hyperparameters are best. Both questions need **cross-validation**.

## The one rule

**Fit on training data only. Score on data the model has never touched.** This is the same rule as the train/test split; cross-validation just applies it more times.

## Hold-out vs k-fold

**Hold-out:** split into train + test once, train, score, done. Fast. The score is a single noisy estimate of test performance.

**K-fold:** split into $k$ equal pieces. For each piece, treat it as the test set and train on the other $k-1$ pieces. Average the $k$ scores.

Suppose you have 1,000 rows and $k = 5$. Each fold trains on 800 rows and tests on 200. You get 5 scores; their average is your cross-validated estimate.

Why bother: the hold-out score depends on which 200 rows you happened to pick. K-fold averages that out. The trade-off is cost — $k$ models instead of 1.

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(model, X, y, cv=5, scoring="accuracy")
print(scores)              # array of 5 scores
print(scores.mean(), "+/-", scores.std())
```

The `+/-` matters: high standard deviation means the model is sensitive to the split, and you don't fully trust the mean.

## Picking hyperparameters

A hyperparameter is a knob set *before* training — regularisation strength, tree depth, kernel bandwidth, learning rate. You pick it by trying values and seeing which generalises best.

**Grid search:** exhaustive over a grid. Works when the space is small.

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    "C": [0.1, 1.0, 10.0],
    "gamma": ["scale", 0.01, 0.1, 1.0],
}
search = GridSearchCV(model, param_grid, cv=5, scoring="f1", n_jobs=-1)
search.fit(X_train, y_train)
print(search.best_params_, search.best_score_)
```

**Random search:** sample $n$ random combinations. Better than grid when some hyperparameters matter more than others — and almost always faster.

```python
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import loguniform

param_dist = {
    "C": loguniform(1e-3, 1e3),
    "gamma": loguniform(1e-4, 1e1),
}
search = RandomizedSearchCV(model, param_dist, n_iter=50, cv=5, n_jobs=-1)
search.fit(X_train, y_train)
```

**Heuristic: use `loguniform`** for hyperparameters that span orders of magnitude (regularisation strength, learning rate, kernel bandwidth). Searching `[0.001, 0.01, 0.1, 1, 10]` covers the space better than `[1, 2, 3, 4, 5]`.

## The leakage trap

If you use cross-validation to pick hyperparameters, then report the cross-validated score as "test performance", you've leaked the test set into the model selection step. The reported score is optimistic.

The right shape:
1. Outer split: hold out a **true test set**, never touched.
2. Inner cross-validation: on the training portion, pick hyperparameters.
3. Refit the best model on the full training portion.
4. Report score on the **held-out test set, once**.

```python
from sklearn.model_selection import train_test_split, cross_val_score

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

# pick best hyperparameters with CV on the training set
best_score = -1
for C in [0.1, 1.0, 10.0]:
    model.set_params(C=C)
    scores = cross_val_score(model, X_train, y_train, cv=5)
    if scores.mean() > best_score:
        best_score = scores.mean()
        best_C = C

# refit on full training set, score on test set ONCE
model.set_params(C=best_C).fit(X_train, y_train)
print(model.score(X_test, y_test))
```

This is a manual nested cross-validation. `GridSearchCV` does it for you if you wrap the whole thing in another `cross_val_score` — that's the proper nested CV, but it's $k$ times more expensive. For most projects, the manual version above is enough.

## The bias-variance frame

Most "hyperparameter tuning" stories reduce to one question: how much should the model fit the training data? See [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md).

- **High bias (underfitting):** model too simple, training score is low, cross-val score is low, both close. Add capacity.
- **High variance (overfitting):** training score is high, cross-val score is much lower. Reduce capacity or add regularisation.
- **Sweet spot:** training score is high, cross-val score is as high as it gets without the gap widening.

## Pitfalls

- **Reporting cross-val score as test score.** It isn't — the hyperparameters were picked to maximise it.
- **Picking the best hyperparameter, then continuing to tune.** Each "best" you pick is the best *given the values you tried*. Try a wider grid, or random search.
- **Using `GridSearchCV` and trusting the default `refit=True`.** It refits on the full training set using the best params — fine, but the score `search.best_score_` is the **CV** score, not the test score.
- **Tuning on the test set "just once".** It's not once if you keep doing it. Either commit to a clean test set or use nested CV.

## Related

- [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md) — the frame behind hyperparameter choices
- [Classification](Classification.md) — picking the metric (`scoring=...`) to optimise
- [Regression](Regression.md) — same logic for regression metrics
- [Pipelines](Pipelines.md) — wraps preprocessing + model so the leakage rule is enforced
- [Data Preprocessing](Data%20Preprocessing.md) — the leakage rule, stated generally