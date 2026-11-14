
## The central story of the lecture
Imagine you are a data scientist at an online learning platform. The company wants to answer one question:
_"How can we improve students' learning outcomes?"_
_At first, the company compares two teaching methods. Later, it wants to compare three._
That's the entire storyline connecting A/B testing, hypothesis testing, t-tests, and ANOVA.

By the end, students should understand:
- Why a difference in sample averages isn't necessarily a real effect.
- How randomized A/B experiments work.
- What hypotheses, p-values, and statistical significance mean.
- Why variation and sample size matter.
- How ANOVA extends the comparison from two groups to several groups.
- How these ideas are used in real ML and product experiments.

## Part 1 — Why do we need statistical testing?
### Start with a question
Say to the students:
> *"Imagine our platform offers two versions of a Machine Learning course. Some students watch recorded lectures, while others attend interactive live classes. At the end, everyone takes the same quiz."*

Method A
Recorded lectures
Average marks: 72/ 100

Method B
Interactive live classes
Average marks: 78 / 100

> *"Which method should the company choose?"*

Give students 15–20 seconds to respond in the chat.
Most will choose B.

Now introduce three twists, one at a time.
Twist 1 — Sample size
> _"What if these averages came from only five students in each class? Would you be equally confident?"_

Twist 2 — Fairness
> _"What if the interactive class happened to contain students who already knew Python and statistics?"_

Twist 3 — Randomness
> _"What if we repeated this comparison next week with different students and got different averages?"_

### Explain sampling variation
Even when two teaching methods are equally effective, the two groups of students will rarely get exactly the same sample average.
Why? Because students differ in their ability, preparation, motivation, and other factors.

This natural fluctuation is called ==_sampling variation_==.

A useful analogy:
"Flip two fair coins ten times each. One might produce six heads and the other four. Does that prove one coin is better at producing heads?"

No. Randomness can create differences even when the underlying processes are identical.

Say this explicitly:
> "Statistics helps us decide whether the difference we observe is large enough, relative to randomness, to provide evidence of a real difference."

Board takeaway
_Observed difference = possible real effect + random variation + possible bias._

Our job is to design fair comparisons and assess the uncertainty.

## Part 2 — A/B testing: How do we compare two versions fairly?
### Introduce A/B testing
_An A/B test is an experiment in which people are randomly assigned to one of two versions, and we compare an outcome of interest._

Return to the learning platform.
The company has 200 students. It randomly assigns 100 students to Method A and 100 students to Method B.

200 students
Random assignment

Group A
100 students
Recorded lectures
Mean score: 72

Group B
100 students
Interactive classes
Mean score: 78

Difference = +6 marks

Is this difference convincing?

### Walk students through designing the experiment

|Step|What we do|Why it matters|
|---|---|---|
|Define objective|Improve student quiz scores|Know what success means|
|Choose metric|Score out of 100 on the same quiz|Measure consistently|
|Randomly assign|100 students per group|Reduce systematic differences|
|Apply treatment|Different teaching methods|Isolate the change|
|Collect scores|Measure outcomes|Obtain evidence|
|Analyze results|Estimate difference and uncertainty|Avoid jumping to conclusions|

### Why random assignment matters
Ask:
_"Suppose all experienced programmers choose Method B and all absolute beginners choose Method A. What happens?"_

Students should recognize that differences in prior knowledge could explain the result.

Explain _confounding_:
==A confounding factor is another variable that influences the outcome and is associated with the treatment assignment, making it hard to attribute the observed difference to the treatment itself.==

Randomization helps balance such factors on average, though it does not guarantee identical groups in any one experiment.

Give another familiar example:
_"An e-commerce website tests two checkout designs, but shows Design B only to returning customers. Is that a fair A/B test?"_
No. Returning customers may already be more likely to purchase.

### Explain control and treatment groups
Method A is the control, representing the existing experience.
Method B is the treatment, representing the new experience.

_An A/B test estimates what happens when users receive the new experience instead of the existing one._

Important: _A/B testing is a way to design an experiment. It is not itself a mathematical significance test. To analyze the collected data, we choose an appropriate statistical method._

Transition:
"We now have a fair experiment and a six-mark difference. But how do we know whether that difference is statistically convincing?"

## Part 3 — Hypothesis testing, p-values, and the t-test
This section introduces the main statistical ideas. You don't need to teach the full derivation of any test.

### 3.1 Hypothesis testing: Start by assuming no difference
Say:
"Scientists often begin by assuming that nothing special is happening. They then check whether the collected evidence is strong enough to challenge that assumption."

For our two teaching methods:
Null hypothesis (H₀): Both teaching methods have the same population mean quiz score.
$$H_0:\mu_A=\mu_B$$

Alternative hypothesis (H₁): The population mean quiz scores are different.
$$H_1:\mu_A\ne\mu_B$$

