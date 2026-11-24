# Random Forest – Building Intuition Behind Ensemble Learning
## 1. Introduction: Why Do We Need Random Forest?
Imagine you want to predict whether a student will pass or fail an examination.
You have information about several students:
- Hours spent studying
- Attendance percentage
- Previous examination scores
- Number of assignments completed
- Participation in class

You want to build a Machine Learning model that learns patterns from historical student data and predicts the result for a new student.
One approach is to build a **Decision Tree**.

A Decision Tree might learn rules such as:
- Did the student study for more than 4 hours?
- Is attendance greater than 75%?
- Is the previous examination score greater than 60?

Based on these questions, the tree eventually predicts whether the student will pass or fail.

But here is an interesting question:
**What if we build another Decision Tree using the same problem? Will it make exactly the same predictions?**
Not necessarily!

_Different training samples, different features, or even small changes in the dataset can produce different trees._
One tree might predict that a student will pass, while another might predict that the same student will fail.

This brings us to an important question:
> _Instead of depending on one decision tree, what if we build multiple decision trees and combine their predictions?_

This is the fundamental intuition behind Random Forest.

### What is Random Forest?
Random Forest is a supervised Machine Learning algorithm that combines multiple Decision Trees to make a final prediction.
It belongs to a family of techniques called ==Ensemble Learning==.

The central idea is:
- Build multiple models.
- Allow each model to make a prediction.
- Combine their predictions.
- Produce a more reliable final prediction.

A collection of decision trees working together forms a Random Forest.

## 2. Quick Revision: Understanding Decision Trees
Before understanding Random Forest, we need to understand the building block used by it: the Decision Tree.

### What is a Decision Tree?
A Decision Tree is a supervised Machine Learning algorithm that makes predictions by asking a sequence of questions about the input features.

It resembles a flowchart.

For example, consider predicting whether someone will play cricket.
Features:
- Weather
- Temperature
- Humidity

A simplified decision tree could look like this:
```text
              Is Weather Sunny?
                  /       \
                Yes        No
                /           \
       Is Humidity High?    Play
           /      \
         Yes       No
         /          \
      Don't Play    Play
```

Each component has a specific meaning:

|Component|Meaning|
|---|---|
|Root Node|Starting point of the tree|
|Decision Node|A question about a feature|
|Branch|Outcome of a decision|
|Leaf Node|Final prediction|

### How does a Decision Tree learn?
A Decision Tree tries to find the best questions that separate the training data into meaningful groups.

For example:
Suppose we have the following dataset:

|Study Hours|Attendance|Result|
|---|---|---|
|1|40%|Fail|
|2|50%|Fail|
|3|60%|Fail|
|5|80%|Pass|
|6|90%|Pass|
|7|95%|Pass|

The tree may discover that study hours are useful for separating students who pass from those who fail.
It might create a rule:
```text
If Study Hours <= 3:
    Predict Fail
Else:
    Predict Pass
```

This is how a Decision Tree converts patterns in data into decision-making rules.

### How does a Decision Tree choose the best split?
The tree needs a way to measure how good a split is.

Two commonly used criteria are:
**1. Gini Impurity**
Gini impurity measures how mixed the classes are within a group.
- A group containing only one class has Gini impurity of 0.
- A group containing a mixture of classes has higher impurity.

Formula:

$$Gini= 1 - \sum_{i=1}^{C} p_i^2$$

Where:
- $C$ = Number of classes
- $p_i$ = Proportion of observations belonging to class $i$

**Example:**
Suppose a node contains:
- 5 Pass students
- 5 Fail students

Then:
$$Gini=1 - (0.5^2 + 0.5^2)= 0.5$$
Now suppose another node contains:
- 10 Pass students
- 0 Fail students
Then:
$$Gini=1 - (1^2 + 0^2) = 0$$
The second node is perfectly pure.

**2. Entropy**
Entropy measures the uncertainty or disorder in a group.
$$Entropy= -\sum_{i=1}^{C} p_i \log_2(p_i)$$
A node containing only one class has zero entropy.
A node containing equally distributed classes has maximum entropy.
The tree chooses splits that reduce impurity or uncertainty.

## 3. The Problem with a Single Decision Tree
Decision Trees are powerful, easy to understand, and capable of learning complex patterns.
However, they have an important weakness: **They can be unstable.**

