---
title: "Pipelines"
date: 2026-08-31
type: concept
tags: [ml, concept/pipelines]
---

# Pipelines

A `Pipeline` chains preprocessing and a model into a single object. The fit happens on training data, the transform happens on every split — and the leakage rule is enforced automatically because there's only one `.fit()` call to make.

## Why bother

Without a pipeline:

```python
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)   # fit + transform on train
X_test  = scaler.transform(X_test)        # transform only on test
model.fit(X_train, y_train)
```

You have to remember to `fit_transform` on train and only `transform` on test. Forget once — the model's evaluation is broken, and the bug is silent. Every ML practitioner hits this at least once.

With a pipeline:

```python
from sklearn.pipeline import Pipeline

pipe = Pipeline([
    ("scale", StandardScaler()),
    ("clf", LogisticRegression()),
])
pipe.fit(X_train, y_train)
pipe.score(X_test, y_test)
```

The pipeline handles the fit/transform distinction. Whatever you wrap inside it is fit on training data only, then applied everywhere. Cross-validation works correctly without extra ceremony.

## Basic structure

A pipeline is a list of `(name, transformer_or_model)` steps. The last step is usually the model; everything before it is preprocessing. Every step except the last must have a `transform` method (or be a `Pipeline` itself, or be a `ColumnTransformer`).

The pipeline exposes the usual model API:
- `pipe.fit(X, y)` — fits every step in order
- `pipe.predict(X)` — passes `X` through every step, returns the model's prediction
- `pipe.predict_proba(X)` — if the model supports it
- `pipe.score(X, y)` — model's score on the transformed data
- `pipe.named_steps["scale"]` — access any step by name for inspection

## ColumnTransformer for mixed data

Real data has numeric and categorical columns mixed. The same `StandardScaler` can't scale strings; the same `OneHotEncoder` can't touch numbers. `ColumnTransformer` applies different transformers to different columns in parallel.

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder

numeric = ["age", "salary"]
categorical = ["country", "plan"]

preprocess = ColumnTransformer([
    ("num", StandardScaler(), numeric),
    ("cat", OneHotEncoder(handle_unknown="ignore"), categorical),
])

pipe = Pipeline([
    ("prep", preprocess),
    ("clf", LogisticRegression()),
])
pipe.fit(X_train, y_train)
```

Each transformer is fit only on its own columns. `handle_unknown="ignore"` keeps the pipeline from crashing on a category it didn't see in training (e.g. a new country in the test set).

## The `memory` trick

If you nest pipelines and use `GridSearchCV`, the same preprocessing runs once per hyperparameter combo. That's wasteful. `Pipeline(memory="path/to/cache")` caches the intermediate results so the same `(X, params)` reuses the cached transform.

```python
from joblib import Memory
cache = Memory(location="cache", verbose=0)

pipe = Pipeline([
    ("prep", preprocess),
    ("clf", LogisticRegression()),
], memory=cache)
```

Only worth it for expensive preprocessing on large datasets. Skip for small ones — the bookkeeping isn't free.

## When to use a pipeline

Use one when:
- The model needs preprocessing (scaling, encoding, imputation) before training.
- You'll use cross-validation — pipelines prevent per-fold leakage.
- You'll do hyperparameter search — same reason.

Skip one when:
- The model is a tree (no scaling needed) and there's no other preprocessing.
- The data is already in the right shape and you're doing a one-off fit.

## Pitfalls

- **Fitting preprocessing outside the pipeline.** If you do `X = scaler.fit_transform(X)` before `pipe.fit(X, y)`, you've fit the scaler on data including the test set. The pipeline's safety net is gone.
- **Wrapping a model that doesn't need preprocessing.** Adds nothing, makes the code longer. Trees don't need scaling — they only care about ordering, not magnitude.
- **Forgetting `handle_unknown="ignore"` on the encoder.** A new category in the test set throws. Costs you one error message in production.
- **Pipelines for everything.** Pipelines don't help for feature engineering that needs the target variable (target encoding, some imputations). Those need to live outside the pipeline, with their own leakage discipline.

## Related

- [Data Preprocessing](Data%20Preprocessing.md) — the leakage rule, in detail
- [Classification](Classification.md) — uses pipelines heavily
- [Regression](Regression.md) — same
- [Fine-Tuning Models](Fine-Tuning%20Models.md) — pipelines + `GridSearchCV` is the standard combination