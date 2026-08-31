---
title: "LaTeX Cheatsheet"
date: 2026-08-31
type: reference
tags: [reference, latex]
---

# LaTeX Cheatsheet

The 12 symbols and 8 structures you need to write the math in the ML concept notes. Nothing here teaches the math — the linked notes do. This teaches the *syntax*.

Render target: GitHub and Obsidian both use KaTeX. Same input, same output. The only delimiters you need are `$...$` for inline and `$$...$$` for display.

## Inline vs display

```markdown
Sigmoid is $\sigma(z) = \frac{1}{1 + e^{-z}}$, the squashing function.
$$
\hat{p} = \sigma(z) = \frac{1}{1 + e^{-z}}, \quad z = w \cdot x + b
$$
```

Inline math flows inside a sentence. Display math is on its own lines, with a blank line above and below.

## The 12 symbols

| LaTeX | Renders as | When to use |
|---|---|---|
| `\hat{y}` | ŷ | "predicted y" (anything that estimates something) |
| `\bar{x}` | x̄ | mean of a sample |
| `\cdot` | · | dot product, e.g. `w \cdot x` |
| `\times` | × | multiplication of two things (not same as `\cdot`) |
| `\le`, `\ge` | ≤, ≥ | comparisons |
| `\ne` | ≠ | not equal |
| `\to` | → | "maps to" (`f: X \to Y`) |
| `\infty` | ∞ | infinity |
| `\sum_{i=1}^{n}` | ∑ | sum over a range |
| `\prod_{i=1}^{n}` | ∏ | product over a range |
| `\partial` | ∂ | partial derivative (the one you'll see in gradient descent) |
| `\nabla` | ∇ | gradient (the vector of all partials) |
| `\in` | ∈ | "is an element of" |

## The 8 structures

### Fractions
```latex
\frac{a}{b}
```
Use for: ratios, the sigmoid, probabilities.

### Powers and subscripts
```latex
x^{2}        e^{-z}        x^{(i)}        x_{i}        x_{i}^{2}
```
Use for: exponents, sample indices, combined. Notice the braces — `x^2` works but `x^2i` is `x^2` then `i`. Always brace multi-character arguments.

### Greek letters (the ones ML actually uses)
```latex
\sigma   \mu   \alpha   \lambda   \theta   \pi   \epsilon
```
Use for: standard deviation (`\sigma`), mean (`\mu`), learning rate (`\alpha`), regularization strength (`\lambda`), model parameters (`\theta`).

### Spacing
```latex
f(x) = a + b, \quad \text{if } x > 0
```
`\quad` adds a visible space. `\text{...}` typesets as normal text inside math. Use these when prose would normally have a comma or "where".

### Vector notation
```latex
w \cdot x    \| w \|    w^{\top} x
```
`\cdot` is the dot product. `\|...\|` is a norm (the double bars for magnitude). `\top` is the transpose superscript.

### Conditional
```latex
\hat{y} = \begin{cases}
  1 & \text{if } p \ge 0.5 \\
  0 & \text{otherwise}
\end{cases}
```
Use for: piecewise definitions (decision rules, activation switches, ReLU).

### Auto-sized brackets
```latex
\left( \frac{a}{b} \right)
```
Use whenever a fraction is inside parentheses — without `\left(` the bracket stays small and looks wrong.

### Putting it together
```latex
J(\theta) = \frac{1}{n} \sum_{i=1}^{n} \left( y^{(i)} - \hat{y}^{(i)} \right)^{2}
```

## Three rules of thumb

- **Prose, then formula, then prose again.** Every formula gets a one-sentence plain-English neighbour. No naked equations.
- **Introduce variables before you use them.** Define `y`, `ŷ`, `n`, `w`, `b` in the sentence above the formula.
- **Read the equation left to right.** If your prose reads "the loss is the average of the squared differences", the formula should read the same way.

## Common mistakes

| Mistake | Fix |
|---|---|
| `x^2i` | `x^{2i}` (brace multi-char exponents) |
| `$\hat y$` (no braces) | `$\hat{y}$` (braces around the argument) |
| `f(x) = 1/1+e^-z` | `f(x) = \frac{1}{1 + e^{-z}}` |
| unbalanced `$$` | one `$$` to open, one to close, on their own lines |
| `sum_{i=1}^n` (inline) | use `\sum_{i=1}^{n}` (brace multi-char bounds) |

## Learn it in three steps (~2-3 hours, no install)

1. **https://katex.org/** → click "Try it!" (20 min). Same engine Obsidian uses. Type on the left, see on the right, click any symbol to see its code. Just learn where to look — don't memorise.
2. **https://en.wikibooks.org/wiki/LaTeX/Mathematics** → read "Inline math" through "Fractions and binomial coefficients" (15 min). Then **https://help.obsidian.md/Editing+and+formatting/Basic+formatting+syntax#Math** (5 min) to confirm the delimiters.
3. Write three formulas in your own vault (60-90 min):
   - The three loss equations in [Loss Functions](02-Machine%20Learning/01-Concepts/Loss%20Functions.md)
   - The bias-variance decomposition in [The Bias-Variance Tradeoff](02-Machine%20Learning/01-Concepts/The%20Bias-Variance%20Tradeoff.md)
   - The sigmoid in [Logistic Regression](02-Machine%20Learning/01-Concepts/Logistic%20Regression.md)

After those three, every other formula in the vault feels like typing.

## Related

- [Loss Functions](02-Machine%20Learning/01-Concepts/Loss%20Functions.md) — the three loss equations
- [The Bias-Variance Tradeoff](02-Machine%20Learning/01-Concepts/The%20Bias-Variance%20Tradeoff.md) — the most important single equation in the vault
- [Logistic Regression](02-Machine%20Learning/01-Concepts/Logistic%20Regression.md) — the sigmoid
- [Classification](02-Machine%20Learning/01-Concepts/Classification.md) — precision/recall/F1 ratios
- [Regression](02-Machine%20Learning/01-Concepts/Regression.md) — R², MAE, RMSE formulas