### Understanding instability
Imagine two students are almost identical.
One student studies for 4.1 hours, while another studies for 4.2 hours.
A small change in the training data might cause the Decision Tree to learn a different splitting rule.
This can result in a significantly different tree.
This is related to a concept called **High Variance**.

### What is Variance in Machine Learning?
Variance describes how much a model's predictions change when it is trained on different datasets.
A high-variance model is sensitive to changes in training data.
Decision Trees, especially deep and unconstrained trees, can have high variance.

### Overfitting
Another common problem with Decision Trees is overfitting.
Imagine a tree that keeps asking questions until it perfectly classifies every training observation.

It may learn:
- Genuine patterns in the data.
- Random noise.
- Unimportant exceptions.
- Accidental relationships.

The model performs extremely well on training data but poorly on unseen data.
This is called overfitting.

### An analogy
Imagine asking one person to predict the weather for tomorrow.
That person might make a good prediction, but their opinion could be wrong.
Now imagine asking 100 independent weather experts.
If most of them agree, you may have more confidence in the combined prediction.

This gives us an important idea:
**Can combining multiple models reduce the weaknesses of an individual model?**

Yes! This is where Ensemble Learning comes in.

## 4. Ensemble Learning: The Foundation of Random Forest
### What is Ensemble Learning?
Ensemble Learning is a Machine Learning technique in which multiple models are combined to solve the same problem.
Instead of depending on one model, we use several models and combine their predictions.

The individual models are often called:
- Base learners
- Weak learners (in some ensemble methods)
- Estimators

The combined system is called an Ensemble Model.

### A simple example
Suppose we want to predict whether an email is spam.
We train five different models.

|Model|Prediction|
|---|---|
|Model 1|Spam|
|Model 2|Spam|
|Model 3|Not Spam|
|Model 4|Spam|
|Model 5|Not Spam|

Using majority voting:
- Spam: 3 votes
- Not Spam: 2 votes

Final prediction: **Spam**
Instead of trusting one model, we trust the combined decision.

### Types of Ensemble Learning
Three important ensemble techniques are:
1. Bagging
2. Boosting
3. Stacking

We will focus primarily on Bagging because Random Forest is based on it.

#### Bagging (Bootstrap Aggregating)
Bagging is an ensemble technique where multiple models are trained independently using different randomly sampled versions of the training dataset.

The process:
1. Start with the original training dataset.
2. Create multiple random samples from it.
3. Train a separate model on each sample.
4. Combine their predictions.

The models are trained independently and can be trained in parallel.

#### Boosting
Boosting builds models sequentially.
Each new model focuses more on the observations that previous models handled poorly.

Examples:
- AdaBoost
- Gradient Boosting
- XGBoost

#### Stacking
Stacking combines predictions from multiple different models using another model called a meta-model.

For example:
- Decision Tree
- Logistic Regression
- Support Vector Machine

Their predictions become inputs to another model that learns how to combine them.

### Comparison

|Technique|How Models Are Trained|How Predictions Are Combined|
|---|---|---|
|Bagging|Independently, in parallel|Voting or averaging|
|Boosting|Sequentially|Weighted combination|
|Stacking|Base models followed by a meta-model|Meta-model|

## 5. Understanding Bagging Through an Example
Let's understand Bagging before moving to Random Forest.
Suppose we have a dataset containing 10,000 student records.
We want to train five Decision Trees.
Instead of giving every tree exactly the same dataset, we create five different training datasets using random sampling with replacement.
This is called **Bootstrap Sampling**.

### What is Bootstrap Sampling?
Bootstrap sampling means randomly selecting observations from the original dataset, with replacement.

The important phrase is:
**With replacement.**

It means that after selecting an observation, we put it back into the dataset before selecting the next observation.

Therefore:
- The same observation can appear multiple times.
- Some observations may not appear in a particular sample.
- Each bootstrap sample can contain a different combination of observations.

### Example
Original dataset:
```
A, B, C, D, E
```

We create three bootstrap samples:
```
Sample 1: A, B, B, D, E

Sample 2: A, C, C, C, E

Sample 3: B, C, D, D, D
```

Notice:
- Sample 1 contains B twice.
- Sample 2 contains C three times.
- Sample 3 does not contain A or E.

Each sample has five observations, but their composition differs.

### Why sampling with replacement?
Because we want each model to see a slightly different version of the original dataset.
This introduces diversity among the models.

### Important concept: Out-of-Bag (OOB) Samples
Since bootstrap sampling happens with replacement, some observations are not selected for a particular model.
These unused observations are called Out-of-Bag samples.

