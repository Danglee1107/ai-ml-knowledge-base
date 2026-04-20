
## 1. Definition
**K-Means++** is an improved version of the K-Means clustering algorithm that focuses on **better initialization of centroids**.

- Standard K-Means initializes centroids randomly → may lead to:
	  - Poor clustering.
	  - Empty clusters.
	  - Overlapping clusters.

- K-Means++ improves this by selecting **well-separated initial centroids**.

Result:
- Better cluster quality.  
- Faster convergence. 
- More stable and consistent results.

---

## 2. Key Idea

Instead of choosing all centroids randomly:

- First centroid → chosen randomly.  
- Remaining centroids → chosen **far away from existing ones**.  

->  Points farther from current centroids have **higher probability** of being selected.

---

## 3. Algorithm (Initialization Process)

### Step-by-step

1. **Choose first centroid** : Randomly select one data point
2. **Choose next centroids**
	-  Calculate the distance from each data point to its nearest existing centroid.
	- Choose the next center with probability proportional to the square of this distance.
	- Points farther from existing centers have a higher chance of being selected.

3. **Repeat until K centroids are chosen**
4. **Run standard K-Means**
   - Assign points.
   - Update centroids.
   - Iterate until convergence.

---

## 4. Mathematical Foundation

### Selection Probability

$$
P(x) = \frac{D(x)^2}{\sum_{x'} D(x')^2}
$$

Where:
- $D(x)$: distance from point \( $x$ \) to nearest centroid.  
- Points farther away → higher probability.  

👉 This is called **D²-weighting**.

---

## 5. Why It Works

- Spreads centroids across the data space.  
- Avoids bad initialization cases.  
- Reduces risk of local minima.  
- Improves clustering performance.  

---

## 6. Complexity Note

- Initialization is **more expensive** than random K-Means.  
- But:
  - Converges **faster**.
  - Needs **fewer iterations**.
  
-> Overall performance is often better.

---

## 7. Implementation (Step-by-step)

### 7.1 Dataset Creation
Four separate **Gaussian** clusters are generated with different **means** and **covariances** to simulate different groupings in the data.

```python
import numpy as np
import matplotlib.pyplot as plt

mean_01 = np.array([0.0, 0.0])
cov_01 = np.array([[1, 0.3], [0.3, 1]])
dist_01 = np.random.multivariate_normal(mean_01, cov_01, 100)

mean_02 = np.array([6.0, 7.0])
cov_02 = np.array([[1.5, 0.3], [0.3, 1]])
dist_02 = np.random.multivariate_normal(mean_02, cov_02, 100)

mean_03 = np.array([7.0, -5.0])
dist_03 = np.random.multivariate_normal(mean_03, cov_01, 100)

mean_04 = np.array([2.0, -7.0])
cov_04 = np.array([[1.2, 0.5], [0.5, 1.3]])
dist_04 = np.random.multivariate_normal(mean_04, cov_01, 100)

data = np.vstack((dist_01, dist_02, dist_03, dist_04))
np.random.shuffle(data)
```

### 7.2 Visualization Function
This function is used to visualize the data points and the selected centroids at each step. All **data points** are shown in **gray**.

- Previously **selected centroids** are marked in **black**.
- The **current centroid** being added is marked in **red**.
- This helps visualize the centroid initialization process step by step.

```python
def plot(data, centroids):
    plt.scatter(data[:, 0], data[:, 1], marker='.', color='gray', label='Data Points')
    
    if centroids.shape[0] > 1:
        plt.scatter(centroids[:-1, 0], centroids[:-1, 1],
                    color='black', label='Selected Centroids')
    
    plt.scatter(centroids[-1, 0], centroids[-1, 1],
                color='red', label='Next Centroid')
    
    plt.title(f'Select {centroids.shape[0]}th Centroid')
    plt.legend()
    plt.show()
```

### 7.3 Distance Function
This is a standard formula to compute the distance between two vectors **`p1`** and **`p2`** in 2D space.

```python
def distance(p1, p2):
    return np.sqrt(np.sum((p1 - p2)**2))
```

### 7.4 K-Means++ Initialization
This function selects initial centroids using the **K-Means++** strategy. The first centroid is chosen randomly from the dataset. For the next centroids:

- It calculates the distance of every point to its **nearest** existing centroid.
- Chooses the point **farthest** from the nearest centroid as the next centroid and ensures centroids are spaced far apart initially, giving better cluster separation.

```python
def initialize(data, k):
    centroids = []

    # Step 1: random first centroid
    centroids.append(data[np.random.randint(data.shape[0])])
    plot(data, np.array(centroids))

    # Step 2: select remaining centroids
    for _ in range(k - 1):
        distances = []

        for point in data:
            min_dist = min([distance(point, c) for c in centroids])
            distances.append(min_dist)

        distances = np.array(distances)
        probabilities = distances**2 / np.sum(distances**2)

        next_centroid = data[np.random.choice(len(data), p=probabilities)]
        centroids.append(next_centroid)

        plot(data, np.array(centroids))

    return np.array(centroids)


# Run
centroids = initialize(data, k=4)
```

### [output]

![[kmeans++_1.png]]

It shows the dataset with the first randomly selected centroid (in **red**). No black points are visible since only one centroid is selected.

![[kmeans++_2.png]]

The second centroid is selected which is the **farthest** point from the first centroid. The **first centroid becomes black** and the new centroid is marked in **red**

![[kmeans++_3.png]]

The third centroid is selected. The two previously selected centroids are shown in black while the newly selected centroid is in red.

![[kmeans++_4.png]]

The final centroid is selected completing the initialization. Three previously selected centroids are in black and the last selected centroid is in red.

## 8. Applications

- **Image Segmentation**
    - Group pixels by color or texture.
- **Customer Segmentation**
    - Group users by behavior or demographics.
- **Recommender Systems**
    - Suggest items based on user similarity.

## Sources
- https://www.geeksforgeeks.org/machine-learning/elbow-method-for-optimal-value-of-k-in-kmeans