
## 1. Definition

### What is Precision?

Precision is a **classification evaluation metric** that measures the proportion of predicted positive instances that are actually positive.

It answers the question:

> “When the model predicts positive, how often is it correct?”

Precision evaluates the **reliability of positive predictions** rather than the overall correctness of the model.

---

### Type of Task

Precision is primarily used in:

- Binary classification
- Multi-class classification
- Multi-label classification
- Information retrieval
- Ranking systems
- Detection systems

---

### Core Idea

Precision focuses on the quality of positive predictions.

A model with high precision produces very few **False Positives (FP)**.

It is especially important when:

- False positives are costly
- Positive predictions must be trustworthy
- The predicted positive class triggers expensive or risky actions

---

## 2. Objective / Evaluation Goal

### What Aspect Does Precision Evaluate?

Precision evaluates:

$$
\text{Correctness of positive predictions}
$$

Specifically:

- Among all samples predicted as positive,
- how many are truly positive?

---

### What Does Good Performance Mean?

High precision means:

- The model rarely labels negative samples as positive
- Positive predictions are highly reliable
- Few false alarms occur

---

### Real-World Objective

Precision reflects situations where:

- Incorrect positive predictions are expensive
- Trust in positive predictions matters more than finding all positives

Examples:

| Application | Why Precision Matters |
|---|---|
| Spam detection | Avoid marking legitimate emails as spam |
| Medical diagnosis | Avoid falsely diagnosing healthy patients |
| Fraud detection | Avoid falsely blocking legitimate transactions |
| Search engines | Returned results should actually be relevant |
| Recommendation systems | Recommended items should truly match user interest |

---

## 3. Mathematical Formulation 

### Confusion Matrix Structure

For binary classification:

| Actual / Predicted | Positive | Negative |
|---|---|---|
| Positive | TP | FN |
| Negative | FP | TN |

Where:

- $TP$ = True Positives
- $FP$ = False Positives
- $FN$ = False Negatives
- $TN$ = True Negatives

---

### Precision Formula

$$
\text{Precision} = \frac{TP}{TP + FP}
$$

---

### Variable Definitions

#### True Positives ($TP$)

Samples that:

- truly belong to the positive class
- and are predicted as positive

---

#### False Positives ($FP$)

Samples that:

- actually belong to the negative class
- but are incorrectly predicted as positive

These are also called:

$$
\text{Type I Errors}
$$

---

### Denominator Interpretation

$$
TP + FP
$$

represents:

> All predicted positive samples

Thus precision measures:

$$
\frac{\text{Correct positive predictions}}{\text{All positive predictions}}
$$

---

### Probabilistic Interpretation

Precision can also be interpreted as:

$$
P(Y=1 \mid \hat{Y}=1)
$$

Meaning:

> Probability that a sample is truly positive given that the model predicted positive.

---

## 4. Input Structure (VERY IMPORTANT)

### Required Inputs

Precision requires:

#### Ground Truth Labels

$$
y_i \in \{0,1\}
$$

Where:

- $1$ = positive class
- $0$ = negative class

---

#### Predicted Labels

$$
\hat{y}_i \in \{0,1\}
$$

Precision operates on discrete predicted labels.

---

### Does Precision Require a Threshold?

Yes.

If the model outputs probabilities:

$$
\hat{p}_i \in [0,1]
$$

a threshold must convert probabilities into class labels:

$$
\hat{y}_i =
\begin{cases}
1 & \text{if } \hat{p}_i \ge t \\
0 & \text{otherwise}
\end{cases}
$$

where:

$$
t = \text{decision threshold}
$$

typically:

$$
t = 0.5
$$

---

### Dependence on Threshold

Precision is highly threshold-dependent.

Increasing the threshold usually:

- decreases FP
- increases precision
- decreases recall

---

### Does Precision Depend on Probability Calibration?

Indirectly.

Precision itself uses only class labels, but:

- thresholds are applied to probabilities
- poorly calibrated probabilities can lead to poor threshold behavior

---

## 5. Properties

### Range of Values

