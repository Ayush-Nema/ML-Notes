## How the pieces connect

$$\text{Scatter plot} \to \text{Correlation idea} \to \text{Pearson } r \to \text{Where } r \text{ fails} \to \text{Spearman } \rho \to \text{Which to use} \to \text{Cautions and ML uses}$$

|Stage|The plain-English question it answers|
|---|---|
|Correlation idea|Do two variables move together, and how tightly?|
|Pearson $r$|How well do the points follow a **straight line**?|
|Where $r$ fails|What kinds of data fool Pearson?|
|Spearman $\rho$|How well do the points follow **any consistently rising or falling pattern**?|
|Which to use|How do I choose for my data?|
|Cautions|Does a strong correlation mean one thing _causes_ the other?|

### How each concept is laid out below
Every concept uses the same sections, so learners always know where they are:
1. **What it is**: the idea in one or two sentences
2. **Intuition**: a story or picture, no math
3. **The math**: the formula, explained in words
4. **Worked example**: small numbers that can be followed by hand
5. **Code**: a few lines of Python
6. **Common confusions**: where learners usually go wrong
7. **Questions to ask**: quick checks of understanding

### Running examples (reuse them so learners don't absorb new stories each time)
- **Example A, study hours vs quiz score:** a clean, believable positive relationship. Used for Pearson.
- **Example B, months since launch vs daily signups:** growth that is rising but not a straight line, with one big value. Used to show why Spearman exists.
- **Example C, one wild data point:** a single typo or glitch in otherwise good data. Used to show robustness.

### A good way to open the topic
Ask: 
"Do taller people tend to weigh more? 
Do people who sleep more tend to score higher on tests? 
Does ice cream consumption have anything to do with sunburns?" 
Let learners answer in their own words ("tends to", "usually", "not always"). 
Then say: _correlation is the precise way to say "tends to", with a number attached._ Draw three scatter plots on the board (upward cloud, downward cloud, shapeless blob) and ask which one has the "strongest relationship".

## 1. What correlation is
### What it is
A **correlation coefficient** is a number between $-1$ and $+1$ describing how two variables move together.

|Value|Meaning|
|---|---|
|$+1$|Perfect positive: when one goes up, the other always goes up in a perfectly predictable way|
|$0$|No relationship of the kind being measured|
|$-1$|Perfect negative: when one goes up, the other always goes down|

Two things are packed into the number:
- **Sign** tells the **direction** (up together, or opposite).
- **Size (distance from 0)** tells the **strength** (how tightly the points hug the pattern).

### Intuition
Picture a scatter plot, with one dot per person (hours studied on the x-axis, quiz score on the y-axis).
- Dots forming a tight band rising to the right: strong positive.
- Dots forming a tight band falling to the right: strong negative.
- Dots all over the page: close to zero.
- A wide, fuzzy band rising to the right: positive but weak.

Correlation is the **tightness of the band**, with a sign for the tilt. It says nothing about **how steep** the band is (that is the slope, a different idea).

### The math
Rough, field-dependent guide for the absolute value of the coefficient (say it out loud as a _rule of thumb_, not a law):

|Size of the coefficient|Typical description|
|---|---|
|0.0 to 0.2|negligible or very weak|
|0.2 to 0.4|weak|
|0.4 to 0.6|moderate|
|0.6 to 0.8|strong|
|0.8 to 1.0|very strong|

What counts as "strong" depends on the field. $0.3$ can be exciting in psychology and disappointing in physics.

### Worked example
Four quick cases for the board:

|Pair of variables|Likely correlation|
|---|---|
|Hours studied and quiz score|Positive, moderate to strong|
|Hours of TV per day and exam score|Negative, weak to moderate|
|Shoe size and favorite color|About 0|
|Temperature and heating bill|Negative, strong|
### Code
```python
import numpy as np
import matplotlib.pyplot as plt

rng = np.random.default_rng(0)
x = rng.normal(size=200)

fig, axes = plt.subplots(1, 4, figsize=(14, 3))
for ax, (r_true, title) in zip(axes, [(0.95, "strong +"), (0.5, "weak +"), (0.0, "none"), (-0.8, "strong -")]):
    y = r_true * x + np.sqrt(1 - r_true**2) * rng.normal(size=200)
    ax.scatter(x, y, s=8)
    ax.set_title(f"{title}  (r = {np.corrcoef(x, y)[0, 1]:.2f})")
plt.tight_layout()
plt.show()
```
Having learners _see_ several correlation values is the fastest way to build the intuition.

