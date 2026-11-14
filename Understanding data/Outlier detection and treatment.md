An **outlier** is an observation that differs unusually from the pattern in a relevant reference group. Outlier detection identifies observations worth investigating. Outlier treatment decides what to do with them, based on their cause and the purpose of the analysis.

The complete workflow is **understand the data → detect unusual observations → investigate their causes → choose a treatment → evaluate the effect**.

---
# What is an outlier
Consider these hypothetical restaurant bills, in dollars:
```text
10, 12, 12, 13, 15, 100
```
The bill of 100 is noticeably larger than the others. Several explanations are possible:

| Explanation | Interpretation | Possible action |
|---|---|---|
| A large group ordered an expensive meal | Valid rare observation | Keep it and consider party size |
| A bill was entered in the wrong units | Data error | Correct it after verifying the source |
| The bill belongs to a different menu or service | Different context | Compare it with an appropriate group |
| The transaction is potentially suspicious | Event of interest | Retain it for investigation |

_Unusual does not necessarily mean incorrect._ An incorrect value may also look ordinary: entering 18 instead of 19 might not trigger any detector.

### Types of outliers
The distinction between **global, contextual and collective** outliers describes what makes an observation unusual.

| Type | Reference for unusualness | Example |
|---|---|---|
| Global or point outlier | The overall dataset | A bill of 100 among bills mostly between 10 and 15 |
| Contextual outlier | A relevant group, time or situation | A 45 dollar solo lunch among much smaller solo lunches, even if 45 is ordinary for large parties |
| Collective outlier | A group or sequence considered together | A sensor reporting 20 repeatedly for an hour when readings normally fluctuate; each individual 20 may be ordinary |

A pooled IQR rule looks for global extremes in the selected variable. Contextual detection needs appropriate group or time information. Collective detection needs a way to represent the sequence or group; applying an independent threshold to every reading can miss it.

**Univariate versus multivariate** is a separate distinction: _it describes whether detection uses one feature or several features_. A contextual anomaly can involve several features, and a collective anomaly can occur in a single-variable time series.

| Type | Meaning | Example |
|---|---|---|
| Univariate | Unusual in one variable | A very large bill |
| Multivariate | An unusual combination of variables | An ordinary bill and an ordinary tip that form an unusual tip-to-bill combination |

These categories can overlap. Multivariate methods examine relationships between features; contextual analysis determines which comparison group makes sense.

### Common causes
- **Data entry problems:** misplaced decimal points, extra zeros, or incorrect units.
- **Measurement problems:** a faulty sensor or a failed recording process.
- **Valid variation:** large purchases, unusually high salaries, or rare natural events.
- **Mixed populations:** combining observations from groups with different normal behaviour.
- **Changes over time:** seasonality, a new process, or a shift in customer behaviour.

A domain rule and a statistical rule provide different information. A negative age violates a domain constraint. A high restaurant bill may simply be uncommon. Even a domain rule needs context: a negative amount can be valid in a table containing refunds.

# Why outliers matter
### Effect on summaries
For the first five bills:
$$
\text{Mean}=\frac{10+12+12+13+15}{5}=12.4
$$
After including the bill of 100:
$$
\text{Mean}=\frac{10+12+12+13+15+100}{6}=27
$$

| Statistic | Without 100 | With 100 |
|---|---:|---:|
| Mean | 12.4 | 27.0 |
| Median | 12.0 | 12.5 |

The mean uses the magnitude of every value, so a single extreme value can move it substantially. The median depends on sorted positions and is less sensitive to extreme magnitudes.

The appropriate summary depends on the question. Mean spending per meal and the amount of a typical meal describe different aspects of the data.

### Effect on machine learning
Outliers can affect:
- **Regression:** extreme observations may pull a fitted line toward themselves, especially when squared errors receive large penalties.
- **Distance-based methods:** large differences in one feature can dominate distances.
- **Scaling:** an extreme minimum or maximum can compress most values under min–max scaling.
- **Evaluation:** removing difficult but valid observations can produce misleading performance estimates.

Influence depends on both the observation and the method. An extreme feature value is not automatically influential in every model. A large prediction error is not automatically evidence of a data error.

