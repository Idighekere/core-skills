---
title: CS229 Lecture 1 - Linear Regression & Gradient Descent
tags:
  - machine-learning
  - cs229
  - linear-regression
  - gradient-descent
  - normal-equation
status: complete
---

# CS229 Lecture 1: Linear Regression & Gradient Descent



## Linear Regression Setup

To perform supervised learning, we decide how to represent our hypotheses $h$.

In Linear Regression:
$$h(x) = \theta_0 + \theta_1 x$$

For multiple input features:
$$h(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2$$

Where $\theta \longrightarrow$ parameters / weights.

$$h(x) = \sum_{j=0}^{n} \theta_j x_j \quad \text{where } x_0 = 1$$

$$\theta = \begin{bmatrix} \theta_0 \\ \theta_1 \\ \theta_2 \end{bmatrix}, \quad x = \begin{bmatrix} x_0 \\ x_1 \\ x_2 \end{bmatrix}$$

$$\text{Training set} \longrightarrow \text{Learning algo} \longrightarrow h \text{ (hypothesis / engine)}$$

$$\text{Input Size } x \longrightarrow h \longrightarrow \text{estimated price } (\hat{y})$$

### Notation
* $m =$ no. of training examples (no. of rows)
* $x =$ "inputs" / features
* $y =$ "outputs" / target
* $(x, y) =$ a training example
* $(x^{(i)}, y^{(i)}) = i\text{-th}$ training example
* $n =$ no. of features

The work of a learning algo is to choose parameters ($\theta$) that will make the best prediction.

### Cost Function

Choose $\theta$ such that $h(x) \approx y$ for training examples.

For Linear Regression, we try to minimize the square difference between our prediction for a data pt with the actual data:

$$\text{minimize } \theta: \quad J(\theta) = \frac{1}{2} \sum_{i=1}^{m} (h(x^{(i)}) - y^{(i)})^2 \longrightarrow \text{Cost fn}$$

*Note: $\frac{1}{2}$ is added just to simplify the expression. It's just a convention. So we minimize $J(\theta)$.*

---

## Working Through an Example (Dry Run)

Assuming a student is to predict his exam scores based on no. of hours studied:

| No. of hours ($x$) | Score ($y$) |
| :--- | :--- |
| $x^{(1)} = 1$ | $y^{(1)} = 20$ |
| $x^{(2)} = 2$ | $y^{(2)} = 40$ |
| $x^{(3)} = 3$ | $y^{(3)} = 60$ |

Here, $m = 3$, $n = 1$, $j = 1$.

Let's ignore intercept term & use only 1 weight:
$$h_\theta(x) = \theta_0 x_0$$

So we put the training data into our learning algo. Then it came with an hypothesis:
$$\theta = 5 \longrightarrow \text{randomly picked (initialized)}$$
$$h_\theta(x) = 5x$$

Meaning if you studied for $1\text{ hr} \longrightarrow$ the model predicts:
$$h_\theta(1) = 5(1) = 5\text{ pt}$$

Our model's predictions:
* $x^{(1)} = 1, \quad \hat{y}^{(1)} = h_\theta(1) = 5(1) = 5$
* $x^{(2)} = 2, \quad \hat{y}^{(2)} = h_\theta(2) = 5(2) = 10$
* $x^{(3)} = 3, \quad \hat{y}^{(3)} = h_\theta(3) = 5(3) = 15$

### Model's Prediction vs Actual

| $\hat{y}^{(i)}$ | $y^{(i)}$ | $\hat{y}^{(i)} - y^{(i)}$ | Notes |
| :--- | :--- | :--- | :--- |
| $5$ | $20$ | $-15$ | |
| $10$ | $40$ | $-30$ | Undershoot by |
| $15$ | $60$ | $-45$ | |

Calculating our cost fn error:
$$J(\theta) = \frac{1}{2} \sum_{i=1}^{m} (\hat{y}^{(i)} - y^{(i)})^2$$

$$= \frac{1}{2} \left[ (5 - 20)^2 + (10 - 40)^2 + (15 - 60)^2 \right]$$

$$= \frac{1}{2} \left[ 225 + 900 + 2025 \right]$$

$$J(\theta) = 1575 \quad (\text{we need to reduce})$$

---

## Gradient Descent

To minimize $J(\theta)$, we need to update/change our weight accordingly by using Gradient Descent (GD):

1. Start with some value of $\theta$ (say $\theta = \vec{0}$) $\longleftarrow$ vector of all zeros.
2. Keep changing $\theta$ to reduce $J(\theta)$.

```
         ^ J(theta)
         |     X  <-- start point
      \  |    / \
       \ |   /   \
________\|__/_____\_____ theta_0, theta_1
```

Assuming you at the point marked X at the top of a hill, you want to get to the lowest elevation as possible. All you will do is look around you and moves towards what's the steepest. This is what gradient descent does.

When GD is runned on LR, there will be no local optimal.

### Gradient Descent Update Rule

Each step of GD:
$$\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)$$

