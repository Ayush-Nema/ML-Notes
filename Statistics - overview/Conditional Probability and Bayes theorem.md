Conditional probability and Bayes’ theorem are closely related, but they answer **different kinds of questions**.

The shortest distinction is:

- **Conditional probability:** “Given that I know \(B\) happened, what is the probability of \(A\)?”
- **Bayes’ theorem:** “I know how likely \(B\) is when \(A\) happens. After observing \(B\), what should I believe about \(A\)?”

The key difference is that **Bayes’ theorem reverses the direction of conditioning**.

---

# 1. Conditional probability

Conditional probability means:

> What is the probability of event \(A\), **given that event \(B\) is already known to have happened?**

It is written

\[ P(A\mid B) \]

and read:

> “Probability of \(A\) given \(B\).”

Its definition is:

\(P(A|B) = \frac{P(A \cap B)}{P(B)}\)

\(P(A\mid B)=\frac{P(A\cap B)}{P(B)}=\frac{\text{0.30}}{\text{0.60}}=\text{0.50}\)

\(P(B)\)

\(P(A\cap B)\)

P(B)=0.60P(A∩B)=0.30P(A|B) ≈ 0.50A∩B is the part of B where A also happens

Give feedback

Conceptually,

\[ P(A\mid B) = \frac{\text{cases where both A and B happen}} {\text{cases where B happens}} \]

The important idea is that **once you know \(B\) happened, you restrict your universe to only the \(B\) cases**.

### Example

Suppose a class has 100 students:

||Plays cricket|Doesn't play cricket|Total|
|---|---|---|---|
|Boys|30|20|50|
|Girls|10|40|50|
|Total|40|60|100|

Let

\[ A = \text{student plays cricket} \]

and

\[ B = \text{student is a boy}. \]

What is

\[ P(A\mid B)? \]

The words are:

> Given that the student is a boy, what is the probability that he plays cricket?

Since we already know the student is a boy, we **ignore all 50 girls**.

Among the 50 boys, 30 play cricket.

Therefore,

\[ P(A\mid B)=\frac{30}{50}=0.6. \]

So the answer is **60%**.

Notice what happened: conditional probability simply **restricted the sample space**.

---

# 2. What kinds of questions does conditional probability answer?

Conditional probability typically appears when a question gives you some information and asks for a probability **within that group**.

For example:

> Among students who passed mathematics, what percentage passed physics?

That asks

\[ P(\text{Physics pass}\mid\text{Math pass}). \]

Or:

> Given that a card is a face card, what is the probability it is a king?

That asks

\[ P(\text{King}\mid\text{Face card}). \]

Or:

> Given that it is raining, what is the probability there is heavy traffic?

That asks

\[ P(\text{Traffic}\mid\text{Rain}). \]

Or:

> Among people who exercise regularly, what proportion have low blood pressure?

That asks

\[ P(\text{Low BP}\mid\text{Exercises}). \]

In all of these, the structure is simply:

\[ \boxed{\text{Given B, what is the probability of A?}} \]

---

# 3. Then what is Bayes' theorem?

Bayes' theorem also calculates a conditional probability, but it is especially useful when the conditional probability you want is **the reverse of the one you know**.

Suppose you know

\[ P(B\mid A) \]

but you actually want

\[ P(A\mid B). \]

These are **not the same thing**.

Bayes' theorem lets you go from one direction to the other.

\(P(A|B) = \frac{P(B|A)P(A)}{P(B)}\)

\(P(A\mid B)=\frac{0.85(0.20)}{0.25}=0.68\)

\(P(A)\)

\(P(B\mid A)\)

\(P(B\mid \neg A)\)

\(P(B)\)= 0.25

\(P(B\cap A)\)= 0.17

\(P(B\cap \neg A)\)= 0.08

Give feedback

This is the central purpose of Bayes' theorem:

\[ \boxed{P(B\mid A)\quad\longrightarrow\quad P(A\mid B)} \]

with the help of some additional probabilities.

---

# 4. The classic example: medical testing

This is where the distinction becomes very clear.

Suppose:

- 1% of people have a disease.
- If someone has the disease, the test is positive 99% of the time.
- If someone does not have the disease, the test is positive 5% of the time.

Let

