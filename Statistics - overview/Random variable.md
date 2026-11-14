# Questions to be answered
- What is a Random variable?
- Examples of RV in different scenarios
- Types

---

# Introduction
In general, to analyze random experiments, we usually focus on some numerical aspects of the experiment. For example, in a soccer game we may be interested in the number of goals, shots, shots on goal, corners kicks, fouls, etc. If we consider an entire soccer match as a random experiment, then each of these numerical results gives some information about the outcome of the random experiment. These are examples of _random variables_. In a nutshell, a random variable is a real-valued variable whose value is determined by an underlying random experiment.
Let's look at an example.

# Definition
A **_random variable_**, usually written _X_, is a variable whose possible values are numerical outcomes of a random phenomenon. There are two types of random variables, _discrete_ and _continuous_.

## Examples
### Example 1
I toss a coin five times. This is a random experiment and the sample space can be written as
$$S=\{TTTTT,TTTTH,...,HHHHH\}$$
Note that here the sample space S has $2^5=32$ elements. Suppose that in this experiment, we are interested in the number of heads. We can define a random variable X whose value is the number of observed heads. The value of X will be one of 0,1,2,3,4 or 5 depending on the outcome of the random experiment.


In essence, _a random variable is a real-valued function that assigns a numerical value to each possible outcome of the random experiment_. For example, the random variable X defined above assigns the value 0 to the outcome TTTTT, the value 2 to the outcome THTHT, and so on. Hence, the random variable X is a function from the sample space S={TTTTT,TTTTH, ⋯ ,HHHHH} to the real numbers (for this particular random variable, the values are always integers between 0 and 5).

> [!definition] Random Variables:
> A random variable ($X$) is a function from the sample space to the real numbers.
>
> $$X:S\to\mathbb{R}$$

We usually show random variables by capital letters such as $X$, $Y$, and $Z$. Since a random variable is a function, we can talk about its _range_. The range of a random variable $X$, shown by $Range(X)$ or $R_X$, is the set of possible values for $X$. In the above example, $Range(X) =R_X=\{0,1,2,3,4,5\}$.

> [!definition]
> The range of a random variable $X$, shown by $Range(X)$ or $R_X$, is the set of possible values of $X$.

### Example 2
- **Weather:** Suppose we consider the weather in Bengaluru on a particular day as a random experiment. We may be interested in the maximum temperature, amount of rainfall, wind speed, or humidity. Each of these numerical measurements describes some aspect of the outcome and can be treated as a RV.
- **Hospital:** Consider the activity in a hospital emergency department during one day as a random experiment. We may record the number of patients admitted, their waiting times, or the number of emergency cases. Each of these quantities is a RV.
- **Online shopping:** Suppose we consider the orders received by an online store during one hour as a random experiment. We may be interested in the number of orders, total sales amount, number of cancelled orders, or average order value. These quantities are examples of RVs.
- **Manufacturing:** Consider the production of a batch of 1,000 components as a random experiment. We may record the number of defective components, their weights, or the time required to produce the batch. Each numerical result can be considered a RV.
- **Students:** Suppose we randomly select a student from a university. We may be interested in the student's exam score, number of courses taken, or number of hours studied per week. Each of these numerical quantities is a RV.

### Example questions
Find the range for each of the following random variables.
1. I toss a coin 100 times. Let X be the number of heads I observe.
2. I toss a coin until the first heads appears. Let Y be the total number of coin tosses.
3. The random variable T is defined as the time (in hours) from now until the next earthquake occurs in a certain city.

Solution:
1. The random variable $X$ can take any integer from $0$ to $100$, so $R_X=\{0,1,2,...,100\}$.
2. The random variable $Y$ can take any positive integer, so $R_Y=\{1,2,3,...\}=ℕ$.
3. The random variable $T$ can in theory get any non-negative real number, so $R_T=[0,∞)$.


# Types of Random variable
### Discrete Random Variables
- A discrete random variable  is one which may take on _only a countable number of distinct values_ such as 0,1,2,3,4,........ Discrete random variables are usually (but not necessarily) counts. 
- If a random variable can take only a finite number of distinct values, then it must be discrete. 
- Examples of discrete random variables include: 
	- the number of children in a family, 
	- the Friday night attendance at a cinema, 
	- the number of patients in a doctor's surgery, 
	- the number of defective light bulbs in a box of ten.
- The probability distribution of a discrete random variable is a list of probabilities associated with each of its possible values. It is also sometimes called the probability function or the _probability mass function_.

### Continuous Random Variables
- A continuous random variable is one which takes an infinite number of possible values. Continuous random variables are usually measurements. 
- Examples include: 
	- height, 
	- weight, 
	- the amount of sugar in an orange, 
	- the time required to run a mile.
- A continuous random variable is not defined at specific values. Instead, it is defined over an _interval_ of values, and is represented by the _area under a curve_ (in advanced mathematics, this is known as an _integral_). 
- The probability of observing any single value is equal to 0, since the number of values which may be assumed by the random variable is infinite.


# Highlights

>[!highlight] Key intuition 
>To analyze random experiments, we usually focus on some numerical aspects of the experiment. This numerical aspect is called ==Random variable==.
>
>A random variable is a function that maps the outcomes of a random experiment to real numbers.
>$$X:S\to\mathbb{R}$$

>[!highlight] 
> RV can be continuous or discrete.

> [!important]
> A random variable is a ==real-valued function== that assigns a numerical value to each possible outcome of the random experiment.

> [!important]
> The ==range== of a random variable $X$, shown by $Range(X)$ or $R_X$, is the set of possible values for $X$.


# References:
1. [3.1.1 Random variables](https://www.probabilitycourse.com/chapter3/3_1_1_random_variables.php)
2. [RV - Yale University](http://www.stat.yale.edu/Courses/1997-98/101/ranvar.htm)