*(where $\alpha$ is learning rate, $:=$ is assignment operator)*

We do this for each value of $j$.

Deriving the partial derivative for 1 training example:
$$\frac{\partial J(\theta)}{\partial \theta_j} = \frac{\partial}{\partial \theta_j} \frac{1}{2} (h(x) - y)^2$$

$$= 2 \cdot \frac{1}{2} (h(x) - y) \cdot \frac{\partial}{\partial \theta_j} (h(x) - y)$$

$$= (h(x) - y) \cdot \frac{\partial}{\partial \theta_j} (\theta_0 x_0 + \theta_1 x_1 + \dots + \theta_n x_n)$$

where $h(x) = \theta_0 x_0 + \theta_1 x_1 + \dots + \theta_n x_n$.

The partial der. of the terms wrt $j$ for the terms will be $0$ with exception for the term with $j$.

So if $j=1$, the terms will be $\dots + \theta_1 x_1$, because that's the only term that is dependent of $j$. So we have $\theta_1 x_1$, so every other term will have $\frac{\partial}{\partial \theta_j} = 0$.

$$= (h(x) - y) \cdot x_j$$

Putting it all together for $m$ examples:
$$\theta_j := \theta_j - \alpha \sum_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)}) x_j^{(i)}$$

$$\theta_j := \theta_j - \alpha \sum_{i=1}^{m} (\hat{y}^{(i)} - y^{(i)}) x_j^{(i)}$$

For our 1 feature example:
$$\theta := \theta - \alpha \sum_{i=1}^{m} (\hat{y}^{(i)} - y^{(i)}) x^{(i)}$$

---

### Dry Run: Step 1 Update

Assuming we pick $\alpha = 0.1$:

$$\theta_{\text{new}} = \theta_{\text{old}} - 0.1 \left[ (-15)(1) + (-30)(2) + (-45)(3) \right]$$

$$\theta_{\text{new}} = 5 - 0.1 \left[ -15 - 60 - 135 \right]$$

$$\theta_{\text{new}} = 5 - 0.1 (-210)$$

$$\theta_{\text{new}} = 5 + 21 = 26$$

We use it again to check:
$$h_\theta(x) = 26x$$

| $x^{(i)}$ | $\hat{y}^{(i)}$ |
| :--- | :--- |
| $1$ | $26(1) = 26$ |
| $2$ | $26(2) = 52$ |
| $3$ | $26(3) = 78$ |

From this we can see that our learning rate was too large & this made our weight update to change so big pass the actual data, but it's much closer.

We can try to update our weight again, but if we do so using the same $\alpha$, we are gonna go down fast past the actual point (global minima) & we will get close to the actual prediction.

$$J(\theta) = 252$$

So we have minimized it, but it's definitely not the global minima.

### Dry Run: Step 2 Update

So we can do the update again using same learning rate $\alpha = 0.1$:
$$\theta_{\text{old}} = 26$$

$$\theta_{\text{new}} = 26 - 0.1 \left[ (6)(1) + (12)(2) + (18)(3) \right]$$

$$= 26 - 0.1 \left[ 6 + 24 + 36 \right]$$

$$= 26 - 6.6 = 19.4$$

$$h_\theta(x) = 19.4x$$

| $x^{(i)}$ | $\hat{y}^{(i)}$ | $y^{(i)}$ |
| :--- | :--- | :--- |
| $1$ | $(19.4)(1) = 19.4$ | $20$ |
| $2$ | $(19.4)(2) = 38.8$ | $40$ |
| $3$ | $(19.4)(3) = 58.2$ | $60$ |

