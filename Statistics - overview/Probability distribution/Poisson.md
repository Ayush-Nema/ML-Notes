# Poisson distribution
Imagine you own a coffee shop called Bean & Brew.
Your shop opens at 8 AM. Customers arrive throughout the day, but you don't know exactly when each person will walk through the door.

After observing your shop for several weeks, you discover something interesting:
> On average, 6 customers arrive every hour.

Now you start wondering:
- What's the probability that exactly 4 customers arrive in the next hour?
- What's the probability that nobody arrives in the next 30 minutes?
- Could 10 customers suddenly arrive in one hour?
- How likely is it that more customers arrive than your barista can handle?

These are precisely the questions that the Poisson distribution helps answer.

## What is a Poisson distribution?
A Poisson distribution is a probability distribution that tells us *how likely a particular number of events is to occur within a fixed interval of time or space, given that we know the average event rate*.

In simple terms:
> _Poisson distribution = Counting how many times something happens within an interval._

An important distinction: 6 customers per hour is an average, not a guarantee.
One hour might bring 3 customers, the next 8, and another only 1. Poisson distribution gives us the probabilities of these different counts.

## The Poisson distribution formula
$$\boxed{P(X=k)=\frac{e^{-\lambda}\lambda^k}{k!}}$$
It may look complicated, but each part has a simple meaning.

| Symbol    | Meaning in our coffee shop                   |
| --------- | -------------------------------------------- |
| $X$       | Number of customers arriving                 |
| $k$       | Specific arrival count we're interested in   |
| $\lambda$ | Average arrivals during the interval         |
| $e$       | Mathematical constant, approximately 2.71828 |
| $k!$      | Factorial of \(k\), e.g. \(4!=24\)           |
For our shop, the average is $\lambda=6$ customers per hour.

We write:
$$X\sim\operatorname{Poisson}(6)$$
This means that the number of customer arrivals in an hour follows a Poisson distribution with an expected count of 6.

## Example: Exactly 4 customers arrive in one hour
Our question:
> What is the probability that exactly 4 customers arrive between 8 AM and 9 AM?

We know:
- Average arrivals: $\lambda=6$
- Desired number of arrivals: $k=4$

Substitute into the formula:
$$P(X=4)=\frac{e^{-6}6^4}{4!}$$
Breaking it down:
$$e^{-6}\approx0.002479$$
$$6^4=1296$$
$$4!=4\times3\times2\times1=24$$
Therefore:
$$P(X=4)=\frac{0.002479\times1296}{24}$$

$$P(X=4)\approx0.13385$$
Probability of exactly 4 arrivals = 13.39%




## What does the full Poisson distribution look like?
Rather than looking at only 4 arrivals, let's see how likely different arrival counts are when the average is 6 per hour.

Customer arrivals per hour
Poisson distribution with an average rate of 6 arrivals per hour.
```chart
type: bar
labels: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
series:
  - title: Probability (%)
    data: [0.25, 1.49, 4.46, 8.92, 13.39, 16.06, 16.06, 13.77, 10.33, 6.88, 4.13]
    backgroundColor: "#28569A"
    borderRadius: 6
legend: false
beginAtZero: true
yMin: 0
yMax: 20
width: 60%
```

The probability is highest around 5–6 arrivals, then becomes smaller for unusually high or low counts. Values above 12 are omitted from this chart.

Here's a more detailed look:

| Customers arriving | Probability |
| ------------------ | ----------- |
| 0                  | 0.25%       |
| 1                  | 1.49%       |
| 2                  | 4.46%       |
| 3                  | 8.92%       |
| 4                  | 13.39%      |
| 5                  | 16.06%      |
| 6                  | 16.06%      |
| 7                  | 13.77%      |
| 8                  | 10.33%      |
| 9                  | 6.88%       |
| 10                 | 4.13%       |
The interesting observation is that even though the average is 6, both 5 and 6 arrivals are equally the most likely individual counts.

## Changing the time interval
This is one of the most useful properties of Poisson distribution.

