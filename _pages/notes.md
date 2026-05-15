---
layout: page
title: Notes
permalink: /notes/
---

#### Probability &amp; Statistics — Foundations

**1. Likelihood: from first principles**

The surface f(x,θ) as a single object with two readings. Gaussian, Bernoulli and Poisson families. Triangular and sigmoidal likelihood fences. Fisher information and score function.

[PDF](/assets/papers/likelihood_git_v10.pdf)
[TeX Source](/assets/papers/likelihood_git_v10.tex)

**2. Foundations of Generalised Linear Models**

Two approaches to the regression function: Radon–Nikodym derivative and conditional expectation. Their equivalence. Optimality, signed measures, and the GLM framework. Canonical links and IRLS.

[PDF](/assets/papers/glm_foundations_git_v11.pdf)
[TeX Source](/assets/papers/glm_foundations_git_v11.tex)

#### Stochastic Analysis

**3. Converting between Gaussian and lognormal families: a bridge to Girsanov's theorem**

Two strategies for converting between distributions: changing the map (pushforward) vs. reweighting the measure (Radon–Nikodym). Worked through for normal and lognormal families, then lifted to Girsanov's theorem and geometric Brownian motion.

[PDF](/assets/papers/girsanov_bridge_git_v7.pdf)
[TeX Source](/assets/papers/girsanov_bridge_git_v7.tex)

**4. Notes on p-variation**

Limit and supremum definitions of p-variation. Examples with step functions. The role of p relative to 1: subadditive vs. superadditive behaviour.

[Read on site]({% link _notes/p-variation.md %})
[PDF](/assets/papers/p_variation_git_v1.pdf)
[TeX Source](/assets/papers/p_variation_git_v1.tex)

**5. Cohort component**

These working notes establish the formal connection between cohort component
methods and linear time-series analysis, showing that under constant attrition
and inflow rates, the standard cohort recursion is algebraically equivalent to
an AR(1) or VAR(1) model. The notes cover the set-theoretic foundations of the
accounting identity, long-run equilibrium analysis, age-profile estimation from
historical data, and the statistical basis for point prediction under mean
squared error. Besides, notes cover some forecast foundations, comparing them to the statistical inference problem.

[Read on site]({% link _notes/cohort-component.md %})
[PDF](/assets/papers/cohort_component.pdf)
[TeX Source](/assets/papers/cohort_component.tex)

**6. MV Interview Prep — Question List + KS Test**

Notes from a conversation with a friend who works in model validation at a Canadian bank — the question list he uses when interviewing MV candidates, plus my own worked discussion of one item (the Kolmogorov–Smirnov test).

- [PDF](/assets/papers/mv_interview_prep_v16.pdf)
- [TeX Source](/assets/papers/mv_interview_prep_v16.tex)
- [Notebook](/assets/code/KS_test.ipynb) (full runnable simulation with plots, used for Section 9)

**What's in the PDF**

- **Sections 2–7:** the question list — linear regression assumptions, logistic regression, hypothesis testing, foundational probability, the structure of an MV project.
- **Section 8:** the mathematical skeleton of statistical testing — test as a Bernoulli-valued statistic, Type I/II errors, simple vs. composite hypotheses, pivots.
- **Section 9:** the KS test — the distribution-free property at finite n, the two convergence results, decision regimes, a worked example, simulation code.
- **Section 10:** quick reference table for regression diagnostics.

**What's in the notebook**

`KS_test.ipynb` is the full runnable version of the simulation sketched in Section 9.6 of the PDF: empirical CDF, KS statistic, Brownian bridge, KS random variable, KS CDF — plus plots of the empirical-vs-theoretical CDF, the simulated KS distribution (CDF and histogram), and a convergence check at n = 10, 100, 1000.

**What I'm working on now**

A fuller treatment of homoscedasticity and the Breusch–Pagan test. That will likely be the next document in this series.
