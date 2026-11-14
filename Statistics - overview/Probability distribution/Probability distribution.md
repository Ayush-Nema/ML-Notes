# Introduction
A probability distribution is simply a way of showing:
> *What values can a Random Variable take, and how likely is each value?*

> *What are the possible outcomes, and how likely is each outcome to happen?*

### Example #1
Suppose you roll a die.
The random variable $X$ = number you roll.

Possible values are:
$$X=1,2,3,4,5,6$$
Each has probability $1/6$.

So the probability distribution is:

| X   | Probability |
| --- | ----------- |
| 1   | 1/6         |
| 2   | 1/6         |
| 3   | 1/6         |
| 4   | 1/6         |
| 5   | 1/6         |
| 6   | 1/6         |
That's a _probability distribution_.

In one sentence
> _A probability distribution is a map between possible outcomes and their probabilities._

So think:
Random variable → possible values → probability of each value.


### Example #2
Imagine you own a small coffee shop, and you want to predict how many customers will visit your shop in the first hour of the morning.

You cannot predict the exact number every day, but based on past experience, you can estimate how likely different numbers of customers are.
This is exactly where probability distribution comes in.

Suppose you observed customer visits over the first hour on 100 mornings.

| Number of customers | Number of mornings | Probability |
| ------------------- | ------------------ | ----------- |
| 0                   | 5                  | 5%          |
| 1                   | 15                 | 15%         |
| 2                   | 30                 | 30%         |
| 3                   | 25                 | 25%         |
| 4                   | 15                 | 15%         |
| 5                   | 10                 | 10%         |
| Total               | 100                | 100%        |
This table is a probability distribution (more precisely, an empirical discrete probability distribution).

```chart
type: bar
labels: [1, 2, 3, 4, 5, 6]
series:
  - title: Frequency
    data: [2, 6, 11, 8, 5, 3]
    backgroundColor: "#3999CC"
legend: false
beginAtZero: true
width: 50%
```

#### What does this graph tell us?
- 2 customers is the most likely outcome (30%). 
- 0 customers is quite unlikely (5%).
- 3 customers has a 25% probability.
- The probability of getting 2 or 3 customers is 30% + 25% = 55%.

Notice something important: the probabilities of all possible outcomes add up to 100%.
That is the fundamental rule of probability distributions.

# How is this useful in real life?
Imagine you need to decide how many cups of coffee to prepare in advance for that first hour.
If you prepare 5 cups, some might be wasted.
If you prepare only 1 cup, you might keep customers waiting.
Using your probability distribution, you can make a better decision.

For example, let's calculate the expected number of customers:
$$E(X)=\sum xP(X=x)$$

Don't worry about the formula. It simply means:
Multiply each possible number of customers by its probability, then add everything.
$$E(X)=0(0.05)+1(0.15)+2(0.30)+3(0.25)+4(0.15)+5(0.10)=2.6$$
So, on average, you expect 2.6 customers per morning hour.
This doesn't mean exactly 2.6 people will arrive. It means that across many mornings, your average will tend to be around 2.6, assuming the pattern stays similar.

```chart

```

# Two main types of probability distributions
#### Discrete distribution
Used when outcomes can be counted.
Examples: Number of customers, defective products, or calls received.
```chart
type: bar
labels: [1, 2, 3, 4, 5, 6]
series:
  - title: Frequency
    data: [2, 6, 11, 8, 5, 3]
    backgroundColor: "#3999CC"
legend: false
beginAtZero: true
width: 50%
```

#### Continuous distribution
Used when outcomes can take any value within a range.
Examples: Customer waiting time, height, temperature.

```chart
type: line
labels: [-3, -2.5, -2, -1.5, -1, -0.5, 0, 0.5, 1, 1.5, 2, 2.5, 3]
series:
  - title: Normal Distribution
    data: [1, 4, 14, 32, 61, 88, 100, 88, 61, 32, 14, 4, 1]
    borderColor: "#22A575"
    backgroundColor: "rgba(34,165,117,0.20)"
    pointRadius: 0
fill: true
tension: 0.4
legend: false
beginAtZero: true
width: 50%
```


One important difference: in a discrete distribution, you can assign probabilities to exact values (like exactly 2 customers). In a continuous distribution, probabilities are calculated over ranges (like waiting between 2 and 5 minutes).


Using our coffee shop example:
What is the probability that at least 3 customers will visit in the first hour?
- 25%
- 40%
- 50%
- 55%
Answer: 50% 
"At least 3" means 3, 4, or 5 customers.
So, 25% + 15% + 10% = 50%

## ## The big picture
Think of it this way:
- Probability: What is the chance that exactly 3 customers arrive? → 25%.
- Probability distribution: What are the chances of every possible number of customers arriving? → The entire table or graph.
- Expected value: How many customers do we expect on average? → 2.6.

In one sentence: _A probability distribution is a mathematical way of describing uncertainty by showing how likely different outcomes are._

