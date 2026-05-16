
## 1. Definition

### What is MAE?

**Mean Absolute Error (MAE)** is a regression evaluation metric that measures the average magnitude of prediction errors without considering their direction.

It computes the average absolute difference between predicted values and true values.

MAE answers the question:

> “On average, how far are predictions from the actual values?”

---

### Task Type

- Regression
- Forecasting
- Continuous value prediction

---

### Core Idea

MAE measures prediction accuracy in the original unit of the target variable by averaging absolute errors.

Unlike squared-error metrics, MAE treats all errors linearly and equally.

---

## 2. Objective / Evaluation Goal

### What does MAE evaluate?

MAE evaluates:

- Average prediction deviation
- Magnitude of prediction errors
- Overall prediction accuracy in interpretable units

---

### What does “good performance” mean?

A lower MAE indicates:

- Predictions are closer to the true values
- Smaller average error magnitude
- Better regression performance

The ideal value is:

$$
\text{MAE} = 0
$$

which means perfect predictions.

---

### Real-world meaning

MAE directly reflects real-world average error.

Examples:

- Weather forecasting:
    
    - MAE = 2°C means predictions are off by 2°C on average
        
- House price prediction:
    
    - MAE = \$10,000 means predictions miss actual prices by \$10,000 on average
        
- Delivery estimation:
    
    - MAE = 5 minutes means average prediction error is 5 minutes
        

This makes MAE highly interpretable.

---

## 3. Mathematical Formulation

### Formula

Given:

- Ground truth values:
    
    $$
    y_1, y_2, \dots, y_n
    $$
    
- Predicted values:
    
    $$
    \hat{y}_1, \hat{y}_2, \dots, \hat{y}_n
    $$
    

The Mean Absolute Error is:

$$
\text{MAE} =
\frac{1}{n}
\sum_{i=1}^{n}
|y_i - \hat{y}_i|
$$

---

### Variable Definitions

### $n$

Number of samples.

---

### $y_i$

True target value for sample $i$.

---

### $\hat{y}_i$

Predicted value for sample $i$.

---

### $|y_i - \hat{y}_i|$

Absolute prediction error.

This measures the magnitude of the error regardless of direction.

Examples:

$$
|5 - 3| = 2
$$

$$
|3 - 5| = 2
$$

Overprediction and underprediction are penalized equally.

---

### Expectation Form

MAE can also be viewed probabilistically:

$$
\text{MAE} =
\mathbb{E}[|Y - \hat{Y}|]
$$

This means:

> MAE estimates the expected absolute prediction error.

---

### 4. Input Structure

### Required Inputs

#### Ground Truth

$$
y \in \mathbb{R}^n
$$

Continuous real-valued targets.

---

#### Predictions

$$
\hat{y} \in \mathbb{R}^n
$$

Continuous predicted values.

---

### Does MAE require probabilities?

No.

MAE uses direct continuous predictions.

---

### Does MAE require thresholds?

No.

There is no thresholding process.

---

### Does MAE depend on calibration?

No.

MAE only evaluates numerical distance between predictions and targets.

---

## 5. Properties

### Range

$$
\text{MAE} \in [0, \infty)
$$

- Minimum:
    
    $$
    0
    $$
    
- No upper bound
    

---

### Scale Dependence

MAE depends on the scale of the target variable.

Examples:

- MAE = 10 may be:
    
    - excellent for predicting house prices
        
    - terrible for predicting temperature
        

---

### Unit Preservation

A major advantage:

MAE has the same unit as the target variable.

Examples:

| Target | MAE Unit |
|---|---|
| Temperature | °C |
| Price | dollars |
| Distance | meters |

This makes MAE highly interpretable.

---

### Sensitivity to Outliers

MAE is:

- More robust to outliers than MSE/RMSE
- Less sensitive to extremely large errors

Because errors grow linearly:

$$
|e|
$$

instead of quadratically:

$$
e^2
$$

---

### Differentiability

MAE is not differentiable at:

$$
e = 0
$$

because absolute value has a sharp corner.

---

### Gradient

For error:

$$
e = y - \hat{y}
$$

the derivative is:

$$
\frac{d|e|}{de} =
\begin{cases}
1 & e > 0 \\
-1 & e < 0
\end{cases}
$$

Undefined at:

$$
e = 0
$$

---

### Optimization Consequence

Because gradients are constant:

- MAE optimization can be harder
- Convergence may be slower
- Gradients do not shrink near optimum

Compared to MSE, MAE provides less smooth optimization behavior.

---

## 6. What It Measures (Model Behavior)

### Core Behavior

MAE measures:

> Average absolute prediction deviation.

It evaluates how wrong predictions are on average.

---

### Error Treatment

MAE penalizes all errors linearly.

Example:

| Error | Contribution to MAE |
|---|---|
| 1 | 1 |
| 5 | 5 |
| 10 | 10 |

Large errors are not disproportionately amplified.

---

### What MAE emphasizes

MAE emphasizes:

- Typical prediction accuracy
- Median-like behavior
- Robust overall performance

---

### What MAE ignores

MAE ignores:

- Error direction
- Whether prediction is above or below truth

Example:

$$
y=10,\ \hat{y}=7
$$

and

$$
y=10,\ \hat{y}=13
$$

produce identical error:

$$
3
$$

---

### Bias Introduced by MAE

MAE implicitly favors predicting the:

$$
\textbf{conditional median}
$$

rather than the mean.

This is a fundamental statistical property.

---

### Why?

The value minimizing expected absolute error is the median.

Formally:

$$
\hat{y}^* =
\arg\min_a \mathbb{E}[|Y-a|]
$$

is the median of the distribution.

---

### Consequence

Models optimized with MAE tend to:

- Be robust to skewed distributions
- Resist outlier influence
- Produce conservative predictions

---

## 7. Intuition

### Intuitive Interpretation

MAE asks:

> “How far away are predictions from reality on average?”

Every prediction contributes proportionally to its error.

---

### Geometric Intuition

MAE measures distance using:

$$
L_1
$$

geometry.

The absolute error corresponds to Manhattan distance in one dimension.

---

### Comparison with MSE

MAE:

- Treats all mistakes proportionally
- Does not strongly punish rare large errors

MSE:

- Explodes large errors via squaring
- Prioritizes avoiding catastrophic mistakes

---

### Optimization Intuition

MAE creates a loss landscape with sharp edges.

Unlike MSE’s smooth quadratic bowl, MAE forms piecewise linear surfaces.

This causes:

- Constant gradients
- Non-smooth optimization
- Greater robustness

---

## 8. Interpretation

### Low MAE

Low MAE means:

- Predictions are close to actual values
- Average prediction error is small
- Model predictions are practically accurate

---

### High MAE

High MAE means:

- Predictions deviate substantially from reality
- Average error magnitude is large
- Poor regression quality

---

### What is “good” MAE?

There is no universal threshold.

MAE must be interpreted relative to:

- Target scale
- Business requirements
- Natural variability of the data

---

### Example

If house prices range:

$$
\$100,000 \text{ to } \$1,000,000
$$

then:

- MAE = \$5,000 → excellent
- MAE = \$200,000 → poor

---

## 9. When to Use / When NOT to Use

### When to Use MAE

#### 1. When interpretability matters

MAE is easy to explain to stakeholders because it uses original units.

---

#### 2. When robustness to outliers is important

MAE is preferred when data contains:

- Extreme values
- Heavy-tailed distributions
- Noise spikes

---

#### 3. When average absolute deviation matters operationally

Examples:

- Forecasting demand
- Predicting travel time
- Estimating delivery duration

---

#### 4. When all errors should be weighted equally

MAE does not excessively prioritize large errors.

---

### When NOT to Use MAE

#### 1. When large errors are extremely costly

MAE under-penalizes catastrophic mistakes.

Examples:

- Medical dosage prediction
- Financial risk estimation
- Safety-critical systems

MSE/RMSE may be better.

---

#### 2. When smooth optimization is needed

MAE is non-smooth and harder to optimize directly.

---

#### 3. When variance sensitivity is desired

MAE does not distinguish strongly between moderate and huge errors.

---

## 10. Comparison with Other Metrics

### MAE vs MSE

#### MSE Formula

$$  
\text{MSE}=  
\frac{1}{n}  
\sum_{i=1}^{n}(y_i-\hat{y}_i)^2  
$$

---

| Aspect | MAE | MSE |
|---|---|---|
| Error growth | Linear | Quadratic |
| Outlier sensitivity | Lower | High |
| Optimization smoothness | Non-smooth | Smooth |
| Penalizes large errors | Mildly | Strongly |
| Optimal statistic | Median | Mean |

---

#### Key Difference

MSE strongly amplifies large errors:

$$
10^2 = 100
$$

while MAE keeps proportional penalty:

$$
|10| = 10
$$

---

### MAE vs RMSE

#### RMSE Formula

$$  
\text{RMSE}=  
\sqrt{  
\frac{1}{n}  
\sum_{i=1}^{n}(y_i-\hat{y}_i)^2  
}  
$$

---

RMSE:

- Same unit as target
- Still heavily penalizes large errors

MAE is generally more robust.

---

### MAE vs Median Absolute Error

Median Absolute Error:

$$
\text{Median}(|y_i-\hat{y}_i|)
$$

Difference:

- MAE averages errors
- Median Absolute Error ignores extreme tails even more aggressively

---

### MAE vs Huber Loss

Huber loss combines:

- MSE behavior for small errors
- MAE behavior for large errors

Used when wanting:

- Robustness
- Smooth gradients

---

## 11. Notes / Pitfalls

### 1. MAE is scale-dependent

Cannot compare MAE across datasets with different scales.

---

### 2. MAE may hide catastrophic failures

Because errors are averaged linearly, rare huge failures may appear diluted.

---

### 3. Lower MAE does not always mean better practical behavior

Depending on application:

- Worst-case errors
- Tail risks
- Variance

may matter more.

---

### 4. MAE favors median predictions

This is often misunderstood.

MAE optimization does not estimate conditional mean behavior.

---

### 5. Optimization can be unstable

Because gradients are discontinuous at zero:

- Training may converge slower
- Gradient-based methods may oscillate

---

### 6. MAE alone is often insufficient

In practice, combine MAE with:

- RMSE
- Residual analysis
- Error distribution plots
- Quantile metrics

to fully understand model behavior.

---

## Summary

Mean Absolute Error (MAE) is a regression metric that measures the average absolute deviation between predictions and true values.

Key characteristics:

- Linear error penalty
- Robustness to outliers
- High interpretability
- Same unit as target variable
- Median-oriented statistical behavior

MAE is best when:

- Typical error magnitude matters
- Interpretability is important
- Extreme errors should not dominate evaluation

It is less suitable when:

- Large mistakes are critically important
- Smooth optimization is required
- Tail-risk sensitivity matters

---
## Links
- [[Means Squared Error (MSE)]]
- [[Root Mean Squared Error (RMSE)]]