Suppose customers arrive at an average rate of 6 per hour. What happens if we look at only 30 minutes, or at 2 hours?
The average count scales with time:

$$\lambda_{\text{interval}}=\text{Rate}\times\text{Duration}$$
For 30 minutes, the average number of arrivals is:
$$\lambda=6\times\frac{30}{60}=3$$
So the probability of exactly 4 customers arriving in 30 minutes is:
$$P(X=4)=\frac{e^{-3}3^4}{4!}$$
$$=\boxed{16.80\%}$$
Notice that the same count of 4 customers has a different probability because we changed the observation interval.


## Example: What if no customers arrive?
Imagine your barista wants to know:
> What is the probability that absolutely nobody enters the coffee shop during the next 30 minutes?

For 30 minutes, $\lambda=3$
We want $P(X=0)$.
$$P(X=0)=\frac{e^{-3}3^0}{0!}$$
Since $3^0=1$ and $0!=1$:
$$P(X=0)=e^{-3}$$
$$=\boxed{4.98\%}$$
So there's roughly a 5% chance of a completely customer-free half hour.
This is a useful property: the probability of zero events in a Poisson model is simply $e^{-\lambda}$.

## A practical business problem: Will the barista be overwhelmed?
Suppose a single barista can comfortably serve up to 8 arriving customers per hour, and your store averages 6 arrivals per hour.
You want to know the probability of more than 8 arrivals in an hour.

Mathematically:
$$P(X>8)=1-P(X\le8)$$
We add the probabilities from 0 through 8, then subtract from 1.
$$P(X>8)\approx\boxed{15.26\%}$$
That means in roughly 15 out of 100 hours, more than 8 customers may arrive.


## The mean and variance of a Poisson distribution
One especially elegant characteristic of the Poisson distribution is:
$$\boxed{\text{Mean}=\text{Variance}=\lambda}$$
And the standard deviation is:
$$\sigma=\sqrt{\lambda}$$
For our coffee shop, when $\lambda=6$:

|Measure|Value|
|---|---|
|Mean|6 customers|
|Variance|6 customers²|
|Standard deviation|2.45 customers|

The standard deviation gives a sense of how much the observed number of arrivals tends to fluctuate around its average.

Note that this equal mean–variance relationship is an important assumption to assess against real-world data. If customer arrivals are far more variable than this model predicts, a basic Poisson model may not fit well.

## Poisson vs. Binomial distribution
Since you've already explored the binomial distribution, here's the key difference.

|Feature|Binomial|Poisson|
|---|---|---|
|Main question|How many successes?|How many events occur?|
|Trials|Fixed number|No fixed trial count|
|Parameters|\(n,p\)|\(\lambda\)|
|Example|How many of 10 visitors buy coffee?|How many visitors arrive in an hour?|
|Possible values|0 through \(n\)|0, 1, 2, 3, …|
|Mean|\(np\)|\(\lambda\)|
|Variance|\(np(1-p)\)|\(\lambda\)|

##### Binomial
10 people enter. Each has a 60% chance of buying a cappuccino.
How many buy?

##### Poisson 
Customers arrive at an average rate of 6 per hour.
How many arrive?

## When can we use Poisson distribution?
The standard Poisson model works when we can reasonably assume that:
1. Events happen independently.
2. The average event rate stays constant over the interval.
3. The likelihood of two or more events in a very tiny interval becomes negligible as the interval shrinks.
4. Events can be counted as distinct occurrences.
    
For a coffee shop, the constant-rate assumption may fail during morning rush hours. You might need separate arrival rates for 8–9 AM and 2–3 PM rather than using one all-day average.

## Takeaway
Think of the two distributions using these two coffee shop questions:
- Binomial: "Of the next 10 customers who enter, how many will buy a cappuccino?"
- Poisson: "How many customers will enter my coffee shop in the next hour?"

Both count occurrences, but the first starts with a fixed number of opportunities, whereas the second starts with a fixed observation interval and an average event rate.