Explain that $\mu$ represents a population mean, whereas 72 and 78 are sample averages.

An analogy:
"In a courtroom, we evaluate the evidence against a starting assumption. In hypothesis testing, we evaluate how surprising our data would be if the null hypothesis were true."

Mention that failing to reject H₀ does not prove it is true, just as insufficient evidence doesn't establish that two methods are exactly equivalent.

### 3.2 Explain the p-value intuitively
Suppose the experiment yields a hypothetical p-value of 0.005.

Ask:
"What does 0.005 tell us?"

Explain:
> "If the two teaching methods truly had the same population mean score, and the statistical assumptions held, results at least as extreme as ours would occur about 0.5% of the time."
That would be quite unusual under H₀.

Interpreting hypothetical p-values

- 0.30: Not especially surprising under H₀
- 0.08: Less compatible with H₀, but above 0.05
- 0.005: Strong evidence against H₀

### 3.3 Statistical significance
Introduce a common decision threshold:
$$\alpha=0.05$$
This is the significance level chosen before analyzing the data.
- If p < 0.05, reject H₀.
- If p ≥ 0.05, fail to reject H₀.

Make three things clear:
1. The 0.05 threshold is a convention, not a guarantee of truth.
2. A statistically significant result is not necessarily a large or useful improvement.
3. A non-significant result doesn't prove that no difference exists.

### 3.4 Why do we need a t-test?
Return to the experiment.
Both methods differ by six marks, but the evidence also depends on how widely students' scores vary and how many students were measured.

Consider two situations with the same means and sample sizes.
Same 6-mark difference, different variability

Illustrative score distributions
```chart
type: line
labels: [54, 57, 60, 63, 66, 69, 72, 75, 78, 81, 84, 87, 90, 93, 96]
series:
  - title: Recorded (Mean 72)
    data: [0, 0, 0.001, 0.011, 0.135, 0.607, 1, 0.607, 0.135, 0.011, 0.001, 0, 0, 0, 0]
  - title: Interactive (Mean 78)
    data: [0, 0, 0, 0, 0.001, 0.011, 0.135, 0.607, 1, 0.607, 0.135, 0.011, 0.001, 0, 0]
tension: 0.4
fill: false
beginAtZero: true
xTitle: Score
yTitle: Relative density
legend: true
legendPosition: bottom
width: 50%
```

```chart
type: line
labels: [56, 58, 60, 62, 64, 66, 68, 70, 72, 74, 76, 78, 80, 82, 84, 86, 88, 90, 92, 94]
series:
  - title: Recorded (72)
    data: [0, 0.038, 0.146, 0.309, 0.5, 0.691, 0.854, 0.962, 1, 0.962, 0.854, 0.691, 0.5, 0.309, 0.146, 0.038, 0, 0, 0, 0]
  - title: Interactive (78)
    data: [0, 0, 0, 0, 0.038, 0.146, 0.309, 0.5, 0.691, 0.854, 0.962, 1, 0.962, 0.854, 0.691, 0.5, 0.309, 0.146, 0.038, 0]
tension: 0.4
fill: false
beginAtZero: true
legend: true
legendPosition: bottom
width: 50%
```

Ask:

"In which situation would you feel more confident that the methods truly differ?"
The answer is Scenario A, where the data are less noisy.

Now introduce the t-statistic:
$$t=\frac{\text{Difference in means}}{\text{Standard error of difference}}$$

Explain the standard error as the uncertainty in the estimated difference caused by sampling.

Key intuition: A bigger observed difference increases the signal, while more uncertainty makes that difference less convincing.

### 3.5 A numerical example students can follow
Use simple, plausible values for the running experiment.

| Quantity           | Method A | Method B |
| ------------------ | -------- | -------- |
| Students           | 100      | 100      |
| Average marks      | 72       | 78       |
| Standard deviation | 15       | 15       |

The difference is:
$$78-72=6\text{ marks}$$
The standard error of the difference is:
$$SE=\sqrt{\frac{15^2}{100}+\frac{15^2}{100}}\approx2.12$$
The t-statistic is approximately:
$$t=\frac{6}{2.12}\approx2.83$$
For an independent two-sample, two-sided t-test, the p-value is approximately 0.005. 
Reject $H_o$ as $0.005 < 0.05(\alpha)$
You can state this without explaining the mathematical machinery that converts t into p.

Interpretation for students
We observed a six-mark advantage for interactive lectures. Because the difference is relatively large compared with its estimated sampling uncertainty, we have statistical evidence that the two population means differ.

But we should still consider how the experiment was conducted and whether six marks represents a useful improvement.

