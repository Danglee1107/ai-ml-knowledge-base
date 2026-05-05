
## 1. Definition
The **Elbow Method** is a technique used to determine the optimal number of clusters (**K**) in K-Means clustering.

- It evaluates how well data points fit into clusters.
- It uses metrics like **WCSS (Within-Cluster Sum of Squares)**, **Distortion**, or **Inertia**.
- The optimal K is chosen at the **“elbow point”**, where improvement slows down.

![[elbow_method.webp]]

---

## 2. Mathematical Formulation

### 2.1 WCSS (Within-Cluster Sum of Squares)

$$
WCSS = \sum_{i=1}^{k} \sum_{j=1}^{n_i} \text{distance}(x_j^{(i)}, c_i)^2
$$

Where:
- $x_j^{(i)}$ :  j-th data point in cluster i.  
- $c_i$ :  centroid of cluster i.  
- $n_i$ : number of points in cluster i.  

-> Measures how compact each cluster is.

---

### 2.2 Distortion

$$
\text{Distortion} = \frac{1}{n} \sum_{i=1}^{n} \min_{c \in \text{clusters}} \|x_i - c\|^2
$$

- Average squared distance from each point to its nearest centroid.  
- Lower = better clustering. 

---

### 2.3 Inertia

$$
\text{Inertia} = \sum_{i=1}^{n} \text{distance}(x_i, c_j^*)^2
$$

- Total squared distance to nearest centroid.  
- Same idea as WCSS (used in sklearn).

---

## 3. How It Works (Flow)

### Step-by-step
1. Choose a range of K (e.g., 1 → 10).
2. For each K:
   - Run K-Means.
   - Compute WCSS / Distortion / Inertia.
1. Plot:
   - X-axis: K.
   - Y-axis: error (WCSS, distortion, or inertia).
1. Find the **elbow point**.

---

### Key Insight
- Before elbow:
	  - Error decreases **rapidly**.
	  - Adding clusters significantly improves model.

- After elbow:
	  - Error decreases **slowly**.
	  - Extra clusters give little benefit → possible **overfitting**.

![[elbowmethod_5.png]]

 -> Optimal K = point where improvement starts diminishing.

---

## 4. Implementation

### Step 1: Import Libraries

```python
from sklearn.cluster import KMeans
from scipy.spatial.distance import cdist
import numpy as np
import matplotlib.pyplot as plt
```

### Step 2: Create Data

```python
x1 = np.array([3, 1, 1, 2, 1, 6, 6, 6, 5, 6,
               7, 8, 9, 8, 9, 9, 8, 4, 4, 5, 4])
x2 = np.array([5, 4, 5, 6, 5, 8, 6, 7, 6, 7,
               1, 2, 1, 2, 3, 2, 3, 9, 10, 9, 10])

X = np.array(list(zip(x1, x2))).reshape(len(x1), 2)

plt.scatter(x1, x2)
plt.xlim([0, 10])
plt.ylim([0, 10])
plt.title('Dataset Visualization')
plt.show()
```

![[elbowmethod_1.png|729]]

### Step 3: Compute Distortion & Inertia

```python
distortions = []
inertias = []
K = range(1, 10)

for k in K:
    kmeanModel = KMeans(n_clusters=k, random_state=42).fit(X)

    distortions.append(
        sum(np.min(cdist(X, kmeanModel.cluster_centers_, 'euclidean'), axis=1)**2) / X.shape[0]
    )

    inertias.append(kmeanModel.inertia_)
```

### Step 4: Plot Elbow Curve

#### Distortion
```python
plt.plot(K, distortions, 'bx-')
plt.xlabel('K')
plt.ylabel('Distortion')
plt.title('Elbow Method (Distortion)')
plt.show()
```

![[elbowmethod_2.png|743]]

#### Inertia

```python
plt.plot(K, inertias, 'bx-')
plt.xlabel('K')
plt.ylabel('Inertia')
plt.title('Elbow Method (Inertia)')
plt.show()
```

![[elbowmethod_3.png|743]]

### Step 5: Visualize Clusters

```python
k_range = range(1, 5)

for k in k_range:
    kmeans = KMeans(n_clusters=k, init='k-means++', random_state=42)
    y_kmeans = kmeans.fit_predict(X)

    plt.scatter(X[:, 0], X[:, 1], c=y_kmeans, cmap='viridis', s=100)
    plt.scatter(kmeans.cluster_centers_[:, 0],
                kmeans.cluster_centers_[:, 1],
                s=300, c='red', label='Centroids')

    plt.title(f'K-means (k={k})')
    plt.legend()
    plt.show()
```

![[elbowmethod_4.png]]

## Sources
- https://www.geeksforgeeks.org/machine-learning/elbow-method-for-optimal-value-of-k-in-kmeans