\[ D=\text{person has disease} \]

and

\[ +=\text{test is positive}. \]

We know:

\[ P(+\mid D)=0.99. \]

This answers:

> **If the person has the disease, what is the probability that the test is positive?**

Answer: 99%.

That is an ordinary conditional probability.

But suppose your test comes back positive.

Now your question is:

> **Given that my test is positive, what is the probability that I actually have the disease?**

Mathematically:

\[ P(D\mid +) \]

Notice what happened:

\[ P(+\mid D) \]

became

\[ P(D\mid +). \]

The condition has been **reversed**.

This is precisely where Bayes' theorem is useful.

---

# 5. Why can't we just say 99%?

A very common mistake is:

\[ P(+\mid D)=99\% \]

therefore

\[ P(D\mid +)=99\%. \]

This is wrong.

The two probabilities ask completely different questions.

Compare the English:

### \(P(+\mid D)\)

> Among people **who have the disease**, how many test positive?

versus

### \(P(D\mid +)\)

> Among people **who test positive**, how many actually have the disease?

Those populations are different.

This confusion is one of the main reasons Bayes' theorem is important.

---

# 6. See it with actual people

Imagine **10,000 people**.

Since 1% have the disease:

\[ 100 \]

people have the disease.

The remaining

\[ 9,900 \]

do not.

### Diseased group

The test detects 99% of them:

\[ 100\times0.99=99. \]

So:

- 99 diseased people test positive.
- 1 diseased person tests negative.

### Healthy group

5% get false positives:

\[ 9,900\times0.05=495. \]

So 495 healthy people also test positive.

Therefore total positive tests:

\[ 99+495=594. \]

Of these 594 positive tests, only 99 actually have the disease.

Thus

\[ P(D\mid +) = \frac{99}{594} \approx16.7\%. \]

So even though

\[ P(+\mid D)=99\%, \]

we get

\[ P(D\mid +)\approx16.7\%. \]

That difference is exactly what Bayes' theorem helps us understand.

---

# 7. Conditional probability versus Bayes' theorem

Here is the central comparison:

|Conditional probability|Bayes' theorem|
|---|---|
|Calculates probability given some condition|Calculates a conditional probability by reversing the conditioning|
|Asks “What is \(P(A\mid B)\)?”|Often starts with \(P(B\mid A)\) and finds \(P(A\mid B)\)|
|Restricts the sample space|Updates/reverses probabilities using additional information|
|Basic concept|Derived from conditional probability|
|Example: Given disease, probability of positive test|Given positive test, probability of disease|
|\(P(A\mid B)=P(A\cap B)/P(B)\)|Uses prior probability, likelihood, and evidence|

So Bayes' theorem is **not an alternative to conditional probability**.

Bayes' theorem is actually built from conditional probability.

---

# 8. Their mathematical connection

From conditional probability,

\[ P(A\mid B) = \frac{P(A\cap B)}{P(B)}. \]

Similarly,

\[ P(B\mid A) = \frac{P(A\cap B)}{P(A)}. \]

Rearranging the second equation:

\[ P(A\cap B) = P(B\mid A)P(A). \]

Put this into the first equation:

\[ P(A\mid B) = \frac{P(B\mid A)P(A)} {P(B)}. \]

And that is Bayes' theorem.

So Bayes' theorem comes directly from the definition of conditional probability.

---

# 9. A useful way to think about the difference

Think of conditional probability as answering:

> **“If I enter this group, what fraction have property A?”**

For example:

> Among smokers, what fraction develop disease?

\[ P(Disease\mid Smoker) \]

Bayes asks:

> **“If I observe the property, how likely is it that the person came from this group?”**

For example:

> Among people with the disease, what fraction are smokers?

\[ P(Smoker\mid Disease) \]

Those sound similar, but mathematically they can be very different.

---

# 10. Direction is everything

Suppose:

\[ A=\text{person is a professional basketball player} \]

and

\[ B=\text{person is taller than 6 feet}. \]

Then

\[ P(B\mid A) \]

means:

> Given that someone is a professional basketball player, what is the probability they are taller than 6 feet?

Probably very high.

But

\[ P(A\mid B) \]

means:

> Given that someone is taller than 6 feet, what is the probability they are a professional basketball player?

