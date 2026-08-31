---
title: "The Bias-Variance Tradeoff"
date: 2026-08-31
type: concept
tags: [ml, concept/bias-variance]
---

# The Bias-Variance Tradeoff

A model is wrong for two distinct reasons: it's wrong on average (bias), and it's wrong in different ways on different training sets (variance). Generalisation error decomposes into both, plus irreducible noise. The tradeoff is that reducing one tends to raise the other.

## The decomposition

For a target $y$, a model $\hat{f}(x)$ trained on a sample of data, and squared error:

$$\mathbb{E}\!\left[ \left( y - \hat{f}(x) \right)^{2} \right] = \underbrace{\left( \text{Bias}\!\left[\hat{f}(x)\right] \right)^{2}}_{\text{systematic error}} + \underbrace{\text{Var}\!\left[\hat{f}(x)\right]}_{\text{sensitivity to data}} + \underbrace{\sigma^{2}}_{\text{noise}}$$

- **Bias²** — how far the model's average prediction is from the truth, averaged over possible training sets. High bias = the model is consistently wrong in the same direction.
- **Variance** — how much the model itself moves around when the training set changes. High variance = a different model each time.
- **$\sigma^2$** — irreducible noise in the data. No model can fix it.

The total is what you measure on a held-out set. You can't reduce $\sigma^2$ — but you can trade bias for variance, and back.

## How to read it

- **High bias, low variance** — model is too simple, fits every training set the same (bad) way. Underfitting.
- **Low bias, high variance** — model fits every training set perfectly but a different way each time. Overfitting.
- **Some of each, total low** — the sweet spot. Different training sets give similar models, all close to the truth.

| Regime | Train score | Held-out score | Gap | Fix |
|---|---|---|---|---|
| Underfit | low | low | small | add capacity (more features, deeper tree, hidden layers) |
| Sweet spot | high | high | small | ship it |
| Overfit | high | lower | large | add regularisation, more data, fewer features, simpler model |

## What knobs affect what

| Model | ↑ bias | ↑ variance |
|---|---|---|
| Linear regression | remove features, raise regularisation | add features, lower regularisation |
| Logistic regression | raise regularisation ($C \downarrow$) | lower regularisation ($C \uparrow$) |
| Decision tree | lower depth, raise `min_samples_leaf` | higher depth, lower `min_samples_leaf` |
| SVM | smaller $C$ (wider margin) | larger $C$ (narrow margin, fits more) |
| k-NN | larger $k$ | smaller $k$ |
| Neural network | smaller network, stronger weight decay | bigger network, weaker weight decay |

The pattern: anything that lets the model fit the training data more closely reduces bias and raises variance. Anything that constrains the model does the opposite.

## Reading learning curves

The most useful diagnostic is to plot training score and held-out score as functions of training-set size.

- **Both curves plateau, high, close together** — more data won't help; the model is biased. Add capacity.
- **Big gap between the two curves, narrow as data grows** — more data closes the gap. The model is well-suited but variance-limited.
- **Both curves low, close** — model is wrong, period. Try a different model class.

A learning curve you can read at a glance is worth more than a dozen metric reports.

## Why more data helps (and when it doesn't)

Adding data reduces variance — the model sees more examples, so the random fluctuations in any one training set get smaller. The bias stays roughly the same.

When more data stops helping:
- You've reached the irreducible noise $\sigma^2$.
- The model is too biased to capture the true pattern (more data can't fix a model that's missing the non-linearity).
- The new data has the same distribution as the old (truly fresh data often helps more than "more of the same").

## Regularisation

A regulariser is a penalty on model complexity. It explicitly trades variance for bias:

$$J_{\text{reg}}(w) = \underbrace{J_{\text{loss}}(w)}_{\text{fit the data}} + \underbrace{\lambda \, \Omega(w)}_{\text{stay simple}}$$

- $\lambda$ is the regularisation strength. Higher $\lambda$ = more bias, less variance.
- $\Omega(w)$ is the complexity penalty. $\ell_2$ (Ridge) is $\|w\|^{2}$. $\ell_1$ (Lasso) is $\|w\|_{1}$ and tends to drive weights to exactly zero — useful for feature selection.

The right $\lambda$ is data-specific. Pick it with [cross-validation](Fine-Tuning%20Models.md).

## Connections

- [Loss Functions](Loss%20Functions.md) — the $J_{\text{loss}}$ that regularisation gets added to.
- [Tree Based Models](Tree%20Based%20Models.md) — the `max_depth` knob is regularisation.
- [Support Vector Machines](Support%20Vector%20Machines.md) — the $C$ knob is the inverse of regularisation.
- [Logistic Regression](Logistic%20Regression.md) — the `C` knob is the inverse of regularisation.
- [Fine-Tuning Models](Fine-Tuning%20Models.md) — the cross-validation that picks the knobs.
- [Classification](Classification.md) — precision/recall is one face of the bias-variance tradeoff (raise the threshold → higher precision, lower recall).
- [Regression](Regression.md) — R² on training vs held-out is the most common bias-variance tell.

## Pitfalls

- **Thinking "more data fixes everything."** It fixes variance; it doesn't fix bias.
- **Adding regularisation until training score matches held-out score.** That's overfitting, and the fix is wrong — the right move is more capacity, not more regularisation.
- **Equating "complex model" with "low bias, high variance."** A complex model with strong regularisation can have low bias and low variance simultaneously.
- **Looking at one number.** The decomposition is a story; a single test score is the punchline, not the plot.