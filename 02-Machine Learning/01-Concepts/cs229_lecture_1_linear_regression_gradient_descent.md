---
id: cs229_lecture_1_linear_regression_gradient_descent
aliases: []
tags:
  - machine-learning
  - cs229
  - linear-regression
  - gradient-descent
  - normal-equation
  - notes
last_tested: "2026-10-30"
status: complete
title: CS229 Lecture 1 - Linear Regression & Gradient Descent
---

# CS229 Lecture 1: Linear Regression, Gradient Descent & The Normal Equation

> **Source:** Stanford CS229 (Andrew Ng)  
> **Goal:** Complete lecture notes from first principles—covering hypothesis formulation, least mean squares cost function, Batch & Stochastic Gradient Descent, and the complete matrix calculus derivation of the Normal Equation.

---

## 1. High-Level Workflow of Supervised Learning

In supervised learning, we take a labeled training dataset and feed it into a learning algorithm to output a hypothesis function $h(x)$ that estimates target values for unseen inputs.

```
+------------------+
|   Training Set   |
+--------+---------+
         |
         v
+------------------+
| Learning Algol   |
+--------+---------+
         |
         v
    [  Input x  ]  ----->  ( Hypothesis h )  ----->  [ Estimated Target y ]
```

### Key Notation Summary
* $m$: Total number of training examples (number of rows in the dataset).
* $n$ (or $d$): Total number of input features (number of columns, excluding the target).
* $x$: Input variables / features vector.
* $y$: Target output / ground truth label.
* $(x, y)$: A single training example.
* $(x^{(i)}, y^{(i)})$: The $i$-th training example (where $(i)$ is an index, not an exponent).
* $x_j^{(i)}$: The value of feature $j$ in the $i$-th training example (row $i$, column $j$).
* $\theta$: Model parameters / weights vector.

---

## 2. Model Representation & Hypothesis

### 2.1 Single Feature Model (Simplified)
Ignoring the intercept term for a single input feature $x$:
$$h_\theta(x) = \theta_0 x_0 \quad (\text{or simply } h_\theta(x) = \theta x)$$

### 2.2 Multiple Feature Model
For $n$ features ($x_1, x_2, \dots, x_n$):
$$h_\theta(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \dots + \theta_n x_n = \sum_{j=0}^{n} \theta_j x_j$$

Where by convention, **$x_0 = 1$** is the intercept term (bias feature).

### 2.3 Vector & Matrix Notation
Expressing parameters and features as column vectors:
$$\theta = \begin{bmatrix} \theta_0 \\ \theta_1 \\ \vdots \\ \theta_n \end{bmatrix} \in \mathbb{R}^{n+1}, \quad x = \begin{bmatrix} x_0 \\ x_1 \\ \vdots \\ x_n \end{bmatrix} \in \mathbb{R}^{n+1}$$

The hypothesis function can be compactly written as a dot product:
$$h_\theta(x) = \theta^T x$$

---

## 3. Cost Function (Least Mean Squares Error)

To choose optimal parameters $\theta$, we want our predictions $h_\theta(x)$ to be close to $y$. We define the **Ordinary Least Squares (OLS / LMS) Cost Function** $J(\theta)$ as:

$$J(\theta) = \frac{1}{2} \sum_{i=1}^{m} \left( h_\theta(x^{(i)}) - y^{(i)} \right)^2$$

> **Why the $\frac{1}{2}$ factor?**  
> The $\frac{1}{2}$ is included purely for mathematical convenience. When taking the derivative of the squared error term $(h_\theta(x) - y)^2$, the exponent $2$ comes down and cancels with $\frac{1}{2}$, simplifying the resulting update formula without changing the location of the minimum.

---

## 4. Optimization Method 1: Gradient Descent

Gradient descent is an iterative optimization algorithm. It starts with an initial guess for $\theta$ (e.g., $\theta = \vec{0}$) and repeatedly updates $\theta$ in the direction of steepest gradient descent to minimize $J(\theta)$.

### 4.1 General Update Rule
$$\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)$$

Where $\alpha$ is the **learning rate** (step size).

### 4.2 Partial Derivative Derivation (Single Example)
For a single example $(x, y)$, differentiating $J(\theta)$:
$$\frac{\partial}{\partial \theta_j} J(\theta) = \frac{\partial}{\partial \theta_j} \frac{1}{2} \left( h_\theta(x) - y \right)^2$$

