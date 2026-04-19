---
layout: page
title: "Cohort Component Models: Time-Series Structure and Forecast Foundations"
---

<script async src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-mml-chtml.js"></script>

$$
\newcommand{\ges}{\geqslant}
\newcommand{\les}{\leqslant}
\newcommand{\ve}{\varepsilon}
\newcommand{\E}{\mathbb{E}}
\newcommand{\diag}{\textrm{diag}}
\newcommand{\vf}{\varphi}
\newcommand{\mbR}{\mathbb{R}}
\newcommand{\mP}{\mathbb{P}}
\newcommand{\mF}{\mathcal{F}}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
$$

## 1. Introduction

At work I have been forecasting workforce volume by standard methods like ARIMA and neural nets. Then I came across the cohort component method---apparently the standard tool for demographic contexts, and something I had never encountered in school. I had to figure out from scratch how to measure stock, inflow, and outflow, and compare it to other approaches. It turned out to be structurally a generalisation of AR(1).

Along the way I wanted to refresh the basics: what does a forecast problem actually look like at the level of definitions, what is the difference between a predictor and an estimator, where does the irreducible error floor come from. And a further question: when we draw a forecast curve, I wanted to visually combine it with the full underlying process---a partial reconstruction of the bundle of sample paths. That connects prediction with statistical inference in a way I had not thought about carefully before.

I moved on from this topic and did not want to lose what I had accumulated. So I put it all together here. It is an eclectic piece. No claims to new results or business insights. But if you have worked on similar problems---workforce planning, membership registries, any population with inflow and attrition---maybe we are on the same wavelength.

The note is structured as follows. Section 2 builds the cohort component model as a set-theoretic accounting identity. Section 3 gives canonical definitions of inflow and outflow. Section 4 shows that under constant rates the recursion is exactly AR(1) with drift, and the multi-group version is VAR(1). Section 5 covers estimation of age profiles. Section 6 develops the statistical foundation of point forecasting in the i.i.d. setting. Section 7 examines what it means to attach a bundle of sample paths to a forecast curve, using AR(1) as the concrete example.

## 2. Cohort Snapshots and the Accounting Identity

### Notation: beginning versus end of period

Let $$S_t^b$$ and $$S_t^e$$ denote the set of cohort members present at the beginning and end of time period $$t$$ respectively. In general $$S_{t+1}^b \neq S_t^e$$, and so $$\abs{S_{t+1}^b} \neq \abs{S_t^e}$$.

**Example 1.** Suppose members $$\{X,Y,Z\}$$ are present during period $$t$$. At the end of $$t$$, member $$Z$$ exits; at the start of $$t+1$$, members $$U$$ and $$V$$ enter. Then

$$
S_t^b = S_t^e = \{X,Y,Z\}, \qquad
S_{t+1}^b = \{X,Y,U,V\}.
$$

The discrepancy between $$S_t^e$$ and $$S_{t+1}^b$$ reflects the simultaneous exit of $$Z$$ and entry of $$U,V$$ at the period boundary.

**Example 2.** Suppose $$\{X,Y,Z\}$$ are present at the start of $$t$$, member $$U$$ enters during $$t$$, member $$V$$ enters at the start of $$t+1$$, and all five members remain through $$t+1$$. Then

$$
S_t^b = \{X,Y,Z\},\quad
S_t^e = \{X,Y,Z,U\},\quad
S_{t+1}^b = \{X,Y,Z,U,V\}.
$$

The choice of whether recorded data represents the cohort at the beginning or end of each period is a modelling decision that affects all subsequent recursions.

### Time-series recursion: beginning-of-period convention

Let $$I_t$$ denote the set of members who enter during period $$t$$ and $$O_t$$ the set who exit. The general recursion is

$$
\abs{S_{t+1}^b} = \abs{S_t^b} + \abs{I_t} - \abs{O_t}. \tag{1}
$$

Define the attrition rate $$r_t = \abs{O_t}/\abs{S_t^b}$$. When the probability of exiting in the same period as entry is negligible, outflow draws only from the beginning-of-period stock, giving

$$
\abs{S_{t+1}^b} = (1-r_t)\abs{S_t^b} + \abs{I_t}. \tag{2}
$$

