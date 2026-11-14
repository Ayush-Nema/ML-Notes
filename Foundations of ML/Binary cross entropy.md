# How Does the Binary Cross-Entropy Equation Arise?

The equation comes from a simple goal: **choose weights that assign high probabilities to the outcomes we actually observed.**

## 1. Find the Probability Assigned to the Actual Outcome

Let $p$ mean the model’s predicted probability of **passing**.

There are two possibilities:

| Actual result | Label $y$ | Probability assigned to that result |
| --- | --- | --- |
| Pass | $1$ | $p$ |
| Fail | $0$ | $1-p$ |

For example, if the model predicts $p=0.8$:

- If the student **passes**, it assigned probability **0.8** to the actual outcome.
- If the student **fails**, it assigned probability **0.2** to the actual outcome.

We can combine both cases into one expression:
$$
P(y\mid x)=p^y(1-p)^{1-y}
$$

The exponents act like switches.

When $y=1$:

$$
p^1(1-p)^0=p
$$

When $y=0$:

$$
p^0(1-p)^1=1-p
$$

This expression is the probability formula for a **Bernoulli outcome**: an outcome with two possible labels, 0 and 1.

> [!summary] Derivation
> The equation is created by **combining two cases into one formula**.
> Let $y$ be the actual label and $p$ the predicted probability of label 1:
>
> - If $y=1$, the probability of that outcome is $p$.
> - If $y=0$, the probability of that outcome is $1-p$.
>
> We need an expression that automatically selects the correct probability. Since $y$ is either 0 or 1, **exponents can act as switches**:
> $$
> P(y\mid x)=p^y(1-p)^{1-y}
> $$
>
> **When $y=1$:**
> $$
> p^1(1-p)^0=p\times1=p
> $$
>
> **When $y=0$:**
> $$
> p^0(1-p)^1=1\times(1-p)=1-p
> $$
>
> **Example:** suppose $p=0.8$, meaning the predicted probability of passing is 80%.
> - For passing ($y=1$), the formula gives $0.8$.
> - For failing ($y=0$), the formula gives $1-0.8=0.2$.
>
> This equation is simply a compact way to write the two-case rule. It is called the **Bernoulli probability mass function**. Here, $P(y\mid x)$ means the probability of outcome $y$ given input $x$.


## 2. Combine Several Students

Suppose we have three students:

| Student | Actual label $y$ | Predicted probability of passing $p$ | Probability assigned to actual outcome |
| --- | --- | --- | --- |
| A | $1$: Pass | $0.8$ | $0.8$ |
| B | $0$: Fail | $0.3$ | $0.7$ |
| C | $1$: Pass | $0.6$ | $0.6$ |

Assuming the outcomes are independent given their inputs and the model parameters, multiply these probabilities:

$$
L=0.8\times0.7\times0.6=0.336
$$

Viewed as a function of the model’s weights, this product is called the **likelihood**.

Training seeks weights that **maximize this likelihood**: weights that assign high probabilities to the observed outcomes.

For $m$ examples:

$$
L=\prod_{i=1}^{m}p_i^{y_i}(1-p_i)^{1-y_i}
$$

Here, $\prod$ means “multiply across all examples.”

## 3. Take the Logarithm to Turn Multiplication into Addition

Products of many probabilities can become extremely small. Logarithms also make the mathematics easier because:

$$
\log(abc)=\log a+\log b+\log c
$$

Since the logarithm is an increasing function, maximizing the likelihood also maximizes its logarithm.

For our three students:

$$
\log L=\log(0.8)+\log(0.7)+\log(0.6)
$$

For one student:

$$
\log\left(p^y(1-p)^{1-y}\right)
=
y\log p+(1-y)\log(1-p)
$$

We used these two rules:

$$
\log(ab)=\log a+\log b
$$

$$
\log(a^b)=b\log a
$$

For all examples, the **log-likelihood** is:

$$
\log L=
\sum_{i=1}^{m}
\left[y_i\log p_i+(1-y_i)\log(1-p_i)\right]
$$

Throughout this explanation, $\log$ means the natural logarithm.

## 4. Negate It to Create a Loss We Minimize

We commonly describe training as **minimizing a loss**, rather than maximizing a score.

Multiplying by $-1$ converts maximization into an equivalent minimization problem.

For one example, the resulting loss is:

$$
\boxed{
\ell(y,p)=
-\left[y\log p+(1-y)\log(1-p)\right]
}
$$

This is **binary cross-entropy loss**, also called **binary log loss**.

The actual label selects the relevant term:

| Actual outcome | Label | Loss |
| --- | --- | --- |
| Pass | $y=1$ | $-\log p$ |
| Fail | $y=0$ | $-\log(1-p)$ |

For $m$ examples, we usually average the losses:

$$
\boxed{
J=-\frac{1}{m}\sum_{i=1}^{m}
\left[y_i\log p_i+(1-y_i)\log(1-p_i)\right]
}
$$

Averaging keeps the scale comparable across datasets of different sizes. It does not change which weights minimize the unregularized objective.

### Calculate the Loss for Our Three Students

| Student | Probability assigned to actual outcome | Individual loss |
| --- | --- | --- |
| A | $0.8$ | $-\log(0.8)\approx0.223$ |
| B | $0.7$ | $-\log(0.7)\approx0.357$ |
| C | $0.6$ | $-\log(0.6)\approx0.511$ |

The average loss is:

$$
J=-\frac{\log(0.8)+\log(0.7)+\log(0.6)}{3}
$$

$$
J\approx\frac{0.223+0.357+0.511}{3}\approx0.364
$$

## 5. Why Does It Punish Confident Mistakes?

Consider student C, who actually **passes**, so $y=1$.

The loss simplifies to:

$$
\ell=-\log p
$$

| Predicted probability of passing | Loss | Interpretation |
| --- | --- | --- |
| $0.9$ | $0.105$ | High probability assigned to the actual outcome: small penalty |
| $0.6$ | $0.511$ | Moderate probability assigned to the actual outcome |
| $0.01$ | $4.605$ | Very little probability assigned to the actual outcome: large penalty |

As the probability assigned to the actual outcome approaches zero, the loss grows without bound.

> [!tip] Intuition
> For each example, binary cross-entropy is the negative logarithm of the probability the model assigned to what actually happened.

## 6. Connect This to Logistic Regression

Logistic regression produces each probability using:

$$
p_i=\sigma(b+wx_i)
$$

where:

$$
\sigma(z)=\frac{1}{1+e^{-z}}
$$

Changing $b$ and $w$ changes the probabilities, which changes the loss.

Training adjusts these weights to reduce the average binary cross-entropy. A regularized implementation adds a penalty on the weights to this objective.

> [!summary] Derivation
> **Assign probabilities to observed outcomes → multiply across examples → take the logarithm → negate → average.**
>
> Before adding regularization, minimizing binary cross-entropy is equivalent to maximizing the likelihood of the observed labels.