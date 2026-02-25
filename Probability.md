# 2.2 Sample Spaces and Events

Probability theory provides the mathematical framework for quantifying uncertainty. In the context of Computer Science and Machine Learning, we can view these definitions as the "schema" for our data.

## Core Definitions

* **Sample Space ($\Omega$):** The set of all possible outcomes of an experiment.
* **Sample Outcomes ($\omega$):** Individual points or realizations within the sample space $\Omega$.
* **Events ($A$):** Subsets of the sample space $\Omega$.

### Examples of $\Omega$

1. **Discrete/Finite:** Tossing a coin twice results in $\Omega = \{HH, HT, TH, TT\}$.
2. **Continuous:** Measuring a physical quantity like temperature where $\Omega = \mathbb{R} = (-\infty, \infty)$.
3. **Infinite Sequences:** Tossing a coin forever results in an infinite set of sequences $\Omega = \{ \omega = (\omega_1, \omega_2, \dots) : \omega_i \in \{H, T\} \}$.

---

## Set Operations & Notation

To manipulate events, we use standard set theory. In an ML context, these operations represent logical gates (AND, OR, NOT) applied to data distributions.

| Symbol | Definition | Logical Equivalent |
| --- | --- | --- |
| $A^c$ | $\{\omega \in \Omega : \omega \notin A\}$ | **NOT** $A$ (Complement) |
| $A \cup B$ | $\{\omega \in \Omega : \omega \in A \text{ or } \omega \in B \text{ or both}\}$ | $A$ **OR** $B$ (Union) |
| $A \cap B$ | $\{\omega \in \Omega : \omega \in A \text{ and } \omega \in B\}$ | $A$ **AND** $B$ (Intersection) |
| $A - B$ | $\{\omega \in \Omega : \omega \in A, \omega \notin B\}$ | $A$ **BUT NOT** $B$ |
| $A \subset B$ | Every element of $A$ is in $B$ | $A$ **IMPLIES** $B$ |

### Key Properties

* **Disjoint / Mutually Exclusive:** Events $A_1, A_2, \dots$ are disjoint if $A_i \cap A_j = \emptyset$ for all $i \neq j$.
* **Partition:** A sequence of disjoint sets $A_1, A_2, \dots$ such that $\bigcup_{i=1}^{\infty} A_i = \Omega$. This is critical for the Law of Total Probability.
* **Indicator Function:** A mapping of an event to a binary value:

$$I_A(\omega) = \begin{cases} 1 & \text{if } \omega \in A \\ 0 & \text{if } \omega \notin A \end{cases}$$



---

## Limits of Sets (Monotone Sequences)

For CS students, this is analogous to the convergence of an iterative algorithm.

* **Monotone Increasing:** $A_1 \subset A_2 \subset A_3 \dots$. The limit is defined as $\lim_{n \to \infty} A_n = \bigcup_{i=1}^{\infty} A_i$.
* **Monotone Decreasing:** $A_1 \supset A_2 \supset A_3 \dots$. The limit is defined as $\lim_{n \to \infty} A_n = \bigcap_{i=1}^{\infty} A_i$.

> **Example 2.4:** Let $\Omega = \mathbb{R}$ and $A_i = [0, 1/i)$.
> As $i \to \infty$, the interval shrinks. The intersection $\bigcap_{i=1}^{\infty} A_i$ becomes the empty set $\emptyset$ because no positive number is smaller than *every* $1/i$.

<img width="614" height="345" alt="image" src="https://github.com/user-attachments/assets/0422f258-db01-4fb0-8874-de265b84ac48" />

---
---

# 2.3 Probability

This section moves from the qualitative geometry of sets to the quantitative measurement of uncertainty. For a CS/AI student, this is where we define the "interface" for any probability function.

---

## The Probability Measure (Definition 2.5)

A probability distribution (or measure) $\mathbb{P}$ is a function that maps an event $A$ to a real number $\mathbb{P}(A)$. To be valid, it **must** satisfy three axioms:

