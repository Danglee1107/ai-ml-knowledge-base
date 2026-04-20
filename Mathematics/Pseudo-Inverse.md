
## 1. Definition

The **pseudo-inverse** (or **Moore–Penrose inverse**) of a matrix $A$, denoted $A^\dagger$, is a generalization of the matrix inverse that exists for **any matrix**, including non-square and singular matrices.

- It provides a way to **solve linear systems** when a true inverse does not exist.
- It yields the **best least-squares solution** to inconsistent or underdetermined systems.

**Core idea**:
- When $A^{-1}$ does not exist, $A^\dagger$ acts as a substitute that gives the **minimum-error** or **minimum-norm** solution using **Singular Value Decomposition (SVD)**.

---

## 2. Problem Setup

We consider a linear system:

$$
A \mathbf{x} = \mathbf{b}
$$

where:

- $A \in \mathbb{R}^{m \times n}$
- $\mathbf{x} \in \mathbb{R}^{n}$
- $\mathbf{b} \in \mathbb{R}^{m}$

### Cases

- **Square & full-rank ($m = n$)**:
  - Unique solution: $\mathbf{x} = A^{-1} \mathbf{b}$

- **Overdetermined ($m > n$)**:
  - No exact solution → solve:
    $$
    \min_{\mathbf{x}} \|A\mathbf{x} - \mathbf{b}\|_2^2
    $$

- **Underdetermined ($m < n$)**:
  - Infinite solutions → choose:
    $$
    \min_{\mathbf{x}} \|\mathbf{x}\|_2 \quad \text{s.t. } A\mathbf{x} = \mathbf{b}
    $$

### Goal

Find a **unified solution**:

$$
\mathbf{x} = A^\dagger \mathbf{b}
$$

---

## 3. Mathematical Derivation

### 3.1 Least Squares Formulation

We solve:

$$
\min_{\mathbf{x}} \|A\mathbf{x} - \mathbf{b}\|_2^2
$$

Expand:

$$
\|A\mathbf{x} - \mathbf{b}\|_2^2 = (A\mathbf{x} - \mathbf{b})^T (A\mathbf{x} - \mathbf{b})
$$

$$
= \mathbf{x}^T A^T A \mathbf{x} - 2\mathbf{b}^T A \mathbf{x} + \mathbf{b}^T \mathbf{b}
$$

---

### 3.2 Taking Gradient

Differentiate w.r.t. $\mathbf{x}$:

$$
\nabla_{\mathbf{x}} = 2A^T A \mathbf{x} - 2A^T \mathbf{b}
$$

Set to zero:

$$
A^T A \mathbf{x} = A^T \mathbf{b}
$$

This is the **normal equation**.

---

### 3.3 Problem with Normal Equation

- Requires $A^T A$ to be **invertible**
- If $A$ is **rank-deficient**, then:
  - $A^T A$ is singular
  - No unique solution

---

### 3.4 Singular Value Decomposition (SVD)

Any matrix $A \in \mathbb{R}^{m \times n}$ can be decomposed as:

$$
A = U \Sigma V^T
$$

where:

- $U \in \mathbb{R}^{m \times m}$: orthogonal matrix
- $V \in \mathbb{R}^{n \times n}$: orthogonal matrix
- $\Sigma \in \mathbb{R}^{m \times n}$: diagonal matrix

$$
\Sigma =
\begin{bmatrix}
\sigma_1 & & \\
& \ddots & \\
& & \sigma_r \\
& & & 0
\end{bmatrix}
$$

- $\sigma_i > 0$ are **singular values**
- $r = \text{rank}(A)$

---

### 3.5 Constructing the Pseudo-Inverse

Define:

$$
\Sigma^\dagger =
\begin{bmatrix}
\frac{1}{\sigma_1} & & \\
& \ddots & \\
& & \frac{1}{\sigma_r} \\
& & & 0
\end{bmatrix}^T
$$

Then:

$$
A^\dagger = V \Sigma^\dagger U^T
$$

---

### 3.6 Final Solution

The least-squares solution is:

$$
\mathbf{x}^* = A^\dagger \mathbf{b}
$$

---

### 3.7 Key Properties (Moore–Penrose Conditions)

The pseudo-inverse satisfies:

1. $A A^\dagger A = A$
2. $A^\dagger A A^\dagger = A^\dagger$
3. $(A A^\dagger)^T = A A^\dagger$
4. $(A^\dagger A)^T = A^\dagger A$

---

### 3.8 Special Cases

#### Full column rank ($m > n$):

$$
A^\dagger = (A^T A)^{-1} A^T
$$

#### Full row rank ($m < n$):

$$
A^\dagger = A^T (A A^T)^{-1}
$$

---

## 4. Intuition

### Geometric View

- $A$ maps $\mathbf{x}$ → a vector in $\mathbb{R}^m$
- When no exact solution exists:
  - We project $\mathbf{b}$ onto the **column space of $A$**

- The solution:
  $$
  \hat{\mathbf{b}} = A \mathbf{x}^*
  $$

is the **closest point** to $\mathbf{b}$ in that space.

---

### Role of SVD

- $U$: rotates output space
- $V$: rotates input space
- $\Sigma$: scales along principal directions

Pseudo-inverse:

- Inverts only **non-zero singular values**
- Ignores directions where information is lost

---

### Key Insight

- Small singular values → unstable directions
- Pseudo-inverse avoids exploding solutions by **not inverting zero values**

---

## 5. Application

### 5.1 Linear Regression

Closed-form solution:

$$
\mathbf{w} = X^\dagger \mathbf{y}
$$

- Works even when $X^T X$ is not invertible
- Handles multicollinearity

---

### 5.2 Underdetermined Systems

- Infinite solutions → pseudo-inverse finds:
  - **Minimum norm solution**

---

### 5.3 Dimensionality Reduction

- Closely related to:
  - **PCA**
  - **Low-rank approximation**

---

### 5.4 Neural Networks (Theory)

- Used in:
  - Linear layer analysis
  - Understanding overparameterized models

---

### 5.5 Numerical Stability

- SVD-based pseudo-inverse is more stable than:
  $$
  (A^T A)^{-1} A^T
  $$

---

## Summary

- The pseudo-inverse generalizes matrix inversion to all matrices.
- It provides:
  - Least-squares solution (overdetermined)
  - Minimum-norm solution (underdetermined)
- Computed via:
  $$
  A^\dagger = V \Sigma^\dagger U^T
  $$
- Deep connection to:
  - Geometry (projection)
  - Optimization (least squares)
  - Linear algebra (SVD)

Understanding pseudo-inverse is essential for mastering advanced machine learning and optimization techniques.