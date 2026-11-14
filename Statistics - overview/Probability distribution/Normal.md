# Understanding Normal Distribution — With a Coffee Shop Example ☕
Imagine you own a coffee shop called Bean & Brew.
Every day, hundreds of customers visit your shop. You notice something interesting:
Some customers spend ₹100, some spend ₹200, and some spend ₹400. But most customers spend somewhere around ₹250.

You start wondering:
- Why do most customers spend around ₹250?
- Why do very few customers spend extremely little or extremely much?
- Can we predict what percentage of customers will spend between ₹200 and ₹300?
    
This is where the Normal Distribution becomes useful.

## What is a normal distribution?
A normal distribution describes a situation where most observations are close to the average, while observations further away from the average become increasingly rare.

Imagine observing the spending patterns of thousands of customers.
Suppose their spending follows an approximately normal distribution with an average of ₹250.


```chart
type: line
labels: ["₹75", "₹100", "", "₹150", "", "₹200", "", "₹250", "", "₹300", "", "₹350", "", "₹400", "₹425"]
series:
  - title: Customer spending
    data: [0, 1, 5, 14, 35, 68, 90, 100, 90, 68, 35, 14, 5, 1, 0]
    borderColor: "#3498DB"
    backgroundColor: "rgba(52, 152, 219, 0.12)"
    borderWidth: 4
    pointRadius: 0
fill: true
tension: 0.4
legend: false
beginAtZero: true
width: 60%
```

This curve is called a bell curve because of its shape.
The curve is highest at the average, and its two sides are symmetrical.

An important clarification: not every real-world measurement follows a normal distribution. Customer spending, for example, is often skewed because a small number of customers may spend unusually large amounts. We're using an idealized example to understand the concept.

## The two ingredients of a normal distribution
You only need two numbers to describe a normal distribution.

### A. Mean (μ) — Where is the center?
The mean is the average.

In our coffee shop:
$\mu$ = ₹250
It tells us where customer spending is centered.

### B. Standard deviation (σ) — How spread out is the data?
Suppose the standard deviation is ₹50.

$\sigma$ = ₹50
This gives us a way to measure how far spending typically varies from the average.

Compare 2 coffee shops
#### Shop A
```chart
type: line
labels: ["₹100", "₹125", "₹150", "₹175", "₹200", "₹225", "₹250", "₹275", "₹300", "₹325", "₹350", "₹375", "₹400"]
series:
  - title: Shop A
    data: [0, 0, 0.03, 1.11, 13.53, 60.65, 100, 60.65, 13.53, 1.11, 0.03, 0, 0]
    borderColor: "#298FDB"
    backgroundColor: "rgba(41,143,219,0.05)"
    borderWidth: 4
    pointRadius: 0
fill: false
tension: 0.4
legend: false
beginAtZero: true
width: 50%
```

Average: ₹250  
Standard deviation: ₹25
Spending is tightly clustered around ₹250.


#### Shop B
```chart
type: line
labels: ["₹100", "₹125", "₹150", "₹175", "₹200", "₹225", "₹250", "₹275", "₹300", "₹325", "₹350", "₹375", "₹400"]
series:
  - title: Shop B
    data: [7.2, 13.3, 21.9, 32.3, 42.6, 48.6, 50, 48.6, 42.6, 32.3, 21.9, 13.3, 7.2]
    borderColor: "#298FDB"
    backgroundColor: "rgba(41,143,219,0.05)"
    borderWidth: 4
    pointRadius: 0
fill: false
tension: 0.4
legend: false
beginAtZero: true
width: 50%
```

Average: ₹250  
Standard deviation: ₹75
Spending is more widely spread out.

Even though both shops have the same average spending, their customers behave differently.
Remember: Mean determines the center. Standard deviation determines the spread.

## The most important rule: 68–95–99.7
This is one of the most useful properties of the normal distribution.
If customer spending is normally distributed with an average of ₹250 and standard deviation of ₹50:

