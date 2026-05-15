
## 1. Definition

**Mean Squared Error (MSE)** is a loss function used to measure the average squared difference between predicted values and true values.

- **Problem type**: Regression
- **Goal**: Quantify how close predictions are to actual continuous targets

**Core idea**:
- Penalize prediction errors by squaring them.
- Larger errors are penalized **quadratically more**, making the model sensitive to large deviations.

---

## 2. Objective

We are given:

- True targets:
  $$
  y_i \in \mathbb{R}, \quad i = 1, ..., N
  $$

- Model predictions:
  $$
  \hat{y}_i \in \mathbb{R}
  $$

- Dataset:
  $$
  \{(x_i, y_i)\}_{i=1}^N
  $$

### Objective

Minimize the discrepancy between prediction and ground truth:

$$
\text{Error} = y_i - \hat{y}_i
$$

MSE measures:

- **Squared error**
- Averaged over all samples

---

## 3. Mathematical Formulation

### 3.1 Loss Function

$$
L = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2
$$

Alternative form (with factor for convenience):

$$
L = \frac{1}{2N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2
$$

---

### 3.2 Component Definitions

- $N$ : number of samples
- $y_i$ : true value
- $\hat{y}_i$ : predicted value
- $(y_i - \hat{y}_i)^2$ : squared residual

---

### 3.3 Gradient Derivation (IMPORTANT)

We compute:

$$
\frac{\partial L}{\partial \hat{y}_i}
$$

Start with:

$$
L = \frac{1}{2N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2
$$

Differentiate w.r.t. $\hat{y}_i$:

$$
\frac{\partial L}{\partial \hat{y}_i}
= \frac{1}{2N} \cdot 2 ( \hat{y}_i - y_i )
$$

$$
= \frac{1}{N} (\hat{y}_i - y_i)
$$

---

### 3.4 Vector Form

Let:

$$
\mathbf{y}, \hat{\mathbf{y}} \in \mathbb{R}^N
$$

Loss:

$$
L = \frac{1}{2} \| \mathbf{y} - \hat{\mathbf{y}} \|_2^2
$$

Gradient:

$$
\nabla_{\hat{\mathbf{y}}} L = \hat{\mathbf{y}} - \mathbf{y}
$$

---

### 3.5 Chain Rule (Model Parameters)

If $\hat{y}_i = f(x_i; \theta)$:

$$
\frac{\partial L}{\partial \theta}
= \sum_{i=1}^{N} \frac{\partial L}{\partial \hat{y}_i} \cdot \frac{\partial \hat{y}_i}{\partial \theta}
$$

---

## 4. Optimization Perspective

### Differentiability

- MSE is **smooth and differentiable everywhere**

---

### Convexity

- MSE is **convex in $\hat{y}$**
- For linear models: convex in parameters → **global minimum exists**

---

### Gradient Behavior

$$
\frac{\partial L}{\partial \hat{y}_i} = \frac{1}{N} (\hat{y}_i - y_i)
$$

- Large error → large gradient
- Small error → small gradient

**Implication**:
- Strong correction for large mistakes
- Fine-tuning for small errors

---

### Optimization Difficulty

- Easy to optimize with Gradient Descent
- No local minima (for convex models)

---

### Potential Issues

- Large gradients for large errors → **gradient explosion risk**
- Sensitive to noisy labels

---

## 5. Properties

### Convexity
- Convex function → stable optimization

### Differentiability
- Fully differentiable → ideal for gradient-based methods

### Sensitivity to Outliers
- **Highly sensitive** (due to squaring)

### Scale Dependence
- Loss grows with scale of $y$

### Smoothness
- Smooth quadratic function → stable gradients

---

## 6. What It Measures (Model Behavior)

- Penalizes **large errors heavily**
- Encourages model to:
  - Fit **average behavior**
  - Avoid large deviations

### Bias Introduced

- Model tends toward **mean of target distribution**

---

## 7. Intuition

### Why Squared Error?

- Squaring:
  - Removes sign
  - Amplifies large errors

### Geometric View

- Minimizing MSE = minimizing Euclidean distance:

$$
\| \mathbf{y} - \hat{\mathbf{y}} \|_2^2
$$

- Equivalent to projecting predictions close to true values

---

### Probabilistic Interpretation

- MSE corresponds to **maximum likelihood estimation** under:

$$
y_i = \hat{y}_i + \epsilon, \quad \epsilon \sim \mathcal{N}(0, \sigma^2)
$$

---

## 8. Interpretation

- Large MSE:
  - Predictions far from true values

- Small MSE:
  - Predictions close to targets

### Human Interpretability

- Not directly interpretable (due to squaring)
- Often use RMSE for interpretability

---

## 9. Practical Usage

### When to Use

- Regression problems
- Gaussian noise assumption
- Continuous targets

---

### When NOT to Use

- Presence of strong outliers
- Heavy-tailed noise distributions

---

### Activation Function

- Typically:
  - **Linear output layer**

---

### Comparison

- MSE vs MAE:
  - MSE: sensitive to outliers
  - MAE: robust but non-smooth

---

## 10. Variants / Extensions

### Root Mean Squared Error (RMSE)

$$
\sqrt{\text{MSE}}
$$

- Same units as target

---

### Weighted MSE

$$
\sum w_i (y_i - \hat{y}_i)^2
$$

- Emphasize certain samples

---

### Huber Loss

- Combines MSE + MAE
- Robust to outliers

---

## 11. Notes / Pitfalls

### Common Mistakes

- Not normalizing targets → unstable gradients
- Using MSE for classification tasks

---

### Practical Tips

- Normalize data
- Monitor gradient scale
- Consider robust alternatives if noise is high

---

### Edge Cases

- Extremely large errors dominate training
- Outliers can shift model significantly

---

## Final Insight

**MSE is not just a loss — it defines the geometry of learning.**  
It shapes optimization by:

- Emphasizing large errors
- Encouraging smooth convergence
- Biasing toward mean predictions

Understanding MSE deeply is essential for mastering regression and optimization behavior in machine learning.