Applying the chain rule:
$$\frac{\partial}{\partial \theta_j} J(\theta) = 2 \cdot \frac{1}{2} \left( h_\theta(x) - y \right) \cdot \frac{\partial}{\partial \theta_j} \left( h_\theta(x) - y \right)$$
$$\frac{\partial}{\partial \theta_j} J(\theta) = \left( h_\theta(x) - y \right) \cdot \frac{\partial}{\partial \theta_j} \left( \sum_{k=0}^{n} \theta_k x_k - y \right)$$

Since all terms in the summation with respect to $\theta_j$ drop out except for $k = j$:
$$\frac{\partial}{\partial \theta_j} J(\theta) = \left( h_\theta(x) - y \right) x_j$$

---

### 4.3 Batch Gradient Descent (BGD)
Summing over all $m$ training examples gives the full gradient update rule:

$$\text{Repeat until convergence \{} \quad \theta_j := \theta_j - \alpha \sum_{i=1}^{m} \left( h_\theta(x^{(i)}) - y^{(i)} \right) x_j^{(i)} \quad (\text{for every } j) \quad \}$$

#### Characteristics of Batch GD:
1. **Scans Entire Dataset:** To make a **single** weight update step, it computes errors and sums gradients across all $m$ rows.
2. **Computational Drawback:** Extremely slow and memory-intensive when $m$ is very large (e.g., millions of rows).
3. **Convexity Guarantee:** Because $J(\theta)$ for Linear Regression is a convex quadratic function (U-shaped hyper-bowl), Batch GD is guaranteed to converge smoothly to the single **global minimum** without getting trapped in local minima (provided $\alpha$ is not too large).

---

### 4.4 Stochastic Gradient Descent (SGD)
Instead of waiting to process all $m$ rows, Stochastic Gradient Descent updates parameters after scanning **a single row**:

$$\text{Repeat until convergence \{} \quad \text{for } i = 1 \text{ to } m \text{ \{} \quad \theta_j := \theta_j - \alpha \left( h_\theta(x^{(i)}) - y^{(i)} \right) x_j^{(i)} \quad (\text{for every } j) \quad \} \quad \}$$

#### Characteristics of SGD:
1. **Immediate Progress:** Starts making parameter adjustments instantly without waiting to scan the whole dataset.
2. **Path Trajectory:** Does not walk directly downhill. Instead, it oscillates randomly around the global minimum.
3. **Learning Rate Decay:** To prevent continuous wild oscillations around the minimum, $\alpha$ is typically decayed gradually over time so the updates shrink as it approaches the minimum.

---

### 4.5 Learning Rate ($\alpha$) Dynamics

* **$\alpha$ too large:** Over-shoots the minimum, causing the cost function $J(\theta)$ to increase/diverge or oscillate wildly.
* **$\alpha$ too small:** Takes tiny steps and requires an excessive number of iterations to converge.
* **Debugging Check:** If $J(\theta)$ increases instead of strictly decreasing at each iteration of Batch GD, it means $\alpha$ is set too high.

---

## 5. Explicit Numerical Dry Run (Hand Calculations)

### Problem Setup
* Dataset ($m = 3$ examples, single feature $x$, ignoring intercept):
  * $x^{(1)} = 1, \quad y^{(1)} = 20$
  * $x^{(2)} = 2, \quad y^{(2)} = 40$
  * $x^{(3)} = 3, \quad y^{(3)} = 60$
* Model: $h_\theta(x) = \theta x$
* Initial Guess: $\theta = 5$
* Learning Rate: $\alpha = 0.1$

---

### Iteration 0: Initial State
Predictions $\hat{y}^{(i)} = 5 x^{(i)}$:
$$\hat{y}^{(1)} = 5(1) = 5, \quad \hat{y}^{(2)} = 5(2) = 10, \quad \hat{y}^{(3)} = 5(3) = 15$$

Residuals $(\hat{y}^{(i)} - y^{(i)})$:
$$\text{Error}_1 = 5 - 20 = -15, \quad \text{Error}_2 = 10 - 40 = -30, \quad \text{Error}_3 = 15 - 60 = -45$$

Initial Cost $J(\theta)$:
$$J(5) = \frac{1}{2} \left[ (-15)^2 + (-30)^2 + (-45)^2 \right] = \frac{1}{2} [225 + 900 + 2025] = \mathbf{1575}$$

---

