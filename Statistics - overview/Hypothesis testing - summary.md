# 1. Test statistic
Think of a **surprise meter**. It reads near $0$ when the data is exactly what you'd expect if nothing is going on, and far from $0$ (positive or negative) when the data would be weird in a world where nothing is going on.

The meter must **account for noise**. A 10-point gap in a very noisy measurement is less surprising than a 10-point gap in a very precise one. That is why we divide by the noise.

Almost every test statistic has the same shape:
$$\text{test statistic} = \frac{\text{observed} - \text{expected under } H_0}{\text{standard error (typical noise)}}$$

| Test                                 | Statistic                                                                 | When used                      |
| ------------------------------------ | ------------------------------------------------------------------------- | ------------------------------ |
| z-test for a proportion              | $z = \dfrac{\hat{p} - p_0}{\sqrt{p_0(1-p_0)/n}}$                          | Yes/no outcomes, large $n$     |
| z-test for a mean ($\sigma$ known)   | $z = \dfrac{\bar{x} - \mu_0}{\sigma/\sqrt{n}}$                            | Numeric data, known spread     |
| t-test for a mean ($\sigma$ unknown) | $t = \dfrac{\bar{x} - \mu_0}{s/\sqrt{n}}$                                 | Numeric data, estimated spread |
| ANOVA                                | $F = \dfrac{\text{between-group variance}}{\text{within-group variance}}$ | 3+ groups                      |
| Chi-squared                          | $\chi^2 = \sum \dfrac{(O-E)^2}{E}$                                        | Counts in categories           |


# 2. p-value
$$p\text{-value} = P\big(\text{test statistic at least as extreme as observed} \big| H_0 \text{ is true}\big)$$
A beginner-friendly definition:
> _The p-value is the probability of seeing a result at least as extreme as the one we observed, assuming the null hypothesis is true._

That last part matters: $\boxed{\text{assuming }H_0\text{ is true}}$
For example: 
$p=0.03$ means:
> _If $H_0$ were true, results this extreme or more extreme would happen about 3% of the time._

It does **not** mean:
> _“There is a 3% probability that $H_0$ is true.”_

$$\boxed{p < \alpha \Rightarrow \text{Reject } H_0}$$
$$\boxed{p \geq \alpha \Rightarrow \text{Fail to reject } H_0}$$
For example:
$p=0.02$
and:
$\alpha=0.05$

Since $0.02 < 0.05$
we reject $H_0$.


# 3. Error chart: Type-I and Type-II error

| Reality     | Reject $H_0$            | Fail to reject $H_0$    |
| ----------- | ----------------------- | ----------------------- |
| $H_0$ true  | Type I error $(\alpha)$ | Correct                 |
| $H_0$ false | Correct detection       | Type II error $(\beta)$ |

| Statistical term | ML analogy     |
| ---------------- | -------------- |
| Type I error     | False positive |
| Type II error    | False negative |

### Example: Cancer Detection Test
Assume:
- $H_0$: Patient does not have cancer.
- $H_1$: Patient has cancer.

|                                | Reality    | Test Result         |
| ------------------------------ | ---------- | ------------------- |
| Type I Error (False Positive)  | No cancer  | Test says cancer    |
| Type II Error (False Negative) | Has cancer | Test says no cancer |

Intuition:
- Type I: False alarm → Unnecessary anxiety and further testing.
- Type II: Missed detection → Cancer goes undetected, potentially delaying treatment.

Remember:
- $\alpha$ = Probability of Type I error.
- $\beta$ = Probability of Type II error.
- $(1-\beta)$ = Statistical power (probability of detecting cancer when present, in this simplified example).


# 4. Steps of Hypothesis Testing
1. State the hypotheses
- $H_{0}$: Null hypothesis (no improvement)
- $H_1$: Alternative hypothesis (improvement)
Example:
$H_0:p=0.80,\qquad H_1:p>0.80$

2. Choose significance level ($\alpha$)
Usually: $\alpha=0.05$
This is the threshold for rejecting $H_0$.

3. Calculate the test statistic
For a one-proportion z-test:
$$z=\frac{\hat p-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}$$
It measures how far the observed estimate is from $H_0$, in standard errors.

4. Calculate the p-value
For our right-tailed test:
$$p\text{-value}=P(Z\ge z_{\text{observed}}\mid H_0)$$
It measures how surprising our result (or something more extreme) would be if $(H_0)$ were true.

5. Make a decision
$$\boxed{ \begin{cases} p\text{-value}\le\alpha &\Rightarrow \text{Reject }H_0\\ p\text{-value}>\alpha &\Rightarrow \text{Fail to reject }H_0 \end{cases} }$$
6. Interpret the result
- Reject $H_0$ → Sufficient statistical evidence of improvement.
- Fail to reject $H_0$ → Insufficient evidence of improvement.

Failing to reject $H_0$ does not mean $H_0$ is true.




