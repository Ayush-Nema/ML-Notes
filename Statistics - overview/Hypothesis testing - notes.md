
> **The one idea behind all of it**
> Data is noisy. Before believing a pattern, ask: **"If nothing real were going on, how surprising would my data be?"** Every concept below is one piece of answering that question.

---

## How the pieces connect
$$\text{Hypotheses} \to \text{Test statistic} \to p\text{-value} \to \text{Decision} \to \text{Possible errors}$$

| Stage          | The plain-English question it answers                                     |
| -------------- | ------------------------------------------------------------------------- |
| Hypotheses     | What is the "nothing is going on" claim, and what is the competing claim? |
| Test statistic | How do I boil my data down to one number that measures surprise?          |
| $p$-value      | How rare is a number that extreme _if nothing is going on_?               |
| Decision       | Is it rare enough to stop believing "nothing is going on"?                |
| Errors         | What can go wrong with that decision?                                     |

### How each concept is laid out below
Every concept uses the same sections, so learners always know where they are:
1. **What it is**: the idea in one or two sentences
2. **Intuition**: a story or picture, no math
3. **The math**: the formula, explained in words
4. **Worked example**: small numbers that can be followed by hand
5. **Code**: a few lines of Python
6. **Common confusions**: where learners usually go wrong
7. **Questions to ask**: quick checks of understanding

### Running examples
- **Example A, the coin:** "Is this coin fair?" Used for intuition.
- **Example B, the model:** "A baseline spam filter has 80% accuracy. Our new model got 90 of 100 test emails right. Is it really better, or did we get lucky?" Used for the math and the ML link.
- **Example C, the smoke alarm:** false alarm vs missed fire. Used for the two error types.

### A good way to open the topic
Ask: "I flip a coin 10 times and get **9 heads**. Is it rigged?" Then ask about **6 heads**, then **7**. Learners will feel for themselves where "believable luck" turns into "suspicious". The rest of these notes turn that gut feeling into a precise procedure.


## 1. Null and alternative hypothesis
### What it is
- **Null hypothesis** $H_0$: the default, boring claim. No effect, no difference, "just chance".
- **Alternative hypothesis** $H_1$ (or $H_a$): the claim we suspect might be true. There _is_ an effect.

### Intuition
Think of a courtroom. A defendant is **presumed innocent**, and the court needs strong evidence to convict. In the same way, we **presume "nothing is going on"** and need strong evidence to say otherwise. We never "prove innocence". We either find enough evidence to convict, or we don't.

Why start from the boring claim? Because "nothing is going on" gives us one concrete world in which we can compute probabilities, and it puts the burden of proof on the exciting claim.

### The math

| Situation  | $H_0$           | $H_1$                            |
| ---------- | --------------- | -------------------------------- |
| Coin       | $p = 0.5$       | $p \neq 0.5$ (two-sided)         |
| Spam model | $p = 0.80$      | $p > 0.80$ (one-sided: "better") |
| Two models | $\mu_A = \mu_B$ | $\mu_A \neq \mu_B$               |

- **Two-sided** test: we care about a difference in _either_ direction ($\neq$).
- **One-sided** test: we care about one direction only ($>$ or $<$). This must be decided **before** seeing the data.

### Worked example
Example B: the baseline has accuracy $0.80$ and we want to know if the new model is better.
$$H_0: p = 0.80 \qquad\qquad H_1: p > 0.80$$
$$
z = \frac{\hat{p} - 0.80}{\sqrt{\frac{0.80(1-0.80)}{n}}}
$$
A good way to say it: "We start by assuming our shiny new model is _no better_. Our data has to talk us out of that."

### Code
Nothing to compute yet. A good exercise is to ask the learner to write $H_0$ and $H_1$ for: "Does a new ad increase click-through rate?" 
Answer: $H_0: p_{\text{new}} = p_{\text{old}}$, $H_1: p_{\text{new}} > p_{\text{old}}$.

