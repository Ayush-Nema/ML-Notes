# Binomial distribution
Every day, customers walk in, browse your menu, and decide whether to buy a cappuccino.
Some buy one. Others don't.

Now imagine you know that 60% of customers typically buy a cappuccino.
A natural question arises:
> If the next 5 customers enter your shop, what is the probability that exactly 3 will buy a cappuccino?

This is exactly the kind of question that the binomial distribution helps us answer.

## What is binomial distribution?
A binomial distribution is a probability distribution that tells us _how likely we are to observe a certain number of successes in a fixed number of independent attempts, when each attempt has the same probability of success._
In statistics, success simply means the outcome we're interested in. It doesn't necessarily mean something good.

### 4 conditions for BD
We can use the binomial distribution when four conditions hold:
1. Fixed number of trials: We observe exactly 5 customers.
2. Two possible outcomes: Each customer either buys a cappuccino or doesn't.
3. Constant probability: Each customer has the same 60% chance of buying.
4. Independent trials: One customer's decision doesn't affect another's.

These last two conditions are assumptions. In a real coffee shop, friends may influence each other, and purchasing probabilities may change throughout the day.

### Let's calculate a probability
Our question:
> What is the probability that exactly 3 out of 5 customers buy a cappuccino?

Imagine this sequence:

| ☕   | ☕   | ✕   | ☕   | ✕   |
| --- | --- | --- | --- | --- |
| #1  | #2  | #3  | #4  | #5  |
3 purchases, 2 non-purchases

The probability of this particular order is:
$$0.6\times0.6\times0.4\times0.6\times0.4$$
$$=0.6^3\times0.4^2=0.03456$$
That's 3.456%.
But here's the important insight: there are many different ways for exactly 3 of the 5 customers to buy cappuccinos.
For example:
- ☕ ☕ ☕ ✕ ✕
- ☕ ✕ ☕ ✕ ☕
- ✕ ☕ ☕ ☕ ✕

All of these have exactly 3 purchases.
In fact, there are $\binom{5}{3}=10$ such arrangements.

Therefore:
$$P(X=3)=10\times0.03456$$
Probability of exactly 3 purchases = 34.56%

### The complete binomial distribution
Instead of calculating only 3 purchases, let's explore every possible outcome from 0 to 5.

The mathematical notation is:
$$X\sim\mathrm{Binomial}(n=5,\ p=0.6)$$

Here:
- $X$ = number of customers who buy cappuccinos
- $n=5$ = number of customers
- $p=0.6$ = probability of purchase per customer

Experiment with the visualization. Change the probability from 60% to 20%, and notice how the likely purchase counts shift toward zero. Then try 90% and see how the distribution shifts toward five.

For our coffee shop, the complete probabilities are:

| Cappuccinos sold | Probability |
| ---------------- | ----------- |
| 0                | 1.024%      |
| 1                | 7.680%      |
| 2                | 23.040%     |
| 3                | 34.560%     |
| 4                | 25.920%     |
| 5                | 7.776%      |
| Total            | 100%        |
Notice that 3 cappuccinos is the most likely single outcome, but there's still a considerable chance of selling either 2 or 4.

## The binomial distribution formula
The general formula is:
$$\boxed{P(X=k)=\binom nk p^k(1-p)^{n-k}}$$

Each piece has a practical meaning:

| Symbol      | Coffee shop meaning                          |
| ----------- | -------------------------------------------- |
| $n$         | Total customers                              |
| $k$         | Number who buy cappuccinos                   |
| $p$         | Probability of buying                        |
| $1-p$       | Probability of not buying                    |
| $\binom nk$ | Number of possible arrangements of purchases |

The combination term is:
$$\binom nk=\frac{n!}{k!(n-k)!}$$

For our example:
$$P(X=3)=\binom 53(0.6)^3(0.4)^2$$
$$=10\times0.216\times0.16=0.3456$$
The formula combines the probability of one particular pattern with the number of patterns that satisfy our requirement.

## What about "at least" or "at most"?
Coffee shop owners rarely ask only about exactly 3 purchases.
Suppose you are planning inventory and want to know the probability of selling at least 4 cappuccinos to your next 5 customers.

"At least 4" means either 4 or 5.
$$P(X\ge4)=P(X=4)+P(X=5)$$
$$=0.2592+0.07776$$
$$=\boxed{33.696\%}$$

Or suppose you want the probability of selling at most 2.
"At most 2" means 0, 1 or 2.
$$P(X\le2)=0.01024+0.0768+0.2304$$
$$=\boxed{31.744\%}$$

## Expected value and standard deviation
There's another powerful use for the binomial distribution: predicting average sales and measuring uncertainty.
### Expected value (mean)
$$E[X]=np$$
For the next 5 customers:
$$E[X]=5\times0.6=\boxed{3}$$
This means that across many groups of 5 customers, the shop will average 3 cappuccino purchases per group.
It doesn't mean every group will buy exactly 3.

### Standard deviation
$$\sigma=\sqrt{np(1-p)}$$
$$=\sqrt{5\times0.6\times0.4}$$
$$\approx\boxed{1.095}$$
This tells us how much the number of purchases typically varies around the expected value of 3.