---
title: "Simple Linear Regression"
date: 2026-08-28
type: concept
tags: [ml, concept/regression]
---

# Simple Linear Regression

## What it answers
Given one input feature `X`, predict a continuous target `y`. The model is a straight line:

```
y = b₀ + b₁·X
```
- `b₁` = **slope** — how much `y` changes per unit of `X`.
- `b₀` = **intercept** — the prediction when `X = 0`.

## The intuition (salary example)
`Salary_Data.csv`: `YearsExperience` → `Salary`. The fitted line says "each extra year ≈ +₦/basar salary increment". Prediction = *read the line*, not memorize a data point.

## How the line is chosen

### Cost function (mean squared error)
For predictions `ŷ` and actuals `y`, the cost quantifies how wrong the line is:

```
J(b₀, b₁) = (1/n) Σ (yᵢ − ŷᵢ)²
```
Squaring makes big errors *much* worse and keeps the function smooth (differentiable, no absolute-value kinks).

### Gradient descent — walking downhill
1. Start somewhere (random `b₀`, `b₁`).
2. Compute the gradient — which way the cost **rises**.
3. Step **against** the gradient: `b = b − α·(∂J/∂b)`.
4. Repeat until the cost stops dropping.

- `α` (learning rate) too big → overshoot & diverge; too small → glacial convergence.
- Each step is "try a slightly better line".

### The closed form (what sklearn really does)
Simple regression has an exact analytic solution (ordinary least squares) — no iteration needed:

```python
import numpy as np
X = np.array([...])  # years of experience
y = np.array([...])  # salary

b1 = np.cov(X, y, ddof=1)[0, 1] / np.var(X, ddof=1)
b0 = np.mean(y) - b1 * np.mean(X)
```
sklearn's `LinearRegression` computes this directly:
```python
from sklearn.linear_model import LinearRegression
reg = LinearRegression()
reg.fit(X.reshape(-1, 1), y)
```
`reg.coef_` → `b₁`, `reg.intercept_` → `b₀`.

## Evaluation

| Metric | Meaning | Value to aim for |
|--------|---------|------------------|
| R² | fraction of variance explained | closer to 1 |
| RMSE | typical error in y-units | as small as domain allows |
| Residuals | `y − ŷ`, should look like random noise | no pattern (no curve/ad-hoc trend) |

## Limitations
- Captures **linear** relationships only; curved data needs polynomial features or other models.
- A high R² on the train set means nothing until checked on held-out data — always split.

## Code
Worked end-to-end: [`02-Code/simple-linear-regression.ipynb`](../02-Code/simple-linear-regression.ipynb) — data: `Salary_Data.csv` (kept local, git-ignored).

## Pitfalls
- Confusing correlation with causation (raise ≠ more experience).
- Interpreting `b₁` as "guaranteed pay raise" instead of "conditional mean increase per year".
- Evaluating on training data alone → overconfident.