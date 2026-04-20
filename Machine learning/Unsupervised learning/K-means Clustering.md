
## 1. Definition

**K-Means Clustering** is an unsupervised learning algorithm that partitions data into $K$ disjoint clusters by minimizing the distance between data points and their assigned cluster centers.

- **Problem type**: Clustering (unsupervised learning)
- **Goal**: Group similar data points together

**Key idea**:
- Each cluster is represented by a **center (centroid)**.
- Each data point is assigned to the **nearest center**, and centers are updated as the **mean of assigned points**.

---

## 2. Input and Output

### Input
- Dataset:
  $$
  X = [\mathbf{x}_1, \mathbf{x}_2, ..., \mathbf{x}_N] \in \mathbb{R}^{N \times d}
  $$
  where:
  - $N$: number of data points
  - $d$: number of features

- Number of clusters:
  $$
  K < N
  $$

---

### Output

- Cluster centers:
  $$
  M = [\mathbf{m}_1, \mathbf{m}_2, ..., \mathbf{m}_K] \in \mathbb{R}^{K \times d}
  $$

- Assignment matrix (one-hot labels):
  $$
  Y = [\mathbf{y}_1; \mathbf{y}_2; ...; \mathbf{y}_N] \in \mathbb{R}^{N \times K}
  $$

where each:
$$
\mathbf{y}_i = [y_{i1}, ..., y_{iK}]
$$

satisfies:
$$
y_{ij} \in \{0,1\}, \quad \sum_{j=1}^{K} y_{ij} = 1
$$

---

## 3. Mathematical Formulation

### 3.1 Objective Function

We aim to minimize the total **within-cluster variance**:

$$
L(Y, M) = \sum_{i=1}^{N} \sum_{j=1}^{K} y_{ij} \| \mathbf{x}_i - \mathbf{m}_j \|_2^2
$$

---

### 3.2 Intuition of the Loss

- $\|\mathbf{x}_i - \mathbf{m}_j\|^2$:
  - Measures how far a point is from a center

- $y_{ij}$:
  - Acts as a **selector** (only one cluster is active)

So:
- Each point contributes **distance to its assigned cluster only**
- Total loss = **sum of squared distances inside clusters**

---

### 3.3 Optimization Problem

$$
\min_{Y, M} \quad \sum_{i=1}^{N} \sum_{j=1}^{K} y_{ij} \| \mathbf{x}_i - \mathbf{m}_j \|_2^2
$$

subject to:

$$
y_{ij} \in \{0,1\}, \quad \sum_{j=1}^{K} y_{ij} = 1
$$

---

### 3.4 Why This Problem is Hard

- $Y$ is **discrete** → combinatorial optimization
- Joint optimization over $(Y, M)$ is **non-convex**

---

### 3.5 Alternating Optimization Strategy

We solve by **coordinate descent**:

---

#### Step 1: Fix $M$, optimize $Y$

For each data point:

$$
\mathbf{y}_i = \arg\min_{\mathbf{y}_i} \sum_{j=1}^{K} y_{ij} \| \mathbf{x}_i - \mathbf{m}_j \|^2
$$

Because of one-hot constraint:

$$
j^* = \arg\min_j \| \mathbf{x}_i - \mathbf{m}_j \|^2
$$

So:
- Assign $\mathbf{x}_i$ to **nearest center**

---

#### Step 2: Fix $Y$, optimize $M$

We solve:

$$
\mathbf{m}_j = \arg\min_{\mathbf{m}_j} \sum_{i=1}^{N} y_{ij} \| \mathbf{x}_i - \mathbf{m}_j \|^2
$$

Expand:

$$
\frac{\partial}{\partial \mathbf{m}_j} = 2 \sum_{i=1}^{N} y_{ij} (\mathbf{m}_j - \mathbf{x}_i)
$$

Set to zero:

$$
\sum_{i=1}^{N} y_{ij} \mathbf{m}_j = \sum_{i=1}^{N} y_{ij} \mathbf{x}_i
$$

$$
\mathbf{m}_j = \frac{\sum_{i=1}^{N} y_{ij} \mathbf{x}_i}{\sum_{i=1}^{N} y_{ij}}
$$

---

### 3.6 Key Result

**Cluster center = mean of assigned points**

This is why it is called **K-means**.

---

### 3.7 Connection to Geometry

- The algorithm partitions space into regions:
  - Each region contains points closer to one center than others
- These regions form a **Voronoi diagram**

![[kmeansclustering.png| 500]]

---

## 4. How It Works (Flow / Intuition)

### Algorithm Steps

1. Initialize $K$ centers randomly ( or optimize with **Elbow Method** and **K-means++**)
2. Repeat until convergence:
   - **Assignment step**:
     - Assign each point to nearest center
   - **Update step**:
     - Recompute each center as mean of its cluster
3. Stop when assignments do not change

---

### Intuition

- Assignment step:
  - “Which center am I closest to?”

- Update step:
  - “Where is the center of my assigned points?”

- Iteratively:
  - Centers move toward dense regions
  - Clusters become tighter

---

## 5. Training Process

### Optimization Method

- **Alternating Minimization (Coordinate Descent)**

---

### No Gradient Descent Needed

- Each step has a **closed-form solution**
- Guarantees:
  - Loss decreases at every iteration
  - Converges in finite steps

---

### Convergence Property

- Loss is non-increasing
- Finite number of cluster assignments → must converge

---

## 6. Assumptions

K-Means implicitly assumes:

- Clusters are **spherical**
- Clusters have **similar variance**
- Distance metric is **Euclidean**
- Data is **numeric and continuous**

---

### When Assumptions Break

- Non-spherical clusters → poor performance
- Different densities → bias toward large clusters
- Outliers → distort centers

---

## 7. When to Use / When NOT to Use

### Use when:

- Clusters are well-separated
- Data is low to moderate dimensional
- You need fast, simple clustering

---

### Avoid when:

- Data is highly non-linear
- Clusters have complex shapes
- Data contains many outliers
- Categorical data (without encoding)

---

## 8. Evaluation Metrics

### Inertia (Within-cluster sum of squares)

$$
\sum_{i=1}^{N} \|\mathbf{x}_i - \mathbf{m}_{c(i)}\|^2
$$

---

### Silhouette Score

Measures:
- Cohesion vs separation

---

### Elbow Method

- Plot loss vs $K$
- Choose point where decrease slows

---

## 9. Pros and Cons

### Pros

- Simple and fast
- Scales well to large datasets
- Easy to implement
- Interpretable

---

### Cons

- Requires $K$ in advance
- Sensitive to initialization
- Sensitive to outliers
- Only finds local minima
- Cannot capture complex shapes

---

## 10. Practical Notes

### Common Pitfalls

- Poor initialization → bad local minima
- Empty clusters
- Feature scaling issues

---

### Tips

- Use **K-Means++ initialization**
- Normalize features
- Run multiple times and choose best result

---

### Implementation Insights

- Complexity:
  $$
  O(N K d \cdot \text{iterations})
  $$

- Libraries:
  - NumPy (manual)
  - Scikit-learn (`KMeans`)

---

### Final Insight

K-Means is essentially solving:

- A **hard assignment clustering problem**
- By iteratively:
  - Assigning points → minimizing distance
  - Updating centers → computing means

It is a powerful baseline and a foundation for understanding more advanced clustering methods.

---
## Links
- [[Elbow Method]]
-  [[K-means++]]