For a dataset of size NN, a bootstrap sample of size NN contains approximately:
- 63.2% unique observations from the original dataset.
- 36.8% observations left out.

Why?
The probability that a particular observation is not selected in one draw is:
$$1 - \frac{1}{N}$$
The probability that it is not selected in any of the $N$ draws is:
$$\left(1-\frac{1}{N}\right)^N$$
As $N$ becomes large:
$$\left(1-\frac{1}{N}\right)^N \approx e^{-1}≈0.368$$
Therefore, approximately 36.8% of observations are left out of each bootstrap sample.
These observations can be used to estimate model performance without requiring a separate validation prediction from every training observation.

## 6. Random Forest: Bringing Everything Together
Now we have all the necessary building blocks:
- Decision Trees
- Ensemble Learning
- Bagging
- Bootstrap Sampling
Let's combine them.

### What is Random Forest?
Random Forest is an ensemble learning algorithm that builds multiple Decision Trees using bootstrap samples and random subsets of features.
Each tree makes its own prediction.
The predictions are then combined to produce the final output.

### Why is it called Random Forest?
Think of a forest.
A forest contains many trees.
Similarly, Random Forest contains many Decision Trees.

The word Random comes from the randomness introduced during:
1. Selection of training observations.
2. Selection of features considered for splitting.

This second point is what makes Random Forest different from ordinary Bagging with Decision Trees.

### How does Random Forest work?
Consider a dataset containing the following features:
- Age
- Income
- Education
- Credit Score
- Employment Duration
- Existing Loans

We want to predict whether a customer will default on a loan.
Imagine building 5 Decision Trees.

**Step 1: Create Bootstrap Samples**
Create five different bootstrap samples from the original training dataset.

**Step 2: Train Multiple Decision Trees**
Train one Decision Tree on each bootstrap sample.

**Step 3: Random Feature Selection**
When a tree needs to select a feature for splitting, it considers only a randomly selected subset of available features.

For example:

|Tree|Features Considered at a Split|
|---|---|
|Tree 1|Age, Income|
|Tree 2|Education, Credit Score|
|Tree 3|Income, Existing Loans|
|Tree 4|Age, Employment Duration|
|Tree 5|Credit Score, Existing Loans|

These are illustrative feature subsets, not necessarily fixed for the entire tree.
At another node, the same tree may consider a different subset.

**Step 4: Make Individual Predictions**
Each tree predicts whether the customer will default.

**Step 5: Combine Predictions**
For classification, use majority voting.
For regression, calculate the average prediction.
The final prediction comes from the combined output.

### Visualizing Random Forest

```
                 Original Dataset
                        |
             Bootstrap Sampling
                        |
          +-------------+-------------+
          |             |             |
       Sample 1      Sample 2      Sample 3
          |             |             |
       Decision       Decision      Decision
         Tree 1         Tree 2        Tree 3
          |             |             |
       Random         Random        Random
       Features       Features      Features
          |             |             |
      Prediction    Prediction    Prediction
          |             |             |
          +-------------+-------------+
                        |
                Combine Predictions
                        |
                 Final Prediction
```

## 7. Why Random Feature Selection Is Important
This is one of the most important concepts to understand in Random Forest.
Let's explore why it is necessary.

### What happens without random feature selection?
Imagine we have 100 features.
Suppose one feature is extremely predictive.
For example, when predicting house prices, the feature `Area` might be very informative.
If every tree considers all 100 features at every split, most trees may repeatedly select `Area`.
As a result, the trees can become very similar.
This reduces the diversity of the ensemble.

### What happens with random feature selection?
Instead of allowing every tree to consider all 100 features at every split, Random Forest selects a random subset.

For example:
- Tree 1 may consider Area, Location, Age, and Bedrooms.
- Tree 2 may consider Location, Bathrooms, and Construction Quality.
- Tree 3 may consider Bedrooms, Age, and Distance from the City.

The trees now have opportunities to discover different patterns.

### Why is diversity important?
Imagine asking 10 people the same question.
If all 10 people have the same background and use the same reasoning, they might make the same mistake.
But if they have different perspectives, combining their answers may produce a better decision.
The same intuition applies to Random Forest.
_The goal is not simply to build many trees. The goal is to build many reasonably accurate but diverse trees._

### Two sources of randomness in Random Forest

