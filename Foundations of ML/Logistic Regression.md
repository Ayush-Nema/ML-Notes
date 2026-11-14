
# Logistic Regression — From a Line to a Probability
## 1. Start with a useful question
> “A student has studied for six hours. Can we estimate the probability that they will pass?”

Suppose we have previous examples of students' study hours and exam outcomes.

| Symbol   | Meaning                                    | Example                    |
| -------- | ------------------------------------------ | -------------------------- |
| $x$      | Input feature: hours studied               | $6$                        |
| $y$      | Observed target label                      | $1$ for pass, $0$ for fail |
| $p$      | Model's estimated probability of passing   | $0.73$                     |
| $\hat y$ | Predicted label after choosing a threshold | $1$                        |

A **feature** is information available when making a prediction. A **label** is the outcome we want to predict. Learning from labeled examples is **supervised learning**.

**Regression** usually predicts a numerical quantity, such as an exam score. **Classification** predicts a category, such as pass or fail. Logistic regression is mainly a classification method despite its name.

Practical motivation: estimated probabilities can help a teacher decide whom to offer additional support. Similar workflows appear in spam filtering, purchase prediction, and customer churn prediction.

**Ask the class:** Why might two students who study for the same number of hours have different outcomes?

**Expected discussion:** prior knowledge, sleep, exam difficulty, and other unobserved factors. A probability allows uncertainty rather than promising an outcome.

## 2. Start from linear regression
Recall the straight-line prediction:
$$
\boxed{\hat y=b+wx}
$$
- $w$ is the slope, also called a weight or coefficient.
- $b$ is the intercept, also called a bias.
Imagine fitting a line to labels that are only 0 and 1. The line might output $-0.2$ or $1.3$. These values cannot be probabilities.

**Important:** a line is still useful as a way to combine information. Keep it, but call its output a **score**:
$$
\boxed{z=b+wx}
$$
The score can be any real number. We need a transformation from that score to a valid probability.

> [!note] What “from linear regression” means
> We reuse the linear predictor, then change the output transformation and the training objective. Logistic regression is not obtained simply by fitting ordinary linear regression and clipping its predictions between 0 and 1.

## 3. Build the logistic model
### A. Introduce odds
If the probability of passing is $p$, then the probability of failing is $1-p$.
$$
\boxed{\text{odds of passing}=\frac{p}{1-p}}
$$
For $p=0.8$, the odds are $0.8/0.2=4$: four to one. Odds and probability are different quantities.

Probabilities lie between 0 and 1. Odds are positive. 
_Taking the natural logarithm of the odds allows a value anywhere on the real number line._

### B. Make a modeling assumption
Instead of assuming that the **probability** is a straight line, assume that the **log-odds** are a straight line:
$$
\boxed{\log\left(\frac{p}{1-p}\right)=b+wx=z}
$$
This is a choice of model, not something forced by linear regression. Students need only understand that the logarithm and exponential undo each other.

