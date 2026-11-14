## 1. Recommended teaching strategy
> "When one variable changes, can we understand how another variable tends to change?"

Imagine you are a data scientist at an EdTech company. You want to understand student learning behavior.
- Question 1: Do students who study more hours generally get higher marks?
- Question 2: Do students who watch more lectures generally perform better?
- Question 3: What if performance improves as study time increases, but the improvement gradually slows down?
- Question 4: What if one unusual student completely changes our correlation result?

These questions naturally lead to correlation, Pearson, Spearman, and their practical applications in ML.

## 2. Detailed teaching notes
### Part 1 — Start with an everyday problem
Open the class by asking:
_"Suppose we have data for 100 students. For each student, we know how many hours they study per week and their final exam marks. Do you think students who study more hours generally score higher?"_
Most will say yes.

Then ask:
_"Does that mean everyone who studies five hours will score exactly 50 marks and everyone studying ten hours will score exactly 100?"_
Obviously not.

Explain that we are interested in the general pattern, not a perfect rule.
Introduce correlation:
> =="Correlation describes the direction and strength of association between two variables."==

Use three quick examples:

| Variables                               | Possible relationship               |
| --------------------------------------- | ----------------------------------- |
| Study hours and exam marks              | Positive                            |
| Number of missed classes and exam marks | Negative                            |
| Student roll number and exam marks      | No meaningful relationship expected |

Emphasize that these are plausible patterns, not guaranteed properties of every dataset.
Teaching point: Correlation helps us describe how two variables vary together. _It does not automatically imply that one causes the other._
See: tylervigen.com/spurious-correlations

Ask the students:
"If ice cream sales and drowning incidents both increase during summer, does buying ice cream cause drowning?"

No. Hot weather and seasonal activity can influence both.
This memorable example introduces correlation does not imply causation early in the lecture.

### Part 2 — Direction and strength of correlation
Explain correlation using scatter plots rather than calculations.
Positive ->

```chart
type: scatter
labels: [1, 2, 3, 4, 5, 6, 7, 8]
series:
  - title: Positive
    data:
      - {x: 1, y: 1}
      - {x: 2, y: 1.7}
      - {x: 3, y: 2.3}
      - {x: 4, y: 3.5}
      - {x: 5, y: 4.5}
      - {x: 6, y: 4.9}
      - {x: 7, y: 7}
      - {x: 8, y: 7.8}
    backgroundColor: "#10958F"
    borderColor: "#10958F"
    borderWidth: 0
    pointRadius: 9
legend: false
width: 25%
```
Negative →

```chart
type: scatter
labels: [1, 2, 3, 4, 5, 6, 7, 8]
series:
  - title: Negative
    data:
      - {x: 1, y: 7.8}
      - {x: 2, y: 6.8}
      - {x: 3, y: 6.2}
      - {x: 4, y: 4.5}
      - {x: 5, y: 3.7}
      - {x: 6, y: 2.6}
      - {x: 7, y: 1.6}
      - {x: 8, y: 0.5}
    backgroundColor: "#2D6AF0"
    borderColor: "#2D6AF0"
    borderWidth: 0
    pointRadius: 9
legend: false
width: 25%
```
No clear linear trend ->

```chart
type: scatter
labels: [1, 2, 3, 4, 5, 6, 7, 8]
series:
  - title: No clear linear trend
    data:
      - {x: 1, y: 3.3}
      - {x: 2, y: 6.8}
      - {x: 3, y: 1.6}
      - {x: 4, y: 5}
      - {x: 5, y: 7.5}
      - {x: 6, y: 1.3}
      - {x: 7, y: 5.6}
      - {x: 8, y: 3.8}
    backgroundColor: "#9257F5"
    borderColor: "#9257F5"
    borderWidth: 0
    pointRadius: 9
legend: false
width: 25%
```


Clarify the two things a correlation coefficient tells us:
Direction comes from its sign.
- Positive: As X increases, Y tends to increase.
- Negative: As X increases, Y tends to decrease.
- Near zero: Little or no association of the type the coefficient measures.
Strength comes from its magnitude.

_For Pearson and Spearman, coefficients range from −1 to +1._

Correlation coefficient scale
Ask:
_"If correlation is −0.9, is it weak because the number is negative?"_
No. Negative indicates direction; 0.9 indicates strong magnitude.
Mention that labels such as weak, moderate, and strong depend on domain context.

*Important distinction: A correlation near zero doesn't necessarily mean there is no relationship. The relationship might be curved or more complicated.*

### Part 3 — Pearson correlation: Looking for straight-line patterns
Introduce Pearson by asking:
*"Suppose every additional hour of study is associated with roughly the same increase in expected marks. What does that pattern look like?"*
A roughly straight line.
Pearson correlation measures the strength and direction of a linear relationship between two numeric variables.