### Common confusions
- $H_0$ always contains an **equals** sign ($=$, $\le$ or $\ge$). $H_1$ never does.
- We **"reject $H_0$"** or **"fail to reject $H_0$"**. We never say "accept $H_0$", because absence of evidence is not evidence of absence.
- One-sided vs two-sided must be chosen **before** looking at the data. Choosing afterwards is cheating.

### Questions to ask
- "Why do we assume the _boring_ hypothesis?"
- "Write the hypotheses for: does a new recommendation algorithm change average session length?" (Two-sided: $\mu_{\text{new}} \neq \mu_{\text{old}}$.)

## 2. Test statistic
### What it is
A **test statistic** is a single number computed from the data that measures how far the observed result is from what $H_0$ predicts, _in units of expected noise_.

### Intuition
Think of a **surprise meter**. It reads near $0$ when the data is exactly what you'd expect if nothing is going on, and far from $0$ (positive or negative) when the data would be weird in a world where nothing is going on.

The meter must **account for noise**. A 10-point gap in a very noisy measurement is less surprising than a 10-point gap in a very precise one. That is why we divide by the noise.

### The math
Almost every test statistic has the same shape:
$$\text{test statistic} = \frac{\text{observed} - \text{expected under } H_0}{\text{standard error (typical noise)}}$$
This is **signal divided by noise**. Common versions:

| Test                                 | Statistic                                                                 | When used                      |
| ------------------------------------ | ------------------------------------------------------------------------- | ------------------------------ |
| z-test for a proportion              | $z = \dfrac{\hat{p} - p_0}{\sqrt{p_0(1-p_0)/n}}$                          | Yes/no outcomes, large $n$     |
| z-test for a mean ($\sigma$ known)   | $z = \dfrac{\bar{x} - \mu_0}{\sigma/\sqrt{n}}$                            | Numeric data, known spread     |
| t-test for a mean ($\sigma$ unknown) | $t = \dfrac{\bar{x} - \mu_0}{s/\sqrt{n}}$                                 | Numeric data, estimated spread |
| ANOVA                                | $F = \dfrac{\text{between-group variance}}{\text{within-group variance}}$ | 3+ groups                      |
| Chi-squared                          | $\chi^2 = \sum \dfrac{(O-E)^2}{E}$                                        | Counts in categories           |

The test names matter less than the pattern: they are all "observed minus expected, divided by noise". Under $H_0$, a z-statistic follows (approximately) the **standard normal** bell curve, centered at $0$ with typical values within $\pm 2$.

### Worked example (Example B)
New model: $90$ correct out of $n = 100$, so $\hat{p} = 0.90$. Null value: $p_0 = 0.80$.
- Observed $0.90$, expected $0.80$, gap $0.10$.
- Noise: $SE = \sqrt{\dfrac{0.80 \times 0.20}{100}} = \sqrt{0.0016} = 0.04$
- Test statistic:
$$z = \frac{0.90 - 0.80}{0.04} = 2.5$$
Interpretation: the result is **2.5 standard errors** above what $H_0$ predicts.

A rule of thumb worth memorizing: $|z| < 1$ is ordinary, $|z| \approx 2$ is unusual, $|z| > 3$ is very unusual.

### Code
```python
import numpy as np

p0, n = 0.80, 100
p_hat = 90 / n
se = np.sqrt(p0 * (1 - p0) / n)
z = (p_hat - p0) / se
print(z)   # 2.5
```
### Common confusions
- The standard error uses $p_0$ (the **null** value), not $\hat{p}$, because we are computing the noise _in the world where $H_0$ is true_.
- A test statistic is itself **random**: collect a new sample and it changes.
- The same raw gap of $0.10$ gives a **smaller** $z$ with fewer emails, because there is more noise.

### Questions to ask
- "If we had tested on $400$ emails and still got $90\%$ , would $z$ be bigger or smaller than $2.5$?" (Bigger: $SE$ halves, so $z = 5$.)
- "Why divide by the standard error at all?" (To measure the gap relative to typical noise.)


## 3. The p-value
### What it is
$$p\text{-value} = P\big(\text{test statistic at least as extreme as observed} \big| H_0 \text{ is true}\big)$$
In words: the probability of seeing data this extreme **assuming the null is true**.