#### Bonus: What happens if the sample is smaller?
Use the same six-mark difference and standard deviation of 15, but suppose there are only 10 students per group.
The standard error rises to approximately 6.71, so the t-statistic falls to about 0.89. The two-sided p-value is around 0.38.
Even though the observed difference is the same, the evidence is weaker.
This example demonstrates why sample size matters without requiring students to learn the sampling distribution.
### 3.6 Practical significance vs statistical significance
Ask:
_"Imagine a large experiment improves a course's mean score from 80.0 to 80.1, and the p-value is extremely small. Should we spend ₹10 lakh changing the entire platform?"_

Students will likely say no.
Explain:
Statistical significance asks whether the evidence is inconsistent with the null hypothesis.
Practical significance asks whether the size and consequences of the difference are important enough to act upon.

Briefly introduce the idea of a confidence interval as a range of effect sizes compatible with the data under the statistical model. For our example, an approximate 95% confidence interval for the improvement is around 1.8 to 10.2 marks. It helps communicate both the estimated benefit and its uncertainty.

## Part 4 — Five-minute checkpoint activity
Present this scenario:
"An e-commerce company tests a new Buy Now button."

Version A
Old button
Conversion rate: 10% (20 of 200 visitors)

Version B
New button
Conversion rate: 12% (24 of 200 visitors)

Ask:
"Should the company immediately replace Version A with Version B?"

Expected response: Not yet. The four additional conversions might be due to random variation.

Explain that the metric here is binary (whether a visitor converts), so instead of the earlier t-test for scores, we might use a two-proportion test.

You can optionally tell students that the approximate two-sided p-value here is 0.52, under standard independent-proportion testing assumptions. Thus, this small experiment doesn't provide strong evidence of a real improvement.

Transition: "The team now wants to compare three different buttons. What do we do?"

This naturally opens the ANOVA discussion, but clarify that for the binary conversion metric we would generally use a different method, such as a chi-square test. To teach ANOVA, return to the numeric student-score example.

## Part 5 — ANOVA: What if we have three teaching methods?
### 5.1 Begin with a familiar problem
Say:
_"Our online learning platform is happy with the experiment. But the management team has another idea: why compare only two teaching methods? Let's try three!"_

The new experiment tests:
- Method A: Recorded lectures.
- Method B: Interactive live classes.
- Method C: Project-based learning.
    

Assume students are randomly assigned to one of these three methods.
Average quiz scores by teaching method

```chart
type: bar
labels: [Recorded, Interactive, Improved]
series:
  - title: Marks
    data: [72, 78, 85]
    backgroundColor: "#28599A"
    borderColor: "#28599A"
    borderWidth: 0
beginAtZero: true
yMin: 0
yMax: 100
yAxisLabel: Marks
legend: false
width: 50%
```

Ask students:
_"Project-based learning has the highest average. Does this prove it is the best teaching method for all students?"_

By now, students should know the answer: no, not without considering variability and uncertainty.
Now ask:
_"Can we compare A with B, A with C, and B with C using separate t-tests?"_

Technically we could, but testing many comparisons separately increases the opportunity for false-positive conclusions.
This motivates ANOVA.

### 5.2 Introduce ANOVA
ANOVA stands for ==Analysis of Variance.==
Say:
> _"ANOVA helps us test whether the population means of several groups are all the same, by comparing variation between groups with variation within groups."_

Students may ask:
"Why is it called Analysis of Variance if we're comparing means?"

Explain that ANOVA uses variation as the tool for investigating differences between means.

### 5.3 The two kinds of variation
This is the most important ANOVA concept.
#### Within-group variation
Even when students receive exactly the same teaching method, their scores vary.
For example, imagine six students in Method A score:

Recorded lectures — individual scores
<div style="display:flex; gap:10px; width:100%;">
  <div style="flex:1; background:#303030; color:#eee; padding:16px 0; border-radius:10px; text-align:center;">69</div>
  <div style="flex:1; background:#303030; color:#eee; padding:16px 0; border-radius:10px; text-align:center;">70</div>
  <div style="flex:1; background:#303030; color:#eee; padding:16px 0; border-radius:10px; text-align:center;">71</div>
  <div style="flex:1; background:#303030; color:#eee; padding:16px 0; border-radius:10px; text-align:center;">73</div>
  <div style="flex:1; background:#303030; color:#eee; padding:16px 0; border-radius:10px; text-align:center;">74</div>
  <div style="flex:1; background:#303030; color:#eee; padding:16px 0; border-radius:10px; text-align:center;">75</div>
</div>

<p style="text-align:center;">Mean = 72</p>
All students experienced the same method, yet their scores aren't identical. This is within-group variation.

#### Between-group variation
Now suppose the three teaching methods have these sample means:

Method A
Mean: 72

Method B
Mean: 78

Method C
Mean: 85

The differences among these averages are between-group variation.

Now make the connection:
_"If group means differ substantially but students within each group score very similarly, the teaching method looks like a potentially important factor."_

_"But if scores vary wildly within every group, the same difference between averages is less convincing."_

