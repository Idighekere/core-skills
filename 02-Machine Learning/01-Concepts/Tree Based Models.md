---
title: "Tree Based Models"
date: 2026-08-31
type: concept
tags: [ml, concept/tree-based-models]
---

# Tree Based Models

A decision tree splits the feature space with axis-aligned cuts and predicts a value at each leaf. CART — Classification And Regression Trees — is the workhorse variant. Trees are fast to fit, easy to interpret, and the building block for random forests and gradient boosting.

## The shape of a tree

- **Root** — the whole dataset, before any split.
- **Internal node** — a single question on one feature ("is `age` ≤ 30?"). Each node has two children.
- **Leaf** — no children; the prediction is made here.

For classification: each leaf predicts the majority class of the training points that landed there. For regression: each leaf predicts the mean of the training targets that landed there.

A tree of depth $d$ has up to $2^{d}$ leaves. Depth 5 is a small tree; depth 20 is huge. See [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md) for what that buys you.

A CART with a max-depth of 1, is know as **Decision stump**
## How a tree picks the next split

For every feature and every threshold, the tree measures how much the split reduces "impurity" — how mixed the labels are in the two resulting children. It picks the split with the biggest reduction.

**Gini impurity** (default for classification):

$$G = 1 - \sum_{k=1}^{K} p_{k}^{2}$$

$p_{k}$ is the fraction of samples in the node belonging to class $k$. A pure node (all one class) has $G = 0$. A 50/50 binary node has $G = 0.5$.

**Entropy** (alternative, behaves similarly in practice):

$$H = -\sum_{k=1}^{K} p_{k} \log_{2} p_{k}$$

A pure node has $H = 0$. Same intuition as Gini, slightly smoother.

**MSE reduction** (for regression):

$$\text{reduction} = \text{MSE}_{\text{parent}} - \frac{n_{\text{left}}}{n} \text{MSE}_{\text{left}} - \frac{n_{\text{right}}}{n} \text{MSE}_{\text{right}}$$

The split that most reduces the weighted mean-squared-error of the two children wins. Equivalent to maximising the between-child variance.

In all three cases, the algorithm is **greedy** — it picks the best split at the current node, then recurses. It does not plan ahead, so it's fast but locally optimal, not globally optimal.

## Code

```python
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor

clf = DecisionTreeClassifier(max_depth=5, random_state=0)
clf.fit(X_train, y_train)

reg = DecisionTreeRegressor(max_depth=5, random_state=0)
reg.fit(X_train, y_train)
```

The `max_depth` knob is the single most important one. Start with 3-5 and increase until cross-validation stops improving. See [Fine-Tuning Models](Fine-Tuning%20Models.md) for the proper search.

## What trees are good at

- **Non-linear relationships** without you having to engineer polynomial features.
- **Mixed feature types** (numeric + categorical) — no scaling needed.
- **Interactions** between features ("when age > 30 and income < 50k, then…") built in.
- **Interpretability** — the tree is a literal flowchart. `export_graphviz` and `plot_tree` draw it for you.

## What trees are bad at

- **Smooth boundaries.** Trees draw rectangles, not curves. A linear model wins on a smooth linear relationship.
- **Extrapolation.** A regression tree's prediction is bounded by the training data's range. It can't predict above the highest training target, no matter the input.
- **Stability.** Small changes in the training set can produce wildly different trees (high variance). The whole point of random forests and gradient boosting is to fix this.

## Controlling complexity

Three knobs, all of which reduce overfitting:

| Knob | Effect | When to use |
|---|---|---|
| `max_depth` | limits tree size | start with 3-5, grow until CV plateaus |
| `min_samples_leaf` | requires ≥ N samples per leaf | stops the tree from carving a single training point into its own leaf |
| `min_samples_split` | requires ≥ N samples to attempt a split | similar to `min_samples_leaf` |

Increasing any of these makes the model simpler and reduces overfitting. Always pick via cross-validation — the right value is data-specific.

## Code: visualising a tree

```python
from sklearn.tree import plot_tree
import matplotlib.pyplot as plt

plt.figure(figsize=(20, 8))
plot_tree(clf, feature_names=feature_names, class_names=class_names,
          filled=True, max_depth=3)  # first 3 levels only
plt.show()
```

For deeper trees, use `export_graphviz` to a `.dot` file and render with Graphviz, or just trust the feature importances.

## Feature importances

After fitting, `clf.feature_importances_` gives the relative contribution of each feature. Computed as the total impurity reduction across all splits involving that feature, normalised to sum to 1.

Useful for:
- Quick feature selection (drop the bottom-importance features).
- Sanity check (a feature you *know* matters should not be zero).
- Communication (a ranked list of what the model uses).

## Hyperparameter Tuning:
We have several methods to get the hyperparameters to obtained a model that is most optimal including Grid Search, Random Search etc

## Pitfalls

- **No `max_depth` set.** The tree grows until every leaf is pure — guaranteed overfitting on real data.
- **Interpreting `predict_proba` as calibrated.** Tree probabilities are crude — they're the leaf's class fractions. For well-calibrated probabilities, wrap the tree in `CalibratedClassifierCV` or use a different model.
- **Comparing tree depth across datasets.** "Depth 5" means different things on different data. Always compare models by held-out score, not by tree shape.
- **Confusing "the tree is interpretable" with "this model is interpretable".** A single tree is interpretable. A random forest of 500 trees is not. If you need interpretability, use a single tree or a linear model.

## Related

- [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md) — what `max_depth` and the other knobs trade off
- [Classification](Classification.md) — classification metrics
- [Regression](Regression.md) — regression metrics, residual plots
- [Fine-Tuning Models](Fine-Tuning%20Models.md) — picking `max_depth` with cross-validation
- [Logistic Regression](Logistic%20Regression.md) — the linear alternative
- [Bagging and Random Forest](Bagging%20and%20Random%20Forest.md)