### Time-series recursion: end-of-period convention

Under the end-of-period convention,

$$
\abs{S_{t+1}^e} = \abs{S_t^e} + \abs{I_{t+1}} - \abs{O_{t+1}}, \tag{3}
$$

with attrition rate $$r_t = \abs{O_t}/\abs{S_{t-1}^e}$$, leading to

$$
\abs{S_t^e} = (1-r_t)\abs{S_{t-1}^e} + \abs{I_t}. \tag{4}
$$

Both recursions have the same algebraic structure; only the index convention differs. In what follows we adopt the end-of-period convention and write $$S_t$$ for $$\abs{S_t^e}$$, dropping the superscript.

### Example

Consider the following sequence of events:

- on day 1 of month $$A$$, the cohort consists of $$\{X,Y,Z\}$$;
- on day 2, $$Z$$ exits and $$U$$ enters;
- on the last day, $$Y$$ exits and $$V$$ enters;
- on day 1 of month $$B$$, $$X$$ exits and $$W$$ enters.

Then:

$$
S_A^b = \{X,Y,Z\},\quad S_A^e = \{X,U,V\},\quad
I_A = \{U,V\},\quad O_A = \{Y,Z\},
$$

and $$W \in I_B$$, $$X \in O_B$$.

## 3. Set-Theoretic Foundations

### Canonical definitions

**Definition 3.** Let $$S_t$$ be the set of cohort members at the end of period $$t$$. Define

$$
I_t = S_t \setminus S_{t-1} \quad\text{(inflow)}, \qquad
O_t = S_{t-1} \setminus S_t \quad\text{(outflow)}.
$$

From these definitions: $$O_t \subset S_{t-1}$$ so $$\abs{O_t} \les \abs{S_{t-1}}$$; $$I_t \subset S_t$$ so $$\abs{I_t} \les \abs{S_t}$$. The pairs $$(I_t, O_t)$$, $$(S_t, I_{t+1})$$, $$(S_t, O_t)$$, and $$(I_t, S_{t-1}\cap S_t)$$ are all disjoint.

**Remark 4.** A member who enters and exits within the same period appears in neither $$I_t$$ nor $$O_t$$, since they are absent from both $$S_t$$ and $$S_{t-1}$$. A member who enters at the very end of $$t-1$$ (appearing in $$S_{t-1}$$) but exits at the start of $$t$$ (absent from $$S_t$$) is counted in $$I_{t-1}$$ and $$O_t$$, even if their effective tenure was shorter than that of the first member. This boundary effect is an inherent feature of the snapshot convention.

### The accounting identity

**Proposition 5.** _For all $$t \ges 1$$,_

$$
\abs{S_{t+1}} = \abs{S_t} - \abs{O_{t+1}} + \abs{I_{t+1}}. \tag{5}
$$

_Proof._ From the definitions, $$S_{t+1} = (S_t \setminus O_{t+1}) \cup I_{t+1}$$ with the two sets on the right disjoint. Since $$O_{t+1} \subset S_t$$, we have $$\abs{S_t \setminus O_{t+1}} = \abs{S_t} - \abs{O_{t+1}}$$, and the result follows. $$\square$$

### Outflow and inflow rates

Writing $$S(t)$$, $$O(t)$$, $$I(t)$$ for the cardinalities, equation (5) becomes

$$
S(t+1) = S(t) - O(t+1) + I(t+1). \tag{6}
$$

Define the rates

$$
\begin{align}
r_t^O &= \frac{O(t+1)}{S(t)}, \tag{7}\\[3pt]
r_t^I &= \frac{I(t+1)}{S(t)}, \tag{8}\\[3pt]
R_t^I &= \frac{I(t)}{S(t)}. \tag{9}
\end{align}
$$

Since $$O(t+1) \les S(t)$$, we have $$r_t^O \in [0,1]$$. The rate $$r_t^I$$ may exceed $$1$$ if inflow is large; $$R_t^I \in [0,1]$$ since $$I_t \subset S_t$$.

## 4. AR(1) and VAR(1) Representations

### VAR(1) structure of the scalar model

