
## 1. Definition

The **Perceptron Learning Algorithm (PLA)** is one of the earliest and simplest supervised machine learning algorithms for **binary classification**.

It learns a **linear decision boundary** that separates two classes using a weighted sum of input features.

The key idea is:

- Compute a linear score from the input features
- Apply a threshold function
- Adjust the weights whenever the model makes a mistake

Mathematically, the Perceptron attempts to find a hyperplane:

$$
\mathbf{w}^\top \mathbf{x} + b = 0
$$

that separates the classes.

---

## 2. Input and Output

### Input

The model takes:

- Feature vector:

$$
\mathbf{x} \in \mathbb{R}^d
$$

where:

- $d$ = number of features
- $\mathbf{x} = [x_1, x_2, ..., x_d]^\top$

Training dataset:

$$
\{(\mathbf{x}^{(i)}, y^{(i)})\}_{i=1}^{N}
$$

where:

- $N$ = number of samples
- $y^{(i)} \in \{-1, +1\}$

---

### Output

The Perceptron outputs a predicted class:

$$
\hat{y} \in \{-1, +1\}
$$

using:

$$
\hat{y} = \text{sign}(\mathbf{w}^\top \mathbf{x} + b)
$$

where:

- $\mathbf{w}$ = weight vector
- $b$ = bias term

---

## 3. Mathematical Formulation

### Linear Decision Function

The Perceptron computes:

$$
f(\mathbf{x}) = \mathbf{w}^\top \mathbf{x} + b
$$

Expanded form:

$$
f(\mathbf{x}) = w_1x_1 + w_2x_2 + \cdots + w_dx_d + b
$$

Prediction:

$$
\hat{y} =
\begin{cases}
+1 & \text{if } f(\mathbf{x}) \ge 0 \\
-1 & \text{otherwise}
\end{cases}
$$

---

### Geometric Interpretation

The equation:

$$
\mathbf{w}^\top \mathbf{x} + b = 0
$$

defines a **hyperplane**.

- In 2D → a line
- In 3D → a plane
- Higher dimensions → hyperplane

The vector $\mathbf{w}$ is:

- **Perpendicular (normal)** to the decision boundary
- Responsible for the orientation of the boundary

The bias $b$ shifts the boundary.

---

### Why the Sign Determines the Class

Suppose:

$$
\mathbf{w}^\top \mathbf{x} + b > 0
$$

Then $\mathbf{x}$ lies on one side of the hyperplane.

If:

$$
\mathbf{w}^\top \mathbf{x} + b < 0
$$

then it lies on the opposite side.

Thus, the sign naturally separates the space into two regions.

---

### Activation Function

The Perceptron uses a hard threshold activation:

$$
\phi(z) = \text{sign}(z)
$$

Unlike sigmoid or softmax:

- Output is discrete
- Non-differentiable
- No probability interpretation

---

### Perceptron Loss Function

The Perceptron only penalizes **misclassified points**.

A sample is misclassified if:

$$
y^{(i)}(\mathbf{w}^\top \mathbf{x}^{(i)} + b) \le 0
$$

because:

- Correct classification:

$$
y^{(i)}f(\mathbf{x}^{(i)}) > 0
$$

- Wrong classification:

$$
y^{(i)}f(\mathbf{x}^{(i)}) \le 0
$$

The Perceptron loss is:

$$
L(\mathbf{w}, b)
=
-\sum_{i \in \mathcal{M}}
y^{(i)}(\mathbf{w}^\top \mathbf{x}^{(i)} + b)
$$

where:

- $\mathcal{M}$ = set of misclassified samples

Equivalent form:

$$
L(\mathbf{w}, b)
=
\sum_{i=1}^{N}
\max\left(0, -y^{(i)}(\mathbf{w}^\top \mathbf{x}^{(i)} + b)\right)
$$

---

### Intuition Behind the Loss

Consider:

$$
y(\mathbf{w}^\top \mathbf{x} + b)
$$

This quantity measures:

- Positive and large → confident correct prediction
- Near zero → uncertain
- Negative → wrong prediction

