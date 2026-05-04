
## 1. Definition

**K-Nearest Neighbors (KNN)** is a **non-parametric, instance-based (lazy) learning algorithm** that makes predictions based on the similarity between a query point and stored training data.

- **Problem type**: Classification and Regression
- **Key idea**:
  - Store all training data
  - Predict by looking at the **$K$ closest points in feature space**
  - Use **majority voting (classification)** or **averaging (regression)**

---

## 2. Input and Output

### Input

- Dataset:
  $$
  X = [\mathbf{x}_1, \mathbf{x}_2, ..., \mathbf{x}_N] \in \mathbb{R}^{N \times d}
  $$

- Labels:
  - Classification:
    $$
    y_i \in \{1, ..., C\}
    $$
  - Regression:
    $$
    y_i \in \mathbb{R}
    $$

- Query point:
  $$
  \mathbf{x}_q \in \mathbb{R}^d
  $$

---

### Output

- Classification:
  $$
  \hat{y} \in \{1, ..., C\}
  $$

- Regression:
  $$
  \hat{y} \in \mathbb{R}
  $$

---

## 3. Mathematical Formulation

---

### 3.1 Distance Metric

Define a distance function:

$$
d(\mathbf{x}_i, \mathbf{x}_q)
$$

Common choices:

- **Euclidean distance**:
  $$
  d(\mathbf{x}_a, \mathbf{x}_b) = \|\mathbf{x}_a - \mathbf{x}_b\|_2
  $$

- **Manhattan distance**:
  $$
  \sum_{j=1}^{d} |x_a^{(j)} - x_b^{(j)}|
  $$

- **Minkowski distance**:
  $$
  \left( \sum_{j=1}^{d} |x_a^{(j)} - x_b^{(j)}|^p \right)^{1/p}
  $$

---

### 3.2 Neighbor Set

Define the set of $K$ nearest neighbors:

$$
\mathcal{N}_K(\mathbf{x}_q) = \{ \mathbf{x}_{(1)}, ..., \mathbf{x}_{(K)} \}
$$

where points are sorted by distance.

---

### 3.3 Prediction Rule

#### Classification

$$
\hat{y} = \arg\max_{c} \sum_{i \in \mathcal{N}_K} \mathbf{1}(y_i = c)
$$

---

#### Regression

$$
\hat{y} = \frac{1}{K} \sum_{i \in \mathcal{N}_K} y_i
$$

---

### 3.4 Distance-Weighted KNN

Assign weights:

$$
w_i = \frac{1}{d(\mathbf{x}_i, \mathbf{x}_q)^2}
$$

#### Weighted Classification

$$
\hat{y} = \arg\max_{c} \sum_{i \in \mathcal{N}_K} w_i \mathbf{1}(y_i = c)
$$

#### Weighted Regression

$$
\hat{y} = \frac{\sum_{i \in \mathcal{N}_K} w_i y_i}{\sum_{i \in \mathcal{N}_K} w_i}
$$

---

### 3.5 Optimization View

KNN implicitly solves:

$$
\hat{y} = \arg\min_{y} \sum_{i \in \mathcal{N}_K} \ell(y, y_i)
$$

- For regression: $\ell = (y - y_i)^2$
- For classification: $\ell = \mathbf{1}(y \neq y_i)$

---

### 3.6 No Gradient

- **No explicit training objective**
- No gradient-based optimization
- Decision is purely **data-driven at query time**

---

## 4. How It Works (Flow / Intuition)

### Algorithm

1. Choose $K$
2. For query $\mathbf{x}_q$:
   - Compute distance to all training points
   - Select $K$ nearest neighbors
3. Aggregate:
   - Classification → majority vote
   - Regression → average

---

### Intuition

- “Similar inputs → similar outputs”
- Model approximates function **locally**

---

### Important Note

- **No model is learned**
- Entire dataset = model

---

## 5. Training Process

### No Training Phase

- KNN is a **lazy learner**
- Stores dataset directly

---

### Computation Happens at Inference

- Complexity per query:
  $$
  O(Nd)
  $$

---

### Optimization

- Efficiency improved using:
  - **KD-tree**
  - **Ball-tree**

These structures **do not change the model**, only **accelerate nearest neighbor search**.

---

## 6. Assumptions

KNN assumes:

- Nearby points share similar labels
- Distance metric reflects similarity
- Feature space is meaningful

---

### When Assumptions Break

- Irrelevant features → misleading distance
- Different feature scales → distorted neighborhoods
- High-dimensional data → distances lose meaning

---

## 7. Curse of Dimensionality 

### Core Problem

As dimensionality $d$ increases:

- Data becomes **sparse**
- Distances become **less informative**

---

### Distance Concentration

Let:

$$
\text{ratio} = \frac{d_{\max} - d_{\min}}{d_{\min}}
$$

As $d \to \infty$:

$$
\text{ratio} \to 0
$$

**Meaning**:
- All distances become similar
- Nearest neighbor ≈ random point

---

### Volume Explosion

Volume of unit hypercube:

$$
V = 1^d = 1
$$

But to capture fraction $\alpha$ of data:

$$
r = \alpha^{1/d}
$$

As $d$ increases:

- $r \to 1$
- Need almost entire space to find neighbors

---

### Sparsity

- Number of samples needed grows exponentially:

$$
N \sim O(2^d)
$$

---

### Impact on KNN

- Nearest neighbors are **not truly “near”**
- Model performance degrades
- Distance metric becomes unreliable

---

### Mitigation

- Feature selection
- Dimensionality reduction (PCA)
- Better distance metrics
- Increase data size

---

## 8. When to Use / When NOT to Use

### Use when:

- Low-dimensional data
- Small to medium dataset
- Smooth decision boundaries
- Interpretability needed

---

### Avoid when:

- High-dimensional data
- Large datasets (slow inference)
- Sparse data
- Irrelevant/noisy features

---

## 9. Evaluation Metrics

### Classification

- Accuracy
- Precision / Recall / F1-score

---

### Regression

- MSE / RMSE
- MAE

---

### Model Selection

- Choose $K$ via cross-validation
- Balance:
  - Small $K$ → high variance
  - Large $K$ → high bias

---

## 10. Pros and Cons

### Pros

- Simple and intuitive
- No training cost
- Flexible (classification + regression)
- Can model complex boundaries

---

### Cons

- Slow inference
- High memory usage
- Sensitive to scaling
- Sensitive to curse of dimensionality
- Sensitive to noise

---

## 11. Practical Notes

### Key Hyperparameters

- $K$
- Distance metric
- Weighting scheme

---

### Important Tips

- Normalize features:
  $$
  x' = \frac{x - \mu}{\sigma}
  $$

- Choose odd $K$ for binary classification
- Use cross-validation

---

### Pitfalls

- Dominance of large-scale features
- Outliers affecting neighbors
- High-dimensional failure

---

### Final Insight

**KNN is a local approximation method**:

- It does not learn a global function
- It approximates behavior based on nearby data

Its power comes from simplicity, but its limitation comes from:

- **distance dependence**
- **curse of dimensionality**

Understanding these trade-offs is essential for effective use.

---
## Links
- Algorithms:
	- [[KD-Tree]]
	- [[Ball-Tree]]
- Evaluation metric:
	- [[Means Squared Error (MSE)]]