Define the state vector $$X(t) = (S(t), O(t+1), I(t+1))^\top$$ and write the VAR(1) model $$X(t+1) = c + AX(t) + e(t+1)$$. In expanded form:

$$
\begin{cases}
S(t+1) = c_1 + A_{11}S(t) + A_{12}O(t+1) + A_{13}I(t+1) + e_1(t+1),\\
O(t+2) = c_2 + A_{21}S(t) + A_{22}O(t+1) + A_{23}I(t+1) + e_2(t+1),\\
I(t+2) = c_3 + A_{31}S(t) + A_{32}O(t+1) + A_{33}I(t+1) + e_3(t+1).
\end{cases}
$$

Comparing the first equation with (6) gives $$c_1 = 0$$, $$A_{11} = 1$$, $$A_{12} = -1$$, $$A_{13} = 1$$. If the rates $$r^O$$ and $$R^I$$ are constant, then $$O(t+2) = r^O S(t+1)$$ and $$I(t+2) = R^I S(t+1)$$, so the second and third equations are proportional to the first.

### AR(1) with drift

Under constant attrition rate $$r^O$$ and constant inflow $$I$$, adding a mean-zero noise term $$\ve(t)$$, equation (6) becomes

$$
S(t) = (1-r^O)\cdot S(t-1) + I + \ve(t). \tag{10}
$$

This is an AR(1) process with drift. The coefficient $$1-r^O \in (0,1)$$ guarantees stationarity of the noise component.

### Long-run equilibrium

Taking expectations with $$m(t) = \E S(t)$$:

$$
m(t) = (1-r^O)\cdot m(t-1) + I.
$$

Iterating and summing the geometric series,

$$
m(t) = (1-r^O)^t\cdot m(0) + \frac{1-(1-r^O)^t}{r^O}\cdot I.
$$

As $$t \to \infty$$, $$(1-r^O)^t \to 0$$ and

$$
m(t) \;\to\; \frac{I}{r^O}.
$$

The long-run cohort size is determined entirely by the ratio of inflow to attrition rate.

### Stationary decomposition

Setting $$S_0(t) = S(t) - m(t)$$, the centred process satisfies

$$
S_0(t) = (1-r^O)\cdot S_0(t-1) + \ve(t),
$$

which is a zero-mean AR(1). Since $$r^O \in (0,1)$$, this process is stationary. The original series decomposes as $$S(t) = S_0(t) + m(t)$$, where the first component fluctuates around zero and the second converges to $$I/r^O$$.

### Multi-age-group VAR(1)

Let $$S_i(t)$$ be the cohort size in age group $$i = 1,\ldots,N$$, with constant age-specific inflows $$I_i$$ and attrition rates $$r_i^O$$. The system

$$
\begin{cases}
S_1(t) = I_1 + \ve_1(t),\\
S_i(t) = (1-r_{i-1}^O)\cdot S_{i-1}(t-1) + I_i + \ve_i(t),
  \quad i = 2,\ldots,N,
\end{cases}
$$

can be written as $$X(t) = AX(t-1) + I + \ve(t)$$, where $$X(t) = (S_1(t),\ldots,S_N(t))^\top$$ and

$$
A =
\begin{pmatrix}
0 & 0 & \cdots & 0\\
1-r_1^O & 0 & \cdots & 0\\
0 & 1-r_2^O & \cdots & 0\\
\vdots & \vdots & \ddots & \vdots\\
0 & 0 & \cdots\; 1-r_{N-1}^O & 0
\end{pmatrix}.
$$

This is a VAR(1) model. The matrix $$A$$ is strictly sub-diagonal with non-negative entries; its spectral radius satisfies $$\rho(A) < 1$$, guaranteeing stability.

**Remark 6.** If age groups span more than one year, each time unit must equal the group width.

## 5. Connection between AR(1) with and without Drift

**Proposition 7.** _Let $$x_n = \vf x_{n-1} + \ve*n$$, $$n \ges 1$$, with $$x_0$$ given and $$\vf \in \mbR$$. Then*

$$
x_n = \vf^n x_0 + \sum_{i=1}^n \vf^{n-i}\ve_i, \qquad n \ges 0.
$$