|Source of Randomness|Purpose|
|---|---|
|Bootstrap Sampling|Creates different training datasets|
|Random Feature Selection|Creates diversity in tree structures|
Together, these mechanisms help reduce the correlation between individual trees.

## 8. How Does Random Forest Make Predictions?
Random Forest handles classification and regression problems.

### A. Random Forest for Classification
Suppose we want to predict whether a customer will purchase a product.
We have trained five trees.

For a new customer, their predictions are:

|Tree|Prediction|
|---|---|
|Tree 1|Yes|
|Tree 2|No|
|Tree 3|Yes|
|Tree 4|Yes|
|Tree 5|No|

Voting:
- Yes = 3 votes
- No = 2 votes

Final prediction:
**Yes**

This is called _Majority Voting_.

Mathematically:
$$\hat{y} = \operatorname{mode}(T_1(x),T_2(x),...,T_n(x))$$
Where:
- $T_i(x)$ is the prediction from the $i^{th}$ tree.
- $n$ is the total number of trees.
- $\operatorname{mode}$ returns the most frequent prediction.

### B. Random Forest for Regression
Suppose we want to predict the price of a house.

Five trees predict:

|Tree|Predicted Price|
|---|---|
|Tree 1|₹45 lakh|
|Tree 2|₹48 lakh|
|Tree 3|₹50 lakh|
|Tree 4|₹47 lakh|
|Tree 5|₹55 lakh|

The final prediction is the average.
$$\hat{y} = \frac{1}{n}\sum_{i=1}^{n}T_i(x)$$
Therefore:
$$\hat{y} = \frac{45+48+50+47+55}{5}$$
$$\hat{y} = 49 \text{ lakh}$$
Final prediction: **₹49 lakh**

### Classification vs Regression

|Aspect|Classification|Regression|
|---|---|---|
|Output|Category/Class|Continuous number|
|Combination|Majority voting|Averaging|
|Example|Spam or Not Spam|House Price|
|Evaluation|Accuracy, Precision, Recall, F1|MAE, MSE, RMSE, R²|

## 9. Important Hyperparameters in Random Forest
Hyperparameters are settings that we choose before training the model.
Random Forest has several hyperparameters.
Let's understand the most important ones.

### 1. `n_estimators`
This determines the number of Decision Trees in the forest.
Example:
```python
RandomForestClassifier(n_estimators=100)
```
This creates 100 Decision Trees.

**What happens when we increase it?**
- More trees can improve prediction stability.
- Performance may improve initially.
- Training and prediction become more computationally expensive.
- Beyond a certain point, adding more trees may provide very little improvement.

Important intuition:
_More trees generally make the ensemble more stable, but they do not guarantee better generalization in every situation._

### 2. `max_depth`
This controls the maximum depth of each Decision Tree.
Example:
```python
RandomForestClassifier(max_depth=5)
```

A shallow tree:
- Has fewer decision rules.
- Is less complex.
- May underfit.

A deep tree:
- Can learn complex patterns.
- May overfit individual training samples.
- Can increase computational cost.

Remember that Random Forest combines trees, so individual trees can be allowed to grow deeply while the ensemble reduces variance.

### 3. `max_features`
This determines how many features are considered when searching for the best split.
Example:
```python
RandomForestClassifier(max_features="sqrt")
```

For classification, `sqrt` is a common default in scikit-learn.
If we have 100 features: $\sqrt{100}=10$

Approximately 10 randomly selected features are considered at each split.

Why is this important?
- Smaller subsets can increase diversity.
- Larger subsets can make individual trees stronger.
- The right balance depends on the dataset.

### 4. `min_samples_split`
Minimum number of samples required to split an internal node.

Example:
```python
RandomForestClassifier(min_samples_split=10)
```
A node needs at least 10 samples to be eligible for splitting.
Increasing this value generally produces simpler trees.

### 5. `min_samples_leaf`
Minimum number of samples required in a leaf node.

Example:
```python
RandomForestClassifier(min_samples_leaf=5)
```
Every leaf must contain at least 5 training observations.
This helps prevent trees from creating extremely small leaves.

### 6. `bootstrap`
Determines whether bootstrap samples are used to train individual trees.
```python
RandomForestClassifier(bootstrap=True)
```
- `True`: Sampling with replacement is used.
- `False`: Bootstrap sampling is disabled.

