
## Effective sample size of test

### Effective Sample Size in a Two-Sample Test (Equal Variances Assumed)

When comparing two groups — treatment (size $n_t$) and control (size $n_c$) — and assuming equal variances, the effective sample size for estimating the variance of the difference in means is given by the harmonic mean:
$$
n_{\text{eff}} = \frac{1}{\frac{1}{n_t} + \frac{1}{n_c}}.
$$

This arises because the variance of the difference in means is:
$$
\text{Var}(\bar{Y}_T - \bar{Y}_C) = \sigma^2 \left( \frac{1}{N_T} + \frac{1}{N_C} \right)
$$

If we treat this as equivalent to the variance under a single sample of size $N_{\text{eff}}$, then:

$$
\text{Var}(\text{difference}) = \frac{2\sigma^2}{N_{\text{eff}}}
$$

Matching both sides gives the harmonic mean as the effective sample size.

### Interpretation

- The harmonic mean weights smaller group sizes more heavily.
- It reflects the information content for estimating differences — imbalanced samples reduce power.





## Relative effects

See zhou2023all, also Statsig docs

## Correlated data

See zhou2023all, hesterberg2024power

## How to choose key parameters

### MDE

- What are you balancing here? The size of the effect you are able to identify and the time it takes to do it.

- All else equal, the smaller a change you want to be able to detect, the longer it will take for the experiment to run because you need more sample size.

- The relevant question to ask here is "what counts as a practically relevant change?"

- To answer that, consider:

  - Maturity of service (the more mature, the smaller a change can be expected)

  - Size of service (the larger, the smaller a change still generates a lot of revenue)

  - Cost of change that need ot be covered

    - Cost of fully building out feature for launch (can be 0 when fully built out for experiment or high if we use painted door)

    - Cost of maintaining new code (new code has higher bugs, may increase code complexity and maintenance)

    - Other costs: e.g. does CPU utilization increase?


### Significance level

- What are you balancing here? The probabilities of making a type I and type II error.

- The higher significance level, the less likely we are to implement useless features (to make a Type I error) but the more likely we are to no implement useful features (to make a Type II error).

- Hence, gotta balance cost of implementing useless feature and cost of not implementing useful feature.

- Things that play into this:

  - How long will feature be in effect (less long lowers risk of implementing)?

  - How widely will it be deployed (less widely lowers risk of implementing)?

  - How many users will see it / where in the funnel is it (later in funnel lowers risk of implementation)

- What to do in practice:

    - Start from baseline values ($alpha = 0.05$)

    - Adjust depending on balance of risks

### Power

- What are you balancing here? The risk of making a Type II error and the time you have to wait for your results.

- All else equal, the higher a level or power you want, the longer you'll have to run the experiment to accumulate the requried sample size.

- Factors to consider:

  - How costly is it to not implement a useful feature.









## How to increase power

- for framing on how to increase power, Integrate larsen2023statistical section 2

- Power can be increased trivially by lowering the significance level, which we often don't want to do, or by increasing sample size, which we're often trying to avoid.

- Increase effect size

  - Ensure that only users who are exposed to the change are in the data to avoid dilution of the effect

- Optimally allocate variance proportions

  - Usually equal for highest power

  - Show why with many treatment variants, higher share in control is better

- Reduce metric variance
	- Choose metric with low variance
		- Indicator variables
		- Avoid count variables which have have increasing variance as experiment duration progresses
	- Use variance reduction technique
	- Trim outliers
	- Only include triggered users

- Use a one-sided test

	Effect of one-sided testing on required sample size.
	
	In general:
	$$
	N =  \frac{(t_a + t_{1-\kappa})^2}{P(1-P)}\left(\frac{\sigma}{\delta}\right)^2
	$$
	
	For $\alpha = 0.05$, we have $t_{\alpha}^{ts} = 1.96$ and $t_{\alpha}^{os} = 1.65$, while for $\kappa = 0.8$ we have $t_{1 - \kappa} = 0.84$. Hence:
	$$
	\frac{N^{os}}{N^{ts}} = \frac{ (1.64 + 0.84)^2}{(1.96 + 0.84)^2} = \frac{6.2}{7.84} = 0.79
	$$
	
	Hence, for given levels of power and significance, a one-sided test requires about 21 percent fewer observations.
	

## Problems with low power

- Truth inflation: underpowered studies only find a significant effect it the effect size is larger than the true effect size, leading to inflated claims of effect sizes.


## Power in online experiments

- @kohavi2014seven point out (in rule 7) that while general advice suggets that the CLT provides a good approximation for n larger than 30, the large skew in online metrics often requires many moer users. They recomment 355 * (skewness coefficient)^2.

The theory for power calculation was developed for metrics with fixed values such as hight or weight.

- (During an experiment, the treatment will still change the metric values, but in practice we often make the sensible assumption that the treatment effect will be small, so that the variance between treatment and control are the same. This, in turn, then justifies use of pre-experiment data under the assumption that pre-experiment and experiment data will be very similar.)

- In online experiments, we often experiment with metrics that are only defined for a specific period of time (e.g. conversion during a 1-month period starting on 15 March 2024).

- This makes power calculation more complicated.

- When calculating required sample size (and/or experiment duration) for an experiment we want to make sure that a Z-test performed at the end of the experiment with the required number of unique units has a certain pre-specified level of power.

- To calculate that required sample size we input a baseline metric value and the metric standard deviation.

