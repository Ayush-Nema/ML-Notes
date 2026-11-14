> We observe a sample result → ask whether it could reasonably happen by chance → quantify that evidence → make a decision about a claim.

# 1. A realistic business question
Suppose an e-commerce company changes the checkout page.

Historically, the conversion rate is: $10\%$
After the redesign, you test it on 1,000 users and observe: 120 purchases

So the new observed conversion rate is: $\hat p = \frac{120}{1000}=12\%$

Ask the class:
> _Did the redesign really improve conversion?_

At first glance: $12\% > 10\%$
So it looks better.

But then ask:
> *Could we have seen 12% just because of random variation?*
That is the motivation for hypothesis testing.

# 2. What is hypothesis testing?
> _Hypothesis testing is a statistical procedure used to decide whether sample evidence is strong enough to challenge a claim about a population._

Or even more simply:
> _Is the difference I observed likely to be real, or could it reasonably have happened by chance?_

This is the core idea.

# 3. Why do we need it?
Because sample results naturally vary.

Suppose the true conversion rate really is 10%.
If you repeatedly sample 1,000 users, you might observe: 
$$[ 9.4\%,\;10.2\%,\;10.7\%,\;9.8\%,\;11.1\% ]$$
even though the true rate never changed.
This is **sampling variability**.

Therefore:
> _Seeing a difference does not automatically mean a real effect exists._

This is one of the most important ideas to teach.

# 4. Null hypothesis and alternative hypothesis
The two competing claims.
## → Null hypothesis
Written:
$$H_0$$
Usually represents:
> _no change / no effect / status quo._

For our conversion example:
$$H_0:p=0.10$$
Meaning:
> _The redesign did not truly improve conversion; the population conversion rate remains 10%._

## → Alternative hypothesis
Written:
$$H_1$$
or:
$$H_a$$
Represents what we want evidence for.

If we're specifically testing improvement:
$$H_1:p>0.10$$
Meaning:
> _The new conversion rate is higher than 10%._

This gives a simple framing:
$H_0$ = business as usual
$H_1$ = something changed

# 5. A courtroom analogy
Think of $H_0$
as:
> _Defendant is innocent._

And: $H_1$
as:
> _Defendant is guilty._

You don't assume guilt and ask someone to prove innocence.
You start with the null assumption.
> _Is the evidence strong enough to reject that assumption?_

Similarly in statistics:
> We start by assuming $H_0$ is true.
Then examine whether the observed data would be unusual under that assumption.

Important wording:
> We _reject_ or *fail to reject* $H_0$.
We usually do not say:
> “We proved $H_0$ true.”

*Because lack of evidence against a claim is not the same as proof that it is correct.*

# 6. The central question
Everything in hypothesis testing comes down to:
> _If $H_0$ were true, how surprising would my observed result be?_

That question creates the need for:
- test statistic
- p-value

# 7. Build intuition before introducing formulas
Suppose a coin is claimed to be fair.
So:
$$H_0:p=0.5$$
You flip it 10 times
and get 6 heads
_Would you conclude the coin is biased?_
Probably not.
Six heads out of ten seems plausible by chance.

Now suppose you flip it 100 times
and get 95 heads
_Now it seems extremely unlikely for a fair coin._

Same logic:
> _The more inconsistent the data is with $H_0$, the stronger the evidence against $H_0$._

# 8. Test statistic
A test statistic measures:
> _How far is my observed result from what $H_0$ predicts, relative to normal sampling variation?_

For many tests, conceptually:
$$\text{Test statistic} = \frac{\text{Observed estimate} - \text{Null value}} {\text{Standard error}}$$
This structure should feel familiar from z-scores.

