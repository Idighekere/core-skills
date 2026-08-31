---
title: "Loss Functions"
date: 2026-08-31
type: concept
tags: [ml, concept/loss-functions]
---

# Loss Functions

A loss function measures how wrong a single prediction is. The model trains by minimising the average loss over the training set — the loss is the score it's trying to drive down. Three losses cover most of supervised ML.

## Notation

- $y$ = the actual (true) value
- $\hat{y}$ = the model's prediction
- $n$ = number of samples in the set

All three losses below are **averaged over $n$** — so they're per-sample costs, not totals.

## Mean squared error (MSE) — for regression

The squared distance between prediction and truth, averaged. Big errors get punished hard (the squaring), and the function stays smooth (no kinks) so gradient-based methods work.

$$J_{\text{MSE}} = \frac{1}{n} \sum_{i=1}^{n} \left( y^{(i)} - \hat{y}^{(i)} \right)^{2}$$

- When to use: regression, when outliers are not a big concern.
- Why "squared" not "absolute": squaring makes big errors dominant and keeps the function differentiable everywhere, so gradient descent works without special-casing kinks.
- `sklearn`: implicit (any regressor that minimises squared error).

## Log loss (binary cross-entropy) — for classification

The negative log of the probability the model assigned to the correct class. If the model says "this is class 1 with probability 0.9" and the truth is 1, the loss is small ($\log 0.9 \approx -0.1$). If it says "0.1" and the truth is 1, the loss is large.

$$J_{\text{log}} = -\frac{1}{n} \sum_{i=1}^{n} \left[ y^{(i)} \log \hat{p}^{(i)} + \left(1 - y^{(i)}\right) \log \left(1 - \hat{p}^{(i)}\right) \right]$$

- When to use: binary classification (and the building block of multi-class cross-entropy).
- Inputs $\hat{p}$ are probabilities, not raw scores. If your model outputs scores, feed them through a sigmoid first.
- `sklearn`: `LogisticRegression` minimises log loss by default.

## Hinge loss — for SVMs

A linear penalty that only kicks in when the prediction is on the wrong side of the decision boundary. Correctly-classified points that are far enough from the boundary contribute zero loss; everything else pays a linear cost.

$$J_{\text{hinge}} = \frac{1}{n} \sum_{i=1}^{n} \max\!\left(0,\ 1 - y^{(i)} \hat{s}^{(i)}\right)$$

- $y^{(i)} \in \{-1, +1\}$ (the SVM convention, not 0/1).
- $\hat{s}^{(i)}$ is the model's raw score (not a probability).
- When to use: SVM classification; you generally don't reach for it outside that context.
- The `max(0, ...)` is the "hinge" — flat at zero when the point is correctly classified with margin, then linear as it crosses the margin.

## Which to use

| Task | Default loss | Why |
|---|---|---|
| Regression | MSE | smooth, well-behaved, what `LinearRegression` minimises |
| Binary classification | log loss | probabilistic output, well-calibrated |
| SVM classification | hinge loss | maximises margin, no probabilities |
| Multi-class classification | categorical cross-entropy | log loss summed across classes |

## Code

```python
import numpy as np

def mse(y, y_hat):
    return np.mean((y - y_hat) ** 2)

def log_loss(y, p):
    p = np.clip(p, 1e-15, 1 - 1e-15)  # avoid log(0)
    return -np.mean(y * np.log(p) + (1 - y) * np.log(1 - p))

def hinge(y, s):
    # y in {-1, +1}, s raw score
    return np.mean(np.maximum(0, 1 - y * s))
```

## Pitfalls

- **Using MSE for classification.** The loss surface is non-convex w.r.t. the model parameters (because of the sigmoid), so gradient descent gets stuck. Log loss is convex — same job, no stuck.
- **Forgetting the `clip` in log loss.** $\log 0$ is $-\infty$, so a single confident-wrong prediction can blow up the average. Always clip probabilities away from 0 and 1.
- **Comparing losses across models.** MSE and log loss live on different scales; a "loss of 0.1" means nothing without the model and the data. Compare a model against itself on different splits, not against a different model.

## Related

- [Logistic Regression](Logistic%20Regression.md) — log loss in action
- [Support Vector Machines](Support%20Vector%20Machines.md) — hinge loss in action
- [Regression](Regression.md) — uses MSE
- [Classification](Classification.md) — uses log loss
- [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md) — loss is what you're trading off against complexity
- [LaTeX Cheatsheet](../../LaTeX%20Cheatsheet.md) — the syntax for these formulas