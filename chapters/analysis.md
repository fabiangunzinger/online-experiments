# Analysis {#sec-analysis}

todo
- Neyman approach
- Fisher sharp null
- Regression
- ML approaches

- Consider producing table for each major case of a 3 x 3 table: method of analysis (Fisher, Neyman, Regression), and source of uncertainty (Sampling, Randomisation, Both)


## Regression for experiment evaluation
- Start with abadie2020sampling
- See @athey2017econometrics
- See also Friedman critique, Lin response, and Imbens Rubin book response

Why would we want to include covariates in a randomised experiment analysis?

1) If assignment to treatment is random within groups but not across groups (e.g. students in rural schools were a bit more likely to be assigned to small classes in the STAR experiment), then we need to check whether including school id changes the result.

2) Including controls that have explanatory power for our outcome of interest helps us estimate the causal effect with more precision (and thus increases power) because it lowers the residual variance of the outcome variable, which in turn reduces the standard error of the causal effect.


## Machine learning approaches

- There are a number of supervised-learning approaches to improve ATE estimation in observational studies, in particular in scenarios where we have a large number of potential covariates. @athey2017state mention three approaches.

- Double selection approach: use LASSO regression to select covariates that are correlated with outcome and one to select covariates correlated with treatment indicator. Then control for the union of them in the final regression of outcome on treatment indicator. This improves ATE estimator properties compared to a simple regularized regression of outcome on treatment and covariates.

- Balance covariates: there are a number of approaches for both large and small covariate cases to use ML models to find weights that balance covariates such that they mimic the data of a randomised experiment.

- Doubly robust estimators (two models: outcome model relating outcome to treatment and covariates, treatment model relating treatment to covariates. Doubly robust estimator remains consistent if at least one of the two models is correctly specified)



## Clustered randomised trials

Based on this talk by Matthias Lux: https://www.youtube.com/watch?v=XyKtuZXvwW8


### Framework

- In experiments, assignment uncertainty matters more than sampling uncertainty. Assignment uncertainty results from randomness created by treatment assignment (different assignment will give different ATE estimates), sampling uncertainty results from randomness created by drawing a sample from a larger population for your experiment (different samples lead to different results). Which is more important depends on how large your sample is relative to population. Model integrating both: https://arxiv.org/pdf/1706.01778.pdf

- If analysis unit is randomisation unit:

    - Simple means estimater is unbiased.
    - Adding covariates can lead to bias (compare with simple means estimater to determine whether this is to) and to worse inference (always interact covariates with treatment)
    
- If randomisation unit is coarser than analysis unit (i.e. cluster randomisation):

    - Simple means estimator is biased if there is a correlation between cluster total and cluster size.
    
    - Solution is to use Des Raj estimator.

- Stable Unit Treatment Value Assumption (SUTVA): unit treatment value independent of treatment status of other units.

### Randomisation unit = analysis unit

- Review OLS assumptions under sampling uncertainty (standard). What changes if we also have assignment uncertainty (e.g. in experiments?)

- OLS no longer unbiassed with experimental data, though bias very small for large samples. Adding controls problematic (freedman2008, lin2013agnostic): always interact.

- Use robust std errors: `cov_type=HCO` in Python (to allow for different variances in treat and control)

### Cluster randomised trial

Entire discussion based [this] talk by Matthias Lux, which walks through [Middleton and Aronow (2015)](https://joelmidd.github.io/papers/MiddletonAronow_Cluster%20Randomized.pdf).

Context: food delivery, randomising at the user level but analysis at order level.

Basic difference in means estimator is:

$$
\hat{\tau} = \frac{\sum_{i \in J_t}\sum_{j=1}^{n^o_i}Y_{ij}}{\sum_{i \in J_t}n^o_i}
           - \frac{\sum_{i \in J_c}\sum_{j=1}^{n^o_i}Y_{ij}}{\sum_{i \in J_c}n^o_i}
           = \frac{\sum_{i \in J_t}Y_{i}}{\sum_{i \in J_t}n^o_i}
           - \frac{\sum_{i \in J_c}Y_{i}}{\sum_{i \in J_c}n^o_i}
$$

where: $J_t$ and $J_c$ are the sets of treatment and control users, $n^o_i$ is the number of orders by user $i$, and $Y_{ij}$ is the value of order $j$ of user $i$.

Problems:
- Main difference to above case is that denominators are random variables (we don't know in advance how many orders users are going to place).
- This creates bias if there is a correlation between the cluster totals ($Y_i$) and the number of observations per cluster ($n^o_i$).
- Standard errors invalid because iid assumption violated. Can cluster, but they can be unreliable if cluster sizes are highly skewed.

Slide below:
- Distribution from simulation has two problems:
    - Much more mass in tails (can largely be solved by clustering)
    - Bimodal shape due to massive outlier - one mode if outlier in treatment, one if in control (solved by Des Raj estimator)

#### Des Raj estimator

- Challenge is to determine k, which is a prior estimate (i.e. from data that's not in the experiment) of the correlation between cluster totals ($Y_i$) and the number of observations per cluster ($n^o_i$).
- However, in many contexts (e.g. tech companies) we can estimate this based on historical data.

To implement in regression, use following:

Regress X on D, where X:

$$
X_i = \frac{N}{N^o}Y_i - k\left(\frac{N}{N^o}n_i^o - 1\right)
$$

where $N$ is total number of clusters and $N^o$ is total number of orders.

And use `cov_type=HCO` std errors.


Applications:
- When randomising at city level and analysing at order level (i.e. large "gap" between randomisation and analysis unit)

Sources:
    
- Rubin causal framework paper: https://www.tandfonline.com/doi/abs/10.1198/016214504000001880

