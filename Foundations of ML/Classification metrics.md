Imagine you built an **AI model that detects whether an email is SPAM or NOT SPAM.** 📧
We'll use this one example to understand all four terms: **Precision, Recall, Accuracy, and Specificity.**

## **Step 1: Understand the situation**
Suppose you have **100 emails**.
- 40 emails are actually SPAM.
- 60 emails are actually NOT SPAM.

Your AI model makes these predictions:

|                        | Actually SPAM | Actually NOT SPAM |
| ---------------------- | ------------- | ----------------- |
| **Predicted SPAM**     | 30 (TP)       | 10 (FP)           |
| **Predicted NOT SPAM** | 10 (FN)       | 50 (TN)           |
This table is called a **Confusion Matrix**.

Four important terms:
- **TP (True Positive):** Spam correctly identified as spam.
- **FP (False Positive):** Normal email incorrectly identified as spam.
- **FN (False Negative):** Spam incorrectly identified as normal.
- **TN (True Negative):** Normal email correctly identified as normal.

> [!Remember]
> - `True` = correct prediction
> - `False` = incorrect prediction 
> - `Positive` = SPAM
> - `Negative` = NOT SPAM

## Step 2: Understand the four metrics
## 1. Precision
> [!note]
> _When the model says SPAM, how often is it correct?_

The model predicted 40 emails as spam, but only 30 were actually spam.
$$
\boxed{\text{Precision} = \frac{TP}{TP + FP}}
$$
$$
\text{Precision} = \frac{30}{30 + 10} = 75\%
$$
**Meaning:** Out of every 100 emails predicted as spam, 75 would actually be spam.

## 2. Recall (Sensitivity)
> [!note]
> *Out of all actual SPAM emails, how many did the model catch?*

There were 40 spam emails, but the model caught only 30.
$$
\boxed{\text{Recall} = \frac{TP}{TP + FN}}
$$
$$
\text{Recall} = \frac{30}{30 + 10} = 75\%
$$
**Meaning:** The model catches 75% of all spam emails and misses 25%.

## 3. Accuracy
> [!note]
> *Out of all emails, how many did the model classify correctly?*

The model correctly classified 30 spam emails and 50 normal emails.
$$
\boxed{\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}}
$$
$$
\text{Accuracy} = \frac{30 + 50}{30 + 50 + 10 + 10} = 80\%
$$
**Meaning:** The model makes correct predictions 80% of the time.

## 4. Specificity
> [!note]
> *Out of all actual NOT SPAM emails, how many did the model correctly recognize?*

There were 60 normal emails, and the model correctly identified 50.
$$
\boxed{\text{Specificity} = \frac{TN}{TN + FP}}
$$
$$
\text{Specificity} = \frac{50}{50 + 10} = 83.33\%
$$

**Meaning:** The model correctly recognizes 83.33% of normal emails.

## Step 3: Visual comparison
Performance of our spam detection model

| Metric          |  Score | Visual Representation |
| --------------- | -----: | --------------------- |
| **Precision**   |    75% | 🟦🟦🟦🟦🟦🟦🟦⬜⬜⬜     |
| **Recall**      |    75% | 🟦🟦🟦🟦🟦🟦🟦⬜⬜⬜     |
| **Accuracy**    |    80% | 🟩🟩🟩🟩🟩🟩🟩🟩⬜⬜    |
| **Specificity** | 83.33% | 🟧🟧🟧🟧🟧🟧🟧🟧⬜⬜    |

Note:
Each square represents approximately 10%. The visual bars are rounded representations; refer to the percentages for exact values.

## Step 4: Easy trick to remember

| Metric          | Remember this question                                      |
| --------------- | ----------------------------------------------------------- |
| **Precision**   | Of what I predicted as positive, how much was correct?      |
| **Recall**      | Of all actual positives, how many did I find?               |
| **Accuracy**    | How many total predictions were correct?                    |
| **Specificity** | Of all actual negatives, how many did I identify correctly? |
>[!important]
> A very useful distinction:
> *==Precision== focuses on false alarms (FP). 
> ==Recall== focuses on missed positives (FN). 
> ==Specificity== focuses on correctly identifying negatives (TN).*