_Proof._ By induction: the formula holds for $$n=0,1$$. If it holds for $$n$$, then

$$
x_{n+1} = \vf x_n + \ve_{n+1}
= \vf^{n+1}x_0 + \sum_{i=1}^n \vf^{n+1-i}\ve_i + \ve_{n+1}
= \vf^{n+1}x_0 + \sum_{i=1}^{n+1} \vf^{n+1-i}\ve_i. \qquad\square
$$

**Proposition 8.** _Let $$y_n = \vf y_{n-1} + \delta + \ve*n$$, where $$\vf,\delta\in\mbR$$, $$\{\ve_n\}$$ are i.i.d. with zero mean, and $$y_0$$ has finite mean. Then $$y_n = x_n + m_n$$, where*

$$
x_n = \vf x_{n-1} + \ve_n \quad\text{(zero-mean AR(1))}, \qquad
m_n = \vf m_{n-1} + \delta \quad\text{(deterministic)}.
$$

_Proof._ Set $$m_n = \E y_n$$. Taking expectations in the recursion gives $$m_n = \vf m_{n-1} + \delta$$. Then $$x_n = y_n - m_n$$ satisfies $$x_n = \vf x_{n-1} + \ve_n$$. $$\square$$

**Proposition 9.** _For any $$n \ges 0$$,_

$$
y_n = \vf^n x_0 + \sum_{i=1}^n \vf^{n-i}\ve_i
      + \vf^n m_0 + \frac{1-\vf^n}{1-\vf}\,\delta
\qquad (\vf \neq 1).
$$

_Proof._ Apply Proposition 7 to both $$x_n$$ and $$m_n$$, then use the decomposition $$y_n = x_n + m_n$$. $$\square$$

When $$\ve_i \equiv 0$$, the deterministic trajectory takes the compact form

$$
y_n = a + b\vf^n, \qquad
a = x_0 + m_0 - \frac{\delta}{1-\vf}, \quad
b = \frac{\delta}{1-\vf} \qquad (\vf \neq 1). \tag{11}
$$

For $$\vf = 1$$ the formula is replaced by $$y_n = x_0 + m_0 + n\delta$$.

## 6. Auxiliary: Estimating Age Profiles from Cohort Data

The AR(1) and VAR(1) models require numerical values for $$r^O$$ and $$I$$. These must be estimated from historical data. This section describes the estimation problem that arises when the data take the form of annual cohort bar charts indexed by age.

### Empirical age profiles

For each annual cohort $$i = 1,\ldots,n$$, two age profiles are of interest:

- the _inflow count profile_: for each age $$a$$, the number of members $$I_i(a)$$ who entered at age $$a$$;
- the _outflow rate profile_: for each age $$a$$, the ratio $$r_i^O(a) = O_i(a)/S_i(a)$$.

Plotting these as bar charts---age on the horizontal axis, the respective quantity on the vertical---gives the raw empirical picture.

Such bar charts are typically irregular for two reasons. First, _gaps_: not every age is represented in every cohort, especially at the extremes where cohort sizes are small. Second, _noise_: the ratio $$O_i(a)/S_i(a)$$ is volatile when $$S_i(a)$$ is small, producing bar-to-bar fluctuations even if the underlying rate is smooth. Naively averaging across cohorts propagates both defects.

### Two objectives of curve fitting

Fitting a parametric curve $$f(a;\theta)$$ to the bar charts serves two distinct purposes that should be kept separate.

The first is _smoothing_: replacing noisy, gappy bars with a continuous curve that can be evaluated at any age, interpolating over missing values and suppressing sampling noise.

The second is _structural modelling_: imposing a functional form that encodes a substantive hypothesis about how the rate or count varies with age. For example:

- an _exponentially increasing_ outflow rate, $$f(a;\theta) = \theta_1 e^{\theta_2 a}$$ with $$\theta_2 > 0$$, captures the hypothesis that exit risk grows proportionally with age;
- a _power-decay_ form, $$f(a;\theta) = \theta_1/(a-\theta_2)^{\theta_3}$$ with $$\theta_3 \in (0,1)$$, models a profile that is steep at low ages and has a heavy tail;
- a _hump-shaped_ form (log-normal or log-logistic in $$a$$) models an inflow profile that peaks at a modal entry age and tapers on both sides.