$$
0 \le \text{Precision} \le 1
$$

---

#### Extreme Cases

##### Perfect Precision

$$
\text{Precision} = 1
$$

occurs when:

$$
FP = 0
$$

Every positive prediction is correct.

---

##### Worst Precision

$$
\text{Precision} = 0
$$

occurs when:

$$
TP = 0
$$

No positive prediction is correct.

---

### Sensitivity to Class Imbalance

Precision is relatively robust to class imbalance because it ignores:

$$
TN
$$

However:

- rare positives can make precision unstable
- small FP changes may heavily affect precision

---

### Sensitivity to Outliers

Precision is not directly sensitive to numerical outliers because it uses categorical outcomes.

However:

- mislabeled samples
- ambiguous boundary cases

can strongly affect it.

---

### Differentiability

Precision is:

- discrete
- non-differentiable

Thus:

- it is generally unsuitable as a direct optimization loss for gradient-based learning

Models instead optimize surrogate losses such as:

- cross-entropy
- logistic loss

---

### Stability

Precision may become unstable when:

$$
TP + FP
$$

is small.

Example:

- only 2 positive predictions
- 1 error changes precision dramatically

---

## 6. What It Measures (Model Behavior)

### What Errors Are Emphasized?

Precision strongly penalizes:

$$
FP
$$

It focuses on avoiding false positives.

---

### What Errors Are Ignored?

Precision does not directly consider:

$$
FN
$$

A model may achieve high precision simply by predicting positive very rarely.

---

### Behavioral Bias

Precision encourages conservative positive predictions.

The model becomes biased toward:

- predicting positive only when highly confident
- reducing false alarms

---

### Precision vs Recall Tradeoff

Precision and recall are fundamentally competing objectives.

Increasing precision often decreases recall.

---

#### High Precision Strategy

The model predicts positive only in highly certain cases.

Result:

- few FP
- many FN

---

#### High Recall Strategy

The model predicts positive more aggressively.

Result:

- fewer FN
- more FP

---

### Example

Suppose:

- 100 actual positives exist

#### Model A

Predicts only 5 positives:

- 5 correct
- 0 incorrect

$$
\text{Precision}=1.0
$$

but recall is poor.

---

#### Model B

Predicts 80 positives:

- 60 correct
- 20 incorrect

$$
\text{Precision}=0.75
$$

but recall is much higher.

---

## 7. Intuition

### Core Intuition

Precision measures:

> “How trustworthy are positive predictions?”

Imagine a doctor diagnosing disease.

High precision means:

- when the doctor says “diseased,”
- the patient is usually truly diseased.

---

### Filtering Interpretation

Precision evaluates the purity of selected samples.

If the model selects samples as positive:

- precision measures contamination by false positives.

---

### Information Retrieval Intuition

In search systems:

- retrieved documents = predicted positives
- relevant documents = actual positives

Precision asks:

> “How many retrieved results are actually relevant?”

---

### Geometric Intuition

Increasing the decision threshold shrinks the positive prediction region.

As the region shrinks:

- FP often decreases faster than TP
- precision increases

But eventually:

- recall collapses

---

### Confidence Interpretation

Precision rewards cautious prediction behavior.

The model is rewarded for:

- predicting positive only when evidence is strong.

---

## 8. Interpretation

### High Precision

High precision means:

- positive predictions are reliable
- false positives are rare
- predicted positives are trustworthy

Example:

$$
\text{Precision}=0.95
$$

means:

95% of predicted positives are correct.

---

### Low Precision

Low precision means:

- many false alarms occur
- positive predictions are unreliable

Example:

$$
\text{Precision}=0.20
$$

means:

only 20% of positive predictions are correct.

---

### What Is “Good” Precision?

Depends heavily on domain requirements.

| Domain | Typical Requirement |
|---|---|
| Medical screening | Often prioritize recall |
| Cancer diagnosis confirmation | Very high precision |
| Fraud detection | High precision preferred |
| Search ranking | Precision at top ranks critical |
| Spam filtering | High precision needed |

---

## 9. When to Use / When NOT to Use