### Intuition
Imagine running the experiment **thousands of times in a world where $H_0$ is true** (the new model is secretly no better). Each run gives a different $z$. Most land near $0$ and a few land far out. The p-value is the fraction of those runs that look **at least as extreme as yours**.

A small fraction means your result would be a rare fluke in the boring world, which is a reason to doubt the boring world.

### The math
Using the standard normal curve ($Z$ is a standard normal variable):
- **One-sided** (upper): $p = P(Z \ge z_{\text{obs}})$
- **Two-sided**: $p = 2,P(Z \ge |z_{\text{obs}}|)$, because "extreme" can mean far in _either_ tail.

Reference values:

|Test|Cutoff $z$|
|---|---|
|One-sided, $p = 0.05$|$1.645$|
|Two-sided, $p = 0.05$|$1.96$|
|Two-sided, $p = 0.01$|$2.576$|

### Worked examples
**Example A, the coin (exact, no formula needed).** Fair coin ($H_0$), 10 flips, observed 9 heads. "Extreme" means 9 or 10 heads:
$$P(\ge 9 \text{ heads}) = \frac{\binom{10}{9} + \binom{10}{10}}{2^{10}} = \frac{10 + 1}{1024} \approx 0.0107$$
- One-sided $p \approx 0.011$.
- Two-sided (also counting 0 or 1 heads) $p \approx 0.021$.
Both are small, so a fair coin producing this would be rare.

**Example B, the model.** With $z = 2.5$:
- One-sided: $p = P(Z \ge 2.5) \approx 0.0062$
- Two-sided: $p \approx 0.0124$
Our hypothesis is $H_1: p > 0.80$, so we use the one-sided value, $p \approx 0.006$.

Plain English: "If the new model were really no better than the baseline, we'd see a result this good in only about 0.6% of experiments."

### Code
```python
from scipy.stats import binomtest, norm

# Example A: coin
print(binomtest(9, 10, 0.5).pvalue)                         # two-sided ~ 0.0215
print(binomtest(9, 10, 0.5, alternative="greater").pvalue)  # one-sided ~ 0.0107

# Example B: model (z = 2.5)
z = 2.5
print(norm.sf(z))           # one-sided ~ 0.0062
print(2 * norm.sf(abs(z)))  # two-sided ~ 0.0124
```

A simulation that builds strong intuition: when $H_0$ is true, p-values are spread evenly between 0 and 1.
```python
import numpy as np
from scipy.stats import norm

rng = np.random.default_rng(0)
n, p0 = 100, 0.80
x = rng.binomial(n, p0, 10_000)            # a world where H0 is TRUE
z = (x / n - p0) / np.sqrt(p0 * (1 - p0) / n)
p = norm.sf(z)                              # one-sided p-values
print((p < 0.05).mean())                    # about 0.05
```

Takeaway: even when nothing is going on, about 5% of tests give $p < 0.05$. That _is_ the false-alarm rate.

### Common confusions
> _The five most common p-value misunderstandings_
> 1. "$p = 0.03$ means a 3% chance $H_0$ is true." **No.** It is the chance of the _data_ given $H_0$, not the chance of $H_0$ given the data.
> 2. "$p = 0.03$ means a 97% chance the effect is real." **No**, same reason.
> 3. "$p > 0.05$ proves there is no effect." **No.** It only means there isn't enough evidence (perhaps too little data).
> 4. "A small $p$ means a big or important effect." **No.** With huge samples, tiny effects get tiny $p$. Always look at **effect size** too.
> 5. "$p = 0.049$ is a discovery and $p = 0.051$ is nothing." **No.** The 0.05 line is a convention, not a law of nature.

### Questions to ask
- "True or false: if $p = 0.001$, the effect must be large." (False. It is detectable, not necessarily big.)
- "In your own words, what is a p-value _not_?" (It is not the probability that $H_0$ is true.)

## 4. Decision rule and significance level $\alpha$
### What it is
The **significance level** $\alpha$ is the threshold, set in advance, that decides when we reject $H_0$. Common choices are $0.05$, $0.01$ and $0.10$.