The choice of family is a modelling decision: the parameters carry an interpretation, and the choice should be guided both by the shape of the empirical bar charts and by domain knowledge about the cohort dynamics.

### Multi-cohort least-squares estimation

Pooling across all $$n$$ annual cohorts under the stationarity assumption that $$f(a;\theta)$$ is stable over calendar time, the data are

$$
\{(a_{ij},\, y_{ij}),\; j = 1,\ldots,m_i\},\qquad i = 1,\ldots,n,
$$

where $$a_{ij}$$ is age and $$y_{ij}$$ the observed rate or count. Estimate $$\theta$$ by minimising

$$
\mathrm{SSE}(\theta)
= \sum_{i=1}^n \sum_{j=1}^{m_i}
  \bigl(f(a_{ij};\theta) - y_{ij}\bigr)^2.
$$

**Remark 10.** Pooling under a common $$\theta$$ assumes the age profile is stable across calendar years. Cohort-to-cohort variation visible in the bar charts is evidence against this assumption and warrants further investigation.

### Integrability constraint

When $$f(a;\theta)$$ models a rate, the implied total probability of the event occurring over all ages must not exceed one:

$$
\sum_{k=1}^{\infty} f(k;\theta) \les 1.
$$

This yields the constrained problem

$$
\begin{cases}
\displaystyle\sum_{i=1}^n \sum_{j=1}^{m_i}
  \bigl(f(a_{ij};\theta) - y_{ij}\bigr)^2 \mapsto \min,\\[6pt]
\displaystyle\sum_{k=1}^{\infty} f(k;\theta) \les 1,
\end{cases}
$$

handled by Lagrange multipliers with the infinite sum replaced by a finite truncation.

## 7. Forecast Foundations: Prediction without a Time Index

Before applying the AR(1) model to produce forecasts, it is worth establishing what "predicting a future observation" means rigorously in the simplest setting: an i.i.d. sequence, no time structure, no covariates.

### Statistics, estimators, and predictors

Fix a probability space $$(\Omega,\mF,\mP)$$ and let $$Y_1,\ldots,Y_n$$ be random variables on it. A _statistic_ is any Borel-measurable function $$T = f(Y_1,\ldots,Y_n)$$. Estimators and predictors are both statistics; what distinguishes them is the nature of their target.

A _point estimator_ targets a non-random functional of the unknown measure $$\mP$$, such as $$\E_\mP Y$$ or $$\mathrm{Var}_\mP(Y)$$.

A _point predictor_ targets a future, as yet unobserved random variable $$Y_{n+1}$$. The target is random rather than fixed, and this leads to different performance metrics: estimator quality is measured relative to a fixed number, predictor quality relative to a random outcome.

Despite this difference, we will see that in the i.i.d. case, and specifically under the squared-error criterion with target functional $$\theta(\mP) = \E_\mP Y$$, the best predictor of $$Y_{n+1}$$ and the best estimator of $$\E Y$$ are the same statistic. The two minimum values are however different, and their gap has a precise meaning established below.

### The MSE criterion for constant predictors

We measure the quality of a candidate constant predictor $$c \in \mbR$$ by its _mean squared error_

$$
\mathrm{MSE}(c) = \E(Y - c)^2.
$$

**Proposition 11.** _Among all constants $$c \in \mbR$$, the MSE is minimised by $$c^* = \E Y$$, and the minimum value is $$\mathrm{Var}(Y)$$._

_Proof._

$$
\E(Y-c)^2
= \mathrm{Var}(Y)
  - 2(c-\E Y)\underbrace{\E(Y-\E Y)}_{=\,0}
  + (c-\E Y)^2
= \mathrm{Var}(Y) + (c-\E Y)^2,
$$

minimised at $$c^* = \E Y$$. $$\square$$

Extending to all measurable functions of $$(Y_1,\ldots,Y_n)$$ does not help: since $$Y_{n+1}$$ is independent of the sample, the conditional expectation $$\E[Y_{n+1} \mid Y_1,\ldots,Y_n] = \E Y$$ is still a constant. The problem shifts from finding the optimal functional form to estimating the unknown $$\E_\mP Y$$ from data.

