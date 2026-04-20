## Compare

| Aspect                                | Batch Gradient Descent                                             | Stochastic Gradient Descent (SGD)                               |
| ------------------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------- |
| Data Processing                       | Uses the whole training dataset to compute the gradient.           | Uses a single training sample to compute the gradient.          |
| Convergence Speed                     | Slower, takes longer to converge.                                  | Faster, converges quicker due to frequent updates.              |
| Convergence Accuracy                  | More accurate, gives precise gradient estimates.                   | Less accurate due to noisy gradient estimates.                  |
| Computational and Memory Requirements | Requires significant computation and memory.                       | Requires less computation and memory.                           |
| Optimization of Non-Convex Functions  | Can get stuck in local minima.                                     | Can escape local minima and find the global minimum.            |
| Suitability for Large Datasets        | Not ideal for very large datasets due to slow computation.         | Can handle large datasets effectively.                          |
| Nature                                | Deterministic: Same result for the same initial conditions.        | Stochastic: Results can vary with different initial conditions. |
| Learning Rate                         | Fixed learning rate.                                               | Learning rate can be adjusted dynamically.                      |
| Shuffling of Data                     | No need for shuffling.                                             | Requires shuffling of data before each epoch.                   |
| Overfitting                           | Can overfit if the model is too complex.                           | Can reduce overfitting due to more frequent updates.            |
| Escape Local Minima                   | Cannot escape shallow local minima.                                | Can escape shallow local minima more easily.                    |
| Computational Cost                    | High due to processing the entire dataset at once.                 | Low due to processing one sample at a time.                     |
| Final Solution                        | Tends to converge to the global minimum for convex loss functions. | May converge to a local minimum or saddle point.                |


---
## Summary
Both Batch Gradient Descent and Stochastic Gradient Descent are useful optimization algorithms that serve different purposes depending on the problem at hand.

- **Batch Gradient Descent** is more accurate but slower and computationally expensive. It is ideal when working with small to medium-sized datasets and when high accuracy is required.
- **Stochastic Gradient Descent**, on the other hand, is faster and requires less computational power, making it suitable for large datasets. It can also escape local minima more easily but may converge less accurately.

Choosing between the two algorithms depends on factors like the size of the dataset, computational resources and the nature of the error surface.

---
## Source
- https://www.geeksforgeeks.org/machine-learning/difference-between-batch-gradient-descent-and-stochastic-gradient-descent/