Normal Distribution — 68–95–99.7 Rule
```chart
type: line
labels: ["₹100", "", "₹150", "", "₹200", "", "₹250", "", "₹300", "", "₹350", "", "₹400"]
series:
  - title: 99.7% of customers
    data: [1.1, 4.4, 13.5, 32.5, 60.7, 88.2, 100, 88.2, 60.7, 32.5, 13.5, 4.4, 1.1]
    backgroundColor: "rgba(174, 213, 241, 0.35)"
    borderWidth: 0
    pointRadius: 0
    fill: true

  - title: 95% of customers
    data: [null, null, 13.5, 32.5, 60.7, 88.2, 100, 88.2, 60.7, 32.5, 13.5, null, null]
    backgroundColor: "rgba(91, 167, 220, 0.50)"
    borderWidth: 0
    pointRadius: 0
    fill: true

  - title: 68% of customers
    data: [null, null, null, null, 60.7, 88.2, 100, 88.2, 60.7, null, null, null, null]
    backgroundColor: "rgba(25, 118, 185, 0.65)"
    borderWidth: 0
    pointRadius: 0
    fill: true

  - title: Normal distribution
    data: [1.1, 4.4, 13.5, 32.5, 60.7, 88.2, 100, 88.2, 60.7, 32.5, 13.5, 4.4, 1.1]
    borderColor: "#258BD2"
    borderWidth: 4
    pointRadius: 0
    fill: false

tension: 0.4
fill: true
legend: false
beginAtZero: true
yMin: 0
yMax: 110
yDisplay: false
xTitle: "Amount spent (₹)"
width: 70%
```

68% of customers — Spend between ₹200 and ₹300 (within 1 SD).
95% of customers — Spend between ₹150 and ₹350 (within 2 SDs).
99.7% of customers — Spend between ₹100 and ₹400 (within 3 SDs).


## Example: What percentage of customers spend more than ₹300?
We know:
- Mean = ₹250
- Standard deviation = ₹50
- ₹300 is one standard deviation above the average.
    
Since approximately 68% of customers spend within one standard deviation of the mean, about 32% fall outside that central range.
Because the curve is symmetrical, half of that 32% lies on the higher-spending side.
$$P(X>300)\approx\frac{100\%-68\%}{2}$$
$$\approx16\%$$
Customers spending above ₹300 ~16% (Approximately 16 out of every 100 customers.)

## Understanding the Z-score
What if a customer spends ₹325? Is that unusually high?
A Z-score tells us how many standard deviations an observation is above or below the mean.
$$Z=\frac{X-\mu}{\sigma}$$
For ₹325:
$$Z=\frac{325-250}{50}=1.5$$
A Z-score of +1.5 means this customer spent 1.5 standard deviations above the average.
- Z = 0: Exactly average
- Z = +1: One standard deviation above average
- Z = −1: One standard deviation below average
- Z = +2: Two standard deviations above average


## How is normal distribution different from Binomial and Poisson?
You've now explored three important distributions.

|Distribution|What it describes|Coffee shop question|
|---|---|---|
|Binomial|Successes out of fixed opportunities|Out of 10 visitors, how many buy coffee?|
|Poisson|Number of events in an interval|How many customers arrive in an hour?|
|Normal|How values are spread around an average|How much do customers spend?|

There's another key difference: Binomial and Poisson describe discrete counts such as 0, 1, 2, or 3 customers. A normal distribution is continuous: it can describe measurements with values between any two amounts.

For a continuous distribution, the probability of an exact single value is zero. We calculate probabilities over ranges, represented by areas under the curve.

## When is normal distribution useful?
In business and daily life, approximately normal models may help with:
- Coffee preparation times: Understanding how long a typical order takes and how often unusually slow orders occur.
- Product quality: Monitoring variation in the amount of coffee dispensed by a machine.
- Manufacturing: Understanding how product measurements vary around a target.
- Student assessments: Describing examination scores when the score distribution is approximately bell-shaped.

Always check whether the data actually resembles a normal distribution before relying on its predictions.

## Takeaway
Normal distribution helps us understand variation around an average.

Think of it this way:
- Binomial: How many successes will I get?
- Poisson: How many events will occur?
- Normal: How far from the average will a measurement be?

For your students, the most important first lesson is that knowing the average is not enough. We also need to understand how observations spread around that average. That's the central idea behind the normal distribution.