### Two MSE criteria for sample-based statistics

Suppose we observe $$(Y_1,\ldots,Y_n)$$ and wish to predict $$Y_{n+1}$$. For any statistic $$f(Y_1,\ldots,Y_n)$$, define

$$
\mathrm{MSE}_{\mathrm{pred}}(f)
= \E\bigl(Y_{n+1} - f(Y_1,\ldots,Y_n)\bigr)^2,
$$

$$
\mathrm{MSE}_{\mathrm{est}}(f)
= \E\bigl(\E Y - f(Y_1,\ldots,Y_n)\bigr)^2.
$$

These are different quantities in general; however, the statistic minimising one also minimises the other, though the two minimal values differ. The exact relationship is established in Proposition 13.

### The permutation lemma

**Lemma 12** _(Permutation equality)._ _Let $$Y_1,\ldots,Y_n,Y_{n+1}$$ be i.i.d. with $$\E Y_1^2 < \infty$$, and let $$f:\mbR^n\to\mbR$$ be Borel-measurable. Then\_

$$
\E\bigl(Y_{n+1} - f(Y_1,\ldots,Y_n)\bigr)^2
= \E\bigl(Y_1 - f(Y_2,\ldots,Y_{n+1})\bigr)^2.
$$

_Proof._ The vector $$(Y_1,\ldots,Y_{n+1})$$ has an exchangeable joint distribution. The cyclic permutation $$(1,\ldots,n,n{+}1) \mapsto (2,\ldots,n{+}1,1)$$ leaves the joint law unchanged and maps the left-hand expression to the right-hand one, so the expectations are equal. $$\square$$

### Relationship between the two minimum values

**Proposition 13** _(MSE decomposition)._ _Under the assumptions of Lemma 12, for any Borel-measurable $$f:\mbR^n\to\mbR$$,_

$$
\mathrm{MSE}_{\mathrm{pred}}(f)
= \mathrm{MSE}_{\mathrm{est}}(f) + \mathrm{Var}(Y). \tag{12}
$$

_In particular: (1) $$\mathrm{MSE}_{\mathrm{pred}}$$ and $$\mathrm{MSE}_{\mathrm{est}}$$ have the same minimiser $$f_0$$; (2) at the common minimiser, $$\mathrm{MSE}_{\mathrm{pred}}(f_0) = \mathrm{MSE}_{\mathrm{est}}(f_0) + \mathrm{Var}(Y)$$, and the gap $$\mathrm{Var}(Y)$$ cannot be reduced by any choice of predictor.\_

_Proof._ Write $$Y_{n+1} - f = (Y_{n+1} - \E Y) + (\E Y - f)$$ and expand the square. The squared terms give $$\mathrm{Var}(Y)$$ and $$\mathrm{MSE}_{\mathrm{est}}(f)$$. For the cross term, independence of $$Y_{n+1}$$ from $$(Y_1,\ldots,Y_n)$$ gives

$$
\E[(Y_{n+1}-\E Y)(\E Y - f)] =
\underbrace{\E[Y_{n+1}-\E Y]}_{=\,0} \cdot \E[\E Y - f] = 0.
$$

Equation (12) follows. Since $$\mathrm{Var}(Y)$$ is independent of $$f$$, both criteria share the same minimiser. $$\square$$

**Remark 14.** Lemma 12 and Proposition 13 establish the same fact---common minimiser---by different arguments. The lemma uses symmetry of the i.i.d. law and gives no information about the gap. The proposition uses independence to obtain the explicit decomposition, which also identifies the size of the gap.

### The ideal predictor and its dependence on the measure

Proposition 13 reduces prediction to estimation of $$\E_\mP Y$$. This quantity depends on the unknown measure $$\mP$$ and cannot appear in any computable statistic. The difficulty takes three equivalent forms.

(i) _The target functional is unknown._ The optimal predictor $$c^* = \E_\mP Y$$ is a functional of the unknown $$\mP$$.