- With metrics defined only for specific periods, how to calculate these two values is not straightforward.

- Let's see what we generally do when a metric value is fixed. 

- Actually, writing this and thinking of an example for the above makes me think that this might be an issue inherent to causal inference analysis. 

- An example where we don't have the problem is if we want to compare the height of Londoners to the height of Berliners. Here, we'd do the following:

	- Draw a random sample of Londoners and measure mean and variance of their heights.
	- Do the same for a random sample of Berliners.
	- Perform a Z-test and calculate its power.

- Writing the above makes me realise that the issue is inherent in ex-ante power calculations:
	- Even in the Londoners and Berliners height example above we'd run into the same problem if we wanted to calculate, before taking any samples, how many samples we'd have to take to have our Z-test be adequately powered.

- The problem arises once we rearrange the power formula from

$$
1 - \beta = f(\sigma^2, \delta, P, z_\alpha, z_{1-\beta})
$$
to
$$
N = \frac{z_\alpha + z_{1-\beta}}{P(1-P)}\frac{\sigma^2}{\delta^2}
$$
- Because we would use the first version at the time we perform the analysis when we have all the required inputs, whereas we perform the second one before the analysis when we have to estimate $\sigma$ and $\delta$.

- The core of the problem is that for many online experiment metrics baseline metric mean and variance change depending on (1) the size of the sample they are calculated from and (2) the period of time and period location they are calculated based on.

- (1) is always the case, even in the Londoners and Berliners example above. It's inherent in performing power calculations.

- (2) is an additional complications in many online experiments. The two components are period length and period location (i.e. do we measure period of length $t$ in January of September).

- Outside of periods that are non-representative because of seasonality reasons, ignoring period location should usually not be a big problem.

- However, period duration might make a difference.

- So, the core problem is that in online experiments, in addition to approximating the sample we calculate metrics based on we also have to approximate the time period.


- How big a difference does calculating means and stds based on different time periods make? The difference can be substantial. The below table shows means and std for order visit conversion for a set of UK users based on different period lengths.

![[order-conversion-visit-different-periods.png|300]]

- Required sample size is directly proportional to the sample variance, which means that using the variance based on one week instead of 1 month of data would increase required sample size by a factor of $\frac{0.36^2}{0.31^2} = 1.35$.

- How hard is it to decently estimate an appropriate time-period? There are two parts we have to estimate: required number of unique units, and how long it takes to gather data from these many units.

- The required number of units is determined by:
	- Metric value mean (for experiment-period-length long period)
	- Metric value variance (for experiment-period-length long period)

- To amount of time it takes us to gather data for the required number of units depends on traffic to the precise point of the user-funnel/app where the bucketing for the experiment takes place.


Solution:
- To ensure that analysis is correctly powered, calculate power every day and stop once adequately powered.
- If you want ex-ante guidance, use sensible approximations.

- First best: to know when analysis is sufficiently powered, calculate power daily and stop experiment when required level of power reached. This ensures that we have both (1) sample we use for analysis and (2) period used for analysis.

- Second best, if we performed power using data from, say, the first 7-days of the experiment period, we would have a subset of (1) and could intelligently estimate (2) because the observed traffic would take into account the bucketing point and we could estimate future traffic based on it (e.g. we can estimate unique user visits based on unique visits during first week, with different traffic being the result of different bucketing points, but we can estimate path for all bucketing points).

- Third best, if we want to perform power calculation before the experiment starts (i.e. because we want to provide duration estimate during experiment config), we could use data from recent history (e.g. calculated monthly), and calculated separately for each metric, market, and based on other relevant dimensions such as different time period. Though, here, taking into account bucketing points might be challenging and hard to scale. So think about good approximations.


## Best practices

- When aiming to estimate a precise effect size rather than just being interested in statistical significance, use assurance instead of power: instead of choosing a sample size to attain a given level of power, choose sample size so that confidence interval will be suitably narrow 99 percent of the time ([Sample-Size Planning for More Accurate Statistical Power: A Method Adjusting Sample Effect Sizes for Publication Bias and Uncertainty](https://www3.nd.edu/~kkelley/publications/articles/Anderson_Kelley_Maxwell_Psychological_Science_2017.pdf) and [Understanding the new statistics](https://tandfbis.s3.amazonaws.com/rt-media/pp/common/sample-chapters/9780415879682.pdf).)




## Experiment duration

- We usually care about power because it determines experiment runtime.

- There we walk about how to translate required N into runtime.

- [Simon Johnson -- Four Customer Characteristics That Should Change Your Experiment Runtime](https://www.geteppo.com/blog/four-customer-characteristics-that-should-change-your-experiment-runtime)

## Useful resources

- @larsen2023statistical for general overview
- @zhou2023all for comprehensive overview of how to calculate power
- @bojinov2023design, section 5, for simulation results for switchbacks and generally good approach to simulation to emulate
- @reich2012empirical power calcs for cluster-randomised experiments

- [Power Analysis for Experiments with Clustered Data, Ratio Metrics, and Regression for Covariate Adjustment](https://arxiv.org/pdf/2406.06834)


- [Statsig sample size calculation formula](https://www.statsig.com/blog/calculating-sample-sizes-for-ab-tests?utm_id=ZmFiaWFuLmd1bnppbmdlckBqdXN0ZWF0dGFrZWF3YXkuY29t&k_is=opl)



[^mutuallyexcl]: We can simply dd up the probability of the test statistic falling into the
upper and lower tail because the two events are independent.