### Common confusions
- Correlation is **not** slope. A flat-looking band that is very tight can still have $r$ near $1$.
- A correlation of $0$ does **not** mean "no relationship at all". It means no relationship of the _measured type_ (more on this below).
- The sign is about direction only. $-0.9$ is a **stronger** relationship than $+0.3$.

### Questions to ask
- "Which is stronger, $r = -0.8$ or $r = +0.5$?" (The first. Strength is the size, sign is direction.)
- "Can you think of two things in your life that are negatively correlated?"

## 2. Pearson correlation
### What it is
The **Pearson correlation coefficient** $r$ measures how well the data follows a **straight line**. It is the default correlation people mean when they just say "correlation".

### Intuition
Start from the averages. Draw a vertical line at the mean of $x$ and a horizontal line at the mean of $y$. This splits the scatter plot into four quadrants.
- A point in the **top-right** or **bottom-left** quadrant is "above average in both" or "below average in both". These points **vote positive**.
- A point in the **top-left** or **bottom-right** is "above in one, below in the other". These points **vote negative**.

Pearson is basically the **average vote**, with bigger votes from points far from the center. If almost all points vote positive, $r$ is near $+1$. If the votes cancel out, $r$ is near $0$.

Another way to see it: convert each variable to **z-scores** ("how many standard deviations above or below average"), multiply the pair for each point, and average. Same thing, scale removed.
 cvxc### The math
For each point, how far is it from the averages? Write $\bar{x}$ and $\bar{y}$ for the means.

$$r = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n} (x_i - \bar{x})^2};\sqrt{\sum_{i=1}^{n} (y_i - \bar{y})^2}}$$

In words:
- **Top:** for each point, multiply how far $x$ is from its mean by how far $y$ is from its mean, then add them up. Same-direction points give positive products, opposite-direction points give negative ones. This is the quadrant vote.
- **Bottom:** divide by the overall spread of $x$ and of $y$, so the answer doesn't depend on units (hours vs minutes, dollars vs cents) and always lands in $[-1, 1]$.

Equivalent forms worth showing once:
$$r = \frac{\text{cov}(X, Y)}{s_X , s_Y} \qquad\qquad r = \frac{1}{n-1}\sum_{i=1}^{n} z_{x_i}, z_{y_i}$$
Here $\text{cov}(X,Y) = \frac{1}{n-1}\sum (x_i-\bar{x})(y_i-\bar{y})$ is the **covariance** (the raw quadrant vote, which depends on units) and $s_X, s_Y$ are the standard deviations.

Useful properties:
- $-1 \le r \le 1$.
- Changing units (multiplying by a positive number, adding a constant) does **not** change $r$.
- Swapping $x$ and $y$ does **not** change $r$.
- Flipping the sign of one variable flips the sign of $r$.

### Worked example (Example A)
Five students. $x$ = hours studied, $y$ = quiz score out of 5.

| Student | $x$ (hours) | $y$ (score) | $x - \bar{x}$ | $y - \bar{y}$ | Product | $(x-\bar{x})^2$ | $(y-\bar{y})^2$ |
| ------- | ----------- | ----------- | ------------- | ------------- | ------- | --------------- | --------------- |
| 1       | 1           | 2           | $-2$          | $-2$          | $4$     | $4$             | $4$             |
| 2       | 2           | 4           | $-1$          | $0$           | $0$     | $1$             | $0$             |
| 3       | 3           | 5           | $0$           | $1$           | $0$     | $0$             | $1$             |
| 4       | 4           | 4           | $1$           | $0$           | $0$     | $1$             | $0$             |
| 5       | 5           | 5           | $2$           | $1$           | $2$     | $4$             | $1$             |
| **Sum** |             |             |               |               | **6**   | **10**          | **6**           |

Means: $\bar{x} = 3$ and $\bar{y} = 4$.
$$r = \frac{6}{\sqrt{10},\sqrt{6}} = \frac{6}{\sqrt{60}} \approx 0.775$$
Interpretation: strong positive. More study hours go with higher scores, though not perfectly (student 4 studied more than student 3 and scored lower).

### Code
```python
import numpy as np
from scipy import stats

hours = np.array([1, 2, 3, 4, 5])
score = np.array([2, 4, 5, 4, 5])

r, p = stats.pearsonr(hours, score)
print(r)   # 0.7746
print(p)   # 0.124

# the same thing with numpy
print(np.corrcoef(hours, score)[0, 1])   # 0.7746
```