### Iteration 1 ($\theta_{\text{old}} = 5, \alpha = 0.1$)
Calculate update:
$$\theta_{\text{new}} = \theta_{\text{old}} - \alpha \sum_{i=1}^{3} \left( h_\theta(x^{(i)}) - y^{(i)} \right) x^{(i)}$$
$$\theta_{\text{new}} = 5 - 0.1 \left[ (-15)(1) + (-30)(2) + (-45)(3) \right]$$
$$\theta_{\text{new}} = 5 - 0.1 \left[ -15 - 60 - 135 \right] = 5 - 0.1(-210) = 5 + 21 = \mathbf{26}$$

Check Progress with $\theta = 26$:
* Predictions: $\hat{y} = [26, 52, 78]$
* Residuals: $\hat{y} - y = [+6, +12, +18]$
* Cost: $J(26) = \frac{1}{2} [6^2 + 12^2 + 18^2] = \frac{1}{2}[36 + 144 + 324] = \mathbf{252}$

*(Note: $\alpha = 0.1$ stepped past the target $\theta^* = 20$, overshooting to 26, but the total cost still dropped significantly from 1575 to 252).*

---

### Iteration 2 ($\theta_{\text{old}} = 26, \alpha = 0.1$)
Calculate update:
$$\theta_{\text{new}} = 26 - 0.1 \left[ (6)(1) + (12)(2) + (18)(3) \right]$$
$$\theta_{\text{new}} = 26 - 0.1 \left[ 6 + 24 + 54 \right] = 26 - 0.1(84) = 26 - 8.4 = \mathbf{17.6}$$

Check Progress with $\theta = 17.6$:
* Predictions: $\hat{y} = [17.6, 35.2, 52.8]$
* Residuals: $\hat{y} - y = [-2.4, -4.8, -7.2]$
* Cost: $J(17.6) = \frac{1}{2} \left[ (-2.4)^2 + (-4.8)^2 + (-7.2)^2 \right] = \frac{1}{2} [5.76 + 23.04 + 51.84] = \mathbf{40.32}$

---

### Iteration 3 ($\theta_{\text{old}} = 17.6, \alpha = 0.1$)
Calculate update:
$$\theta_{\text{new}} = 17.6 - 0.1 \left[ (-2.4)(1) + (-4.8)(2) + (-7.2)(3) \right]$$
$$\theta_{\text{new}} = 17.6 - 0.1 [-2.4 - 9.6 - 21.6] = 17.6 - 0.1(-33.6) = 17.6 + 3.36 = \mathbf{20.96}$$

Cost at $\theta = 20.96$:
$$J(20.96) = \frac{1}{2} \left[ (0.96)^2 + (1.92)^2 + (2.88)^2 \right] = \mathbf{6.45}$$

---

### Iteration Trajectory Summary

| Epoch | Parameter $\theta$ | Cost $J(\theta)$ | Behavior |
| :--- | :--- | :--- | :--- |
| **0 (Start)** | $5.00$ | $1575.00$ | Severe undershoot |
| **1** | $26.00$ | $252.00$ | Overshot optimal $\theta^* = 20$ |
| **2** | $17.60$ | $40.32$ | Corrected direction back downward |
| **3** | $20.96$ | $6.45$ | Converging closely onto global minimum ($\theta = 20$) |

---

## 6. Optimization Method 2: The Normal Equation

For Linear Regression, instead of using iterative Gradient Descent, we can compute the analytical solution directly in closed form by setting $\nabla_\theta J(\theta) = \vec{0}$.

### 6.1 Matrix Notation Setup

Construct the **Design Matrix** $X$ of size $(m \times (n+1))$ by stacking training feature vectors as transposed rows:

$$X = \begin{bmatrix} — & (x^{(1)})^T & — \\ — & (x^{(2)})^T & — \\ & \vdots & \\ — & (x^{(m)})^T & — \end{bmatrix}, \quad \vec{y} = \begin{bmatrix} y^{(1)} \\ y^{(2)} \\ \vdots \\ y^{(m)} \end{bmatrix}$$

Computing $X\theta$:
$$X\theta = \begin{bmatrix} (x^{(1)})^T \theta \\ (x^{(2)})^T \theta \\ \vdots \\ (x^{(m)})^T \theta \end{bmatrix} = \begin{bmatrix} h_\theta(x^{(1)}) \\ h_\theta(x^{(2)}) \\ \vdots \\ h_\theta(x^{(m)}) \end{bmatrix}$$

Thus, the residual error vector across all training examples is:
$$X\theta - \vec{y} = \begin{bmatrix} h_\theta(x^{(1)}) - y^{(1)} \\ \vdots \\ h_\theta(x^{(m)}) - y^{(m)} \end{bmatrix}$$