# Detection of outliers
## 1. Visual detection
### Histogram
A histogram groups values into intervals and counts observations in each interval. It reveals the overall shape, tails, gaps and possible clusters.

A **right-skewed distribution** has a long tail toward large values. Spending data often has this shape. A long tail alone is not evidence that the data is wrong.

### Boxplot
A standard boxplot contains:
- A line at the **median**.
- A box from **Q1 to Q3**, representing the middle half of the data.
- Whiskers extending to the most extreme observed values within the chosen fences.
- Individual points outside the whiskers.

With the common 1.5 IQR rule, those individual points are statistical review candidates. Whiskers do not necessarily end at the minimum and maximum of the entire dataset, or exactly at the numerical fences.

### Scatterplot
A scatterplot shows the relationship between two variables. For restaurant data, plotting bill amount against party size helps distinguish a large bill for one person from a large bill for six people.

An observation can look ordinary in separate histograms but unusual in a scatterplot because its feature combination differs from the normal relationship.

## 2. Detection with the interquartile range
### Quartiles and IQR
After sorting observations:
- **Q1** is the 25th percentile.
- **Q2** is the median, or 50th percentile.
- **Q3** is the 75th percentile.

The **interquartile range**, or IQR, measures the spread of the middle half of the data:
$$
\mathrm{IQR}=Q_3-Q_1
$$
The usual review fences are:
$$
L=Q_1-1.5\times\mathrm{IQR}
$$
$$
U=Q_3+1.5\times\mathrm{IQR}
$$
Flag a value when $x<L$ or $x>U$. 
A value exactly on a fence is not flagged by this definition.

The multiplier 1.5 is a convention. It does not identify a known error rate or establish that every flagged observation is invalid.

### Worked example
For `10, 12, 12, 13, 15, 100`, using pandas' default linear quantile interpolation:
$$
Q_1=12,\qquad Q_3=14.5
$$
$$
\mathrm{IQR}=14.5-12=2.5
$$
$$
L=12-1.5(2.5)=8.25
$$
$$
U=14.5+1.5(2.5)=18.25
$$
The value 100 exceeds the upper fence and is flagged. Its cause still needs investigation.

Different quartile conventions can produce slightly different answers in small samples. Consistency between explanation and software matters more than memorizing an interpolation procedure.

### Python implementation
```python
import pandas as pd

bills = pd.Series([10, 12, 12, 13, 15, 100])

q1, q3 = bills.quantile([0.25, 0.75])
iqr = q3 - q1
lower = q1 - 1.5 * iqr
upper = q3 + 1.5 * iqr

flags = (bills < lower) | (bills > upper)
print(bills[flags])  # 100
```

### Strengths and limitations
The IQR rule is _simple_, _explainable_ and _less sensitive to extreme magnitudes_ than a rule based on mean and standard deviation. It _does not require a normal distribution_, but its behaviour still depends on the distribution's shape.

It _can flag valid observations in skewed data_, _overlook unusual combinations_, and _give unstable results in small groups_. When IQR is zero, the fences collapse; repeated values require inspection rather than blind application of the rule.

Increasing the multiplier widens the fences and cannot increase the number of flags on the same values. Fewer flags do not necessarily mean a better detector.

## 3. Detection with z scores
A **z score** measures how far a value is from the mean in standard deviation units:
$$
z=\frac{x-\mu}{\sigma}
$$
Here, $\mu$ is the reference mean and $\sigma$ is the reference standard deviation. A positive score is above the mean; a negative score is below it.
For example, if the mean is 20 and the standard deviation is 5, a value of 40 has:
$$
z=\frac{40-20}{5}=4
$$

A common review rule is $|z|>3$.
Equivalently, flag values with $z<-3$ or $z>+3$. Values exactly at −3 or +3 are not flagged under this strict rule.

For normally distributed data, approximately 99.7% of observations lie within three standard deviations of the mean. This statement does not establish that the remaining observations are errors, and the percentage does not apply to arbitrary distributions.

### Masking
Extreme values can move the mean and inflate the standard deviation, making themselves appear less unusual. This effect is called **masking**.

