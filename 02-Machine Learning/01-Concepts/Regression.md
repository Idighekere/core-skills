---
title: "Regression"
date: 2026-08-31
type: concept
tags: [ml, concept/regression]
---

# Regression

Predicting a **continuous number** — price, temperature, salary, distance. Not a class. The output is a real value; the metrics measure how far off the prediction is from the truth.

## The residuals

For every prediction $\hat{y}^{(i)}$, the residual is how wrong you were:

$$e^{(i)} = y^{(i)} - \hat{y}^{(i)}$$

Three numbers characterise the whole set of residuals: their **average size**, their **typical size**, and the **fraction of variance they explain**. The three metrics below.

## The metrics

**MAE (Mean Absolute Error)** — average of the absolute residuals. Same unit as the target, easy to interpret ("on average you're off by 0.4").

$$\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} \left| y^{(i)} - \hat{y}^{(i)} \right|$$

**RMSE (Root Mean Squared Error)** — the square root of the average squared residual. Same unit as the target (because of the square root), but punishes large errors more than MAE.

$$\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} \left( y^{(i)} - \hat{y}^{(i)} \right)^{2}}$$

**R² (coefficient of determination)** — the fraction of the target's variance that the model explains. 1.0 = perfect, 0.0 = "as good as predicting the mean", negative = "worse than the mean".

$$R^{2} = 1 - \frac{\sum_{i=1}^{n} \left( y^{(i)} - \hat{y}^{(i)} \right)^{2}}{\sum_{i=1}^{n} \left( y^{(i)} - \bar{y} \right)^{2}}$$

- $R^2$ can be misleading on its own — a model can hit $R^2 = 0.99$ on training data and still generalise terribly. Always evaluate on held-out data.
- $R^2$ is **not** the same as the correlation between $y$ and $\hat{y}$. A model can be perfectly anti-correlated with the truth and still have negative $R^2$.

## Which to use

| Situation | Use |
|---|---|
| Errors are all roughly the same size | MAE |
| Big errors are much worse than small ones | RMSE |
| Comparing models on the same dataset | R² |
| Communicating to a non-technical audience | MAE (units are intuitive) |
| Optimising a model (gradient descent is easier on RMSE) | RMSE or MSE |

## Residual plots

The metrics tell you *how much* error, not *where* it lives. A residual plot shows error as a function of prediction (or input), and is the single best diagnostic for a regression model.

A good residual plot:
- Centres around zero.
- Has constant vertical spread (the model is wrong by the same amount everywhere).
- Shows no pattern (no curve, no fan, no clusters).

Patterns to look for:
- **Curve (U-shape, inverted U):** the model is missing a non-linearity. Add polynomial features.
- **Funnel (spread grows with $y$):** variance is not constant (heteroscedasticity). Try log-transforming $y$, or use a model that allows changing variance.
- **Clusters of large residuals:** there are sub-populations the model treats as one. Add a feature that distinguishes them, or use a model with more capacity.

## Code

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import numpy as np

y_pred = reg.predict(X_test)

mae  = mean_absolute_error(y_test, y_pred)
rmse = np.sqrt(mean_squared_error(y_test, y_pred))
r2   = r2_score(y_test, y_pred)

# residual plot
import matplotlib.pyplot as plt
plt.scatter(y_pred, y_test - y_pred, alpha=0.4)
plt.axhline(0, color="k", linewidth=0.5)
plt.xlabel("predicted")
plt.ylabel("residual")
```

## Pitfalls

- **Reporting R² on the training set.** Always overfits. Report on held-out data.
- **Treating RMSE and MAE as interchangeable.** RMSE is more sensitive to outliers. If the dataset has a few wild values, RMSE will look bad even when the model is "fine" on most points.
- **Ignoring residual patterns.** A great $R^2$ with a U-shaped residual plot is a model that's missing a non-linearity, not a model that's working.
- **Comparing R² across datasets.** R² depends on the variance of the target. The same model on the same data with a constant added to $y$ has a different R².

## Related

- [Simple Linear Regression](Simple%20Linear%20Regression.md) — the simplest case
- [Tree Based Models](Tree%20Based%20Models.md) — regression trees and the residuals they leave
- [Fine-Tuning Models](Fine-Tuning%20Models.md) — using cross-validation to compare metrics honestly
- [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md) — R² is one face of over/underfitting