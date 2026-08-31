---
title: "Linear Classifiers"
date: 2026-08-31
type: concept
tags: [ml, concept/linear-classifiers]
---

# Linear Classifiers

Models that split the feature space with a single hyperplane. The "linear" refers to the boundary, not the data — a linear classifier on polynomial features draws a curve.

## The shared idea

A linear classifier takes a feature vector $x$ and assigns it to a class based on a linear score:

$$\hat{y} = f(w \cdot x + b)$$

- $w$ is a weight vector, one per feature.
- $b$ is the bias (intercept).
- $f$ is the activation — the part that turns a real-valued score into a class.

Different linear classifiers differ only in $f$ and in the loss they train against. Everything else is the same.

## What $f$ looks like for each model

| Model | $f$ (activation) | Output | What it means |
|---|---|---|---|
| Logistic Regression | sigmoid $\sigma(z) = \frac{1}{1 + e^{-z}}$ | probability in $(0, 1)$ | confidence the input is class 1 |
| SVM (linear) | $\text{sign}(z)$ | class label $-1$ or $+1$ | which side of the margin |
| Perceptron | $\text{sign}(z)$ | class label | predecessor of logistic regression, no margin |

The score $z = w \cdot x + b$ is the same for all three. The decision boundary is always $z = 0$.

## Choosing between them

Default to **logistic regression** when you need probabilities or calibration. Default to **SVM** when you mainly need class labels, the margin story is the right framing, or you need a non-linear boundary via the kernel trick.

You don't need to choose once. The two are often near-identical in practice on well-behaved data — fit both, compare on a held-out set, pick the one with the metric you care about.

## What's *not* a linear classifier

- Decision trees, random forests, gradient boosting (axis-aligned or hierarchical splits, not a hyperplane).
- k-Nearest Neighbours (no learned boundary at all).
- Neural networks with hidden layers (the hidden layer is the whole point — the boundary is non-linear in the input space).

## Related

- [Logistic Regression](Logistic%20Regression.md) — sigmoid activation, log loss
- [Support Vector Machines](Support%20Vector%20Machines.md) — margin maximisation, hinge loss, kernel trick
- [Loss Functions](Loss%20Functions.md) — log loss and hinge loss side by side
- [Tree Based Models](Tree%20Based%20Models.md) — non-linear alternative