### C. Solve for the probability
Exponentiate both sides:
$$
\frac{p}{1-p}=e^z
$$
Multiply and rearrange:
$$
p=e^z(1-p)
$$
$$
p+pe^z=e^z
$$
$$
\boxed{p=\frac{e^z}{1+e^z}=\frac{1}{1+e^{-z}}}
$$
The last expression is the **sigmoid function**, writte`n $\sigma(z)$:
$$
\boxed{p=P(y=1\mid x)=\sigma(b+wx)}
$$
Read this as: “The estimated probability of label 1, given the input, is the sigmoid of a linear score.”

| Score $z$ | Probability $\sigma(z)$ | Interpretation |
| --- | --- | --- |
| $-2$ | $0.119$ | Passing is less likely |
| $0$ | $0.500$ | Equal estimated chances |
| $2$ | $0.881$ | Passing is more likely |
The sigmoid has an S-shaped curve. For any finite score, the mathematical probability is strictly between 0 and 1, approaching the ends as the score becomes extreme.

**Board takeaway:** input → linear score → sigmoid → probability.

## 4. Work one example by hand
For illustration, suppose: 
$$
b=-4,\qquad w=0.8
$$
For a student who studied six hours:
$$
z=-4+0.8(6)=0.8
$$
$$
p=\frac{1}{1+e^{-0.8}}\approx0.690
$$
The model estimates about a **69% chance of passing**. This does not mean an exam score of 69%, and it does not guarantee that the student passes.

| Hours | Score | Estimated probability of passing |
| --- | --- | --- |
| $2$ | $-2.4$ | $0.083$ |
| $5$ | $0$ | $0.500$ |
| $8$ | $2.4$ | $0.917$ |
These are hand-chosen teaching weights. The fitted notebook model will learn different values from data.

**Interpretation:** a positive $w$ means that increasing hours raises the predicted probability. It does **not** mean that every extra hour adds 80 percentage points. The probability change depends on where we are on the curve.

## 5. Turn probability into a decision
Choose a ==decision threshold== $t$:
$$
\hat y=\begin{cases}
1 & p\geq t\\
0 & p<t
\end{cases}
$$
At $t=0.5$, the student with probability $0.690$ is classified as pass. At $t=0.8$, that same student is classified as fail.

**The estimated probability has not changed. The decision rule has.**

At threshold 0.5, the decision boundary occurs where $z=0$. In our hand example:
$$
-4+0.8x=0\quad\Rightarrow\quad x=5
$$
This is a boundary in the model's predictions, not a rule that every student studying five hours will pass.

## 6. How does the model learn its weights?
Until now, we supplied $b$ and $w$. During **training**, the model learns them from labeled examples.
1. Start with candidate weights.
2. Compute each example's score and probability.
3. Measure the prediction error with a loss function.
4. Adjust the weights to reduce the average loss.
5. Repeat until an optimization stopping rule is reached.

### A loss that rewards sensible probabilities
For one example:
$$
\ell(y,p)=-\left[y\log p+(1-y)\log(1-p)\right]
$$
This is **binary cross-entropy**, also called **log loss**.

| Actual outcome | Loss simplifies to | Good prediction |
| --- | --- | --- |
| $y=1$ | $-\log p$ | $p$ near 1 |
| $y=0$ | $-\log(1-p)$ | $p$ near 0 |

If the actual label is 1:

| Predicted $p$ | Approximate loss |
| --- | --- |
| $0.9$ | $0.105$ |
| $0.5$ | $0.693$ |
| $0.1$ | $2.303$ |

**Confident mistakes receive larger penalties.** The training objective averages this loss over the training examples.

## 7. Run the practical workflow
Use the notebook in this order:
1. **Inspect the data.** One row per simulated student; `hours_studied` is the feature and `passed` is the label.
2. **Split before fitting.** The training set teaches the model; the test set checks predictions on held-out examples.
3. **See the linear-regression problem.** Fit a line on training data and inspect its unbounded predictions.
4. **Inspect the sigmoid and loss.** Connect the graphs to the formulas above.
5. **Fit logistic regression.** Use `fit(X_train, y_train)`.
6. **Predict probabilities.** Use `predict_proba`; identify the column corresponding to label 1.
7. **Inspect the learned curve.** Compare individual outcomes with the smooth probability curve.
8. **Try thresholds using training data.** Treat this as a demonstration, not final performance evidence.
9. **Evaluate once on the test set at the preselected threshold 0.5.** Compare against a simple baseline.
10. **Predict a new example and discuss limitations.**

## 8. Evaluate predictions without overloading students
A **confusion matrix** counts correct and incorrect decisions.

| | Predicted fail: 0 | Predicted pass: 1 |
| --- | --- | --- |
| **Actual fail: 0** | True negative (TN) | False positive (FP) |
| **Actual pass: 1** | False negative (FN) | True positive (TP) |

Here **positive means pass**, not “needs support.” Always state the positive class.
- **False positive:** predicted pass, but actually failed. A student might miss support.
- **False negative:** predicted fail, but actually passed. Extra support might be offered unnecessarily.

$$
\text{Accuracy}=\frac{TP+TN}{TP+TN+FP+FN}
$$

Also compare with a **baseline**: always predict the most common training label. An accuracy value is more meaningful when we know whether a simple strategy already achieves something similar.

Accuracy is not enough when one class is much more common or when the two error types have different consequences. For this class, read the matrix and discuss the mistakes; precision, recall, and AUC can wait.

> [!warning] Keep the test set honest
> The test set is not training material. If we repeatedly change features, weights, or thresholds after seeing its results, it becomes part of our model selection process. Use a separate validation set or cross-validation for those choices in a fuller workflow.

## 9. Check understanding

Ask students to answer before revealing the key.
1. Why can a raw linear score not always represent a probability?
2. If $z=0$, what is the predicted probability?
3. A model gives $p=0.65$. What is the decision at thresholds 0.5 and 0.7?
4. Does increasing the threshold require retraining the model?
5. If a student passes, which prediction has lower log loss: 0.9 or 0.2?
6. Why should we not evaluate only on the training examples?
7. In this lesson, what does a false positive mean?

> [!success]- Answer key
> 1. A linear score can be below 0 or above 1.
> 2. 0.5.
> 3. Pass at 0.5; fail at 0.7.
> 4. No. It changes the decision rule applied to the existing probabilities.
> 5. 0.9, because it assigns greater probability to the observed outcome.
> 6. That checks examples already used to learn weights, so it can overstate performance on new examples.
> 7. Predicting pass for a student who actually fails.


## Topics intentionally omitted — for later lessons

| Topic | Why return to it? |
| --- | --- |
| Multiple features and categorical encoding | Real tasks use more than one input. |
| Feature scaling and preprocessing pipelines | Keep transformations consistent and avoid leakage. |
| Gradient derivation and optimization details | Understand how weights are updated. |
| Maximum likelihood and the Bernoulli model | Explain the origin of cross-entropy mathematically. |
| L1/L2 regularization and hyperparameter tuning | Control model complexity. |
| Validation sets and cross-validation | Choose models and thresholds without using the test set. |
| Precision, recall, F1, ROC-AUC, and PR-AUC | Compare different kinds of errors and rankings. |
| Class imbalance and class weights | Handle rare outcomes. |
| Probability calibration | Check whether predicted probabilities match observed frequencies. |
| Multiple classes and softmax | Extend beyond binary classification. |
| Nonlinear features and interactions | Model relationships that a simple linear score misses. |
| Coefficient uncertainty, correlated features, and separation | Understand statistical interpretation and fitting limitations. |
| Fairness, causality, and deployment monitoring | Assess consequences and behavior outside a classroom dataset. |

## Optional appendix A — Where log loss comes from

For a binary label, one expression gives the probability assigned to the observed result:
$$
P(y\mid x)=p^y(1-p)^{1-y}
$$
When $y=1$, this is $p$. When $y=0$, this is $1-p$.

Assuming examples are independent, multiply these probabilities across the data. This product is the **likelihood**. Choose weights that maximize it. Taking the logarithm converts the product into a sum without changing the maximizing weights:
$$
\log L=\sum_{i=1}^{m}\left[y_i\log p_i+(1-y_i)\log(1-p_i)\right]
$$
Negating and averaging gives binary cross-entropy. Maximizing likelihood and minimizing this unregularized loss are equivalent.

## Optional appendix B — One gradient update
Let $J$ be the average unregularized log loss. For one feature:
$$
\frac{\partial J}{\partial w}=\frac{1}{m}\sum_{i=1}^{m}(p_i-y_i)x_i,
\qquad
\frac{\partial J}{\partial b}=\frac{1}{m}\sum_{i=1}^{m}(p_i-y_i)
$$
With a positive learning rate $\alpha$, gradient descent updates:
$$
w\leftarrow w-\alpha\frac{\partial J}{\partial w},
\qquad
b\leftarrow b-\alpha\frac{\partial J}{\partial b}
$$