Start with a simple dataset:

|Student|Study hours (X)|Exam marks (Y)|
|---|---|---|
|A|1|40|
|B|2|50|
|C|3|60|
|D|4|70|
|E|5|80|

Here, the relationship is perfectly linear and increasing. Pearson correlation is +1.
Say:
_"Every time study hours increase by one, marks increase by ten. All five points fall on a straight line."_
Now change some marks to 45, 55, 57, 74, and 79. The pattern is still generally upward, but the points no longer fall on a perfect line.
Pearson will remain positive but below +1.

_Pearson is sensitive to outliers because extreme numeric values can strongly influence the result._

#### A little mathematics
Show the Pearson formula once, but don't derive it:

$$r = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n} (x_i - \bar{x})^2}\;\sqrt{\sum_{i=1}^{n} (y_i - \bar{y})^2}}$$

$$r=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y}$$
Explain in plain language:
- Covariance: Do X and Y tend to move together, or in opposite directions?
- Standard deviation: How spread out is each variable?
- Division by the standard deviations: Standardizes the result to the range −1 to +1.

A useful comparison:
"Whether study time is measured in hours or minutes, Pearson correlation stays the same if we simply multiply by a positive constant."

That is why Pearson can compare relationships across different measurement units.

#### What Pearson is good at
Pearson is appropriate when the main question is about a linear relationship between quantitative variables. For example, examining how total study hours relate to exam marks, provided a scatter plot supports a roughly linear pattern.

Transition question:
"What if higher study time still means higher performance, but the improvement slows down as students study longer? Is a straight line still the best description?"

That brings us to Spearman.
### Part 4 — Spearman correlation: Focus on order, not exact values
This is where I recommend spending the most teaching effort. Students should understand why Spearman exists rather than just memorizing that it uses ranks.
#### 4.1 Start with a problem Pearson doesn't describe perfectly
Say:
"Imagine student performance increases with study time, but not at a constant rate. The first few hours help a lot, while additional hours give smaller improvements."

Consider:

|Student|Study hours|Exam marks|
|---|---|---|
|A|1|20|
|B|2|50|
|C|3|70|
|D|4|80|
|E|5|85|

Ask:
_"Does increasing study time consistently correspond to increasing marks?"_
Yes.
_"Do the marks increase by the same amount for every extra hour?"_
No. The improvements are +30, +20, +10, and +5.
The pattern is increasing, but not perfectly linear.


### Increasing, but not a straight line

```chart
type: line
labels: [1, 2, 3, 4, 5]
series:
  - title: Score
    data: [20, 50, 70, 80, 85]
    backgroundColor: "#28599A"
    borderColor: "#28599A"
    borderWidth: 2
    pointRadius: 5
    pointBackgroundColor: "#28599A"
    pointBorderColor: "#414141"
    pointBorderWidth: 2
beginAtZero: true
yMin: 0
yMax: 100
legend: false
width: 50%
```

*Illustrative example: increasing but diminishing score improvements.*

Introduce monotonic relationships.
_A monotonic relationship means that as X increases, Y consistently moves in one direction (non-decreasing or non-increasing). The rate of change doesn't have to stay constant._

==Pearson asks: How closely do the points follow a straight line?==
==Spearman asks: As one variable increases, does the other consistently increase or decrease?==

#### 4.2 Explain ranks using a simple example
This is the best way to teach Spearman.

Say:
_"Instead of looking at actual study hours and exam marks, imagine we replace every value with its rank."_

|Student|Study hours|Study rank|Marks|Marks rank|
|---|---|---|---|---|
|A|1|1|20|1|
|B|2|2|50|2|
|C|3|3|70|3|
|D|4|4|80|4|
|E|5|5|85|5|

Ask:
_"Notice something interesting? The ranks are exactly the same!"_

That means the student with the least study time also has the lowest marks, the second-lowest has the second-lowest marks, and so on.
Spearman correlation = +1, because the rankings match perfectly.

But Pearson isn't exactly +1, because the points do not form a perfect straight line.

##### Another example
Suppose we have students' study hours and exam scores:

| Student | Study hours | Rank (X) | Exam score | Rank (Y) |
| ------- | ----------- | -------- | ---------- | -------- |
| A       | 2           | 1        | 40         | 1        |
| B       | 4           | 2        | 60         | 2        |
| C       | 6           | 3        | 90         | 4        |
| D       | 8           | 4        | 80         | 3        |

How ranking works:
- Smallest value gets rank 1.
- Next smallest gets rank 2, and so on.
- We rank study hours and exam scores separately.

