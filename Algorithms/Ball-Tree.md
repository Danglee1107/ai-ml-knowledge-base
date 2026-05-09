
## 1. Definition

**Ball Tree** is a **hierarchical space-partitioning data structure** used to accelerate nearest neighbor search in metric spaces.

- **Problem type**:
  - K-Nearest Neighbors (KNN)
  - Radius search
  - Similarity search

- **Core idea**:
  - Organize data into nested hyperspheres (“balls”)
  - Each node stores a region of space defined by a center and radius
  - During search, entire regions can be discarded using distance bounds

Unlike KD-tree, Ball Tree partitions data based on **distance geometry** rather than axis-aligned splits.

---

## 2. Problem Formulation

### Input

Dataset:

$$
X = \{\mathbf{x}_1, \mathbf{x}_2, ..., \mathbf{x}_N\}, \quad \mathbf{x}_i \in \mathbb{R}^d
$$

Query point:

$$
\mathbf{x}_q \in \mathbb{R}^d
$$

Distance metric:

$$
d(\mathbf{x}_a, \mathbf{x}_b)
$$

Usually:

- Euclidean distance
- Minkowski distance
- Cosine distance
- Any valid metric space distance

---

### Output

Find:

$$
\mathcal{N}_K(\mathbf{x}_q)
$$

the set of the $K$ nearest neighbors of the query point.

---

### Assumptions

- Data lies in a metric space
- Triangle inequality holds:

$$
d(a,c) \le d(a,b) + d(b,c)
$$

This property is essential for pruning.

---

## 3. Core Idea

### Key Insight

Instead of partitioning space using hyperplanes like KD-tree, Ball Tree groups nearby points into **balls (hyperspheres)**.

Each node contains:

- A center:
  $$
  \mathbf{c}
  $$

- A radius:
  $$
  r
  $$

such that all points inside the node satisfy:

$$
d(\mathbf{x}, \mathbf{c}) \le r
$$

---

### Why This Works

Suppose we already found a candidate nearest neighbor at distance:

$$
d_{\text{best}}
$$

For another node:

- center:
  $$
  \mathbf{c}
  $$

- radius:
  $$
  r
  $$

If:

$$
d(\mathbf{x}_q, \mathbf{c}) - r > d_{\text{best}}
$$

then **every point inside that ball must be farther away** than the current best point.

Therefore:

- entire subtree can be skipped
- huge reduction in search cost

---

### Why Better Than Brute Force

Brute force KNN:

$$
O(Nd)
$$

Ball Tree:

- avoids checking all points
- prunes large spatial regions
- exploits geometric clustering

---

### Why Better Than KD-tree in Some Cases

KD-tree:

- uses axis-aligned splits
- performs poorly in moderate/high dimensions

Ball Tree:

- partitions using distance geometry
- adapts better to irregular distributions

---

## 4. Algorithm Process

### 4.1 Tree Construction

---

#### Step 1 — Create Root Ball

Compute:

- center:
  $$
  \mathbf{c}
  $$

- radius:
  $$
  r = \max_i d(\mathbf{x}_i, \mathbf{c})
  $$

This forms a ball containing all points.

---

#### Step 2 — Split Data

Choose two pivot points:

$$
\mathbf{p}_1, \mathbf{p}_2
$$

Common strategy:

- choose farthest pair approximately

Assign each point to nearest pivot:

$$
\mathbf{x}_i \to
\begin{cases}
\text{left child} & d(\mathbf{x}_i, \mathbf{p}_1)
<
d(\mathbf{x}_i, \mathbf{p}_2)
\\
\text{right child} & \text{otherwise}
\end{cases}
$$

---

#### Step 3 — Recursively Build

Repeat until:

- node size small enough
- or leaf threshold reached

---

### 4.2 Query Process

---

#### Step 1 — Start at Root

Maintain:

- current best neighbors
- current best distance

---

#### Step 2 — Traverse Closest Child First

Visit child whose center is closer to query.

---

#### Step 3 — Prune Using Distance Bounds

For node:

$$
(\mathbf{c}, r)
$$

Compute lower bound:

$$
d_{\min} = d(\mathbf{x}_q, \mathbf{c}) - r
$$

If:

$$
d_{\min} > d_{\text{best}}
$$

prune subtree.

---

#### Step 4 — Continue Recursively

Only explore nodes that may contain closer neighbors.

---

## 5. Mathematical Perspective

### Ball Definition

A ball in metric space:

$$
B(\mathbf{c}, r)
=
\{
\mathbf{x}
:
d(\mathbf{x}, \mathbf{c}) \le r
\}
$$

---

### Distance Lower Bound

For any point inside the ball:

$$
d(\mathbf{x}_q, \mathbf{x})
\ge
d(\mathbf{x}_q, \mathbf{c}) - r
$$

This follows from triangle inequality.

---

### Pruning Criterion

If:

$$
d(\mathbf{x}_q, \mathbf{c}) - r
>
d_{\text{best}}
$$

then:

$$
d(\mathbf{x}_q, \mathbf{x})
>
d_{\text{best}}
\quad \forall \mathbf{x} \in B
$$

Thus subtree is impossible to improve solution.

---

## 6. Complexity Analysis