### Intuition
A jury needs "beyond reasonable doubt", and that standard is fixed _before_ the trial. We must likewise **choose the doubt threshold before seeing the data**. Otherwise we could pick whatever threshold makes our result look good.

### The math
$$\text{If } p \le \alpha \Rightarrow \text{reject } H_0 \quad(\text{"statistically significant"})$$ $$\text{If } p > \alpha \Rightarrow \text{fail to reject } H_0$$
An equivalent view uses the test statistic: reject when it falls in the **rejection region**, beyond the critical value (for example $z > 1.645$ for a one-sided test at $\alpha = 0.05$).

Importantly:
$$\alpha = P(\text{reject } H_0 \mid H_0 \text{ true}) \quad\text{(the false-alarm rate we are willing to tolerate)}$$
### Worked example (Example B)
With $\alpha = 0.05$: $p \approx 0.006 \le 0.05$, so **reject $H_0$**. In words: "There is statistically significant evidence that the new model's accuracy exceeds 80%."

With a stricter $\alpha = 0.001$: $p \approx 0.006 > 0.001$, so we would **not** reject. Same data, different standard of proof, which is exactly why $\alpha$ must be fixed beforehand.

### Code
```python
alpha = 0.05
p = 0.0062
print("reject H0" if p <= alpha else "fail to reject H0")
```

### Common confusions
- Report the actual $p$-value and the effect size, not just the word "significant".
- Trying several $\alpha$ values or several tests until something passes is p-hacking.
### Questions to ask
- "$p = 0.03$. Do we reject at $\alpha = 0.05$? At $\alpha = 0.01$?" (Yes; no.)
- "Why can't we choose $\alpha$ after seeing the p-value?"


## 5. Type I and Type II errors, and power
### What it is
Our decision is based on random data, so it can be wrong in two distinct ways.

|                             | $H_0$ actually **true** (nothing going on)                            | $H_0$ actually **false** (real effect)                                  |
| --------------------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **We reject $H_0$**         | **Type I error** (false alarm / false positive), probability $\alpha$ | Correct. Probability = **power**, $1-\beta$                             |
| **We fail to reject $H_0$** | Correct                                                               | **Type II error** (missed effect / false negative), probability $\beta$ |

### Intuition (the smoke alarm)
A smoke alarm can fail in two ways:
- **False alarm:** it screams but there is no fire (annoying).
- **Missed fire:** there is a fire but it stays silent (dangerous).

Making the alarm more sensitive reduces missed fires but causes more false alarms. **You cannot push both errors to zero** with a fixed amount of data.

### The math
$$\alpha = P(\text{Type I}) = P(\text{reject } H_0 \mid H_0 \text{ true})$$ $$\beta = P(\text{Type II}) = P(\text{fail to reject } H_0 \mid H_0 \text{ false})$$ $$\text{Power} = 1 - \beta = P(\text{reject } H_0 \mid H_0 \text{ false})$$

What controls each:
- $\alpha$: **you choose it** directly.
- Power goes up when the true effect is **bigger**, the sample size $n$ is **bigger**, the noise is **smaller**, or $\alpha$ is **larger**.
- The classic tradeoff: lowering $\alpha$ (fewer false alarms) raises $\beta$ (more misses) unless you also raise $n$.

**ML translation:** Type I is a **false positive** and Type II is a **false negative**. It is the same tension as precision vs recall.

### Worked example (Example B, power)
Setup: baseline $p_0 = 0.80$, $\alpha = 0.05$ one-sided, $n = 100$. Suppose the new model is **truly** $85%$ accurate, a real improvement. How likely are we to detect it?

1. Under $H_0$, we reject when $\hat{p} > p_0 + 1.645 \times 0.04 \approx 0.866$.
2. If the truth is $0.85$, the typical spread of $\hat{p}$ is $\sqrt{0.85 \times 0.15/100} \approx 0.0357$.
3. The chance that $\hat{p}$ exceeds $0.866$ is only about $33%$.