(ii) _No statistic is universally optimal._ For a different measure $$\mP'$$, the target $$\E_{\mP'} Y \neq \E_\mP Y$$ in general. No fixed statistic minimises MSE simultaneously for all measures.

(iii) _Only data-computable statistics are admissible._ A predictor must be a function of $$(Y_1,\ldots,Y_n)$$ alone; any dependence on $$\mP$$ is not available to the analyst.

### Plug-in estimators

The three formulations above all lead to the same practical step. Suppose a candidate expression for the predictor contains a quantity $$\theta(\mP)$$ that depends on the unknown measure---for instance, $$\E_\mP Y$$ or $$\mathrm{Var}_\mP(Y)$$. Such a candidate is not a statistic: it mixes observable data with unobservable distributional parameters. The plug-in procedure replaces each such quantity by a sample-based estimator $$\widehat\theta(Y_1,\ldots,Y_n)$$, turning the candidate into a genuine statistic.

**Definition 15** _(Plug-in estimator)._ Given a candidate expression involving distributional quantities $$\theta_1(\mP),\ldots,\theta_k(\mP)$$, the _plug-in estimator_ is obtained by substituting consistent estimators $$\widehat\theta_j(Y_1,\ldots,Y_n)$$ for each $$\theta_j(\mP)$$.

For the present setting, the natural plug-in for $$\E_\mP Y$$ is the sample mean:

$$
\overline{Y}_n = \frac{1}{n}\sum_{i=1}^n Y_i.
$$

**Remark 16.** The plug-in procedure is a construction, not a theorem. It does not by itself guarantee that $$\overline{Y}_n$$ is optimal among all statistics. Establishing optimality requires a separate argument: in this case, that $$\overline{Y}_n$$ is the UMVUE of $$\E Y$$ for exponential families, or that it achieves the Cramér-Rao lower bound under regularity conditions.

## 8. Inference vs. Prediction: the Bundle of Paths

The forecast sections above answer the question: given data, what single number should we report as our prediction of $$Y_{t_0+h}$$? A different question is: what is the full probabilistic picture that goes with that number? When we draw a forecast curve, what is the bundle of sample paths it represents?

### The forecast curve and what it does not determine

Suppose we observe $$Y_0, Y_1, \ldots, Y_{t_0}$$ and assume the process is AR(1):

$$
Y_t = \varphi_0 + \varphi_1 Y_{t-1} + \ve_t, \qquad \ve_t \sim (0, \sigma^2).
$$

The optimal $$h$$-step-ahead forecast (in the MSE sense) is

$$
\hat Y_{t_0+h} = \varphi_0 \frac{1-\varphi_1^h}{1-\varphi_1} + \varphi_1^h Y_{t_0}
  \qquad (|\varphi_1| < 1). \tag{13}
$$

For AR(1) without drift ($$\varphi_0 = 0$$, $$\lvert\varphi_1\rvert<1$$) the forecast decays toward zero (Figure (1)). For AR(1) with drift it converges to the equilibrium $$\varphi_0/(1-\varphi_1)$$ from below or above (Figure (2)).

![Figure 1](/assets/images/notes/cohort-component/Figure-1.png)

Notice what (13) depends on: only $$\varphi_0$$, $$\varphi_1$$, and $$Y_{t_0}$$. The noise parameter $$\sigma$$ does not appear. The same forecast curve is consistent with an entire family of AR(1) processes, indexed by $$\sigma > 0$$.

### Many processes, one forecast

Fix $$\varphi_0$$ and $$\varphi_1$$ with $$\lvert\varphi_1\rvert<1$$, and let $$\sigma$$ vary. Each choice of $$\sigma$$ defines a different probability measure on path space, yet all produce the same optimal point forecast. The forecast curve does not determine the underlying process.

Figure (1a) extends Figure (1) by adding the bundle of sample paths (green) consistent with the observed history and the AR(1) model for any $$\sigma>0$$. The red line is the same MSE-optimal forecast regardless. Figure (1b) shows a more concentrated bundle, corresponding to a smaller $$\sigma$$---the red forecast is identical.

![Figure 2](/assets/images/notes/cohort-component/Figure-2.png)

The red line is the same in both figures. This illustrates the decoupling: $$\sigma$$ determines the width of the bundle but not the forecast.