Note the p-value of $0.124$. With only 5 points, even $r \approx 0.77$ is not statistically significant. See the section on small samples below.

### Common confusions
- Pearson measures **linear** association only. A perfect curve can score low.
- It is **sensitive to outliers** (next section).
- It assumes both variables are numeric (not categories), and the usual significance test assumes roughly bell-shaped data.
- A correlation is computed from a **sample**, so it is itself noisy. Always think about how many points you have.

### Questions to ask
- "If I measure hours in minutes instead, does $r$ change?" (No, units cancel.)
- "If $r = 0.775$, can I say 77.5% of the scores are explained by hours?" (No. The share of variation explained is $r^2 \approx 0.6$, and "explained" is not "caused".)
- "What would the quadrant votes look like if $r$ were near $-1$?" (Almost all points in the top-left and bottom-right.)

## 3. Where Pearson breaks (the bridge to Spearman)
### What it is
Three situations where the Pearson coefficient gives a misleading or low-quality summary of a real relationship.

### Intuition
Pearson asks "how close are the points to a **straight line**?" So anything that is _not_ a straight line, or any point that sits far from the pack, can distort the answer.

### Case 1: a curved relationship that always goes up
Example B: signups doubling every month. Compute on $x = 1, \dots, 8$ and $y = e^{x}$:

|Measure|Value|
|---|---|
|Pearson $r$|about $0.78$|
|Spearman $\rho$|exactly $1.00$|

The relationship is **perfectly predictable** (higher $x$ always means higher $y$), but not a straight line, so Pearson is not $1$.

### Case 2: one wild point (Example C)
Take clean data: $x = 1 \ldots 8$ and $y = 2, 3, 3, 4, 5, 5, 6, 7$. Then replace the last $y$ with a bad value, $-20$ (a sensor glitch or typo).

|                 | Clean data | With one bad point |
| --------------- | ---------- | ------------------ |
| Pearson $r$     | $0.99$     | $-0.45$            |
| Spearman $\rho$ | $0.99$     | $0.31$             |

One point flipped Pearson from "nearly perfect" to **negative**. Spearman was hurt too, but far less, because it only cares about the _rank_ of the bad value, not how extreme it is.

### Case 3: a relationship that is not one-directional
$y = x^2$ for $x = -5, \dots, 5$ is a perfect, deterministic relationship, yet:
$$r = 0 \qquad \rho = 0$$
Both coefficients miss it, because the relationship goes down and then up. **Neither Pearson nor Spearman can detect non-monotonic patterns.** This is why you should always **plot the data**.

### Anscombe's quartet (great classroom moment)
Four small datasets with almost identical Pearson correlation ($r \approx 0.816$) but completely different shapes: a clean line, a smooth curve, a line with one outlier, and a vertical stack with one leverage point. The point: **the number alone can hide the story.**

### Code
```python
import numpy as np
from scipy import stats

# Case 1: always rising, not linear
x = np.arange(1, 9)
y = np.exp(x)
print(stats.pearsonr(x, y)[0])    # ~0.776
print(stats.spearmanr(x, y)[0])   # 1.0

# Case 2: one bad point
y_clean = np.array([2, 3, 3, 4, 5, 5, 6, 7])
y_bad = y_clean.copy(); y_bad[-1] = -20
print(stats.pearsonr(x, y_clean)[0], stats.pearsonr(x, y_bad)[0])     # 0.986, -0.448
print(stats.spearmanr(x, y_clean)[0], stats.spearmanr(x, y_bad)[0])   # 0.988, 0.313

# Case 3: U-shape
xu = np.arange(-5, 6)
print(stats.pearsonr(xu, xu**2)[0])   # ~0
```

Anscombe's quartet, first two datasets:
```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

x  = np.array([10, 8, 13, 9, 11, 14, 6, 4, 12, 7, 5])
y1 = np.array([8.04, 6.95, 7.58, 8.81, 8.33, 9.96, 7.24, 4.26, 10.84, 4.82, 5.68])
y2 = np.array([9.14, 8.14, 8.74, 8.77, 9.26, 8.10, 6.13, 3.10, 9.13, 7.26, 4.74])

fig, axes = plt.subplots(1, 2, figsize=(8, 3))
for ax, y in zip(axes, [y1, y2]):
    ax.scatter(x, y)
    ax.set_title(f"r = {stats.pearsonr(x, y)[0]:.3f}")
plt.show()   # same r, very different pictures
```