With this we are pretty much closer to the actual, so this value of $\theta$ (parameter) is good for our model.

$$J(\theta) = 2.52$$

So our $J(\theta)$ started:
$$1575 \longrightarrow 252 \longrightarrow 2.52$$

The drop changed so fast because of how relatively large our learning rate is.

If we try to do one more update we will end up with:
$$\theta_{\text{new}} = 20.24, \quad \text{where } \alpha = 0.1$$

---

## Batch Gradient Descent vs. Stochastic Gradient Descent

Since we are taking the squared difference for $J(\theta)$ in Linear Regression, we always get a quad function. So the GD curve is always a U-shaped curve having only 1 local minima & which only local minima is also the global minima.

For $\alpha$, we take a smaller value so that the descending/movement will take smaller steps & not go past the global minima.

If you notice $J(\theta)$ increasing instd of decreasing, it means that $\alpha$ is too big.

GD algo calculates the derivative $\frac{\partial J(\theta)}{\partial \theta_j}$ by summing up entire dataset ($m$), that's why it's aka **Batch Gradient Descent**.

BGD is disadvantaged at the fact that for every step of GD, you have to go through the entire dataset.

A better approach to BGD is **Stochastic GD**:

$$\text{Repeat } \{$$
$$\quad \text{for } i = 1 \text{ to } m \ \{$$
$$\quad\quad \theta_j := \theta_j - \alpha (h_\theta(x^{(i)}) - y^{(i)}) x_j^{(i)} \quad \text{(update this for every } j \text{)}$$
$$\quad \}$$
$$\}$$

SGD does this randomly & it will not converge but will be going around the global minima.

Decreasing $\alpha$ will help SGD oscillate close to the global minima.

We stop stepping when we notice that $J(\theta)$ is no longer going down.

---

## Normal Equation

For Linear Regression, it's possible to jump to the global optima without going through the iterative GD method. The method is called **Normal Eqn**.

$$\nabla_\theta J(\theta) = \begin{bmatrix} \frac{\partial J}{\partial \theta_0} \\ \frac{\partial J}{\partial \theta_1} \\ \frac{\partial J}{\partial \theta_2} \end{bmatrix} \longleftarrow \text{3D vector for } n=2 \text{ we get } 3 \ (n+1) \text{ no. of } \theta$$

$$\theta \in \mathbb{R}^{n+1} \longrightarrow \text{derivative of } J(\theta) \text{ wrt } \theta$$

### Matrix Derivative Concept

Generally assuming $A \in \mathbb{R}^{2 \times 2} \longrightarrow A = \begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{bmatrix}$

e.g. $f(A) = (A_{11} + A_{12})^2$

e.g. $f\left(\begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}\right) = 5 + 6 = 11^2$

$$\nabla_A f(A) = \begin{bmatrix} \frac{\partial f}{\partial A_{11}} & \frac{\partial f}{\partial A_{12}} \\ \frac{\partial f}{\partial A_{21}} & \frac{\partial f}{\partial A_{22}} \end{bmatrix} \longrightarrow \nabla_A f(A) = \begin{bmatrix} 1 & 2A_{12} \\ 0 & 0 \end{bmatrix}$$

The max / min of a fn is when the derivative is zero (0).

$$\nabla_\theta J(\theta) = \text{set } \vec{0}$$

### Trace Properties

If $A$ is a square matrix i.e. $A = m \times m$:
$$\operatorname{tr} A = \text{sum of diagonal entries} = \sum A_{ii} \quad \text{("tr}(A)\text{" or "trace of } A\text{")}$$

Properties of trace of a matrix:
* $\operatorname{tr} A = \operatorname{tr} A^T$
* If $f(A) = \operatorname{tr} AB$, then $\nabla_A f(A) = B^T$
* $\operatorname{tr} AB = \operatorname{tr} BA$
* $\operatorname{tr} ABC = \operatorname{tr} CAB$
* $\nabla_A \operatorname{tr} A A^T C = CA + C^T A \equiv \frac{d}{da} a C a = 2aC$

---

## Matrix Vector Notation & Derivation

$$J(\theta) = \frac{1}{n} \sum_{i=1}^{n} (h(x^{(i)}) - y^{(i)})^2$$