### 5.4 Use a simple analogy
Imagine three cricket batters. You want to compare their typical performance.
- Batter A averages 40 runs.
- Batter B averages 45 runs.
- Batter C averages 50 runs.

If their scores are consistently near their respective averages, you can distinguish their performances more confidently.
But if each batter sometimes scores 0 and sometimes 100, those averages alone tell you less.
This is precisely the kind of signal-versus-noise reasoning ANOVA uses.

## Part 6 — ANOVA's F-statistic and interpreting results
### 6.1 Introduce the F-statistic
Write:
$$F=\frac{\text{Between-group variation}}{\text{Within-group variation}}$$
Explain that the exact calculation uses two variance estimates, called mean squares. We can understand the principle without deriving them.

Large F: Group differences are substantial relative to within-group variation.
Small F: Group differences are relatively small compared with within-group variation.

### 6.2 ANOVA hypotheses
Write:
$$H_0:\mu_A=\mu_B=\mu_C$$
All three population means are equal.

The alternative is:
$$H_1:\text{Not all population means are equal}$$
In ordinary language: at least one population mean differs.

### 6.3 Example result
For teaching purposes, suppose the statistical software produces:
Hypothetical one-way ANOVA output
F-statistic: 8.40
p-value: 0.001
Values are hypothetical and represent a sufficiently large experiment, not the small sample shown earlier.

Interpret it:
"Because p = 0.001 is below 0.05, we reject the null hypothesis. We have evidence that the three population means are not all equal."

Then ask:
"Does this tell us that Method C is significantly better than Method A and Method B?"
No.
The overall ANOVA only indicates evidence that at least one population mean differs. It does not identify which pairs are different.

### 6.4 Post-hoc tests
Say:
"Once ANOVA indicates a difference, we often want to know exactly where that difference lies."

Introduce a post-hoc test such as Tukey's HSD, which can compare pairs while controlling the overall false-positive risk.

A hypothetical outcome might be:

|Pair|Adjusted p-value|Interpretation|
|---|---|---|
|A vs B|0.12|Insufficient evidence of a difference|
|A vs C|0.001|Evidence of a difference|
|B vs C|0.03|Evidence of a difference|

Explain that these are illustrative results, not calculations from the previously displayed scores.
In this scenario, Method C appears to outperform both A and B, while we don't have sufficient evidence that A and B differ.

### 6.5 Why not run many t-tests?
Suppose you have three groups and test each pair at significance level 0.05 without any correction.

There are three comparisons:
A vs B, A vs C, and B vs C.

Under an idealized assumption that the tests were independent and all three null hypotheses were true, the probability of at least one false positive would be:
$$1-(0.95)^3\approx14.3\%$$

Actual pairwise comparisons using the same groups are not independent, so 14.3% is an illustration rather than the exact rate for this scenario.

The takeaway is that multiple comparisons require attention. An omnibus ANOVA and appropriately adjusted post-hoc tests are one common solution.

Finish by briefly naming assumptions: independent observations, approximately normal errors within groups, and similar population variances for classical ANOVA. Explain that Welch's ANOVA can handle unequal variances better.

## Part 7 — When to use A/B testing, t-tests, and ANOVA
Give students this table as their practical reference.

|Situation|Appropriate approach|
|---|---|
|Comparing two website designs experimentally|A/B test|
|Comparing mean scores of two independent groups|Two-sample t-test|
|Comparing mean scores across three teaching methods|One-way ANOVA|
|Comparing conversion rates of two website versions|Two-proportion test|
|Comparing conversion rates across three variants|Chi-square test or logistic regression|
|Finding which means differ after significant ANOVA|Adjusted post-hoc comparisons|

The important distinction:
- A/B testing is about experiment design and causal comparison.
- A t-test or ANOVA is about statistical analysis of particular kinds of outcomes.
They can be used together.

### Applications in machine learning
Recommendation systems: Experimentally compare a new recommendation algorithm with an existing one. Measure engagement, conversions, or another meaningful outcome.

ML-enabled products: Randomly assign users to the old or new model-assisted experience and evaluate whether it improves product outcomes.

Model comparison: When comparing models on the same test cases or across repeated datasets, account for the paired or repeated nature of the measurements. A simple independent-groups t-test or ANOVA may not be valid.

Feature experiments: Compare different feature engineering or preprocessing strategies across an appropriately designed evaluation.

Tell students:
"Training an ML model tells us what the model learned from the data. Statistical testing helps us assess how convincing the measured improvement is."

### Three mistakes worth warning about
First, statistical significance is not the same as business value. A tiny improvement can have a very small p-value.

Second, correlation is not necessarily causation. Comparing existing groups without random assignment is different from a randomized experiment.

Third, experiment duration matters. Repeatedly checking ordinary p-values and stopping when one becomes significant can increase false-positive risk.