Example:
Suppose we test a population mean.
$$H_0:\mu=100$$
We observe:
$$\bar x=105$$
and the standard error is (let's say): $SE=2$
Then:
$$z= \frac{105-100}{2} = 2.5$$
Interpretation:
> _Our observed sample mean is 2.5 standard errors above the value predicted by the null hypothesis._
The larger the absolute test statistic, the more unusual the result under $H_0$.

# 9. The p-value
This is where students often get lost, so keep it conceptual.

A beginner-friendly definition:
> _The p-value is the probability of seeing a result at least as extreme as the one we observed, assuming the null hypothesis is true._

That last part matters: $\boxed{\text{assuming }H_0\text{ is true}}$
For example: 
$p=0.03$ means:
> _If $H_0$ were true, results this extreme or more extreme would happen about 3% of the time._

It does **not** mean:
> _“There is a 3% probability that $H_0$ is true.”_

That is a major misconception.

# 10. Make p-value visual
Imagine under $H_0$, sample means follow something like:
```
                    *
                 *******
              *************
           *******************
---------|---------|---------|---------
       95        100        105
                   ^
             H0 predicts here
```
Suppose your observed mean is way out at: $107$
The p-value corresponds to the tail area at least as extreme as 107.

If very little probability lies out there:
> *your observation is hard to reconcile with $H_0$.*

# 11. Significance level
Before seeing the data, we choose a threshold: $(\alpha)$ called the **significance level**.

Commonly:
$\alpha=0.05$

Then the decision rule is:
$$\boxed{p < \alpha \Rightarrow \text{Reject } H_0}$$
$$\boxed{p \geq \alpha \Rightarrow \text{Fail to reject } H_0}$$
For example:
$p=0.02$
and:
$\alpha=0.05$

Since $0.02 < 0.05$
we reject $H_0$.

# 12. What does 0.05 mean?
Be careful here.
It does not mean:
> _“95% probability that the alternative hypothesis is true.”_

It means we are using a decision procedure that tolerates a particular false-positive rate under repeated testing.
This connects to Type I error.

# 13. Type I error
A Type I error happens when:
> _$H_0$ is actually true, but we reject it._

This is a **false positive**.

Example:
A new feature actually does **not** improve conversion.
But your experiment concludes that it does.
You launch it based on a false signal.
Probability of a Type I error is controlled by: $\alpha$
So if: $\alpha=0.05$
you're accepting a 5% long-run false-positive rate under the null model.

# 14. Type II error
A Type II error happens when:
> _$H_0$ is false, but we fail to reject it._

This is a **false negative**.

Example:
The redesign genuinely improves conversion.
But your test fails to detect the improvement.
We often denote this probability by: $\beta$
And: $(1-\beta)$
is called the **power** of the test.

# 15. A simple error table

| Reality       | Reject $H_0$    | Fail to reject $H_0$ |
| ------------- | ----------------- | ---------------------- |
| $H_0$ true  | Type I error      | Correct                |
| $H_0$ false | Correct detection | Type II error          |

Then translate:

| Statistical term | ML analogy     |
| ---------------- | -------------- |
| Type I error     | False positive |
| Type II error    | False negative |

# 16. One-tailed vs two-tailed tests
Suppose baseline conversion is: $10\%$

If your question is:
> _Did conversion **increase**?_
Then:
$$H_1:p>0.10$$
That's a **one-sided / one-tailed test**.

But if your question is:
> _Did conversion **change** in either direction?_
Then:
$$H_1:p\neq0.10$$
That's a **two-sided / two-tailed test**.

Simple rule:
-  One-tailed: You care about a specific direction i.e. $>, <$
- Two-tailed: Any difference matters i.e. $\neq$

# 17. A complete worked example
Use something very tangible.

### Problem
A coffee company claims its packets contain an average of: 500g
A quality manager suspects the filling machine may be underfilling.

She samples: $n=36$ packets.
Sample mean: $\bar{x}=496g$

Population standard deviation is known: $\sigma=12g$
Test at: $\alpha=0.05$

### Step 1: Define hypotheses
We're asking whether packets are underfilled.
So:
  $$H_0:\mu=500$$    $$H_1:\mu<500$$

This is a one-tailed test.
### Step 2: Calculate standard error
  $$SE= \frac{\sigma}{\sqrt n}$$
So:
  $$SE= \frac{12}{\sqrt{36}} = \frac{12}{6} = 2$$

### Step 3: Compute test statistic
  $$z= \frac{\bar{x}-\mu_0}{SE}$$    $$z= \frac{496-500}{2}$$    $$z=-2$$
Interpretation:
> _The sample mean is 2 standard errors below the null value._

### Step 4: Find p-value
For: $z=-2$  
the one-tailed p-value is approximately: $p\approx0.0228$

### Step 5: Compare with alpha
  $$0.0228 < 0.05$$
So:
  $$\boxed{\text{Reject }H_0}$$
Conclusion:
> _There is statistically significant evidence that the true average packet weight is below 500g._

Notice the wording.
Not:
> _“We have proven the packets weigh less than 500g.”_

Use:
> _“There is evidence…”_

# 18. A useful five-step framework

### Step 1 — State the question
What population claim are we testing?
### Step 2 — Define hypotheses
$H_0$ and $H_1$
### Step 3 — Compute test statistic
How far is the observed estimate from the null value?
### Step 4 — Calculate p-value
How surprising is the observed result if $H_0$ is true?
### Step 5 — Make decision
Compare: $p$ with $\alpha$  
Then interpret in business language.

# 19. Give them the mental model
```
Start by assuming H0 is true
          ↓
What would samples normally look like?
          ↓
Observe actual sample
          ↓
How unusual is it under H0?
          ↓
Calculate p-value
          ↓
     p < α ?
      /    \
    Yes     No
     |       |
 Reject    Fail to reject
   H0          H0
```

# 20. Connect hypothesis testing to confidence intervals
Suppose:
  $$H_0:\mu=100$$
and your 95% confidence interval is: $[102,\;108]$
Does: 100 lie inside the interval?
No.
That suggests the null value is inconsistent with the data at the 5% significance level for a corresponding two-sided test.
But if the interval were: $[98,\;108]$
then: 100 is plausible.

So for a corresponding two-sided test:
> _If the null value lies outside the 95% CI, you typically reject $H_0$ at $\alpha=0.05$._

This makes CI and hypothesis testing feel like two views of the same inferential idea.

# 21. Statistical significance is not practical significance
This is extremely important for business and ML students.
Suppose an e-commerce company has: 10,000,000 users.

Conversion increases from: $10.00\%$ to: $10.03\%$  
With millions of observations, that tiny difference could easily become statistically significant.

You might get: $p<0.001$
But ask:
> _Is a 0.03 percentage-point improvement worth the engineering effort, product complexity, and maintenance cost?_
Maybe not.

So:
> _Statistical significance asks whether an effect is unlikely to be due to random variation._
> _Practical significance asks whether the effect is large enough to matter._

# 22. Sample size matters
Suppose two experiments observe the same improvement: $10\%\rightarrow12\%$
Experiment A: $n=50$
Experiment B: $n=50,000$
Same observed difference.
But B provides much stronger evidence.
Why?

Larger sample $\rightarrow$ smaller standard error $\rightarrow$ more precise estimate $\rightarrow$ larger test statistic for the same effect $\rightarrow$ smaller p-value.

# 23. ML-related example: comparing two models
Suppose you've trained:
- Model A
	- Accuracy: $84.5\%$
- Model B
	- Accuracy: $85.2\%$

Can we immediately say Model B is better?
No.

The test-set result is based on a sample.

The observed: $0.7\%$ 
difference may partly reflect sampling variation.

Hypothesis testing asks something like:
$$H_0: \text{performance difference}=0$$
$$H_1: \text{performance difference}\neq0$$

You would then use an appropriate statistical test depending on the experimental setup.

For example:
- paired tests,
- McNemar's test for paired classification predictions,
- bootstrap or permutation procedures,
- tests across repeated runs/folds.

The key insight is:
> _A slightly higher ML metric does not automatically mean the model is genuinely better._

# 24. Another very relatable example: A/B testing
A/B testing is essentially hypothesis testing in business clothes.

Suppose:
→ Version A
1,000 users
100 convert $(10\%)$  

→ Version B
1,000 users
120 convert $(12\%)$

We test:
$$H_0:p_A=p_B$$
against:
$$H_1:p_A\neq p_B$$
or perhaps:
  $$H_1:p_B>p_A$$
Then ask:
> _Is the observed 2-point difference larger than we would reasonably expect from sampling variability?_
That is hypothesis testing.

# 25. Common misconceptions slide
##### 1. Wrong
> _$p=0.03$ means there is a 3% chance $H_0$ is true._
##### 1. Correct
> _Assuming $H_0$ is true, there is about a 3% chance of seeing data at least this extreme._

##### 2. Wrong
> $p>0.05$ proves $H_0$ is true.
##### 2. Correct
> _We don't have strong enough evidence to reject $H_0$._

##### 3. Wrong
> _p<0.05 means the effect is important._
##### 3. Correct
> _It suggests evidence against $H_0$; the effect size may still be tiny._

##### 4. Wrong
> $0.05$ is a magical threshold.
##### 4. Correct
> $\alpha$ is a decision threshold chosen according to the context and error costs.

# 26. Give students quick scenarios
Ask them to formulate $H_0$ and $H_1$.
#### Scenario 1
> _A delivery company claims average delivery time is 30 minutes. You suspect it is longer._
  $$H_0:\mu=30$$
  $$H_1:\mu>30$$
#### Scenario 2
> _A machine is supposed to fill bottles with 1 litre. You want to know if calibration has changed._
  $$H_0:\mu=1$$
  $$H_1:\mu\neq1$$
#### Scenario 3
> _A new recommendation algorithm is expected to increase CTR._
  $$H_0:p_{\text{new}}\le p_{\text{old}}$$
Conceptually, no improvement.
  $$H_1:p_{\text{new}}>p_{\text{old}}$$

# 27. The one diagram I'd keep returning to
```
Business claim
      ↓
Define H0 and H1
      ↓
Collect a random sample
      ↓
Calculate sample statistic
      ↓
Assume H0 is true
      ↓
How unusual is the result?
      ↓
Test statistic
      ↓
p-value
      ↓
Compare p with α
      ↓
Statistical decision
      ↓
Business interpretation
```

# 28. Final recap
### What is hypothesis testing?
A framework for evaluating claims about a population using sample data.

### Why do we need it?
Because samples vary randomly.

### What is $H_0$?
The baseline / no-effect claim.

### What is the p-value?
How unusual our result is under $H_0$.

### What does $p<0.05$ mean?
The result is sufficiently unusual under $H_0$ according to our chosen threshold.