---

### 6.2 Expressing Cost Function $J(\theta)$ in Matrix Vector Form

Using the vector identity $z^T z = \sum_{i} z_i^2$:

$$J(\theta) = \frac{1}{2} (X\theta - \vec{y})^T (X\theta - \vec{y})$$

Expanding the matrix multiplication:
$$J(\theta) = \frac{1}{2} \left( (X\theta)^T - \vec{y}^T \right) (X\theta - \vec{y})$$
$$J(\theta) = \frac{1}{2} \left( \theta^T X^T - \vec{y}^T \right) (X\theta - \vec{y})$$
$$J(\theta) = \frac{1}{2} \left( \theta^T X^T X \theta - \theta^T X^T \vec{y} - \vec{y}^T X \theta + \vec{y}^T \vec{y} \right)$$

#### Important Scalar Transpose Identity
Since $\theta^T X^T \vec{y}$ evaluates to a real-valued scalar $(1 \times 1)$, it equals its own transpose:
$$(\theta^T X^T \vec{y})^T = \vec{y}^T (X\theta)^{TT} = \vec{y}^T X \theta$$

Combining the middle matching terms:
$$J(\theta) = \frac{1}{2} \left( \theta^T X^T X \theta - 2\theta^T X^T \vec{y} + \vec{y}^T \vec{y} \right)$$

---

### 6.3 Matrix Calculus & Derivation Rules

To minimize $J(\theta)$, we take its gradient with respect to $\theta$ and set it to zero vector $\vec{0}$:

#### Matrix Derivative Identities:
1. $\nabla_\theta (A \theta) = A^T$
2. $\nabla_\theta (\theta^T A) = A$
3. $\nabla_\theta (\theta^T A \theta) = (A + A^T)\theta$
   * If $A$ is symmetric (like $X^T X$), then $\nabla_\theta (\theta^T A \theta) = 2A\theta = 2 X^T X \theta$.

---

### 6.4 Solving for $\theta$

Applying derivatives to $J(\theta)$:
$$\nabla_\theta J(\theta) = \nabla_\theta \left( \frac{1}{2} \theta^T X^T X \theta - \theta^T X^T \vec{y} + \frac{1}{2} \vec{y}^T \vec{y} \right)$$
$$\nabla_\theta J(\theta) = \frac{1}{2} \left( 2 X^T X \theta \right) - X^T \vec{y} + 0$$
$$\nabla_\theta J(\theta) = X^T X \theta - X^T \vec{y}$$

Set the gradient to $\vec{0}$:
$$X^T X \theta - X^T \vec{y} = \vec{0}$$
$$X^T X \theta = X^T \vec{y} \quad \text{(Normal Equation)}$$

Multiplying both sides by the inverse matrix $(X^T X)^{-1}$:
$$\mathbf{\theta = (X^T X)^{-1} X^T \vec{y}}$$

---

### 6.5 Invertibility Issues & Non-Invertible $(X^T X)$

If $X^T X$ is singular (non-invertible / matrix determinant is zero), the standard inverse $(X^T X)^{-1}$ cannot be computed.

#### Common Causes of Non-Invertibility:
1. **Redundant Features (Multicollinearity):** Two or more features are linearly dependent (e.g., house size in sq ft and house size in sq meters).
2. **Too Many Features ($n > m$):** Number of features exceeds total training examples.

#### Solutions:
* Remove redundant linearly dependent features.
* Use regularization (L1 / L2 Ridge Regression).
* Compute the pseudo-inverse (`np.linalg.pinv` in Python) instead of standard matrix inverse.

---

## 7. Comparison: Gradient Descent vs. Normal Equation

| Metric / Aspect | Gradient Descent | Normal Equation |
| :--- | :--- | :--- |
| **Learning Rate ($\alpha$)** | Requires choosing $\alpha$ manually | No learning rate needed |
| **Iterations** | Requires multiple iterative epochs | Closed-form solution (1 step) |
| **Computational Complexity** | $\mathcal{O}(k n^2)$ per epoch | $\mathcal{O}(n^3)$ due to matrix inversion $(X^T X)^{-1}$ |
| **Dataset Scalability** | Scales well to massive $m$ ($m > 1,000,000$) | Very slow when $n$ is large ($n > 10,000$) |
| **Feature Scaling** | **Required** (otherwise overflows/crashes) | Not required |
