---
title: "Support Vector Machines"
date: 2026-08-31
type: concept
tags: [ml, concept/svm]
---

# Support Vector Machines

A linear classifier that picks the **decision boundary farthest from both classes** — the maximum-margin line. Optionally extended to non-linear boundaries via the kernel trick.

## The intuition

Two classes can usually be split by many lines. SVM picks the one that **maximises the margin** — the gap between the line and the nearest point of either class. The intuition: a wider margin = a more confident boundary, which generalises better.

The points that sit exactly on the edges of the margin are called the **support vectors**. They're the only training points that matter for the final model — move any other point and the boundary doesn't change. Move a support vector and the boundary shifts.

## The objective

For binary labels $y \in \{-1, +1\}$ and a score $\hat{s} = w \cdot x + b$, the margin-maximising objective is:

$$\min_{w, b} \ \frac{1}{2} \|w\|^{2} \quad \text{subject to} \quad y^{(i)} \left( w \cdot x^{(i)} + b \right) \ge 1 \ \text{for all } i$$

The $\|w\|^{2}$ term shrinks the weight vector (a small $\|w\|$ = a wide margin). The constraint says every point must be correctly classified **with at least unit margin** — i.e. on the correct side, beyond the margin line.

In the soft-margin version (the practical default), some points are allowed to violate the constraint. Each violation costs a slack variable $\xi^{(i)}$:

$$\min_{w, b, \xi} \ \frac{1}{2} \|w\|^{2} + C \sum_{i=1}^{n} \xi^{(i)} \quad \text{subject to} \quad y^{(i)}(w \cdot x^{(i)} + b) \ge 1 - \xi^{(i)}, \quad \xi^{(i)} \ge 0$$

$C$ is the regularisation knob. Big $C$ = "punish violations hard" (narrow margin, low bias, high variance). Small $C$ = "allow violations" (wide margin, more bias, less variance). See [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md).

## The loss view

Equivalent framing: SVM minimises [hinge loss](Loss%20Functions.md) plus a regularisation term.

$$J_{\text{SVM}} = C \sum_{i=1}^{n} \max\!\left(0,\ 1 - y^{(i)} \hat{s}^{(i)}\right) + \frac{1}{2} \|w\|^{2}$$

The first term is hinge (linear penalty for misclassification / margin violation). The second is the margin-maximisation regulariser. Same objective, different lens.

## The kernel trick

Real data is rarely linearly separable. The **kernel trick** projects the features into a higher-dimensional space (via a kernel function) and finds a linear boundary there. In the original space, that boundary becomes non-linear.

Common kernels:
- `linear` — no projection; the boundary is linear in the original space.
- `rbf` (radial basis function) — projects into an infinite-dimensional space; the default for non-linear problems.
- `poly` — projects into a polynomial feature space.

You don't have to know the projection — that's the trick. The kernel computes the dot product in the higher-dimensional space without ever building it.

## Code

```python
from sklearn.svm import SVC
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline

pipe = Pipeline([
    ("scale", StandardScaler()),
    ("clf", SVC(kernel="rbf", C=1.0, gamma="scale")),
])

pipe.fit(X_train, y_train)
pipe.predict(X_test)        # class labels
pipe.predict_proba(X_test)  # only if probability=True; slower
```

- `SVC` (Support Vector Classifier) for classification; `SVR` for regression.
- `kernel="rbf"` + `gamma="scale"` is the safe default for non-linear problems.
- SVMs are **not scale-invariant** — always scale the features. The `Pipeline` above enforces it.
- For large datasets ($n > 10{,}000$ ish), SVMs get slow. Use `LinearSVC` or a different model.

## SVM vs Logistic Regression

Both are linear classifiers. Differences that matter:

| Aspect | Logistic Regression | SVM (linear) |
|---|---|---|
| Output | probability $\hat{p} \in (0, 1)$ | raw score $\hat{s}$ |
| Loss | log loss | hinge loss |
| What it optimises | calibration of probabilities | margin width |
| Probabilities | native | needs calibration or `probability=True` |
| Default choice | when probabilities matter | when you mainly need class labels |

When in doubt, start with logistic regression. Reach for SVM when the margin story is the right framing or when you need a non-linear boundary via the kernel trick.

## Pitfalls

- **Forgetting to scale.** SVMs are far more sensitive to feature scale than logistic regression. Without scaling, one feature dominates the distance and the kernel becomes useless.
- **`C` too high.** Forces the model to fit every training point — overfits hard, especially with `rbf`.
- **`gamma` too high (`rbf` kernel).** Makes each support vector a tiny bump in the decision surface. The model memorises the training set. Tune with cross-validation.
- **Using `SVC` on 100k+ rows.** The training time is roughly $O(n^2)$ to $O(n^3)$. Switch to `LinearSVC` (linear only) or another model.

## Related

- [Logistic Regression](Logistic%20Regression.md) — the other linear classifier
- [Loss Functions](Loss%20Functions.md) — hinge loss in detail
- [Linear Classifiers](Linear%20Classifiers.md) — index
- [Classification](Classification.md) — metrics, threshold tuning
- [Pipelines](Pipelines.md) — why scaling is in the pipeline
- [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md) — what $C$ and $\gamma$ trade off