This is not a bug. For the purpose of minimising MSE of a point predictor, $$\sigma$$ is simply irrelevant---it drops out of the first-order condition. It becomes relevant only when we ask about prediction intervals, or about the density of $$Y_{t_0+h}$$ rather than its expectation.

### Causal AR(1) and its MA($$\infty$$) representation

An AR(1) process is called **causal** if it can be expressed as a linear combination of present and past innovations only. This holds if and only if $$\lvert\varphi_1\rvert < 1$$. A causal AR(1) then has an MA($$\infty$$) representation:

$$
Y_t = \frac{\varphi_0}{1-\varphi_1} + \sum_{k=0}^{\infty} \varphi_1^k \ve_{t-k}.
$$

The coefficient sequence $$\{1, \varphi_1, \varphi_1^2, \ldots\}$$ decays geometrically. Any two AR(1) processes with the same $$\varphi_0, \varphi_1$$ but different $$\sigma$$ share this coefficient sequence; they differ only in the scale of the driving noise.

### Known vs. estimated coefficients; the plug-in step

The analysis above assumed $$\varphi_0$$ and $$\varphi_1$$ are known. In practice they are estimated from data, and the plug-in principle is applied: replace $$\varphi_0, \varphi_1$$ by $$\hat\varphi_0, \hat\varphi_1$$ in (13). This step is almost always performed without comment in textbooks, and the resulting uncertainty in the forecast---from estimating the coefficients---is typically ignored at the level of point prediction.

Formally, the plug-in forecast is no longer optimal in the MSE sense for the true unknown $$\varphi_1$$; it is optimal only if the estimates are exact. The additional estimation variance contributes to the total forecast error but is absorbed into the residual term.

### Optimality for multi-step forecasts

A natural question is whether the recursive formula---use the one-step forecast as a pseudo-observation, then apply the AR(1) recursion again---is optimal for $$h > 1$$.

The answer is yes, under the assumption that the process is AR(1) with known coefficients and the error is mean-zero. The $$h$$-step conditional expectation $$\E[Y_{t_0+h} \mid Y_0, \ldots, Y_{t_0}]$$ equals (13), and this conditional expectation is the MSE-optimal predictor by the standard argument (it minimises $$\E[(Y_{t_0+h} - f(Y_0,\ldots,Y_{t_0}))^2]$$ over all measurable $$f$$).

When coefficients are estimated via plug-in, the recursive formula is still standard practice; its optimality in that setting requires a separate argument which is often omitted in applied texts.

### Summary

- A forecast curve determines the conditional mean, not the full distribution. Many AR(1) processes (differing in $$\sigma$$) share the same forecast.
- $$\sigma$$ is invisible to point forecasting but essential for prediction intervals.
- AR(1) is causal when $$\lvert\varphi_1\rvert<1$$, and causal AR(1) has an MA($$\infty$$) representation. This makes explicit why $$\sigma$$ decouples: it enters only as a scale on the driving noise, not in the coefficient sequence.
- Plug-in of estimated coefficients is universally applied but introduces additional uncertainty that point-forecast theory does not account for.
- Multi-step recursive forecasts are MSE-optimal when coefficients are known; the plug-in case is left implicit in most treatments.

## 9. Conclusion

This note is eclectic by design. Three threads that happened to cross during the same week at work.

The cohort component recursion, under constant rates, is AR(1). Not an approximation---the same equation. Equilibrium size is $$I/r^O$$, the fixed point; convergence speed is $$1-r^O$$. The multi-group case is VAR(1). If you are building a workforce or membership forecast on cohort dynamics, the time-series toolkit applies directly.

Point forecasting in the i.i.d. setting: predictive MSE and estimation MSE differ by exactly $$\mathrm{Var}(Y)$$, for every predictor. That gap is irreducible. Best predictor and best estimator of $$\E Y$$ are the same statistic; they just measure against different targets.

And the question that connects the two: the forecast curve does not determine the underlying process. For AR(1), the same point forecast is consistent with any $$\sigma > 0$$. Prediction and inference are related but separate problems, and the plug-in step that bridges them is usually performed without comment.