### 7. `oob_score`
Determines whether to calculate the Out-of-Bag score.
```python
RandomForestClassifier(
    bootstrap=True,
    oob_score=True
)
```
This provides an estimate of generalization performance using observations left out of individual bootstrap samples.

### Hyperparameter Summary

|Hyperparameter|Controls|General Effect of Increasing|
|---|---|---|
|n_estimators|Number of trees|More stability, more computation|
|max_depth|Maximum tree depth|More model complexity|
|max_features|Features considered per split|Potentially less diversity|
|min_samples_split|Minimum samples to split|Simpler trees|
|min_samples_leaf|Minimum samples per leaf|Smoother predictions|
|bootstrap|Bootstrap sampling|Boolean setting|
|oob_score|OOB evaluation|Boolean setting|

## 10. Implementing Random Forest Using Python
Let's implement Random Forest using Scikit-learn.
We will use the Iris dataset, a popular beginner-friendly classification dataset.
The goal is to predict the species of an Iris flower using its measurements.
### Step 1: Import Libraries
```python
import numpy as np
import pandas as pd

from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier

from sklearn.metrics import (
    accuracy_score,
    classification_report,
    confusion_matrix
)
```

### Step 2: Load the Dataset
```python
iris = load_iris()

X = iris.data
y = iris.target
```

Here:
- `X` contains the input features.
- `y` contains the target classes.

The four features are:
- Sepal length
- Sepal width
- Petal length
- Petal width

The target contains three Iris species.

### Step 3: Split the Dataset
```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```
We divide the dataset into:
- 80% training data
- 20% testing data

The training data is used to learn patterns.
The testing data is used to evaluate the model on unseen observations.

### Step 4: Create the Random Forest Model
```python
rf_model = RandomForestClassifier(
    n_estimators=100,
    max_depth=None,
    max_features="sqrt",
    random_state=42
)
```

Interpretation:
- `n_estimators=100`: Build 100 Decision Trees.
- `max_depth=None`: Trees can grow until other stopping conditions are reached.
- `max_features="sqrt"`: Consider approximately the square root of the available features at each split.
- `random_state=42`: Makes the randomization reproducible.

### Step 5: Train the Model
```python
rf_model.fit(X_train, y_train)
```

The model now learns from the training dataset.

Internally, it:
1. Creates bootstrap samples.
2. Builds multiple Decision Trees.
3. Randomly selects features at each split.
4. Stores the trained trees.

### Step 6: Make Predictions
```python
y_pred = rf_model.predict(X_test)
```
Each tree predicts the class, and the Random Forest combines their predictions.

### Step 7: Evaluate the Model
```python
accuracy = accuracy_score(y_test, y_pred)

print("Accuracy:", accuracy)
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
```

We can use:
- Accuracy
- Precision
- Recall
- F1-score
- Confusion Matrix
to understand the model's performance.

### Step 8: Feature Importance
One useful feature of Random Forest is that it provides feature importance scores.
```python
importance = rf_model.feature_importances_

for feature, score in zip(
    iris.feature_names,
    importance
):
    print(feature, score)
```
This helps us understand which features contributed most to reducing impurity across the trees.

Important note:
Feature importance does not necessarily imply causation. Also, impurity-based importance can favor certain types of features, such as continuous or high-cardinality features.

## 11. Advantages of Random Forest
### 1. Reduces Overfitting Compared to a Single Deep Tree
Combining multiple trees generally reduces variance and makes the model more stable.

### 2. Handles Non-Linear Relationships
Random Forest does not assume that relationships between features and the target must be linear.
It can learn complex decision boundaries.

### 3. Handles Classification and Regression
The same general approach can solve both types of supervised learning problems.

### 4. Works with Many Features
Random Forest can handle datasets with a relatively large number of input features.

### 5. Less Sensitive to Individual Training Observations
Because it uses multiple bootstrap samples, the ensemble is generally less dependent on any one observation than a single tree.

### 6. Provides Feature Importance
It provides a useful starting point for understanding which features are influential in the model.

### 7. Requires Less Feature Scaling
Unlike algorithms such as KNN, SVM, and Logistic Regression, Random Forest generally does not require standardization or normalization of numerical features.

## 12. Limitations of Random Forest
Random Forest is powerful, but it is not perfect.
### 1. Less Interpretable
A single Decision Tree can be visualized and understood relatively easily.
A forest containing hundreds of trees is much harder to interpret.

### 2. Higher Computational Cost
Training 100 or 500 trees requires more resources than training one Decision Tree.