The Perceptron tries to push all samples to the correct side of the boundary.

---

### Gradient of the Loss

For a misclassified sample:

$$
L_i = -y^{(i)}(\mathbf{w}^\top \mathbf{x}^{(i)} + b)
$$

Gradient w.r.t. weights:

$$
\frac{\partial L_i}{\partial \mathbf{w}}
=
-y^{(i)}\mathbf{x}^{(i)}
$$

Gradient w.r.t. bias:

$$
\frac{\partial L_i}{\partial b}
=
-y^{(i)}
$$

---

### Deriving the Update Rule

Gradient descent update:

$$
\mathbf{w}
\leftarrow
\mathbf{w}
-
\eta
\frac{\partial L}{\partial \mathbf{w}}
$$

Substitute gradient:

$$
\mathbf{w}
\leftarrow
\mathbf{w}
+
\eta y^{(i)}\mathbf{x}^{(i)}
$$

Bias update:

$$
b
\leftarrow
b + \eta y^{(i)}
$$

where:

- $\eta$ = learning rate

---

### Intuition of the Update

If a positive sample is classified incorrectly:

$$
y = +1
$$

then:

$$
\mathbf{w}
\leftarrow
\mathbf{w} + \eta \mathbf{x}
$$

The boundary moves toward correctly classifying that sample.

If a negative sample is wrong:

$$
y = -1
$$

then:

$$
\mathbf{w}
\leftarrow
\mathbf{w} - \eta \mathbf{x}
$$

Thus, weights are adjusted in the direction that reduces future mistakes.

---

### Linear Separability

The Perceptron Convergence Theorem states:

> If the data is linearly separable, PLA converges in a finite number of updates.

This is a fundamental theoretical result.

If data is not linearly separable:

- PLA may never converge
- Weights may oscillate forever

---

## 4. How It Works (Flow / Intuition)

## Step-by-Step Process

### Step 1 — Initialize Parameters

Initialize:

$$
\mathbf{w} = \mathbf{0}
,\quad
b = 0
$$

or random small values.

---

### Step 2 — Compute Prediction

For each sample:

$$
\hat{y} = \text{sign}(\mathbf{w}^\top \mathbf{x} + b)
$$

---

### Step 3 — Check Classification

If:

$$
\hat{y} \ne y
$$

the sample is misclassified.

---

### Step 4 — Update Parameters

Apply:

$$
\mathbf{w}
\leftarrow
\mathbf{w} + \eta y\mathbf{x}
$$

$$
b
\leftarrow
b + \eta y
$$

---

### Step 5 — Repeat

Continue until:

- No classification errors
- Or maximum iterations reached

---

### Intuitive Picture

The decision boundary keeps rotating and shifting until it separates the classes.

Each misclassified point "pulls" the boundary toward the correct orientation.

---

## 5. Training Process

### Learning Procedure

The Perceptron learns iteratively using **online learning**.

At each step:

1. Pick a training sample
2. Predict its label
3. Update only if prediction is wrong

---

### Optimization Method

Unlike modern neural networks:

- PLA does not use full batch optimization
- Typically uses **stochastic updates**

This makes it computationally simple.

---

### Update Rule

For misclassified sample:

$$
(\mathbf{x}^{(i)}, y^{(i)})
$$

update:

$$
\mathbf{w}
\leftarrow
\mathbf{w}
+
\eta y^{(i)}\mathbf{x}^{(i)}
$$

$$
b
\leftarrow
b + \eta y^{(i)}
$$

No update occurs if the sample is correctly classified.

---

### Learning Rate

The learning rate:

$$
\eta > 0
$$

controls update magnitude.

- Small $\eta$ → stable but slower
- Large $\eta$ → faster but unstable

For PLA, $\eta = 1$ is often sufficient.

---

## 6. Assumptions

### Main Assumption

The Perceptron assumes:

$$
\text{Classes are linearly separable}
$$

Meaning there exists some hyperplane that perfectly separates the data.

---

### What Happens if This Fails?

If the dataset is not linearly separable:

- PLA may never converge
- Boundary keeps changing indefinitely
- Performance becomes unstable

