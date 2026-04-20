
## 1. Definition
**Linear Regression** is a fundamental supervised learning model used to predict a continuous target variable based on one or more input features.

- **Problem type**: Regression
- **Goal**: Learn a mapping from input features to a real-valued output

**Key idea**:
- The model assumes a **linear relationship** between inputs and output.
- It fits a **hyperplane** that best approximates the data by minimizing prediction error.

---

## 2. Input and Output

### Input
- Feature vector:
  $$
  \mathbf{x} = [x_1, x_2, ..., x_d]^T \in \mathbb{R}^d
  $$
- Dataset:
  $$
  X \in \mathbb{R}^{N \times d}
  $$
  where:
  - $N$: number of samples
  - $d$: number of features

### Output
- Target values:
  $$
  \mathbf{y} \in \mathbb{R}^N
  $$

- Prediction:
  $$
  \hat{y} = f(\mathbf{x}) \in \mathbb{R}
  $$

---

## 3. Mathematical Formulation

### 3.1 Model Representation

We extend the input with a bias term:

$$
\bar{\mathbf{x}} = [1, x_1, x_2, ..., x_d]^T
$$

Parameter vector:

$$
\mathbf{w} = [w_0, w_1, ..., w_d]^T
$$

Prediction:

$$
\hat{y} = \bar{\mathbf{x}}^T \mathbf{w}
$$

For the full dataset:

$$
\hat{\mathbf{y}} = \bar{X} \mathbf{w}
$$

where:
- $\bar{X} \in \mathbb{R}^{N \times (d+1)}$

---

### 3.2 Objective Function (Loss)

We minimize the **Mean Squared Error (MSE)**:

$$
L(\mathbf{w}) = \frac{1}{2} \sum_{i=1}^{N} (y_i - \bar{\mathbf{x}}_i^T \mathbf{w})^2
$$

Matrix form:

$$
L(\mathbf{w}) = \frac{1}{2} \| \mathbf{y} - \bar{X} \mathbf{w} \|_2^2
$$

---

### 3.3 Why Squared Error?

- Ensures **non-negativity**
- Penalizes large errors more strongly
- Differentiable everywhere → enables gradient-based optimization

---

### 3.4 Gradient Derivation

Let:

$$
L(\mathbf{w}) = \frac{1}{2} (\mathbf{y} - \bar{X}\mathbf{w})^T (\mathbf{y} - \bar{X}\mathbf{w})
$$

Taking gradient:

$$
\nabla_{\mathbf{w}} L(\mathbf{w}) = \bar{X}^T (\bar{X}\mathbf{w} - \mathbf{y})
$$

---

### 3.5 Closed-form Solution (Normal Equation)

Set gradient to zero:

$$
\bar{X}^T \bar{X} \mathbf{w} = \bar{X}^T \mathbf{y}
$$

If invertible:

$$
\mathbf{w} = (\bar{X}^T \bar{X})^{-1} \bar{X}^T \mathbf{y}
$$

If not invertible (singular matrix):

$$
\mathbf{w} = (\bar{X}^T \bar{X})^{\dagger} \bar{X}^T \mathbf{y}
$$

where $^\dagger$ is the **pseudo-inverse**.

---

### 3.6 Intuition Behind the Math

- $\bar{X}\mathbf{w}$ → projection of data onto a linear subspace
- Minimizing $\| \mathbf{y} - \bar{X}\mathbf{w} \|^2$ → finding the **closest projection**
- The solution is essentially a **least-squares approximation**

---

## 4. How It Works (Flow / Intuition)

1. Collect dataset $(\mathbf{x}_i, y_i)$
2. Add bias term → $\bar{X}$
3. Initialize parameters $\mathbf{w}$
4. Compute predictions:
   $$
   \hat{\mathbf{y}} = \bar{X} \mathbf{w}
   $$
5. Measure error using loss function
6. Update $\mathbf{w}$ (optimization)
7. Repeat until convergence

**Intuition**:
- The model tries to find the best **fitting line/plane/hyperplane**
- It adjusts weights to reduce the gap between predictions and actual values

---

## 5. Training Process

### Method 1: Closed-form Solution
- Direct computation using:
  $$
  \mathbf{w} = (\bar{X}^T \bar{X})^{-1} \bar{X}^T \mathbf{y}
  $$
- Fast for small datasets
- Expensive for large $d$

---

### Method 2: Gradient Descent

Update rule:

$$
\mathbf{w} := \mathbf{w} - \eta \cdot \nabla_{\mathbf{w}} L(\mathbf{w})
$$

$$
\mathbf{w} := \mathbf{w} - \eta \cdot \bar{X}^T (\bar{X}\mathbf{w} - \mathbf{y})
$$

where:
- $\eta$ : learning rate

Variants:
- Batch Gradient Descent
- Stochastic Gradient Descent (SGD)
- Mini-batch Gradient Descent

---

## 6. Assumptions

Linear Regression assumes:

- **Linearity** : Relationship between $x$ and $y$ is linear
- **Independence** : Observations are independent
- **Homoscedasticity** : Constant variance of errors
- **No multicollinearity** : Features are not highly correlated
- **Normality (optional)** : Errors are normally distributed

### When assumptions break:
- Non-linear patterns → poor fit
- Strong feature correlation → unstable weights
- Heteroscedasticity → unreliable predictions

---

## 7. When to Use / When NOT to Use

### Use when:
- Relationship is approximately linear
- Interpretability is important
- Dataset is not extremely large or complex

### Avoid when:
- Data is highly non-linear
- Complex feature interactions exist
- High-dimensional sparse data (better with **regularization**)

---

## 8. Evaluation Metrics

Common metrics:

### Mean Squared Error (MSE)

$$
MSE = \frac{1}{N} \sum (y_i - \hat{y}_i)^2
$$

### Root Mean Squared Error (RMSE)
$$
\sqrt{\text{MSE}}
$$

### Mean Absolute Error (MAE)

$$
MAE = \frac{1}{N} \sum |y_i - \hat{y}_i|
$$

### R² Score

$$
R^2 = 1 - \frac{\sum (y - \hat{y})^2}{\sum (y - \bar{y})^2}
$$

**Why these metrics?**
- They directly measure prediction error for continuous values

---

## 9. Pros and Cons

### Pros
- Simple and interpretable
- Fast to train
- Works well with small datasets
- Analytical solution available

### Cons
- Cannot capture non-linear relationships
- Sensitive to outliers
- Assumes strong conditions about data
- Multicollinearity issues

---

## 10. Practical Notes

### Common Pitfalls
- **Overfitting** (especially with many features)
- **Underfitting** if data is non-linear
- **Feature scaling** affects gradient descent
- **Outliers** can distort results

---

### Tips

- Normalize or standardize features
- Use **Regularization**:
  - Ridge (L2)
  - Lasso (L1)
- Check correlation between features
- Visualize residuals to validate assumptions

---

### Implementation Insights

- Use closed-form only when $d$ is small
- Prefer gradient-based methods for large-scale data
- Libraries:
  - NumPy (manual implementation)
  - Scikit-learn (`LinearRegression`)

---

**Summary**:  
Linear Regression is the foundation of many machine learning models. Understanding its mathematical structure and assumptions is critical before moving to more complex (non-linear) models.

---
## Links
- [[Pseudo-Inverse]]
-  [[Batch Gradient Descent vs Stochastic Gradient Descent]]
-  [[Means Squared Error (MSE)]]