$$X = \begin{bmatrix} — & (x^{(1)})^T & — \\ — & (x^{(2)})^T & — \\ & \vdots & \\ — & (x^{(n)})^T & — \end{bmatrix}$$

$X$ is a column vector so we stack up the training examples in rows.

Assuming $x^{(1)} = \begin{bmatrix} 2104 \\ 3 \end{bmatrix}$ a $2 \times 1$ col vector:
$$(x^{(1)})^T = \begin{bmatrix} 2104 & 3 \end{bmatrix} \text{ a } 1 \times 2 \text{ row vector}$$

$$X \theta = \begin{bmatrix} — & (x^{(1)})^T & — \\ — & (x^{(2)})^T & — \\ & \vdots & \\ — & (x^{(n)})^T & — \end{bmatrix} \begin{bmatrix} \theta_0 \\ \theta_1 \\ \theta_2 \\ \vdots \\ \theta_n \end{bmatrix} = \begin{bmatrix} (x^{(1)})^T \theta \\ (x^{(2)})^T \theta \\ \vdots \\ (x^{(n)})^T \theta \end{bmatrix} = \begin{bmatrix} h_\theta(x^{(1)}) \\ \vdots \\ h_\theta(x^{(n)}) \end{bmatrix}$$

$$\vec{y} = \begin{bmatrix} y^{(1)} \\ y^{(2)} \\ \vdots \\ y^{(n)} \end{bmatrix}$$

So:
$$J(\theta) = \frac{1}{2} (X\theta - y)^T (X\theta - y)$$

$$X\theta - \vec{y} = \begin{bmatrix} h_\theta(x^{(1)}) - y^{(1)} \\ \vdots \\ h_\theta(x^{(n)}) - y^{(n)} \end{bmatrix}$$

### Proof: $Z^T Z = \sum Z_i^2$

$$Z = \begin{bmatrix} z_1 \\ z_2 \\ \vdots \\ z_n \end{bmatrix}, \quad Z^T = \begin{bmatrix} z_1 & z_2 & \dots & z_n \end{bmatrix}$$

$$Z^T Z = \begin{bmatrix} z_1 & z_2 & \dots & z_n \end{bmatrix} \begin{bmatrix} z_1 \\ z_2 \\ \vdots \\ z_n \end{bmatrix}$$

$$Z^T Z = (z_1 \cdot z_1) + (z_2 \cdot z_2) + \dots + (z_n \cdot z_n)$$

Which evaluates to a sum of squares:
$$Z^T Z = z_1^2 + z_2^2 + \dots + z_n^2$$

$$Z^T Z = \sum_{i=1}^{n} z_i^2$$

### Deriving Normal Equation

$$\nabla_\theta J(\theta) = \nabla_\theta \frac{1}{2} (X\theta - y)^T (X\theta - y)$$

$$= \frac{1}{2} \nabla_\theta (X^T \theta^T - y^T) (X\theta - y)$$

$$\text{Correct order: } \quad \frac{1}{2} \nabla_\theta \left[ \theta^T X^T X \theta - \theta^T X^T y - y^T X \theta + y^T y \right]$$

*(similar to $(ax - b)(ax - b) = a^2 x^2 - axb - bxa + b^2$)*

Combining middle term:
For any scalar value, its transpose = itself (e.g. $3^T = 3$).

In lin algebra, a vector transpose $\times$ vector = scalar.

So $y^T \times X\theta = \text{scalar (a single real number)}$.

For $(y^T X \theta)^T = \theta^T X^T y$:
$$-\theta^T X^T y - y^T X \theta = -2 y^T (X\theta) = -2 y (X\theta)^T$$

$$\nabla_\theta J(\theta) = \frac{1}{2} \nabla_\theta \left[ X^T \theta^T X \theta - 2 y (X\theta)^T + y^T y \right]$$

Differentiating wrt $\theta$:
$$\frac{1}{2} \left[ 2 X^T X \theta - 2 X^T y \right] = 0$$

$$X^T X \theta - X^T y = 0$$

$$X^T X \theta = X^T y \longleftarrow \text{Normal eq.}$$

$$\theta = (X^T X)^{-1} X^T y$$

When $X^T X$ is non-invertible (Singular), it means that inverse $(X^T X)^{-1}$ can't be calculated.