- A ML algorithm does not understand the real world. 
- It understands numbers, patterns, and mathematical relationships. 
- Feature engineering is ==how we translate real-world information into a representation that the algorithm can learn== from.


# What is FE?
Feature engineering is the process of transforming, creating, selecting, and representing variables so that a machine learning model can learn useful patterns from the data.

A _feature_ is an input variable used to make predictions. The _target_ is what we want to predict.

# Why does feature engineering matter so much?
Because the quality and representation of the inputs strongly influence what a model can learn.

### Five reasons it matters
1. It reveals useful relationships
2. It reduces irrelevant complexity
3. It makes features compatible with algorithms
4. It helps certain algorithms learn more effectively
5. It incorporates domain knowledge

```ad-note
Feature engineering does not always improve results. A poorly designed feature can introduce noise, overfitting, or data leakage.
```

# How does a computer or ML algorithm perceive features?
Humans interpret values based on meaning and experience.

A human sees:
`Age = 8`, `Sex = Female`, `class = 1`
and understands a young girl traveling in first class.

A typical machine learning algorithm sees a row of input values, such as:
$$[ X=[8,\ 1,\ 1]$$

# What are the different types of features?
### Numerical
- Discrete
- Cont

### Categorical
- Nominal → gender, city, colour
- Ordinal → low/ medium/ high, ranks