1. **Non-negativity:** $\mathbb{P}(A) \geq 0$ for every event $A$.
2. **Normalization:** $\mathbb{P}(\Omega) = 1$ (The probability of "something" happening is 100%).
3. **Countable Additivity:** If you have a sequence of **disjoint** events $A_1, A_2, \dots$, then the probability of their union is the sum of their individual probabilities:

$$\mathbb{P}\left(\bigcup_{i=1}^{\infty} A_i\right) = \sum_{i=1}^{\infty} \mathbb{P}(A_i)$$



> **CS Note:** In code, think of Axiom 3 as the reason why you can calculate the total probability of a categorical variable (like classes in a Softmax output) by simply summing the individual probabilities of the mutually exclusive classes.

---

## Interpretations of Probability

The text notes two major schools of thought that dominate ML and Statistics:

* **Frequentist:** $\mathbb{P}(A)$ is the long-run proportion of times $A$ occurs in repeated trials. (e.g., "If I run this simulation 1 million times, how often does it crash?")
* **Bayesian (Degree of Belief):** $\mathbb{P}(A)$ measures an observer’s strength of belief that $A$ is true. This is foundational for Bayesian Neural Networks and updating model weights as new data arrives.

---

## Derived Properties

From the three axioms, several critical properties emerge that you will use constantly when simplifying math in ML papers:

* **Null Event:** $\mathbb{P}(\emptyset) = 0$.
* **Monotonicity:** If $A \subset B$, then $\mathbb{P}(A) \leq \mathbb{P}(B)$.
* **Range:** $0 \leq \mathbb{P}(A) \leq 1$.
* **Complement:** $\mathbb{P}(A^c) = 1 - \mathbb{P}(A)$.
* **Addition Rule (Disjoint):** If $A \cap B = \emptyset$, then $\mathbb{P}(A \cup B) = \mathbb{P}(A) + \mathbb{P}(B)$.

### Lemma 2.6: The General Addition Rule

If events are **not** disjoint, we must subtract the overlap to avoid double-counting:


$$\mathbb{P}(A \cup B) = \mathbb{P}(A) + \mathbb{P}(B) - \mathbb{P}(A \cap B)$$

---

## Continuity of Probabilities (Theorem 2.8)

This is a "limit" theorem. If a sequence of events $A_n$ converges to $A$ (meaning the sets are growing or shrinking toward a limit), then the probabilities also converge:


$$\text{If } A_n \to A, \text{ then } \mathbb{P}(A_n) \to \mathbb{P}(A) \text{ as } n \to \infty$$

**ML Intuition:** This theorem ensures that our probability models are stable. If we take increasingly larger samples of data that "fill up" a space, the probability we assign to those samples will smoothly converge to the true probability of that space.

---
---


# 2.4 Probability on Finite Sample Spaces

This section covers the most common scenario in introductory probability: situations where the total number of possible outcomes is limited and countable.

---

## 1. The Uniform Probability Distribution

When a sample space $\Omega = \{\omega_1, \dots, \omega_n\}$ is finite and every outcome is **equally likely**, we use the **Uniform Probability Distribution**.

The probability of an event $A$ is simply the ratio of the number of favorable outcomes to the total number of possible outcomes:


$$\mathbb{P}(A) = \frac{|A|}{|\Omega|}$$

* $|A|$ is the number of elements in event $A$.
* $|\Omega|$ is the total number of elements in the sample space.

> **Example:** Rolling two dice. There are $|\Omega| = 36$ possible pairs. If $A$ is the event that the sum is 11, then $A = \{(5,6), (6,5)\}$, so $|A| = 2$. Thus, $\mathbb{P}(A) = 2/36$.

---

## 2. Combinatorics: The Art of Counting

To calculate $\mathbb{P}(A)$ for complex problems, you need to count large sets without listing every element. The text highlights two primary tools:

### Factorials (Ordering)

The number of ways to order $n$ distinct objects is $n!$ (read "$n$ factorial"):


$$n! = n \times (n-1) \times (n-2) \times \dots \times 1$$

* **CS Context:** This represents the complexity of brute-forcing all permutations of a list of size $n$.
* **Convention:** $0! = 1$.

### Combinations (Choosing)

