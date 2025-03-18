# Outcomes {#sec-outcomes}

## Potential outcomes

We have a population of units $i = 1, \dots, \N$. Each unit in the population can be exposed to one of two treatments, which are identical across units. Each unit $i$ has potential outcomes $\ypc$ and $\ypt$ corresponding to each of the two possible treatments. $\ypc$ is the outcome unit $i$ would experience if they received the control treatment; $\ypt$, the outcome they would experience if they received the active treatment.


Each unit is either assigned to treatment or control, so we'll only ever observe one of their potential outcomes. We denote the observed outcome for unit $i$ as $\yo$. This observed outcome equals $\ypt$ if the unit was assigned to treatment and $\ypc$ if they were assigned control. That is, if we write $\ti_i = 1$ if unit $i$ is allocated to treatment and $\ti_i = 0$ if they are allocated to control, then we can express the **observed outcome** for unit $i$ as

$$
\yo = \ti_i \ypt + (1 - \ti_i) \ypc.
$$ {#eq-yo}


## Effect of interest

What's the use of potential outcomes we can only partially observe? They provide
us with a simple and coherent way to think about the causal effect of the
treatment; for unit $i$, the **individual causal effect** is given by

$$
\ypt - \ypc,
$$

which tells us how the outcome for that unit is different when given the
active treatment compared to the counterfactual state of the world where they
were given the control treatment. This is what we mean by the causal effect of
the treatment.

We are usually interested in the average causal effect of the treatment across
all units, which is given by the **average treatment effect (ATE)**

$$
\te = \tefs = \tef
$$ {#eq-ate}

This is the quantity we're trying to estimate in a typical experiment (the estimand), though
other choices are possible.[^alternative_choices]


## Estimating the outcome of interest

Given that we only observe one potential outcome per unit, how can we estimate
@eq-ate?

In online experiments, we typically use simple random assignment to allocate
units to the expeirment sample and treatment variants (see
@sec-treatment-assignment for details). Random assignment to treatment implies
that

$$
\begin{align}
E[\ypt | \ti_i = 1] &= E[\ypt]\\
E[\ypt | \ti_i = 0] &= E[\ypt]
\end{align}
$$

That is: given that assignment is random, the expected outcome under treatment
is the same for units in the treatment and control group, and simply equals the
expected outcome under treatment across the entire population. The same is true
for expected outcomes under control, so that we have

$$
\begin{align}
E[\ypc | \ti_i = 1] &= E[\ypc]\\
E[\ypc | \ti_i = 0] &= E[\ypc]
\end{align}
$$

Now, expanding @eq-ate and using the above expressions we have:

$$
\begin{align}
\te &= \tef \\ 
&= E[\ypt] - E[\ypc] \\
&= E[\ypt | \ti_i = 1] - E[\ypc | \ti_i = 0],
\end{align}
$$

which says that we can express the true effect as the difference between the
expected outcomes of treated and untreated untis -- we have identified a way for
expressing the treatment effect in quantities we can actually estimate based on
available data. In particular, the above suggests the following
estimation approach:

$$
\tee = \teef
$$ {#eq-ate-est}

where

$$
\yotb= \yotbf \qquad \yocb = \yocbf.
$$

We can show that this is an unbiased estimator of $\te$:[^alternative_proof]

$$
\begin{align}
E\left[\tee\right] &= E\left[\yotb - \yocb\right] \\
&=E\left[\yotb\right]- E\left[\yocb\right] \\
&=E\left[\yotbf\right] - E\left[\yocbf\right] \\
&=\frac{1}{\Nt}\sum_{i:\ti_i=1} E\left[\yoi\right]
-\frac{1}{\Nc}\sum_{i:\ti_i=0} E\left[\yoi\right] \\
&=\frac{\Nt}{\Nt}E\left[\yoi|\tii=1\right]
-\frac{\Nc}{\Nc}E\left[\yoi|\tii=0\right] \\
&=E\left[\yoi|\tii=1\right] - E\left[\yoi|\tii=0\right] \\
&=E\left[\ypt\right] - E\left[\ypc\right] \\
&=\yptb - \ypcb \\
&=\te
\end{align}
$$


## Causal estimands

We can generalise the standard ATE calculation in a number of ways.

- We can focus only on a subset of the population, which can also happen in different ways.

- We can condition on covariates, such as when we focus only on women:
$$
\tau_{fs, f} = \frac{1}{N_f}\sum_{i: X_i = f} \left(Y_i(1) - Y_i(0)\right), \quad \text{where $X_i = \{m , f\}$ and $N_f = \sum_{i=1}^N \mathbb{1}_{X_i = f}$}
$$

- We can condition on treatment status, such as when we focus only on units that were exposed to the treatment: 

$$
\tau_{fs, t} = \frac{1}{N_t}\sum_{i: W_i = 1} \left(Y_i(1) - Y_i(0)\right), \quad \text{where $W_i = \{0 , 1\}$ and $N_t = \sum_{i=1}^N \mathbb{1}_{W_i = 1}$}
$$

- We can condition on potential outcomes, such as when we focus only on units with positive potential outcomes (e.g. positive earnings) regardless of treatment status:

$$
\tau_{fs, pos} = \frac{1}{N_{pos}}\sum_{i: Y_i(0)>0, Y_i(1)>0} \left(Y_i(1) - Y_i(0)\right), \quad \text{where $N_{post} = \sum_{i=1}^N \mathbb{1}_{Y_i(0)>0, Y_i(1)>0}$}
$$

- We can also generalise the estimand by focusing on more general functions of the potential outcomes (e.g. we may focus on the median outcome of the entire population or a subpopulation).

- In all these cases, we can write the causal estimand as a row-exchangeable function (a function that takes vectors or matrices as arguments and the result of which does not change if the rows in its input are permuted):

$$
\tau = \tau(Y(0), Y(1), X, W)
$$


## Quantile treatment effects

- Diff in quantiles between T and C observed outcome distributions, not quantiles of $Y_i(1) - Y_i(0)$.

- see 4.3 in athey2017econometrics



## Treatment effects

ATE is the average difference in potential outcomes of all subjects. E[y1 - y0]

ATE is a weighted average of ATET and average treatment effect of untreated.

ATE is also a weighted average of the compliers, always-takers, and never-takers.

ATE = ATET if treatment effects are homogenous (if they are not, then experiment measures ATET).

ATE = ITT under homogenous effects or under perfect compliance.


ATET (or TOT) is the average difference in potential outcomes of treated subjects. E[y1 - y0 | D=1] (D is treatment)

ATET = LATE in an experiment with no always-takers. This is relevant because there are many cases of onesided non-compliance (if participants who are randomised to treatment do not take it up but nobody who wasn't randomised to treatment has access).


ITT is the average difference in potential outcomes of those assigned to treatment (a weighted average between those who accepted treatment and those who didn't).

LATE is the average difference in potential outcomes of compliers.