Extremely low.

So:

\[ P(B\mid A)\neq P(A\mid B). \]

Bayes' theorem tells you how those two quantities are related.

---

# 11. The four pieces in Bayes' theorem

When Bayes' theorem is used,

\[ P(A\mid B) = \frac{P(B\mid A)P(A)} {P(B)}, \]

the terms often get special names.

### \(P(A)\): Prior probability

Your probability of \(A\) **before observing \(B\)**.

In the medical example:

\[ P(D)=0.01. \]

Before seeing the test result, there's a 1% chance the person has the disease.

---

### \(P(B\mid A)\): Likelihood

How likely the observed evidence \(B\) would be if \(A\) were true.

For example:

\[ P(+\mid D)=0.99. \]

If the person has the disease, a positive test is very likely.

---

### \(P(B)\): Evidence

Overall probability of seeing \(B\).

For example:

\[ P(+). \]

It includes positive tests from both diseased and healthy people.

---

### \(P(A\mid B)\): Posterior probability

Your probability of \(A\) **after observing \(B\)**.

For example:

\[ P(D\mid +). \]

This is your updated probability of disease after seeing a positive test.

So Bayes can be summarized conceptually as:

\[ \boxed{\text{Prior belief + new evidence } \rightarrow \text{ updated belief}} \]

---

# 12. How to recognize which one a question wants

Look carefully at the wording.

Suppose the problem says:

> 80% of students who study pass the exam.

That gives:

\[ P(Pass\mid Study)=0.8. \]

If the question asks:

> What is the probability that a student passes, given that they studied?

You already have exactly what you need:

\[ P(Pass\mid Study). \]

That's straightforward **conditional probability**.

But suppose the question asks:

> A student passed. What is the probability that they studied?

Now it wants:

\[ P(Study\mid Pass). \]

But you were given:

\[ P(Pass\mid Study). \]

The conditioning has reversed:

\[ P(Pass\mid Study) \rightarrow P(Study\mid Pass). \]

That's a strong signal that **Bayes' theorem** is needed.

---

# 13. A practical decision rule

When solving a problem, first write down exactly what is being asked.

If the question asks:

> “Given \(B\), what is the probability of \(A\)?”

write

\[ P(A\mid B). \]

Then examine what information you have.

### Case 1: You directly know the relevant counts

For example:

- 40 people are in group \(B\).
- 10 of them also satisfy \(A\).

Then simply use conditional probability:

\[ P(A\mid B)=\frac{10}{40}. \]

### Case 2: You know \(P(B\mid A)\), but need \(P(A\mid B)\)

That's the classic Bayes situation.

Use Bayes' theorem.

---

# 14. One subtle point

Technically, **Bayes' theorem is always a statement about conditional probabilities**.

So it's not quite correct to think:

> conditional probability vs Bayes' theorem

as though they were unrelated methods.

The relationship is more like:

\[ \boxed{\text{Conditional probability is the concept}} \]

while