### When to Use Precision

Use precision when:

- false positives are expensive
- positive predictions must be trusted
- alarm fatigue is dangerous
- human review is costly

---

### Suitable Scenarios

#### Medical Confirmation Tests

False positive diagnoses may cause:

- unnecessary stress
- expensive procedures

---

#### Fraud Detection

Incorrect fraud alerts may:

- block legitimate users
- reduce customer trust

---

#### Recommendation Systems

Low precision recommendations:

- reduce user engagement
- create noisy experiences

---

#### Search Engines

Users prefer:

- fewer but highly relevant results

---

### When NOT to Use Precision Alone

Do not rely solely on precision when:

- missing positives is dangerous
- recall matters equally or more

---

### Unsuitable Scenarios

#### Disease Screening

Missing sick patients may be catastrophic.

High recall is more important.

---

#### Safety Systems

Example:

- fire detection
- intrusion detection

False negatives may be unacceptable.

---

### Imbalanced Data Considerations

Precision is often useful in imbalanced datasets because accuracy becomes misleading.

Example:

- 99% negatives
- always predicting negative yields 99% accuracy
- but precision becomes meaningful once positives are predicted

---

## 10. Comparison with Other Metrics

### Precision vs Accuracy

#### Accuracy

$$
\frac{TP+TN}{TP+FP+FN+TN}
$$

Measures overall correctness.

---

#### Precision

Focuses only on positive predictions.

---

### Key Difference

Accuracy includes:

$$
TN
$$

Precision ignores:

$$
TN
$$

Thus precision is often better for imbalanced classification.

---

### Precision vs Recall

#### Recall Formula

$$
\text{Recall} = \frac{TP}{TP+FN}
$$

---

#### Difference

| Metric | Focus |
|---|---|
| Precision | Avoid FP |
| Recall | Avoid FN |

---

### Precision vs F1 Score

#### F1 Formula

$$
F1 = \frac{2PR}{P+R}
$$

where:

- $P$ = precision
- $R$ = recall

---

#### Difference

F1 balances:

- precision
- recall

Precision alone ignores recall.

---

### Precision vs ROC-AUC

ROC-AUC evaluates ranking quality across thresholds.

Precision evaluates performance at a specific threshold.

---

### Precision vs PR-AUC

PR-AUC summarizes:

- precision-recall tradeoff over all thresholds

Better than ROC-AUC for heavily imbalanced datasets.

---

## 11. Notes / Pitfalls

### Precision Can Be Artificially Inflated

A model can achieve extremely high precision simply by:

- predicting positive very rarely

Example:

- predict only 1 sample as positive
- if correct:

$$
\text{Precision}=1
$$

but recall may be terrible.

---

### Precision Alone Is Incomplete

Precision does not indicate:

- how many positives were missed
- overall model usefulness

Always examine alongside:

- recall
- F1
- PR curves

---

### Threshold Choice Is Critical

Changing threshold changes precision dramatically.

Thus:

- precision values are meaningless without threshold context

---

### Precision Depends on Class Distribution

Precision may change when class prevalence changes.

A model evaluated on one dataset may show different precision on another.

---

### Undefined Precision

If:

$$
TP + FP = 0
$$

then precision becomes undefined.

This occurs when:

- the model predicts no positives

Libraries usually define this as:

- 0
- or return warning/NaN

---

### Precision Is Not Symmetric

Precision depends on which class is defined as positive.

Switching positive/negative labels changes precision completely.

---

### Practical Recommendation

For imbalanced classification:

- use precision together with recall
- inspect Precision-Recall curves
- optimize threshold based on business cost

---

## Summary

Precision measures:

$$
\text{How correct positive predictions are}
$$

Core characteristics:

- Penalizes false positives
- Ignores true negatives
- Threshold-dependent
- Useful for imbalanced classification
- Encourages conservative prediction behavior

Precision is most valuable when:

> False positives are expensive and positive predictions must be trustworthy.

---
## Links
- [[Recall]]
- [[F1]]
- [[ROC-AUC]]
- [[PR-AUC]]