In the six-bill example, 100 has a z score of approximately 2.23 when the mean and population standard deviation are calculated from all six values. The $|z|>3$ rule therefore misses it.

Z-score thresholds need particular care with skewed data. If the standard deviation is zero, the score is undefined.

### Python with scipy.stats.zscore
`scipy.stats.zscore()` calculates scores relative to the mean and standard deviation of the array supplied to it. The example below explicitly uses `ddof=0`, the default standard deviation convention.

```python
import numpy as np
import pandas as pd
from scipy import stats

bills = pd.Series([10, 12, 12, 13, 15, 100], dtype=float)
scores = stats.zscore(bills.to_numpy(), ddof=0)
flags = np.abs(scores) > 3

comparison = pd.DataFrame({
    "bill": bills,
    "z_score": scores,
    "flagged": flags,
})
print(comparison.round(3))
```

The value 100 has a score near 2.23 and is not flagged: this illustrates masking in a small sample. The code runs correctly even though this particular rule misses the candidate.

For held-out observations, reuse the training mean and standard deviation:
```python
# train_bills and test_bills are the previously separated bill columns.
mean_train = train_bills.mean()
sd_train = train_bills.std(ddof=0)
if sd_train == 0:
    raise ValueError("Z scores are undefined for zero training spread.")

test_scores = (test_bills - mean_train) / sd_train
test_flags = test_scores.abs() > 3
```

Calling `stats.zscore(test_bills)` would instead calculate a new reference from the test batch.

## 4. Detection with median absolute deviation
The **median absolute deviation**, or MAD, measures spread around the median:
$$
\mathrm{MAD}=\operatorname{median}\left(|x_i-\operatorname{median}(x)|\right)
$$

Calculation proceeds by finding the median, calculating each absolute distance from it, and taking the median of those distances.
For `10, 12, 12, 13, 15, 100`, the median is 12.5. The sorted absolute deviations are:
```text
0.5, 0.5, 0.5, 2.5, 2.5, 87.5
```

Therefore, MAD is $(0.5+2.5)/2=1.5$.

A common modified z score is:

$$
z_{\mathrm{modified}}=\frac{0.67449(x-\operatorname{median}(x))}{\mathrm{MAD}}
$$

For 100, this score is approximately 39.35. A common heuristic flags values with $|z_{\mathrm{modified}}|>3.5$.

The factor 0.67449 provides a normal-reference scaling. The modified score is less affected by a few extreme values than the ordinary z score. It still needs context and an appropriate threshold. If MAD is zero, the score is undefined.

| Method | Reference centre and spread | Main limitation |
|---|---|---|
| IQR | Quartiles and middle-half spread | Skew, small groups, zero IQR |
| Z score | Mean and standard deviation | Skew and masking |
| Modified z score | Median and MAD | Zero MAD and context dependence |

## 5. Detection using several features
### Isolation Forest
Isolation Forest repeatedly partitions observations using randomly chosen features and split values. An isolated observation often takes fewer splits to separate from the rest.

The resulting score can identify unusual combinations, such as a bill amount that is uncommon for its party size. The method does not establish why an observation is unusual.

### Local Outlier Factor
**Local Outlier Factor**, or LOF, compares an observation's local density with the density around its neighbours. An observation may be unusual if it is much less densely surrounded than nearby observations.

Because LOF uses distances, feature units matter. Bill amounts and party sizes should be scaled using reference statistics learned from training data. Neighbour count also affects the comparison.

### Scores, thresholds and labels
An anomaly score ranks unusualness. A threshold turns that ranking into a flag. Neither automatically supplies a probability of error.

Setting `contamination=0.05` in the notebook's detectors uses a 5% training-tail assumption to set a threshold. It does not discover that 5% of the data is truly wrong. The proportion flagged in a new batch can differ.

Without verified anomaly labels, agreement between detectors is not detection accuracy. Evaluation may require reviewed examples, source checks, investigation costs and downstream performance.

# Outlier treatment
The correct treatment depends on whether the observation is valid, why it is unusual, and what the analysis needs to preserve.