Examples of non-linearly separable problems:

- XOR problem
- Complex image classification
- Highly overlapping distributions

---

### Feature Assumptions

PLA also implicitly assumes:

- Features are informative
- Linear combination is meaningful
- Scale differences are manageable

Feature normalization often improves learning.

---

## 7. When to Use / When NOT to Use

### Suitable Scenarios

#### Use PLA When

- Problem is approximately linearly separable
- Dataset is small
- Need a fast baseline classifier
- Educational purposes
- Online learning settings

Examples:

- Simple spam detection
- Binary text classification
- Basic signal classification

---

### Unsuitable Scenarios

#### Avoid PLA When

- Data is highly nonlinear
- Need probability outputs
- Multi-class complexity is high
- Noise is significant
- Accuracy requirements are strict

Better alternatives:

- Logistic Regression
- SVM
- Neural Networks
- Decision Trees

---

## 8. Evaluation Metrics

## Common Metrics

### Accuracy

$$
\text{Accuracy}
=
\frac{\text{Correct Predictions}}{\text{Total Samples}}
$$

Useful when classes are balanced.

---

### Precision

$$
\text{Precision}
=
\frac{TP}{TP + FP}
$$

Measures false positive quality.

---

### Recall

$$
\text{Recall}
=
\frac{TP}{TP + FN}
$$

Measures ability to detect positives.

---

### F1 Score

$$
F1
=
\frac{2PR}{P+R}
$$

Balances precision and recall.

---

### Why These Metrics Fit PLA

PLA performs hard classification:

$$
\hat{y} \in \{-1,+1\}
$$

Therefore classification metrics naturally evaluate its performance.

---

## 9. Pros and Cons

### Strengths

#### Simple and Fast

- Extremely lightweight
- Easy to implement

#### Interpretable

Weights indicate feature importance.

#### Online Learning Capability

Can update incrementally with streaming data.

#### Strong Historical Importance

Foundation of neural networks and deep learning.

---

### Limitations

#### Only Linear Boundaries

Cannot solve nonlinear problems.

---

#### No Probability Output

Predictions are binary only.

---

#### Sensitive to Non-Separable Data

May fail to converge.

---

#### Hard Threshold Function

Non-differentiability limits optimization flexibility.

---

## 10. Practical Notes

### Feature Scaling Matters

Large feature magnitudes can dominate updates.

Common preprocessing:

- Standardization
- Normalization

---

### Bias Trick

Instead of separate bias:

$$
\tilde{\mathbf{x}}
=
[x_1,x_2,\dots,x_d,1]
$$

$$
\tilde{\mathbf{w}}
=
[w_1,w_2,\dots,w_d,b]
$$

Then:

$$
f(\mathbf{x})
=
\tilde{\mathbf{w}}^\top \tilde{\mathbf{x}}
$$

This simplifies implementation.

---

### Shuffling Training Data

Randomizing sample order helps avoid cyclic behavior.

---

### Non-Convergence Detection

In practice:

- Set maximum epochs
- Stop if updates stop improving

---

## Relation to Logistic Regression

Both models use:

$$
\mathbf{w}^\top \mathbf{x} + b
$$

Difference:

| Model | Output | Loss |
|---|---|---|
| Perceptron | Hard class | Perceptron loss |
| Logistic Regression | Probability | Log loss |

Logistic Regression is smoother and more stable.

---

## Relation to Neural Networks

A single artificial neuron is essentially:

- Weighted sum
- Activation function
- Bias

The Perceptron is therefore the ancestor of modern deep learning architectures.

---

## Key Takeaways

- PLA is a binary linear classifier
- Learns using mistake-driven updates
- Works only for linearly separable data
- Decision boundary is a hyperplane
- Update rule comes directly from Perceptron loss
- Historically foundational for neural networks
- Simple mathematically, but extremely important conceptually

---
## Links
- Metrics:
	- [[Accuracy]]
	- [[Precision]]
	- [[Recall]]
	- [[F1]]
- Related:
	- [[Logistic Regression]]
	- [[Neural Network]]