So **power $\approx 0.33$** and $\beta \approx 0.67$. Even though the model really is better, we would **miss it about two times out of three**.

With $n = 400$ emails, the cutoff tightens to about $0.833$ and the noise shrinks, so power rises to roughly $0.8$.

The lesson: "not significant" often just means _underpowered_. More data is how you reduce Type II errors without raising Type I errors.

### Code
Simulating both error types by brute force makes them concrete.
```python
import numpy as np
from scipy.stats import norm

rng = np.random.default_rng(0)
p0 = 0.80
alpha = 0.05

def reject_rate(true_p, n, reps=10_000):
    x = rng.binomial(n, true_p, reps)
    z = (x / n - p0) / np.sqrt(p0 * (1 - p0) / n)
    return (norm.sf(z) < alpha).mean()

print("Type I rate (H0 true, n=100):", reject_rate(0.80, 100))   # about 0.05
print("Power (true=0.85, n=100):   ", reject_rate(0.85, 100))    # about 0.33
print("Power (true=0.85, n=400):   ", reject_rate(0.85, 400))    # about 0.8
```

Exact numbers wobble a little because the data is discrete and random.

### Common confusions
- Neither error is automatically "worse". It depends on the cost:
    - Spam filter blocking real email, or approving an ineffective drug: Type I (false positive) may be costly.
    - Cancer screening, fraud detection: Type II (missed case) may be far more costly, so we accept more false alarms.
- Lowering $\alpha$ alone is **not** free, because it lowers power.
- Running many tests inflates Type I errors: 20 independent tests at $\alpha = 0.05$ give $1 - 0.95^{20} \approx 64\%$ chance of at least one false alarm.
- Power should be planned **before** collecting data.

### Questions to ask
- "A medical test is tuned to catch nearly every sick patient. Which error did we shrink, and what did we pay?" (Shrank Type II, paid with more Type I.)
- "You lower $\alpha$ from $0.05$ to $0.01$ with the same data. What happens to Type II error?" (It increases.)

## Putting it all together
1. State $H_0$ ("nothing is going on") and $H_1$.
2. Choose $\alpha$, and plan $n$ for decent power, **before** seeing data.
3. Compute the **test statistic**: $\dfrac{\text{observed} - \text{expected under } H_0}{\text{noise}}$.
4. Convert it to a **p-value**: how rare is this _if $H_0$ were true_?
5. **Decide:** reject $H_0$ if $p \le \alpha$.
6. Remember we may still be wrong: **Type I** (rate $\alpha$) or **Type II** (rate $\beta$).

### Concept cheat table

|Term|One-line meaning|
|---|---|
|Null hypothesis $H_0$|"Nothing is going on" default claim|
|Alternative $H_1$|The effect we suspect|
|Test statistic|Observed minus expected, in units of noise|
|p-value|Chance of data this extreme _if $H_0$ is true_|
|$\alpha$|Pre-chosen false-alarm rate / evidence threshold|
|Type I error|Reject a true $H_0$ (false positive)|
|Type II error|Miss a real effect (false negative), rate $\beta$|
|Power $1-\beta$|Chance of detecting a real effect|

### Three takeaways for learners
1. A p-value is about the **data given the null**, not the probability that the null is true.
2. **Small p does not mean a big effect**, and **large p does not mean no effect**.
3. Error rates trade off, and **more data** is the way to improve both at once.

## Formula quick reference

$$\text{test statistic} = \frac{\text{observed} - \text{expected under } H_0}{\text{standard error}}$$ $$z = \frac{\hat{p} - p_0}{\sqrt{p_0(1-p_0)/n}} \qquad t = \frac{\bar{x} - \mu_0}{s/\sqrt{n}}$$ $$p_{\text{one-sided}} = P(Z \ge z_{\text{obs}}) \qquad p_{\text{two-sided}} = 2,P(Z \ge |z_{\text{obs}}|)$$ $$\alpha = P(\text{reject } H_0 \mid H_0 \text{ true}) \qquad \beta = P(\text{fail to reject} \mid H_0 \text{ false}) \qquad \text{Power} = 1-\beta$$