The number of distinct ways to choose $k$ objects from a set of $n$ objects, where the **order does not matter**, is given by the binomial coefficient:


$$\binom{n}{k} = \frac{n!}{k!(n-k)!}$$


This is read as **"$n$ choose $k$"**.

> **Example:** Choosing a committee of 3 students from a class of 20:
> 
> $$\binom{20}{3} = \frac{20 \times 19 \times 18}{3 \times 2 \times 1} = 1140 \text{ possibilities.}$$
> 
> 

---

## 3. Useful Properties of $\binom{n}{k}$

These identities help simplify complex algebraic expressions in ML derivations:

1. **Boundary Cases:** $\binom{n}{0} = \binom{n}{n} = 1$ (There is only one way to pick nothing or everything).
2. **Symmetry:** $\binom{n}{k} = \binom{n}{n-k}$ (Picking $k$ items to keep is the same as picking $n-k$ items to throw away).

---
---

# 2.5 Independent Events

This section defines one of the most important concepts in Machine Learning: **Independence**. Independence allows us to simplify complex joint probability distributions into products of simpler marginal distributions.

---

## 1. Definition of Independence

Two events $A$ and $B$ are **independent** if the occurrence of one does not change the probability of the other. Mathematically, this is defined by the product rule:


$$\mathbb{P}(AB) = \mathbb{P}(A)\mathbb{P}(B)$$


*Note: $AB$ is shorthand for the intersection $A \cap B$.*

For a set of events $\{A_i : i \in I\}$ to be independent, the probability of any finite intersection must equal the product of their individual probabilities:


$$\mathbb{P}\left(\bigcap_{i \in J} A_i\right) = \prod_{i \in J} \mathbb{P}(A_i)$$


where $J$ is any finite subset of $I$.

> **AI Engineering Context:** In a **Naive Bayes Classifier**, we assume that features are independent given the class. This "independence assumption" is what allows us to multiply feature probabilities together, even if they aren't actually independent in the real world.

---

## 2. Assumption vs. Derivation

Independence can arise in two ways:

1. **Explicit Assumption:** We assume independence based on the nature of the physical process (e.g., a coin has "no memory" of previous tosses).
2. **Derivation:** We calculate $\mathbb{P}(AB)$, $\mathbb{P}(A)$, and $\mathbb{P}(B)$ separately and verify if the equation $\mathbb{P}(AB) = \mathbb{P}(A)\mathbb{P}(B)$ holds true.

---

## 3. Disjoint vs. Independent (Common Pitfall)

A common mistake is confusing "disjoint" (mutually exclusive) with "independent."  "A and B are disjoint means A and B cannot happen together"

* **Disjoint events** are **not** independent (if they both have positive probability).
* **Why?** If $A$ and $B$ are disjoint, knowing $A$ occurred tells you with 100% certainty that $B$ *did not* occur. Since the probability of $B$ changed (to zero), they are highly dependent.

---

## 4. Key Examples from the Text

### Example 2.10: The Complement Rule + Independence

To find the probability of "at least one Head" in 10 tosses:

1. Calculate the probability of the complement (zero heads/all tails).
2. Since tosses are independent, $\mathbb{P}(\text{all tails}) = \mathbb{P}(T_1)\mathbb{P}(T_2)\dots\mathbb{P}(T_{10}) = (\frac{1}{2})^{10}$.
3. Subtract from 1: $\mathbb{P}(A) = 1 - (\frac{1}{2})^{10} \approx 0.999$.

### Example 2.11: Geometric Series in Probability

This example models a sequence of events where two people take turns until someone succeeds. It uses the property of independent trials to determine the probability of success on a specific trial $j$:


$$\mathbb{P}(A_j) = \left(\frac{1}{2}\right)^{j-1}\left(\frac{1}{3}\right)$$


The total probability is the sum of a geometric series:


$$\mathbb{P}(E) = \sum_{j=1}^{\infty} \frac{1}{3} \left(\frac{1}{2}\right)^{j-1} = \frac{2}{3}$$


This uses the sum formula for a geometric series where $0 < r < 1$: $\sum_{j=k}^{\infty} r^j = \frac{r^k}{1-r}$.

---
---