### Construction Complexity

---

#### Time

Typical:

$$
O(N \log N)
$$

because recursive partitioning is performed.

---

#### Space

$$
O(N)
$$

for storing nodes and points.

---

### Query Complexity

---

#### Best Case

$$
O(\log N)
$$

when pruning is highly effective.

---

#### Average Case

Approximately:

$$
O(\log N)
$$

for low/moderate dimensions.

---

#### Worst Case

$$
O(N)
$$

when pruning fails.

---

### Why Complexity Degrades

In high dimensions:

- balls overlap heavily
- lower bounds become weak
- many branches cannot be pruned

This is another manifestation of the: [[Curse of Dimensionality]]

---

## 7. Properties

### Deterministic or Stochastic?

- Usually deterministic
- Depends on pivot selection strategy

---

### Exact or Approximate?

- Standard Ball Tree = exact search
- Approximate variants also exist

---

### Sensitivity to Input Size

- Scales better than brute force
- Still expensive for massive datasets

---

### Sensitivity to Dimensionality

Performance decreases as:

$$
d \uparrow
$$

but often better than KD-tree for moderate dimensions.

---

## 8. What It Actually Does (Behavior) 

### What Pattern Does It Exploit?

Ball Tree exploits:

- local clustering
- geometric locality
- metric structure

---

### Behavior on Clustered Data

Very effective because:

- nearby points grouped into compact balls
- pruning becomes strong

---

### Behavior on Uniform Data

Less effective because:

- balls overlap significantly
- weak pruning

---

### Behavior in High Dimensions

Distances become concentrated:

$$
d_{\max} \approx d_{\min}
$$

Consequences:

- all balls appear similarly close
- pruning deteriorates

---

### Practical Strength

Works well when:

- data has meaningful local structure
- dimension is moderate
- metric distance is informative

---

## 9. Intuition

### Geometric Intuition

Imagine:

- each node = bubble containing points
- query = point in space

If a bubble is too far away:

- ignore everything inside it

---

### Core Mental Model

Instead of asking:

> “Which points are close?”

Ball Tree asks:

> “Which regions could possibly contain close points?”

This changes search from:

- point-by-point comparison

to:

- region-based elimination

---

### Spatial Reasoning

KD-tree:
- cuts space into rectangles

Ball Tree:
- groups points into spheres

Ball Tree is often better because many real datasets are naturally clustered radially rather than axis-aligned.

---

## 10. Pros & Cons

### Pros

- Faster KNN search than brute force
- Better than KD-tree in moderate dimensions
- Supports arbitrary metric distances
- Adapts better to clustered data

---

### Cons

- Still suffers from curse of dimensionality
- More complex than KD-tree
- Construction can be expensive
- Overlapping balls reduce pruning efficiency

---

## 11. When to Use / When NOT to Use

### Use When

- Moderate-dimensional data
- Distance metric is meaningful
- Data has cluster structure
- Many repeated nearest-neighbor queries

---

### Avoid When

- Extremely high-dimensional data
- Distance concentration occurs
- Small datasets (brute force simpler)
- Streaming/dynamic datasets

---

### Real-World Examples

- Recommendation systems
- Image retrieval
- Embedding similarity search
- Biological sequence search

---

## 12. Variants / Improvements 

### 1. Approximate Ball Tree

#### Improvement
- Faster search

#### Trade-off
- Not always exact

#### Use When
- Real-time systems

---

### 2. Dual-Tree Search

Uses:
- Ball tree for dataset
- Ball tree for queries

Improves:
- batch query efficiency

---

### 3. Cover Tree

Improves:
- scalability in metric spaces

Trade-off:
- more complex implementation

---

### 4. VP-Tree (Vantage Point Tree)

Uses:
- radial partitioning around pivots

Better for:
- non-Euclidean metric spaces

---

## 13. Visualization

Best visualization:

- Show nested circles containing points
- Query point moving through space
- Highlight:
  - visited nodes
  - pruned balls
  - nearest neighbor updates

---

## 14. Related Concepts 

- [[K-Nearest Neighbors (KNN)]]
- [[KD-Tree]]
- [[Curse of Dimensionality]]
### Animation
- https://youtu.be/FnJj28u7rF0?si=V1w3etT5tZNK2B4p

---

## 15. Notes / Pitfalls

### Common Misunderstanding

Ball Tree does NOT remove curse of dimensionality.

It only delays performance degradation compared to KD-tree.

---

### Important Edge Case

If:

$$
d_{\max} \approx d_{\min}
$$

then pruning becomes almost useless.

---

### Practical Trap

Using Ball Tree on:
- sparse embeddings
- extremely high-dimensional vectors

may be slower than brute force.

---

### Implementation Tip

Leaf size matters:

- small leaf:
  - deeper tree
  - more traversal

- large leaf:
  - shallower tree
  - more brute-force checking

Need balance.

---

## Final Insight

**Ball Tree accelerates nearest-neighbor search by reasoning about regions instead of individual points.**

Its effectiveness depends on one crucial assumption:

> Nearby points form compact geometric structures.

When this assumption holds, Ball Tree can dramatically reduce computation.  
When dimensionality destroys geometric locality, its advantage disappears.