## Step 5: Test your understanding
### Question
An AI model detects cancer from medical scans.
There are 100 patients who actually have cancer, but the AI correctly detects cancer in only 90 patients.

Q. Which metric tells us that the AI detected 90% of actual cancer cases?
- [ ] A. Precision
- [x] B. Recall
- [ ] C. Accuracy
- [ ] D. Specificity

> [!SUCCESS] Correct Answer: B. Recall (Sensitivity)
> Recall measures how many actual positive cases are successfully detected by the model.

### Detailed Explanation
#### 1. Identify the Positive Class
In this example:
- **Positive (P):** Patient has cancer.
- **Negative (N):** Patient does not have cancer.
#### 2. Understand the Given Information
We know:
- Total patients who actually have cancer = 100
- Patients correctly detected with cancer = 90
- Patients with cancer incorrectly classified as healthy = 10

Therefore:

| Term | Value | Explanation |
|---|---:|---|
| True Positive (TP) | 90 | Cancer patients correctly detected |
| False Negative (FN) | 10 | Cancer patients incorrectly classified as healthy |

#### 3. Apply the Recall Formula
$$
\text{Recall} = \frac{TP}{TP + FN}
$$
Substitute the values:
$$
\text{Recall} = \frac{90}{90 + 10} = 0.90
$$
$$
\boxed{\text{Recall} = 90\%}
$$
#### 4. Interpret the Result
The model successfully identifies **90 out of every 100 patients who actually have cancer**.
However, it misses **10 out of every 100 cancer patients**.
These 10 missed patients are called **False Negatives (FN)**.

### Why Is Recall Important in Cancer Detection?
In cancer detection, missing an actual cancer case can have serious consequences.
Consider two models:

| Metric | Model A | Model B |
|---|---:|---:|
| Actual cancer patients | 100 | 100 |
| Cancer patients detected (TP) | 90 | 98 |
| Cancer patients missed (FN) | 10 | 2 |
| Recall | 90% | 98% |

**Model B has higher recall** because it misses fewer cancer patients.

> [!IMPORTANT]
> In medical screening, high recall (sensitivity) is often prioritized because identifying as many actual disease cases as possible is important. However, precision and specificity also matter because false alarms can lead to unnecessary tests, anxiety, and costs.

### Why Not Precision, Accuracy, or Specificity?
**Precision:**
Precision answers: "Of all patients the model predicted as having cancer, how many actually have cancer?"
$$
\text{Precision} = \frac{TP}{TP + FP}
$$
We cannot calculate precision because the number of false positives (FP) is not provided.

**Accuracy:**
Accuracy answers: "Out of all patients, how many were classified correctly?"
$$
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
$$
We cannot calculate accuracy because we do not know TN and FP.

**Specificity:**
Specificity answers: "Out of all patients who do not have cancer, how many were correctly classified as healthy?"
$$
\text{Specificity} = \frac{TN}{TN + FP}
$$
We cannot calculate specificity because TN and FP are not provided.


## Final Takeaway

| Metric          | Main Question                                                           |
| --------------- | ----------------------------------------------------------------------- |
| **Precision**   | When the model predicts positive, how often is it correct?              |
| **Recall**      | Out of all actual positives, how many did the model identify?           |
| **Accuracy**    | Out of all predictions, how many were correct?                          |
| **Specificity** | Out of all actual negatives, how many did the model correctly identify? |

> [!TIP] Easy Way to Remember
> - **Precision:** How trustworthy are positive predictions?
> - **Recall:** How many actual positives did we catch?
> - **Accuracy:** How often are we correct overall?
> - **Specificity:** How many actual negatives did we correctly identify?