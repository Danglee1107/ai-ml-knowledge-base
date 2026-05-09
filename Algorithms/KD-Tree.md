
## 1. Definition

**KD-Tree (k-dimensional tree)** is a **space-partitioning data structure** used to organize points in a $d$-dimensional space for efficient nearest neighbor queries.

- **Problem type**: Nearest Neighbor Search (used in KNN)
- **Core idea**:
  - Recursively partition the space using axis-aligned splits
  - Organize data into a tree to **avoid brute-force distance computation**
  - Enable **logarithmic-time search (on average)**

---

## 2. Problem Formulation

### Input

- Dataset:
  $$
  X = \{\mathbf{x}_1, ..., \mathbf{x}_N\}, \quad \mathbf{x}_i \in \mathbb{R}^d
  $$

- Query point:
  $$
  \mathbf{x}_q \in \mathbb{R}^d
  $$

- Number of neighbors:
  $$
  K
  $$

---

### Output

- Set of nearest neighbors:
  $$
  \mathcal{N}_K(\mathbf{x}_q)
  $$

---

### Constraints / Assumptions

- Data lies in **Euclidean space**
- Distance metric is typically:
  $$
  \|\mathbf{x}_i - \mathbf{x}_q\|_2
  $$
- Works best for **low to moderate dimensions**

---

## 3. Core Idea

### Key Insight

Instead of comparing the query point with **all $N$ points**, KD-tree:

- **Partitions space into regions**
- Quickly eliminates large portions of data that cannot contain nearest neighbors

---

### Why It Works

- Uses **axis-aligned hyperplanes** to split data
- Each node represents a **subspace**
- During search:
  - Only explore regions that could contain closer points

---

### Compared to Brute Force

- Brute force:
  $$
  O(Nd)
  $$
- KD-tree:
  - Avoids unnecessary distance computations
  - Reduces search to:
    $$
    O(\log N) \text{ (average)}
    $$

---

## 4. Algorithm Process

---

### 4.1 Tree Construction

1. Choose splitting dimension:
   $$
   k = \text{depth} \mod d
   $$

2. Sort points along dimension $k$
3. Choose median as root

4. Recursively build:
   - Left subtree: points < median
   - Right subtree: points > median

---

### 4.2 Query Process (Nearest Neighbor)

1. Traverse tree:
   - Follow branch where query lies

2. Reach leaf node → initialize best candidate

3. Backtrack:
   - At each node:
     - Update best distance
     - Check if other branch could contain closer point

4. Prune if:

$$
|\mathbf{x}_q^{(k)} - \mathbf{x}_{node}^{(k)}| > \text{current best distance}
$$

---

## 5. Mathematical Perspective

---

### Splitting Rule

At depth $l$:

$$
k = l \mod d
$$

Split along dimension $k$:

$$
x^{(k)} = \text{median}
$$

---

### Distance Pruning Condition

Let:

- Current best distance:
  $$
  r = \|\mathbf{x}_q - \mathbf{x}_{best}\|
  $$

- Distance to splitting plane:
  $$
  \delta = |x_q^{(k)} - x_{node}^{(k)}|
  $$

If:

$$
\delta > r
$$

→ No need to explore the other subtree

---

## 6. Complexity Analysis 

---

### Construction

- Time:
  $$
  O(N \log N)
  $$

- Space:
  $$
  O(N)
  $$

---

### Query

#### Best Case

$$
O(\log N)
$$

- Balanced tree
- Effective pruning

---

#### Average Case

$$
O(\log N)
$$

---

#### Worst Case

$$
O(N)
$$

- High-dimensional data
- Poor splits
- Degenerate tree

---

### Why Complexity Degrades

- In high dimensions:
  - Pruning becomes ineffective
  - Most nodes must be visited

---

## 7. Properties

- **Deterministic**
- **Exact search** (not approximate)
- Sensitive to:
  - Data distribution
  - Dimensionality

---

### Dimensionality Sensitivity

- Performance degrades as:
  $$
  d \uparrow
  $$

- Due to [[Curse of Dimensionality]]

---

## 8. What It Actually Does 

---

### Exploits

- Spatial locality
- Geometric structure of data

---

### Works Well When

- Data is low-dimensional
- Data is well-distributed
- Clusters are separable

---

### Fails When

- High dimensions → all regions overlap
- Data is sparse → no meaningful partition

---

### Key Limitation

- Splits are **axis-aligned**
- Cannot adapt to arbitrary data geometry

---

## 9. Intuition 

---

### Geometric View

- KD-tree divides space into **rectangular regions**
- Each node = hyperplane split

---

### Search Intuition

- Move toward region containing query
- Keep track of best distance
- Only explore other regions if necessary

---

### Mental Model

- “Search locally, prune globally”

---

## 10. Pros & Cons

### Pros

- Faster than brute-force KNN (in low dimensions)
- Exact nearest neighbor search
- Simple to implement conceptually

---

### Cons

- Performance degrades in high dimensions
- Sensitive to data distribution
- Requires rebuilding if data changes
- Axis-aligned splits may be suboptimal

---

## 11. When to Use / When NOT to Use

### Use when

- $d$ is small (typically $d < 20$)
- Static dataset
- Frequent queries

---

### Avoid when

- High-dimensional data
- Streaming / dynamic data
- Non-Euclidean metrics

---

## 12. Variants / Improvements 

---

### 1. Balanced KD-Tree

- Ensures:
  - Tree height = $O(\log N)$
- Improves:
  - Query efficiency

---

### 2. Approximate KD-Tree

- Allows approximate neighbors
- Faster queries
- Trade-off: accuracy vs speed

---

### 3. Randomized KD-Tree

- Random split selection
- Avoids worst-case structures

---

### 4. Hybrid Structures

- Combine with:
  - Ball Tree
  - Hashing methods

---

## 13. Visualization

Best visualization:

- 2D space with:
  - Recursive vertical and horizontal splits
  - Tree structure mapped to partitions
- Show:
  - Query point
  - Search path
  - Pruned regions

![[kd_tree.gif|743]]

---

## 14. Notes / Pitfalls

---

### Common Issues

- Poor split selection → unbalanced tree
- High dimension → no pruning benefit

---

### Edge Cases

- All points aligned → degenerate tree
- Duplicate points → ambiguous splits

---

### Practical Tips

- Always normalize data
- Use KD-tree only for low-dimensional problems
- Benchmark against brute force

---

### Final Insight

**KD-tree accelerates KNN by turning a global search into a structured local search.**

However:

- Its efficiency depends heavily on **geometry of data**
- It is not universally better than brute force

Understanding when KD-tree fails is as important as knowing how it works.

---
## 15. Related concepts
- [[Ball-Tree]]
-  [[Curse of Dimensionality]]

### Animation 
- https://youtu.be/FnJj28u7rF0?si=V1w3etT5tZNK2B4p