### 1. Correct a verified error
If a receipt shows 17.31 dollars but the dataset stores 1731 dollars, the source supports correcting the value. A detector alone cannot establish that division by 100 is the right correction.
Keep an audit record containing the row identifier, field, previous value, corrected value and reason.

### 2. Keep and flag
A plausible large purchase may be useful information. Keeping it with a review flag preserves both the record and its magnitude. Contextual features such as party size can help explain its behaviour.

### 3. Remove an observation
Removal can be appropriate for a verified invalid record or a predefined exclusion aligned with the task. Removing every flagged bill can systematically remove large parties and change the population represented in the data.

Removal reduces row count. It also discards the other fields in the removed rows, even if those fields are valid.

### 4. Cap a feature
_==Winsorization== replaces tail values with boundary values, usually chosen from percentiles_. More generally, capping can also use domain limits or IQR fences. All these choices limit magnitudes, but they need not produce the same boundaries.

_==Capping== limits values below or above specified bounds_:
$$
x_{\mathrm{capped}}=\min(\max(x,L),U)
$$
With an upper cap of 40:

| Original value | Capped value |
|---:|---:|
| 15 | 15 |
| 45 | 40 |
| 100 | 40 |

Capping preserves row count but loses information about how far extreme values extend beyond the cap. It can be tested as a modelling choice, but capped bills must not be reported as actual observed revenue.

Percentile winsorization caps values at chosen percentiles, such as the 1st and 99th. IQR capping uses IQR fences instead. These are different rules and usually produce different limits.

### 5. Transform a feature
A logarithmic transformation compresses large magnitudes while preserving order:
$$
x_{\mathrm{new}}=\log(1+x)
$$
In Python, `np.log1p(x)` implements this expression. It is convenient for zero and positive values. Logarithms change the feature's scale and interpretation; they do not verify observations or correct a unit error.

Transformations must match the variable's valid range. A policy designed for positive sales amounts may be unsuitable for data containing refunds.

### 6. Use a less sensitive summary or model
The median can summarize a typical value when a few extremes strongly affect the mean. Robust regression can reduce the influence of certain extreme residuals when its assumptions fit the task.

`RobustScaler` uses medians and interquartile ranges to change feature scale. **It does not remove or cap outliers.** Scaling, transformation, robust modelling and source correction serve different purposes.

### 7. Mark an invalid field as missing
When a field is confirmed invalid but no correct value is available, marking that field as missing can preserve valid information elsewhere in the row. Any subsequent imputation requires a suitable rule, fitted on training data. An invalid-value indicator can preserve useful context.

Replacing all statistically flagged values with the mean assumes they are errors and erases potentially valid information.

### Choosing a treatment

| Situation | Reasonable starting action |
|---|---|
| Source confirms an entry or unit error | Correct using the verified value |
| Record is confirmed invalid and cannot be recovered | Apply the predefined missing-value or exclusion policy |
| Extreme value is plausible | Keep it and investigate relevant context |
| Valid extremes interfere with a modelling objective | Compare capping, transformation or robust approaches through validation |
| Anomalies are the events being studied | Preserve them for detection and investigation |

The original observations should remain available. Treatment copies should retain traceability to the source.

## Before and after distribution comparison
Treatment should be checked with both plots and numerical summaries. The relevant questions include how many rows remain, which values change, and whether important groups or valid tail behaviour become less visible.

| View | What to examine |
|---|---|
| Original histogram | The initial shape and tail |
| Histogram after removal | Missing tail observations and lower row count |
| Histogram after capping | Tail values gathered at a boundary |
| Histogram after log transformation | A compressed high end on a new scale |
| Scatterplot before and after treatment | Changes to relationships between features |
| Summary table | Row count, mean, median, range and group representation |

The following example runs from the extracted teaching-pack folder. It compares the same training bills under each choice and keeps the original data intact.
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split

df = pd.read_csv("data/tips.csv")
train, test = train_test_split(df, test_size=0.25, random_state=42)
bills = train["total_bill"]
q1, q3 = bills.quantile([0.25, 0.75])
iqr = q3 - q1
lower, upper = q1 - 1.5 * iqr, q3 + 1.5 * iqr
flags = (bills < lower) | (bills > upper)

