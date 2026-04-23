# Hypothesis Testing

A structured guide to the theory, logic, and vocabulary of classical hypothesis testing.

---

## Table of Contents

1. [What Hypothesis Testing Is](#1-what-hypothesis-testing-is)
2. [The Core Logic: A Courtroom Analogy](#2-the-core-logic-a-courtroom-analogy)
3. [Population vs Sample](#3-population-vs-sample)
4. [The Central Limit Theorem (CLT)](#4-the-central-limit-theorem-clt)
5. [Standard Error](#5-standard-error)
6. [The Test Statistic z](#6-the-test-statistic-z)
7. [The Null Distribution](#7-the-null-distribution)
8. [Decision Rules: Critical Value and p-value](#8-decision-rules-critical-value-and-p-value)
9. [One-Sided vs Two-Sided Tests](#9-one-sided-vs-two-sided-tests)
10. [Significance Levels](#10-significance-levels)
11. [Confidence Intervals](#11-confidence-intervals)
12. [Comparing Two Groups](#12-comparing-two-groups)
13. [The Universal Pipeline](#13-the-universal-pipeline)
14. [Common Misinterpretations](#14-common-misinterpretations)
15. [When to Use z vs t](#15-when-to-use-z-vs-t)
16. [Why This Matters for ML and AI](#16-why-this-matters-for-ml-and-ai)

---

## 1. What Hypothesis Testing Is

Hypothesis testing is a formal statistical procedure for deciding whether an observed pattern in data reflects a **real effect** in the underlying population, or whether it could reasonably be explained by **random chance alone**.

It doesn't prove truth. It quantifies evidence.

Every hypothesis test answers a single question:

> *"If nothing interesting were happening, how likely would we be to see data that looks like this?"*

If the answer is "very unlikely," we conclude that something interesting probably *is* happening.

---

## 2. The Core Logic: A Courtroom Analogy

Hypothesis testing mirrors a courtroom trial:

| Courtroom | Statistics |
|---|---|
| Defendant presumed innocent | H₀ presumed true |
| Prosecutor presents evidence | We collect sample data |
| Proof "beyond reasonable doubt" required | Evidence must be strong (small p-value) |
| Verdict: "guilty" or "not guilty" | Decision: "reject H₀" or "fail to reject H₀" |
| "Not guilty" ≠ "innocent" | "Fail to reject" ≠ "H₀ is true" |

We never *prove* H₀ true — we only gather evidence that may or may not be strong enough to overturn it.

### The two hypotheses

**Null hypothesis (H₀):** the "nothing is happening" claim. The status quo, no effect, no change.
Example: *μ = 4.0 minutes* (the wait time hasn't changed).

**Alternative hypothesis (H_A):** the claim we're trying to find evidence for.
Example: *μ > 4.0 minutes* (wait times have increased).

Both hypotheses are stated **before** looking at the data.

---

## 3. Population vs Sample

A foundational distinction that the whole framework builds on.

- **Population:** the full group we care about. Often huge or infinite. Has fixed but unknown parameters (**μ**, **σ**, **p**).
- **Sample:** the subset we actually measure (size n). Gives us statistics (**X̄**, **S**, **p̂**) that estimate the population parameters.

### Greek vs Latin convention

Statistics uses a universal notation:

- **Greek letters** (μ, σ, p) = true *population* parameters — unknown, fixed.
- **Latin letters** (X̄, S, p̂) = *sample* estimates — computed, random.

Since samples are random, anything computed from them (including X̄) is itself a random variable with its own distribution. That randomness is what makes inference both possible and necessary.

---

## 4. The Central Limit Theorem (CLT)

The single most important result enabling classical inference.

> **CLT:** If you take a random sample of size n from any population with mean μ and standard deviation σ, then for sufficiently large n (typically n ≥ 30):
>
> $$\bar{X} \sim \mathcal{N}\!\left(\mu,\ \frac{\sigma^2}{n}\right)$$

Regardless of the underlying distribution — skewed, bimodal, weird — the **sample mean** is approximately normally distributed.

This is almost magical: it means we can use the bell curve to compute probabilities even when we know nothing about the original data's shape. Every z-test, t-test, and confidence interval for a mean relies on it.

---

## 5. Standard Error

**Standard deviation (σ)** measures the spread of individual data points. **Standard error (SE)** measures the spread of a *statistic* computed from those data points.

For the sample mean:

$$\text{SE}(\bar{X}) = \frac{\sigma}{\sqrt{n}}$$

### The √n effect

Individual values wobble by σ, but averages of n of them wobble by only σ/√n. This means:
- To halve the SE, you need **4× the data**.
- To cut SE by 10×, you need **100× the data**.

This is why bigger samples give sharper estimates — and why averaging is such a powerful noise-reduction tool.

**Example:** if σ = 1.2 and n = 50, then SE = 1.2/√50 ≈ 0.17. Individual observations deviate by ~1.2 units, but the *average of 50* deviates by only ~0.17.

---

## 6. The Test Statistic z

The z-score is a **universal ruler of surprise**. It measures how many standard errors the observed statistic sits from what H₀ predicts:

$$z = \frac{\text{observed} - \text{expected under } H_0}{\text{SE}}$$

For a sample mean:

$$z = \frac{\bar{X} - \mu_0}{\sigma/\sqrt{n}}$$

Why convert to z? Because it erases units. Whether you're testing minutes, dollars, points, or infection rates, z = 2 always means the same thing: "about two standard errors from the null."

### Rough intuition

| \|z\| | Rarity under H₀ | Interpretation |
|---|---|---|
| < 1 | common | totally unsurprising |
| ≈ 1.645 | top 5% (one-sided) | borderline significant |
| ≈ 1.96 | top 2.5% each tail (two-sided) | significant at 5% |
| ≈ 2.58 | top 0.5% each tail | significant at 1% |
| ≥ 3 | extremely rare | strong evidence against H₀ |

---

## 7. The Null Distribution

The **null distribution** is the distribution the test statistic would follow *if H₀ were true*. For a z-test, it's the standard normal N(0, 1).

We plot our observed z on this distribution and ask: *"How far out in the tails did we land?"* The further out, the harder it becomes to believe H₀.

---

## 8. Decision Rules: Critical Value and p-value

Two equivalent ways to make the reject/don't-reject decision. They always agree.

### Critical value approach

Choose a significance level α (commonly 0.05). Find the critical z-value z\* that marks off the top α of the null distribution.

**Rule:** reject H₀ if |z_obs| ≥ z\*.

For α = 0.05:
- One-sided: z\* = 1.645
- Two-sided: z\* = ±1.96

### p-value approach

The p-value is the probability, under H₀, of observing data *at least as extreme* as what we actually saw.

**Rule:** reject H₀ if p < α.

Formulas:
- One-sided: $p = P(Z \geq z_{\text{obs}})$
- Two-sided: $p = 2 \cdot P(Z \geq |z_{\text{obs}}|)$

### The critical interpretation

The p-value is:

$$p = P(\text{data} \mid H_0)$$

It is **not**:

$$p \neq P(H_0 \mid \text{data})$$

A p-value of 0.02 means: *"If H₀ were true, there'd be a 2% chance of seeing data this extreme."* It does NOT mean "there's a 2% chance H₀ is true."

This distinction is the wall between frequentist and Bayesian statistics.

---

## 9. One-Sided vs Two-Sided Tests

The shape of H_A determines which kind of test to run.

| Type | H_A form | Example | Critical z (α = 0.05) | p-value |
|---|---|---|---|---|
| **One-sided (right)** | μ > μ₀ | "wait time increased" | +1.645 | P(Z ≥ z) |
| **One-sided (left)** | μ < μ₀ | "infection rate decreased" | −1.645 | P(Z ≤ z) |
| **Two-sided** | μ ≠ μ₀ | "score changed, either way" | ±1.96 | 2·P(Z ≥ \|z\|) |

### Which to choose

- Use **one-sided** when you have a clear directional prediction *and* a change in the opposite direction would be irrelevant.
- Use **two-sided** by default. It's more conservative and lets the data reveal direction on its own.

In a two-sided test, rejecting H₀ in the left tail means the parameter *decreased*; rejecting in the right tail means it *increased*. The sign of z tells you which way.

---

## 10. Significance Levels

The significance level α is the probability of a **Type I error** — rejecting H₀ when it's actually true (a false positive).

| α | Threshold | Terminology |
|---|---|---|
| 0.05 | p < 0.05 | **statistically significant** |
| 0.01 | p < 0.01 | **strongly significant** |
| 0.001 | p < 0.001 | **highly significant** |

"Significant" only means the effect is **real** (not random noise). It does *not* mean the effect is large or practically important. With enough data, tiny effects become significant. That's why serious analysis always reports **effect size** alongside significance.

---

## 11. Confidence Intervals

A confidence interval gives a *range* of plausible values for the unknown parameter.

$$\text{95\% CI for } \mu = \bar{X} \pm 1.96 \cdot \text{SE}$$

Interpretation: if we repeated this sampling procedure many times, about 95% of the resulting intervals would contain the true μ.

### Duality with hypothesis tests

Confidence intervals and two-sided tests are two sides of the same coin:

- If μ₀ is **inside** the 95% CI → fail to reject H₀ at α = 0.05.
- If μ₀ is **outside** the 95% CI → reject H₀ at α = 0.05.

CIs often communicate more than p-values because they show both significance *and* effect size at a glance.

---

## 12. Comparing Two Groups

When comparing two samples (treatment vs control, before vs after), the same framework applies — we just test a **difference** instead of a single value.

### Difference of two means

$$z = \frac{\bar{X}_1 - \bar{X}_2}{\sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}}$$

### Difference of two proportions

Under H₀ (both groups share the same rate), we pool the data:

$$\hat{p} = \frac{x_1 + x_2}{n_1 + n_2}$$

$$z = \frac{\hat{p}_1 - \hat{p}_2}{\sqrt{\hat{p}(1 - \hat{p})\left(\frac{1}{n_1} + \frac{1}{n_2}\right)}}$$

The null becomes H₀: *p₁ = p₂* (or equivalently, *difference = 0*).

This is the machinery behind A/B testing, clinical trials, vaccine efficacy studies, and most comparative research.

---

## 13. The Universal Pipeline

Every hypothesis test — whether z-test, t-test, chi-square, F-test, or ML model comparison — follows the same seven steps:

1. **State H₀ and H_A** before looking at the data.
2. **Assume H₀ is true.**
3. **Collect a sample** and compute the relevant statistic (X̄, p̂, etc.).
4. **Compute the standard error.**
5. **Compute the test statistic** (z, t, etc.).
6. **Compare** to a critical value or convert to a p-value.
7. **Decide and translate back** to the real-world question.

Memorize this pattern and every inferential procedure you'll ever meet becomes a variation on a theme.

---

## 14. Common Misinterpretations

These mistakes are extremely common, even in published research. Avoid them rigorously.

**"The p-value is the probability that H₀ is true."** ❌
It's P(data | H₀), not P(H₀ | data).

**"A non-significant result proves H₀ is true."** ❌
Absence of evidence is not evidence of absence. You just didn't find enough evidence to reject.

**"Statistically significant means the effect is large or important."** ❌
Significance only means the effect is real. A trivially small effect can be highly significant if n is huge. Always look at effect size.

**"1 − p is the probability the result is real."** ❌
This confuses the direction of conditional probability. The p-value says nothing about P(H_A | data).

**"If p > 0.05, the experiment failed."** ❌
A non-rejection is a legitimate, informative outcome. Sometimes the honest answer *is* "we didn't find evidence of an effect."

**"z = 2.06 means the mean increased by 2.06 units."** ❌
z is unitless. The raw effect is in original units (minutes, points, etc.); z rescales it by SE.

---

## 15. When to Use z vs t

The tests in this document assume the population standard deviation **σ is known**. In reality, σ is usually unknown and must be estimated from the sample by S.

When that happens, the test statistic follows a **t-distribution** (with n − 1 degrees of freedom), not a standard normal:

$$t = \frac{\bar{X} - \mu_0}{S/\sqrt{n}}$$

The t-distribution has heavier tails than the normal, which correctly accounts for extra uncertainty from estimating σ.

| Situation | Use |
|---|---|
| σ known (rare in practice) | **z-test** |
| σ unknown, n < 30 | **t-test** (required) |
| σ unknown, n ≥ 30 | **t-test** (though z gives nearly identical results) |

For large n, z and t converge, which is why many introductory examples use z even when σ is technically unknown.

---

## 16. Why This Matters for ML and AI

Hypothesis testing isn't confined to academic statistics. It runs silently under most of modern data science and machine learning:

- **A/B testing** of websites, features, or ML models — difference of two proportions or means.
- **Feature significance** in linear and logistic regression — each coefficient comes with a t-test.
- **Comparing model accuracies** on held-out test sets — paired tests for classifier performance.
- **Cross-validation confidence intervals** — applying the CLT to fold-level performance metrics.
- **Bootstrap and permutation tests** — nonparametric cousins of the same framework.
- **Early stopping** based on validation-loss changes — implicitly asking whether improvement is real or noise.

Once the pattern — *assume nothing's happening, compute a test statistic, measure how surprised we are, decide* — becomes second nature, a huge amount of ML literature becomes readable as "applied inference under different names."

---

## The One Sentence to Remember

> **Hypothesis testing is the art of separating signal from noise by asking: "How weird would this data look if nothing interesting were happening?"**

---

*This document covers the theory of classical (frequentist) hypothesis testing. For worked examples demonstrating these concepts on real problems, see the problems in this folder.*