Then calculate Pearson correlation on these ranks, not the original values.
For this example: $\boxed{\rho=0.8}$

#### 4.3 How Spearman works
Explain:
_Spearman correlation is simply Pearson correlation calculated on the ranks instead of the original values._

You can write:
$$\rho_s=\operatorname{Corr}(\operatorname{rank}(X),\operatorname{rank}(Y))$$
Explain the process:
1. Rank X values.
2. Rank Y values.
3. Compare how closely the two rankings align.

If students' rankings match perfectly, Spearman is +1. If the rankings are exactly reversed, it is −1.
If ranks are tied, we usually assign average ranks.

#### 4.4 A memorable example: Salary vs job seniority
Imagine an organization with five employees:

|Employee|Experience (years)|Salary (₹ lakh)|
|---|---|---|
|A|1|4|
|B|2|5|
|C|3|6|
|D|4|7|
|E|5|30|

The most experienced employee has an unusually high salary.
Pearson is affected by the extreme salary because it works with actual numerical values.
Spearman, however, sees that salary rankings perfectly match experience rankings.
Therefore Spearman = +1.

Ask students:
_"Does Spearman care whether the most experienced employee earns ₹10 lakh or ₹30 lakh, as long as they remain highest-paid?"_
No. The ranking remains unchanged.

==Critical nuance: Spearman can be more resistant to the size of extreme observations, but it is not immune to outliers, especially those that substantially change the ordering.==

### Part 5 — Pearson vs Spearman: Understand the difference
At this stage, students know both techniques. Now compare them directly.

## One visual to remember

```chart
type: line
labels: [1, 2, 3, 4, 5, 6]
series:
  - title: Linear relationship
    data: [1.2, 2.5, 3.6, 5.2, 6.3, 7.6]
    backgroundColor: "#2D6AF0"
    borderColor: "#94A3B8"
    borderWidth: 3
    borderDash: [8, 6]
    pointRadius: 6
    pointBackgroundColor: "#2D6AF0"
    pointBorderColor: "#2D6AF0"
    pointBorderWidth: 0
tension: 0
beginAtZero: true
yMin: 0
yMax: 8.5
xTickDisplay: false
yTickDisplay: false
legend: false
width: 50%
```
Linear relationship: Both Pearson and Spearman can be close to +1.


```chart
type: line
labels: [1, 2, 3, 4, 5, 6]
series:
  - title: Monotonic nonlinear relationship
    data: [0.7, 3.4, 5.1, 6.2, 6.9, 7.3]
    backgroundColor: "#10958F"
    borderColor: "#94A3B8"
    borderWidth: 3
    borderDash: [8, 6]
    pointRadius: 6
    pointBackgroundColor: "#10958F"
    pointBorderColor: "#10958F"
    pointBorderWidth: 0
tension: 0.4
beginAtZero: true
yMin: 0
yMax: 8.5
xTickDisplay: false
yTickDisplay: false
legend: false
width: 50%
```
Monotonic nonlinear relationship: Spearman can be +1 while Pearson is below +1.

| Feature                     | Pearson                        | Spearman                  |
| --------------------------- | ------------------------------ | ------------------------- |
| Measures                    | Linear association             | Monotonic association     |
| Uses                        | Actual numeric values          | Ranks                     |
| Relationship can be curved? | May underestimate its strength | Captures monotonic curves |
| Outlier sensitivity         | Often higher                   | Often lower               |
| Suitable for ordinal data?  | Generally not preferred        | Yes                       |
| Value range                 | −1 to +1                       | −1 to +1                  |

### Important example: Neither correlation captures everything
Consider:
$$Y=X^2$$
For X values −3, −2, −1, 0, 1, 2, 3, Y first decreases and then increases.

There is a perfect mathematical relationship between X and Y, but neither Pearson nor Spearman captures it well because it is neither linear nor monotonic across the full range.

```chart
type: line
labels: [-3, -2, -1, 0, 1, 2, 3]
series:
  - title: Nonlinear Relationship
    data: [9, 4, 1, 0, 1, 4, 9]
    backgroundColor: "#28599A"
    borderColor: "#28599A"
    borderWidth: 2
    pointRadius: 5
    pointBackgroundColor: "#28599A"
    pointBorderColor: "#414141"
    pointBorderWidth: 2
tension: 0
beginAtZero: true
yMin: 0
yMax: 12
legend: false
width: 50%
```

Despite a deterministic relationship, both coefficients are zero for these symmetric sampled points.

Ask students:
=="Does correlation equal to zero mean X and Y have absolutely no relationship?"==
No. It means that the specific form of association measured by the coefficient is absent or weak.

This is one of the most important misconceptions to correct.