versions = {
    "Before treatment": bills,
    "After removal": bills[~flags],
    "After capping": bills.clip(lower, upper),
    "After log transform": np.log1p(bills),
}

fig, axes = plt.subplots(2, 2, figsize=(10, 7))
common_bins = np.linspace(bills.min(), bills.max(), 16)
for ax, (name, values) in zip(axes.flat, versions.items()):
    is_log = name == "After log transform"
    ax.hist(values, bins=15 if is_log else common_bins,
            color="steelblue", edgecolor="white")
    ax.set_title(f"{name} (n={len(values)})")
    ax.set_xlabel("log(1 + bill in USD)" if is_log else "Bill (USD)")
    ax.set_ylabel("Number of meals")
plt.tight_layout()
plt.show()

summary = pd.DataFrame({
    name: {"rows": len(values), "mean": values.mean(),
           "median": values.median(), "min": values.min(),
           "max": values.max()}
    for name, values in versions.items()
}).T
print(summary.round(3))
```

The first three histograms use the same bin edges. Removal changes the sample size, so raw counts can decrease. Log-transformed values use different units; their means and ranges cannot be directly interpreted as dollar amounts. None of these plots proves that a treatment improves a model: that requires validation for the intended task.

## Preventing data leakage
**Data leakage** occurs when information unavailable at training time influences a fitted rule or evaluation.

For an ML workflow:
1. Split the data into training and held-out sets.
2. Learn thresholds, scaling statistics and imputation values from training data.
3. Apply those same fitted rules to validation or test observations.
4. Choose methods using validation results.
5. Evaluate the chosen method on all eligible held-out test observations.

During cross-validation, each training fold must learn its own preprocessing statistics. A fitted transformer inside a pipeline supports this separation.

Do not recompute IQR fences separately on the test batch. Do not remove difficult but valid test cases merely to improve a model's score. Confirmed data errors should follow an established, evidence-based data policy.

An initial exploratory description can summarize a whole dataset. Once the goal is evaluating performance on unseen observations, data-dependent decisions require an appropriate training and evaluation split. Time-dependent data may need chronological splits rather than random splits.

## Worked example with real restaurant data
### Dataset
The `tips` dataset contains 244 meals recorded by one waiter at one restaurant over a few months. Bill and tip amounts are in US dollars, and `size` is party size. It is a small historical convenience sample with no verified error labels.

| Field | Meaning |
|---|---|
| `total_bill` | Recorded bill amount in dollars |
| `tip` | Recorded tip amount in dollars |
| `size` | Number of people in the party |
| `day` | Recorded day of the week |
| `time` | Lunch or dinner |

The accompanying notebook reserves 25% of rows with `random_state=42`, leaving 183 training rows and 61 held-out rows.

### Detection results
Using the training bills:

| Quantity | Value |
|---|---:|
| Q1 | 13.6600 |
| Q3 | 24.8550 |
| IQR | 11.1950 |
| Lower fence | −3.1325 |
| Upper fence | 41.6475 |
| Flagged training meals | 5 of 183 |
| Flagged held-out meals using the same fences | 2 of 61 |

The negative lower fence is an arithmetic result, not permission for negative bills. The upper fence identifies review candidates, not verified mistakes.

The largest bill in the complete dataset is 50.81 dollars for three people. This is unusual under the training rule, but the available data does not establish that it is erroneous.

### Investigation
Relevant evidence includes party size, amount per person, menu, service context and original receipts. Bill per person is a contextual clue, not proof of correctness. Very small groups may not contain enough examples for reliable group-specific thresholds.

### Treatment and evaluation
The notebook compares keeping and flagging, capping a copied feature, removing flagged training rows, and applying a logarithmic transformation. It checks row counts, summaries and representation of larger parties.

A separate copy deliberately introduces a 100-times unit error. The original value is known because the error is simulated. That demonstration illustrates source-supported correction; it is not a claim that such an error exists in the real dataset.

For modelling, treatment should also be evaluated on validation performance and relevant subgroups. A visually smoother distribution is insufficient evidence that a treatment is useful.

## Python libraries

| Library | Relevant capability | Key caution |
|---|---|---|
| pandas and NumPy | Quantiles, Boolean flags, clipping and transformations | Keep original values and reuse fitted limits |
| SciPy | Z scores and median absolute deviation | Standardize new data with training reference statistics |
| scikit-learn | Isolation Forest, LOF, scalers and pipelines | Choose suitable features and distinguish batch detection from novelty scoring |
| PyOD | A common interface for multiple anomaly detectors | Its label and score conventions differ from sklearn |
| Feature-engine | Fitted capping with `Winsorizer` | Learn limits on training rows or training folds |

### Label and score conventions

| API used in the companion notebook | Outlier label | More unusual score |
|---|---:|---|
| sklearn Isolation Forest | `-1` | Lower `decision_function` |
| sklearn LOF in novelty mode | `-1` | Lower `decision_function` |
| PyOD IForest | `1` | Higher `decision_function` |

These scores are not probabilities and should not be compared directly across packages.

With sklearn LOF, the default batch mode uses `fit_predict` on the reference batch. With `novelty=True`, fit on the reference and use `predict` only on unseen observations. A reviewed, mostly normal reference is preferable for novelty monitoring.

### Companion notebooks

- [Outlier detection and treatment notebook](01_outliers_live_class.ipynb): the real-data workflow, with optional statistical and modelling appendices.
- [Python libraries notebook](02_outlier_python_libraries.ipynb): SciPy, Isolation Forest, LOF, and optional PyOD and Feature-engine examples.

Both notebooks include saved core outputs and an embedded CSV fallback. The optional PyOD and Feature-engine examples require their additional dependencies; those branches were syntax-checked but not runtime-tested during preparation.

## Common mistakes

| Mistake | Why it is misleading |
|---|---|
| Deleting every boxplot dot | Statistical rarity does not prove invalidity |
| Applying one rule to unrelated groups | Normal behaviour may differ by context |
| Using a z-score rule without inspecting shape | Skew and masking can distort the comparison |
| Treating scaling as cleaning | Changing units does not correct observations |
| Learning preprocessing limits from test data | Evaluation no longer reflects an unseen-data workflow |
| Calling detector agreement accuracy | Agreement supplies no verified ground truth |
| Reporting a capped bill sum as revenue | The amounts have been changed |
| Choosing the method that produces the fewest flags | Flag count alone does not measure usefulness |

## Practice questions
1. Why can the same bill amount be ordinary for six people and unusual for one person?
2. Calculate the IQR fences when Q1 is 20 and Q3 is 30. Is 50 flagged?
3. What happens to flags if the IQR multiplier increases from 1.5 to 3 on the same data?
4. What information is lost by capping both 45 and 100 at 40?
5. Does `contamination=0.05` prove that 5% of the dataset contains errors?
6. Why should held-out observations use the training IQR fences?

### Answers
1. Party size changes the relevant comparison. Amount per person and meal context can explain some differences.
2. IQR is 10. The lower fence is 5 and the upper fence is 45. The value 50 is flagged for review.
3. Fences widen, so the number of flags cannot increase. This alone does not establish a better rule.
4. Both retain the same capped value, so their original magnitudes and difference are lost in that feature.
5. No. It is a threshold-setting assumption, not a verified error rate.
6. New observations must be compared with the reference rule learned from training, without redefining that rule using evaluation data.

## References
- [R tips dataset documentation and original Bryant and Smith reference](https://search.r-project.org/CRAN/refmans/reshape/html/tips.html)
- [seaborn tips CSV](https://github.com/mwaskom/seaborn-data/blob/master/tips.csv)
- [scikit-learn novelty and outlier detection guide](https://scikit-learn.org/stable/modules/outlier_detection.html)
- [scikit-learn Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html)
- [scikit-learn Local Outlier Factor](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.LocalOutlierFactor.html)
- [PyOD IForest implementation](https://pyod.readthedocs.io/en/latest/_modules/pyod/models/iforest.html)
- [Feature-engine Winsorizer](https://feature-engine.trainindata.com/en/latest/user_guide/outliers/Winsorizer.html)
- [SciPy zscore](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.zscore.html)