\[ \boxed{\text{Bayes' theorem is a rule for manipulating/reversing conditional probabilities}} \]

It's similar to how multiplication is a general operation, while a specific algebraic identity tells you how to rearrange certain products.

---

# 15. One more example: factories

Suppose a company has two factories:

- Factory A produces 70% of all products.
- Factory B produces 30%.
- 2% of products from A are defective.
- 5% of products from B are defective.

You know:

\[ P(D\mid A)=0.02 \]

and

\[ P(D\mid B)=0.05. \]

These are conditional probabilities.

They answer:

> If I know the product came from factory A, what is the probability it is defective?

and

> If I know it came from factory B, what is the probability it is defective?

Now imagine you pick a product and discover it is defective.

You ask:

> What is the probability this defective product came from Factory B?

That is

\[ P(B\mid D). \]

But what you were given was

\[ P(D\mid B). \]

You've reversed the condition.

That is a Bayes problem.

Using Bayes:

\[ P(B\mid D) = \frac{P(D\mid B)P(B)} {P(D)}. \]

Here,

\[ P(D) = P(D\mid A)P(A) + P(D\mid B)P(B). \]

Thus

\[ P(D) = (0.02)(0.70)+(0.05)(0.30) \]\[ =0.014+0.015 \]\[ =0.029. \]

Therefore,

\[ P(B\mid D) = \frac{(0.05)(0.30)} {0.029} \]\[ = \frac{0.015}{0.029} \approx0.517. \]

So there is about a **51.7% chance** that a defective product came from Factory B.

Notice something interesting: Factory B makes only 30% of products, but because its defect rate is higher, once you observe a defective product, the probability it came from B rises to about 51.7%.

That is the idea of **updating beliefs using evidence**.

---

# 16. The wording clues to watch for

Certain wording often signals ordinary conditional probability:

- “given that…”
- “among those who…”
- “of the people who…”
- “assuming that…”
- “knowing that…”

For example:

> Among people who own cars, what fraction own electric cars?

That's

\[ P(Electric\mid CarOwner). \]

Bayes problems often contain a **cause/evidence reversal**:

> 90% of fraudulent transactions trigger an alert.  
> A transaction triggered an alert.  
> What is the probability that it is fraudulent?

Given:

\[ P(Alert\mid Fraud) \]

Asked:

\[ P(Fraud\mid Alert). \]

That reversal is the giveaway.

---

# 17. A cause-and-evidence interpretation

Another useful mental model is:

Suppose \(A\) is a possible **cause** and \(B\) is observed **evidence**.

Conditional probability might ask:

\[ P(B\mid A) \]

> If this cause were present, how likely would this evidence be?

Bayes asks:

\[ P(A\mid B) \]

> Now that I saw this evidence, how likely is this cause?

Examples:

|Cause \(A\)|Evidence \(B\)|
|---|---|
|Disease|Positive test|
|Fraud|Suspicious transaction|
|Rain|Wet road|
|Spam email|Certain words|
|Machine failure|Warning signal|
|Guilty suspect|Forensic evidence|

Forward direction:

\[ Cause\rightarrow Evidence \]

is often easier to know.

Bayes allows reasoning in the reverse direction:

\[ Evidence\rightarrow Cause. \]

---

# 18. Why Bayes needs the prior probability

Imagine:

> If someone has Disease X, they test positive 99% of the time.

Can we determine:

> If someone tests positive, what is the probability they have Disease X?

No.

We're missing something crucial:

> **How common is Disease X in the first place?**

Compare two diseases.

### Disease A

Occurs in:

\[ 1 \text{ out of } 2 \]

people.

A positive test probably gives strong reason to think someone has it.

### Disease B

Occurs in:

\[ 1 \text{ out of } 1,000,000 \]

people.

Even an accurate test may generate many false positives relative to the extremely small number of real cases.

That base probability

\[ P(A) \]

is the **prior** or **base rate**.

Bayes' theorem forces you to take it into account.

Ignoring it is sometimes called the **base-rate fallacy**.

---

# 19. One diagram to keep in your head

Think of the two directions:

\[ \boxed{A\rightarrow B} \]

If you ask:

> Assuming \(A\), how likely is \(B\)?

that's

\[ P(B\mid A). \]

Now reverse your reasoning:

\[ \boxed{B\rightarrow A} \]

If you observe \(B\) and ask:

> Given \(B\), how likely is \(A\)?

that's

\[ P(A\mid B). \]

Bayes' theorem connects these two directions.

---

# 20. Final summary

The most important distinction is this:

### Conditional probability

Answers:

\[ \boxed{\text{“Given B happened, what is the probability that A happens?”}} \]

Example:

> Given that a person has a disease, what is the probability their test is positive?

\[ P(+\mid Disease) \]

It means **restrict your attention to cases satisfying the condition**.

### Bayes' theorem

Answers:

\[ \boxed{\text{“I know how likely B is given A; after observing B, how likely is A?”}} \]

Example:

> Given that someone's test is positive, what is the probability they have the disease?

\[ P(Disease\mid +) \]

It is especially useful for turning

\[ \boxed{P(B\mid A)} \]

into

\[ \boxed{P(A\mid B)}. \]

So if you remember only one thing, remember:

\[ \boxed{ \text{Conditional probability: probability under a condition} } \]\[ \boxed{ \text{Bayes' theorem: reverse the condition / update belief using evidence} } \]

And never assume

\[ \boxed{P(A\mid B)=P(B\mid A)} \]

because, in general, they can be **dramatically different**.