### 3. Larger Memory Requirements
The model needs to store multiple trees.

### 4. Regression Predictions Are Limited by the Training Target Range
Because standard Random Forest regression averages predictions from trees, it generally cannot extrapolate beyond the target values represented in the training data.

### 5. Feature Importance Can Be Misleading
Impurity-based feature importance can be biased toward features with many possible split points.
Permutation importance can be a useful complementary technique.

### 6. Not Always the Best Choice
For some datasets, simpler models or other algorithms may perform better.
The correct approach is to compare models using appropriate validation techniques.

## 13. Decision Tree vs Random Forest
This is an important comparison to remember.

| Aspect             | Decision Tree                | Random Forest                     |
| ------------------ | ---------------------------- | --------------------------------- |
| Model structure    | Single tree                  | Multiple trees                    |
| Training           | One tree is trained          | Multiple trees are trained        |
| Random sampling    | Not inherently required      | Typically uses bootstrap sampling |
| Feature selection  | Considers available features | Considers random feature subsets  |
| Variance           | Can be high                  | Generally lower                   |
| Overfitting        | More prone to overfitting    | Generally more resistant          |
| Interpretability   | Easy to visualize            | Harder to interpret               |
| Computational cost | Lower                        | Higher                            |
| Prediction         | One tree's output            | Combined tree predictions         |
### Important intuition
A Decision Tree tries to learn a good set of rules.
Random Forest tries to learn many different sets of rules and combines their decisions.

## 14. Important Conceptual Questions
Use these questions to check whether students understand the algorithm rather than simply memorizing definitions.
### Question 1
Why does Random Forest use multiple Decision Trees instead of one?
**Expected answer:**
A single Decision Tree can have high variance and may overfit. Combining multiple trees can produce more stable and reliable predictions.

### Question 2
Why does Random Forest use bootstrap sampling?
**Expected answer:**
To create different training datasets for individual trees and introduce diversity into the ensemble.

### Question 3
What is the difference between Bagging and Random Forest?
**Expected answer:**
Bagging trains multiple models on bootstrap samples. Random Forest adds random feature selection at each split when using Decision Trees as base learners.

### Question 4
Why do we select random features at each split?
**Expected answer:**
To prevent all trees from repeatedly choosing the same dominant features and becoming too similar.

### Question 5
How does Random Forest make a classification prediction?
**Expected answer:**
By combining predictions from all trees using majority voting.

### Question 6
How does Random Forest make a regression prediction?
**Expected answer:**
By averaging the predictions from all trees.

### Question 7
Does increasing the number of trees always improve accuracy?
**Expected answer:**
No. It can improve stability and performance initially, but the improvement eventually becomes small. It also increases computation.

### Question 8
Why is Random Forest generally less prone to overfitting than an individual Decision Tree?
**Expected answer:**
Because combining multiple diverse trees helps reduce variance. However, Random Forest can still overfit in some circumstances.

### Question 9
Does Random Forest require feature scaling?
**Expected answer:**
Generally, no. Tree-based models are largely insensitive to the scale of numerical features.

### Question 10
Can Random Forest solve regression problems?
**Expected answer:**
Yes. RandomForestRegressor combines predictions by averaging the outputs of individual regression trees.

## 15. Summary: The Big Picture
Let's connect everything we learned.
```text
Problem with a Single Decision Tree
                |
                v
       High Variance / Overfitting
                |
                v
         Ensemble Learning
                |
                v
             Bagging
                |
                v
       Bootstrap Sampling
                |
                v
       Multiple Decision Trees
                |
                v
       Random Feature Selection
                |
                v
          Random Forest
                |
         +------+------+
         |             |
         v             v
   Classification   Regression
         |             |
         v             v
   Majority Vote    Averaging
         |             |
         +------+------+
                |
                v
         Final Prediction
```

### Five Things to Remember
1. Random Forest is an ensemble learning algorithm based on Decision Trees.
2. It uses bootstrap sampling to create different training datasets.
3. It uses random feature selection to introduce diversity among trees.
4. For classification, it combines predictions using majority voting.
5. For regression, it combines predictions using averaging.

### Final Takeaway
> _A single Decision Tree makes a decision based on one learned structure. A Random Forest combines the decisions of many diverse trees to produce a more stable prediction._
The real strength of Random Forest is not simply the number of trees. It is the combination of multiple reasonably strong and diverse trees.