### Common confusions
- "$r = 0$ means the variables are independent." **No.** The $y = x^2$ case has $r = 0$ and is completely determined by $x$.
- "$r$ is high, so a straight line is a good model." **Not necessarily.** Check the plot for curvature and outliers.
- "Remove the outlier so the correlation looks better." Only if you have a **reason** to believe it is an error, not because it is inconvenient.

### Questions to ask
- "Before computing any correlation, what is the first thing you should do?" (Plot the data.)
- "Can you sketch data with $r = 0$ but a clear relationship?" (A U-shape.)

## 4. Spearman correlation
### What it is
The **Spearman rank correlation** $\rho$ (the Greek letter _rho_) measures how well the relationship is **monotonic**: does $y$ _consistently_ go up (or consistently go down) as $x$ goes up, regardless of the exact shape?

The trick is simple: **replace the raw values with their ranks, then compute Pearson on the ranks.**

### Intuition
Imagine a race, and instead of recording finish _times_ you only record finish _positions_ (1st, 2nd, 3rd...). Ranks forget "by how much" and keep only "who is ahead of whom".

Now compare two rankings:
- Rank students by hours studied and also by quiz score. If the two rankings are nearly the same order, Spearman is near $+1$.
- If one ranking is the exact reverse of the other, Spearman is $-1$.
- If the orderings look unrelated, Spearman is near $0$.

Because it only uses order:
- A **curved but always-rising** pattern gets $\rho = 1$.
- A **huge outlier** is just "the biggest value", rank $n$, no matter how enormous it is, so it cannot dominate.

### The math
**Step 1:** replace every value by its rank (smallest = 1). If values tie, give each the **average** of the ranks they would have taken. For example, $10, 20, 20, 40$ get ranks $1, 2.5, 2.5, 4$.

**Step 2:** compute Pearson on the ranks:
$$\rho = r\big(\text{rank}(x), \text{rank}(y)\big)$$

**Shortcut when there are no ties.** Let $d_i$ be the difference between the two ranks of point $i$:
$$\rho = 1 - \frac{6\sum_{i=1}^{n} d_i^2}{n(n^2 - 1)}$$
In words: if the ranks match exactly, every $d_i = 0$ and $\rho = 1$. The bigger the rank disagreements, the smaller $\rho$ gets.

Properties:
- $-1 \le \rho \le 1$, read the same way as Pearson.
- $\rho = \pm 1$ means perfectly **monotonic**, not necessarily a straight line.
- Unaffected by any increasing transformation of the data (log, square root, exponential), because ranks do not change.
- Works for ordinal data (ratings like 1 to 5 stars) where distances between values are not meaningful.

### Worked example (Example B)
Months since launch $x$ and daily signups $y$:

| Month $x$ | Signups $y$ | Rank of $x$ | Rank of $y$ | $d$     | $d^2$ |
| --------- | ----------- | ----------- | ----------- | ------- | ----- |
| 1         | 10          | 1           | 1           | 0       | 0     |
| 2         | 30          | 2           | 3           | $-1$    | 1     |
| 3         | 20          | 3           | 2           | 1       | 1     |
| 4         | 50          | 4           | 4           | 0       | 0     |
| 5         | 200         | 5           | 5           | 0       | 0     |
|           |             |             |             | **Sum** | **2** |
$$\rho = 1 - \frac{6 \times 2}{5(25 - 1)} = 1 - \frac{12}{120} = 0.90$$
Compare with Pearson on the raw numbers: $r \approx 0.81$. The $200$ drags Pearson away from a line, while Spearman just sees "almost perfectly ordered, with one adjacent pair swapped".

### Code
```python
import numpy as np
import pandas as pd
from scipy import stats

month   = np.array([1, 2, 3, 4, 5])
signups = np.array([10, 30, 20, 50, 200])

rho, p = stats.spearmanr(month, signups)
print(rho, p)                           # 0.9, 0.037
print(stats.pearsonr(month, signups)[0])  # 0.805

# Spearman really is "Pearson on ranks"
print(stats.pearsonr(stats.rankdata(month), stats.rankdata(signups))[0])  # 0.9

# Ties get average ranks
print(stats.rankdata([10, 20, 20, 40]))   # [1.  2.5 2.5 4. ]

# Whole table at once
df = pd.DataFrame({"hours": [1, 2, 3, 4, 5], "score": [2, 4, 5, 4, 5], "signups": [10, 30, 20, 50, 200]})
print(df.corr(method="pearson").round(2))
print(df.corr(method="spearman").round(2))
```

