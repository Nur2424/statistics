# Hypothesis Testing

A complete walkthrough of classical (frequentist) hypothesis testing — from the logic of H₀ and H_A, through the Central Limit Theorem and z-statistics, all the way to p-values, confidence intervals, and decision rules.

## What's inside

- **`theory.md`** — the full theoretical guide: sample vs population, CLT, standard error, z-statistic, one-sided vs two-sided tests, p-value interpretation, confidence intervals, and common misconceptions.
- **`hypothesis_testing.ipynb`** — a runnable Jupyter notebook with Python implementations: basic sample statistics, CLT visualization, z-tests for means and proportions, reusable helper functions, and Monte Carlo simulation.
- **`problems/`** — worked example problems solved end-to-end with plots and full math:
  - Coffee shop wait times (one-sided, one mean)
  - NBA scoring after rule change (two-sided, one mean)
  - Salk polio vaccine trial (one-sided, two proportions)

## Quick start

```bash
pip install numpy scipy matplotlib pandas statsmodels
jupyter notebook hypothesis_testing.ipynb
```

## Core idea in one sentence

> *Hypothesis testing is the art of separating signal from noise by asking: "How weird would this data look if nothing interesting were happening?"*

## Why this matters

The same framework runs silently under most of modern data science: A/B testing, feature significance in regression, comparing ML model accuracies, cross-validation confidence intervals, clinical trials. Learn it once, recognize it everywhere.

## Source material

Concepts based on Steve Brunton's [Probability & Statistics Bootcamp](https://www.youtube.com/@Eigensteve) (University of Washington), with additional explanations and worked examples.
