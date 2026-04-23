**The data — 36 measured delivery times (minutes)**

$$30, 25, 28, 35, 22, 27, 31, 26, 29, 33, 24, 28,$$
$$26, 30, 27, 32, 25, 28, 29, 27, 31, 23, 28, 30,$$
$$26, 29, 27, 34, 25, 28, 30, 26, 29, 27, 31, 28$$

That's n = 36 values. These represent your random sample from the population of all pizza deliveries.

---

**Step 1 — Compute the sample mean X̄**

$$\bar{X} = \frac{1}{n}\sum_{i=1}^{n} X_i = \frac{1}{36}\sum_{i=1}^{36} X_i$$

Adding all 36 numbers:

Row 1: 30+25+28+35+22+27+31+26+29+33+24+28 = **338**
Row 2: 26+30+27+32+25+28+29+27+31+23+28+30 = **336**
Row 3: 26+29+27+34+25+28+30+26+29+27+31+28 = **340**

Total = 338 + 336 + 340 = **1014**

$$\bar{X} = \frac{1014}{36} \approx 28.17 \text{ minutes}$$

(Close enough to the 27.2 I used earlier — I'll keep going with 28.17 to be honest to the actual numbers.)

---

**Step 2 — Compute the sample variance S² and standard deviation S**

$$S^2 = \frac{1}{n-1}\sum_{i=1}^{n}(X_i - \bar{X})^2$$

For each Xᵢ, compute (Xᵢ − 28.17)². I'll show a few and then give the sum:

| Xᵢ | Xᵢ − 28.17 | (Xᵢ − 28.17)² |
|----|-----------|---------------|
| 30 | 1.83 | 3.35 |
| 25 | −3.17 | 10.05 |
| 28 | −0.17 | 0.03 |
| 35 | 6.83 | 46.65 |
| 22 | −6.17 | 38.07 |
| 27 | −1.17 | 1.37 |
| … | … | … |

Summing all 36 squared deviations:

$$\sum_{i=1}^{36}(X_i - \bar{X})^2 \approx 290.97$$

Now divide by n − 1 = 35 (Bessel's correction — because we used X̄ instead of μ):

$$S^2 = \frac{290.97}{35} \approx 8.31 \text{ minutes}^2$$

$$S = \sqrt{8.31} \approx 2.88 \text{ minutes}$$

So the typical deviation from the mean is about 2.88 minutes.

---

**Step 3 — Standard error of X̄**

$$\text{SE}(\bar{X}) = \frac{S}{\sqrt{n}} = \frac{2.88}{\sqrt{36}} = \frac{2.88}{6} \approx 0.48 \text{ minutes}$$

Interpretation: if you repeated this whole sampling experiment many times, the sample means X̄ would wobble around the true μ with a typical spread of about half a minute. That's the √n effect — individual deliveries vary by ~2.88 min, but the *average of 36 of them* varies by only ~0.48 min.

---

**Step 4 — Finite population correction (if applicable)**

If the population is effectively infinite (all future deliveries), skip this. But suppose the chain made only N = 500 deliveries this month and you sampled 36 of them without replacement:

$$\text{correction} = \sqrt{\frac{N-n}{N-1}} = \sqrt{\frac{500-36}{499}} = \sqrt{\frac{464}{499}} \approx \sqrt{0.930} \approx 0.964$$

$$\text{SE}_{\text{corrected}} = 0.48 \times 0.964 \approx 0.463 \text{ minutes}$$

Small adjustment here — confirms it's fine to ignore when n is a small fraction of N.

---

**Step 5 — Apply the CLT: build a 95% confidence interval for μ**

By the Central Limit Theorem, with n = 36 the sample mean is approximately normal:

$$\bar{X} \sim \mathcal{N}\!\left(\mu,\; \frac{\sigma^2}{n}\right) \approx \mathcal{N}(\mu,\; 0.48^2)$$

The 95% confidence interval uses the critical value 1.96 from the standard normal:

$$\bar{X} \pm 1.96 \cdot \text{SE} = 28.17 \pm 1.96 \times 0.48$$

$$= 28.17 \pm 0.94$$

$$\boxed{\text{95\% CI for } \mu \approx [27.23,\ 29.11] \text{ minutes}}$$

---

**What all these numbers are telling you**

You measured 36 pizza deliveries. You found an average of **28.17 minutes** with a spread of **2.88 minutes** around that average. Because you averaged 36 values, your estimate of the true mean is pretty precise — its typical wobble is only **0.48 minutes**. Combining this with the Gaussian shape the CLT guarantees, you can say with about 95% confidence that the chain's *true* long-run mean delivery time lies somewhere between **27.2 and 29.1 minutes**.

And this is exactly the logical move at the heart of all statistical inference: 36 noisy numbers → a confident, quantified claim about an unseen population parameter. Same machinery, different dress-up, runs under the hood of every ML evaluation you'll ever do.

---
---

## 📋 Problem — Coffee Shop Wait Times

A coffee shop's manager claims that the **average wait time** for a customer is **μ = 4.0 minutes**, based on years of historical data, with a known population standard deviation **σ = 1.2 minutes**.

You suspect service has gotten slower. To test this, you randomly time **n = 50 orders** over a week and compute a sample mean of **X̄ = 4.35 minutes**.

**Question:** At the 5% significance level, is there statistical evidence that the true mean wait time is *longer* than 4.0 minutes?

---

## 🎯 Step 1 — State the hypotheses

Because you specifically suspect the wait time is *longer*, this is a **one-sided (right-tailed) test**:

$$H_0:\ \mu = 4.0 \quad \text{(nothing has changed)}$$
$$H_A:\ \mu > 4.0 \quad \text{(wait times have increased)}$$

**Assumption:** we begin by *assuming* H₀ is true, then check whether our data is surprising under that assumption.

---

## 🎯 Step 2 — Why the CLT lets us do this

Individual wait times could follow any distribution (possibly skewed — some orders take forever). But by the **Central Limit Theorem**, since n = 50 ≥ 30:

$$\bar{X} \sim \mathcal{N}\!\left(\mu,\ \frac{\sigma^2}{n}\right)$$

So under H₀ (assuming μ = 4.0):

$$\bar{X} \sim \mathcal{N}\!\left(4.0,\ \frac{1.2^2}{50}\right)$$

This is the "null distribution" — what X̄ would look like across many repeated samples if the manager is right.

---

## 🎯 Step 3 — Compute the standard error

$$\text{SE} = \frac{\sigma}{\sqrt{n}} = \frac{1.2}{\sqrt{50}} = \frac{1.2}{7.0711} \approx 0.1697$$

So under H₀, sample means would typically wobble about **0.17 minutes** around 4.0.

---

## 🎯 Step 4 — Compute the test statistic z

The z-statistic measures *how many standard errors* away from μ our observed X̄ falls:

$$z = \frac{\bar{X} - \mu_0}{\text{SE}} = \frac{4.35 - 4.00}{0.1697} = \frac{0.35}{0.1697} \approx 2.063$$

So our observed sample mean is **about 2.06 standard errors above** what H₀ predicts. Is that surprising? Let's make the picture.Here is the full picture of what's happening:

<img width="1416" height="693" alt="image" src="https://github.com/user-attachments/assets/68d62fa0-8ee6-45ee-ada3-706cfba247f5" />

The blue curve is the *null distribution* — where X̄ would land if the manager's claim (μ = 4.0) were true. The orange region is the rejection zone (the 5% most extreme values in the right tail). The red line and shaded area show where our observed X̄ = 4.35 falls — clearly well inside the rejection region.

---

## 🎯 Step 5 — Two equivalent ways to decide

### Method A — Critical value approach

At α = 0.05 for a one-sided test, the critical z-value is **z\* = 1.645**. The decision rule is:

$$\text{Reject } H_0 \text{ if } z_{\text{obs}} \geq 1.645$$

Our z_obs = **2.063** > 1.645 → **reject H₀**.

(Equivalently: reject if X̄ ≥ μ₀ + 1.645·SE = 4.0 + 1.645 × 0.1697 ≈ **4.279 minutes**. Our X̄ = 4.35 > 4.279 → reject.)

### Method B — p-value approach

The p-value is the probability of observing a sample mean *at least as extreme* as 4.35, assuming H₀ is true:

$$p = P(\bar{X} \geq 4.35 \mid \mu = 4.0) = P(Z \geq 2.063)$$

Using the standard normal CDF:

$$p = 1 - \Phi(2.063) \approx 1 - 0.9804 = 0.0196$$

So **p ≈ 0.0196 or about 1.96%**.

Decision rule: reject H₀ if p < α.

$$0.0196 < 0.05 \ \Rightarrow\ \textbf{reject } H_0$$

Both methods always give the same conclusion — they're just two angles on the same picture.

---

## 🎯 Step 6 — Interpret the p-value correctly

This is the single most misunderstood number in statistics. The p-value = 0.0196 means:

> *"If the true mean really were 4.0 minutes, there would be about a 1.96% chance of seeing a sample mean as high as 4.35 (or higher) just from random sampling noise."*

It does **not** mean:

- "There's a 1.96% chance H₀ is true" ❌
- "There's a 98.04% chance wait times increased" ❌

It's **P(data | H₀)**, not **P(H₀ | data)**. That distinction is the whole Bayesian-vs-frequentist split.

---

## 🎯 Step 7 — Build a 95% confidence interval for μ (bonus)

A two-sided 95% CI gives a *range* of plausible values for the true mean:

$$\bar{X} \pm 1.96 \cdot \text{SE} = 4.35 \pm 1.96 \times 0.1697 = 4.35 \pm 0.333$$

$$\boxed{\text{95\% CI for } \mu \approx [4.02,\ 4.68]\ \text{minutes}}$$

Notice that **4.0 is not inside this interval** (just barely) — another way of seeing that the data are inconsistent with μ = 4.0.

---

## 🎯 Step 8 — Final conclusion in plain words

> At the 5% significance level, we reject the null hypothesis. The observed sample mean of 4.35 minutes (from 50 orders) provides statistically significant evidence that the true average wait time is longer than the manager's claimed 4.0 minutes (z = 2.06, p ≈ 0.0196). A 95% confidence interval suggests the true mean likely lies between 4.02 and 4.68 minutes.

Since p ≈ 0.02 is below 0.05 but above 0.01, we call this result **statistically significant** — but not *strongly* significant. The effect is real, but we shouldn't treat the evidence as overwhelming.

---

## 🧠 How every concept plugged in

| Concept from your notes | Where it appeared |
|---|---|
| Null hypothesis H₀ | Step 1: μ = 4.0 |
| Alternative H_A | Step 1: μ > 4.0 (one-sided) |
| Central Limit Theorem | Step 2: X̄ ≈ Normal even for unknown underlying distribution |
| Sample mean X̄ | Given: 4.35 |
| Population SD σ, sample size n | Given: 1.2 and 50 |
| Standard error SE = σ/√n | Step 3: 0.17 |
| Test statistic z | Step 4: 2.063 |
| Critical value / rejection region | Step 5A: z* = 1.645 |
| p-value = P(Z ≥ z_obs) | Step 5B: 0.0196 |
| Significance level α | 0.05 |
| Decision rule | Step 5: reject H₀ |
| Confidence interval | Step 7: [4.02, 4.68] |
| Interpretation | Step 8: evidence of change |

---

## 🔧 One key caveat: known vs unknown σ

In this example I gave you the **population σ = 1.2** (known). That's why we used a **z-test**. In real life σ is usually unknown and you estimate it with the sample S — then you use a **t-test** (same logic, the *t*-distribution replaces the normal). For n ≥ 30 the two give nearly identical numbers, which is why Brunton's football and website examples get away with the z approach.

That's the whole frequentist hypothesis-testing pipeline in one problem. Once this pattern feels natural — *assume H₀, compute a test stat, locate it on a known distribution, measure how extreme, decide* — you'll recognize it everywhere: A/B testing, clinical trials, model comparison, feature significance in regression. It's all the same game.

---
---

Absolutely — this is a great problem because it introduces **two-sided testing**, a new twist on what you already know. Let me walk through it fully, step by step, like the coffee shop one.

---

## 📋 Problem — NBA Scoring Change

Last NBA season:
- Average points per game: **μ₀ = 94.81**
- Population standard deviation: **σ = 7.16**

The league introduced **new rules intended to increase scoring**. But critics argued the rules might actually *decrease* scoring instead. So nobody knows which direction the change will go — only that it *might* change.

This season's first **n = 432 games** had an average of **X̄ = 94.69** points per game.

**Question:** Has anything *really changed*, or is 94.69 vs 94.81 just random noise?

---

## 🎯 Step 1 — State the hypotheses (two-sided!)

This is the new twist. Because critics argue it could go *either direction*, we test for **any change**, up or down:

$$H_0:\ \mu = 94.81 \quad \text{(nothing changed)}$$
$$H_A:\ \mu \neq 94.81 \quad \text{(something changed — either up or down)}$$

That **≠** sign is what makes this a **two-sided (two-tailed) test**. We'll reject H₀ if X̄ is *too far* from 94.81 in *either* direction.

---

## 🎯 Step 2 — Why the CLT lets us proceed

With n = 432 games (huge), the Central Limit Theorem guarantees:

$$\bar{X} \sim \mathcal{N}\!\left(\mu,\ \frac{\sigma^2}{n}\right)$$

Under H₀ (assuming μ = 94.81):

$$\bar{X} \sim \mathcal{N}\!\left(94.81,\ \frac{7.16^2}{432}\right)$$

---

## 🎯 Step 3 — Standard error

$$\text{SE} = \frac{\sigma}{\sqrt{n}} = \frac{7.16}{\sqrt{432}} = \frac{7.16}{20.785} \approx 0.3445$$

So under H₀, sample means of 432 games would typically wobble about **0.34 points** around 94.81.

---

## 🎯 Step 4 — Compute the test statistic z

$$z = \frac{\bar{X} - \mu_0}{\text{SE}} = \frac{94.69 - 94.81}{0.3445} = \frac{-0.12}{0.3445} \approx -0.348$$

So our observed X̄ is only about **0.35 standard errors below** what H₀ predicts. That's very close to the center — not suspicious at all.

Let's see it visually.

<img width="1416" height="692" alt="image" src="https://github.com/user-attachments/assets/d69b8aaf-e4e5-4427-8dc7-e292e2eefbd2" />

Notice the **two orange rejection regions** — one on each tail. Our observed X̄ = 94.69 (red line) sits right near the center, nowhere near either rejection zone.

---

## 🎯 Step 5 — Decide (two equivalent ways)

### Method A — Critical value approach (two-sided!)

For a two-sided test at α = 0.05, we split α between the two tails (2.5% each). The critical z-values are **±1.96**. Decision rule:

$$\text{Reject } H_0 \text{ if } |z_{\text{obs}}| \geq 1.96$$

Our |z_obs| = |−0.348| = 0.348 < 1.96 → **fail to reject H₀**.

### Method B — p-value approach (two-sided!)

Because extreme values on *either* side count as evidence against H₀, the p-value doubles:

$$p = 2 \cdot P(Z \geq |z_{\text{obs}}|) = 2 \cdot P(Z \geq 0.348)$$

$$p = 2 \cdot (1 - 0.636) = 2 \cdot 0.364 \approx 0.728$$

Since **p ≈ 0.73 ≫ 0.05** → **fail to reject H₀**.

---

## 🎯 Step 6 — 95% confidence interval (two-sided)

$$\bar{X} \pm 1.96 \cdot \text{SE} = 94.69 \pm 1.96 \times 0.3445 = 94.69 \pm 0.675$$

$$\boxed{\text{95\% CI for } \mu \approx [94.01,\ 95.37]}$$

And notice: **94.81 is comfortably inside this interval** → consistent with failing to reject H₀.

---

## 🎯 Step 7 — Conclusion in plain words

> *"At the 5% significance level, we fail to reject H₀ (z = −0.35, p ≈ 0.73). The observed drop from 94.81 to 94.69 points is not statistically significant — it's well within what we'd expect from random game-to-game variation if the rule change had no effect. We don't have evidence that scoring has changed in either direction."*

Important nuance: *"fail to reject"* ≠ *"the vaccine / rule does nothing."* It just means the data doesn't give us enough evidence to claim a real change. The true mean could still have shifted slightly; we just can't tell from this sample.

---

## 🆚 One-sided vs two-sided — the key difference

| | **One-sided** (coffee shop) | **Two-sided** (NBA) |
|---|---|---|
| H_A | μ > 4.0 | μ ≠ 94.81 |
| Question | "Did it *increase*?" | "Did it *change*?" |
| Rejection zone | one tail | both tails |
| Critical z | 1.645 | ±1.96 |
| p-value formula | P(Z ≥ z) | 2·P(Z ≥ \|z\|) |
| When to use | you have a *directional* prediction | change in *either* direction matters |

**Rule of thumb:** use two-sided by default. Only use one-sided if you have a strong prior reason to care about only one direction (and you'd genuinely ignore a change in the opposite direction).

---

## 🧠 Why this problem is instructive

It shows the *other* possible outcome — that sometimes data **doesn't** overturn H₀, and that's a legitimate, informative answer. A non-significant result doesn't mean the experiment "failed" — it means critics and supporters of the new rule both have to admit: **so far, the evidence doesn't show the rule change moved scoring in either direction**.

---

<img width="1052" height="1148" alt="image" src="https://github.com/user-attachments/assets/116b1d37-6618-441e-a322-10c978adfef2" />

### 🔴 Scenario A — X̄ = 94.00 falls in the **LEFT tail**

| | |
|---|---|
| z-score | −2.35 |
| p-value | 0.019 |
| Decision | Reject H₀ |
| **Conclusion** | Scoring **decreased** — the critics were right |

The observed average dropped far enough below 94.81 that it would only happen ~2% of the time by chance if nothing changed.

---

### ⚫ Scenario B — X̄ = 94.69 falls in the **CENTER** (our actual data)

| | |
|---|---|
| z-score | −0.35 |
| p-value | 0.728 |
| Decision | Fail to reject H₀ |
| **Conclusion** | No evidence of change |

The observed mean is well within the range of normal random fluctuation. We can't tell whether the rule change had any effect.

---

### 🟢 Scenario C — X̄ = 95.70 falls in the **RIGHT tail**

| | |
|---|---|
| z-score | +2.58 |
| p-value | 0.010 |
| Decision | Reject H₀ |
| **Conclusion** | Scoring **increased** — the rule change worked as intended |

The observed average rose far enough above 94.81 that it would only happen ~1% of the time by chance if nothing changed.

---

### 🧠 Key insight about two-sided testing

Notice how the **same test** answers three different questions depending on where X̄ lands:

```
     REJECT          FAIL TO REJECT          REJECT
   "decreased"      "no evidence"          "increased"
   ←──────────|═══════════════════════|──────────→
             94.13                   95.49
                       μ₀ = 94.81
```

You don't have to decide in advance which direction you expect — the two-sided test is *direction-agnostic*. It just asks: *"Is X̄ too far from μ₀, in either direction?"* Once you've rejected H₀, you simply look at the **sign of z** (or whether X̄ is above or below μ₀) to say which way the change went.

That's the beauty of two-sided testing: it lets the data tell you both *whether* something changed **and** *in which direction*. 📊

---
---

## 📋 Problem — Salk Polio Vaccine Trial (1954)

- **Treatment group:** n₁ = 200,000 kids got the real vaccine → 57 got polio
- **Control group:** n₂ = 200,000 kids got a placebo → 142 got polio

**Question:** Does the vaccine actually *reduce* polio rates, or is 57 vs 142 just random luck?

---

## 🎯 Step 1 — State the hypotheses (one-sided)

We're specifically testing whether the vaccine *lowers* the infection rate — a directional claim. So this is a one-sided (left-tailed) test.

Let:
- **p₁** = true polio rate in vaccinated population
- **p₂** = true polio rate in placebo population

$$H_0:\ p_1 = p_2 \quad \text{(vaccine does nothing — both groups have the same rate)}$$
$$H_A:\ p_1 < p_2 \quad \text{(vaccine lowers the rate)}$$

---

## 🎯 Step 2 — Compute the sample proportions

$$\hat{p}_1 = \frac{57}{200{,}000} = 0.000285 \quad (\text{0.0285\% of vaccinated kids got polio})$$

$$\hat{p}_2 = \frac{142}{200{,}000} = 0.000710 \quad (\text{0.0710\% of placebo kids got polio})$$

Observed difference:

$$\hat{p}_1 - \hat{p}_2 = 0.000285 - 0.000710 = -0.000425$$

So the vaccine group had **0.0425 percentage points lower** polio rate — looks promising, but is it statistically real?

---

## 🎯 Step 3 — Pooled proportion (assumed under H₀)

Under H₀, both groups come from the *same* population with a common rate. We estimate that shared rate by **pooling** all 400,000 kids:

$$\hat{p} = \frac{57 + 142}{200{,}000 + 200{,}000} = \frac{199}{400{,}000} = 0.0004975$$

---

## 🎯 Step 4 — Compute the standard error

For a difference of two proportions under H₀:

$$\text{SE} = \sqrt{\hat{p}(1-\hat{p})\left(\frac{1}{n_1} + \frac{1}{n_2}\right)}$$

Plug in:

$$\text{SE} = \sqrt{0.0004975 \times 0.9995025 \times \left(\frac{1}{200{,}000} + \frac{1}{200{,}000}\right)}$$

$$= \sqrt{0.0004975 \times 0.9995025 \times 0.00001}$$

$$= \sqrt{4.973 \times 10^{-9}}$$

$$\approx 0.0000705$$

So under H₀, the difference p̂₁ − p̂₂ would typically wobble about **0.0000705** around 0 across hypothetical repeated trials.

---

## 🎯 Step 5 — Compute the test statistic z

$$z = \frac{(\hat{p}_1 - \hat{p}_2) - 0}{\text{SE}} = \frac{-0.000425}{0.0000705} \approx -6.03$$

The observed difference is about **6 standard errors below zero** — astronomically far from what H₀ predicts.

Let me visualize this.

<img width="1412" height="736" alt="image" src="https://github.com/user-attachments/assets/e3c8842d-72c7-4f28-9703-34ce93d63556" />

Look at where the red line (observed result) lands — it's *way* out in the left tail, completely outside the visible curve. That's how overwhelming the evidence was.

---

## 🎯 Step 6 — Decide (both methods)

### Method A — Critical value

For a one-sided (left-tailed) test at α = 0.05: critical z = **−1.645**.

Our z_obs = **−6.03** ≪ −1.645 → **reject H₀ decisively**.

### Method B — p-value

$$p = P(Z \leq -6.03) \approx 8.4 \times 10^{-10}$$

That's about **0.00000008%** — roughly **1 in 1.2 billion** chance.

Since p ≪ 0.05 (or even ≪ 0.001) → **reject H₀ with overwhelming confidence**.

---

## 🎯 Step 7 — Effect size (not just significance!)

Statistical significance tells us the effect is real, but we also want to know *how big* it is. Two ways to measure:

**Relative risk reduction:**

$$\frac{\hat{p}_2 - \hat{p}_1}{\hat{p}_2} = \frac{0.000710 - 0.000285}{0.000710} \approx 0.60$$

The vaccine **reduced polio incidence by about 60%** — a massive, life-saving effect.

**Absolute risk reduction:** 0.0425 percentage points — sounds tiny, but at population scale it means preventing **hundreds of thousands of cases**.

---

## 🎯 Step 8 — Conclusion in plain words

> *"At any reasonable significance level (α = 0.05, 0.01, 0.001), we reject H₀ decisively (z = −6.03, p ≈ 10⁻⁹). The polio rate among vaccinated children (0.0285%) is dramatically and statistically significantly lower than among placebo children (0.0710%). The chance of seeing a gap this big purely by random luck, if the vaccine truly did nothing, is about 1 in a billion. The Salk vaccine works — it reduces polio incidence by roughly 60%."*

---

## 🧠 What's new compared to the coffee shop problem

| | Coffee shop | Salk vaccine |
|---|---|---|
| What's being compared | One sample mean vs a fixed μ₀ | **Two** sample proportions against each other |
| Null parameter | μ = 4.0 | p₁ − p₂ = 0 |
| Statistic | X̄ | p̂₁ − p̂₂ |
| Standard error | σ/√n | √[ p̂(1−p̂)(1/n₁ + 1/n₂) ] with **pooled** p̂ |
| Direction | one-sided (μ > 4.0) | one-sided (p₁ < p₂) |
| Result | z = 2.06, p = 0.02 (modest) | z = −6.03, p ≈ 10⁻⁹ (crushing) |

Everything else is **identical**: assume H₀, compute a test statistic, compare to the standard normal, decide. Once you see this pattern, you can apply it to any comparison — two means, two proportions, paired data, whatever.

---

## 🧠 Why this trial is legendary

The p-value of 10⁻⁹ is one of the most convincing results in medical history. It's not "probably works" — it's "the universe would have to break for this to happen by chance." That's why, on April 12, 1955, when the results were announced, church bells rang across America, and within months the vaccine was mass-distributed. Polio cases dropped by 90%+ within a decade.