### Common confusions
- Spearman is **not** "more accurate" than Pearson. It answers a _different_ question (monotonic vs linear).
- It still misses non-monotonic patterns such as a U-shape.
- It **throws away information** about magnitude. If the actual distances matter and the data is clean and linear, Pearson is more informative.
- The shortcut formula with $d_i$ is only exact when there are **no ties**. With ties, compute Pearson on the average ranks (software does this automatically).

### Questions to ask
- "If I take the logarithm of $y$, does Spearman change?" (No, ranks don't change.) "Does Pearson?" (Yes.)
- "Two judges rank 10 dishes. How would you measure how much they agree?" (Spearman between the two rankings.)
- "Why can a huge outlier hurt Pearson more than Spearman?" (Pearson uses the raw size of the deviation, Spearman uses only its rank.)

## 5. Pearson or Spearman: how to choose
### What it is
A practical decision guide, not a ritual.

### Intuition
Ask two questions about your scatter plot:
1. Does it look like a **straight line** (with ordinary scatter)? Pearson is a natural fit.
2. Does it **consistently rise or fall**, but bend, or contain outliers, or the data is ranks or ratings? Spearman is a safer fit.

### Comparison
| |**Pearson $r$**|**Spearman $\rho$**|
|---|---|---|
|Detects|Linear relationships|Monotonic relationships (any rising or falling shape)|
|Works on|Raw numeric values|Ranks of the values|
|Sensitive to outliers|**Yes, a lot**|Much less|
|Unchanged by...|Linear changes of units|**Any** increasing transformation (log, exp, square root)|
|Good for ordinal data (ratings)|No|**Yes**|
|Uses magnitude information|Yes|No (only order)|
|Perfect score ($\pm 1$) means|Exactly on a straight line|Perfectly ordered|
|Typical assumption|Roughly linear, numeric, ideally bell-shaped|Only that the relationship is monotonic|

### Decision guide
1. **Always plot first.**
2. Roughly linear, no wild outliers, numeric data? Use **Pearson**.
3. Curved but one-directional, heavy outliers or skew (income, followers, prices), or ordinal ratings? Use **Spearman**.
4. Not sure? **Compute both.** If they agree, relax. If they differ a lot, that disagreement is itself a clue (curvature or outliers). Go look at the plot.
5. The plot shows a U-shape or other non-monotonic pattern? **Neither works.** Consider other tools (a model, binning, or a measure such as mutual information).

### Worked example
Reading the same three datasets side by side:

| Data                          | Pearson | Spearman | What the gap tells you                                  |
| ----------------------------- | ------- | -------- | ------------------------------------------------------- |
| Hours vs score (Example A)    | $0.77$  | $0.74$   | Close, a fairly linear story                            |
| Months vs signups (Example B) | $0.81$  | $0.90$   | Spearman higher, so rising but bending with a big value |
| Clean data plus one bad point | $-0.45$ | $0.31$   | Big gap, so go check for an outlier                     |

### Code
```python
def compare(x, y):
    r = stats.pearsonr(x, y)[0]
    rho = stats.spearmanr(x, y)[0]
    print(f"Pearson {r:.2f} | Spearman {rho:.2f} | gap {abs(rho - r):.2f}")
```

### Common confusions
- "Use Spearman whenever the data isn't normal." Roughly right, but the real question is whether the pattern is **linear** or just **monotonic**.
- A large gap between the two is not an error. It is information.

### Questions to ask
- "You are correlating house size with price, and a few mansions exist. Which one first?" (Compute both, expect Spearman to be steadier.)
- "Customer satisfaction on a 1 to 5 scale vs. number of support tickets. Which one?" (Spearman, the scale is ordinal.)

## 6. Cautions and how it is used in ML
### Correlation is not causation
Ice cream sales and sunburns are positively correlated. Ice cream does not cause sunburn: **hot sunny weather** drives both. That hidden driver is called a **confounder**.

Three explanations whenever $X$ and $Y$ are correlated:
1. $X$ causes $Y$.
2. $Y$ causes $X$.
3. A third variable $Z$ causes both (or it is a coincidence).

Correlation alone cannot tell these apart. Randomized experiments (like the A/B tests in the earlier notes) can.

### Small samples are noisy
Unrelated random variables will still show some correlation by luck. A quick simulation with truly independent variables:

|Sample size $n$|Chance of seeing $\lvert r \rvert > 0.5$ by luck alone|
|---|---|
|5|about 39%|
|30|about 0.5%|
|500|essentially 0|

So $r = 0.77$ from 5 points (Example A, $p = 0.124$) is much less convincing than $r = 0.3$ from 500 points. This is the same signal-vs-noise story as hypothesis testing: the null hypothesis is "true correlation is 0", and the p-value returned by `pearsonr` or `spearmanr` tests it.

### Strength is not importance
With a huge dataset, even $r = 0.02$ can be "statistically significant" yet practically useless. Report the size of $r$, not just the p-value.

### Uses in ML
- **Feature selection:** features that correlate with the target may be useful. (Only linear or monotonic signal is detected, so non-monotonic features can be missed.)
- **Multicollinearity:** two features that are strongly correlated with _each other_ carry duplicate information and can make linear models unstable. Check the correlation matrix and consider dropping one.
- **Correlation heatmaps:** the standard first look at a tabular dataset.
- **Target leakage warning:** a feature with a suspiciously huge correlation with the target (like $0.99$) often means it secretly contains the answer.
- **Model evaluation:** correlation between predictions and true values is a quick sanity check for regression (though it ignores bias, so pair it with error metrics like MAE).
- **Ranking and recommendation systems:** Spearman is the natural measure for agreement between two rankings.

### Code
```python
import seaborn as sns
import matplotlib.pyplot as plt

corr = df.corr(method="spearman")   # or "pearson"
sns.heatmap(corr, annot=True, vmin=-1, vmax=1, cmap="coolwarm")
plt.show()
```

### Common confusions
- "Highly correlated features should both be kept because both predict the target." Often one is redundant. Check, don't assume.
- "No correlation with the target, so drop the feature." A non-monotonic or interaction effect can hide behind $r \approx 0$.

### Questions to ask
- "Cities with more firefighters have more fire damage. Should we cut firefighters?" (Confounder: bigger fires bring more firefighters.)
- "Two of your features have $r = 0.98$. What do you do?" (Keep one, or combine, and check the model.)

## Putting it all together
1. Make a **scatter plot** first, always.
2. Correlation compresses "do they move together?" into one number from $-1$ to $+1$: sign is direction, size is strength.
3. **Pearson** $r$ measures **linear** association, using the raw values.
4. **Spearman** $\rho$ measures **monotonic** association, using only ranks, so it handles curves, skew and outliers better.
5. Compute **both** when unsure. A big gap is a hint to look at the plot.
6. A coefficient from few points is **noisy**: check the sample size and the p-value.
7. **Correlation is not causation**, and $r = 0$ does not mean "unrelated".

### Concept cheat table

|Term|One-line meaning|
|---|---|
|Correlation|How tightly two variables move together, from $-1$ to $+1$|
|Covariance|Raw "do they move together" score, depends on units|
|Pearson $r$|Covariance divided by both standard deviations: strength of the **straight-line** relationship|
|Rank|Position of a value when sorted (1 = smallest)|
|Spearman $\rho$|Pearson computed on the ranks: strength of the **monotonic** relationship|
|Monotonic|Consistently rising (or consistently falling), any shape|
|Outlier|A point far from the pack that can distort $r$|
|Confounder|A hidden third variable driving both|
|$r^2$|Share of variation in $y$ linearly accounted for by $x$|

### Three takeaways for learners
1. Correlation measures **tightness and direction**, not slope and not cause.
2. Pearson looks for a **line**, Spearman looks for a **trend**.
3. The number can lie. **Plot the data** and mind the sample size.

## Formula quick reference

$$r = \frac{\sum_{i}(x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum_{i}(x_i-\bar{x})^2},\sqrt{\sum_{i}(y_i-\bar{y})^2}} = \frac{\text{cov}(X,Y)}{s_X, s_Y} = \frac{1}{n-1}\sum_{i} z_{x_i} z_{y_i}$$

$$\rho = r\big(\text{rank}(x), \text{rank}(y)\big) \qquad\qquad \rho = 1 - \frac{6\sum_i d_i^2}{n(n^2-1)} ;; (\text{no ties})$$

$$-1 \le r \le 1 \qquad -1 \le \rho \le 1 \qquad r^2 = \text{share of variance linearly accounted for}$$