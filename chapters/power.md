
# Power {#sec-power}

todo:
- for framing on how to increase power, Integrate larsen2023statistical section 2
- Introduce separate symbol for MDES -- sample size formula is very confusing otherwise
- integrate zhou2023all
- Integrate hesterberg2024power
- Integrate list2011so
- Integrate [[power_in_abtesting]]


Power is the probability that we reject the null hypothesis if it is false. It is a key component of experiment design because it determines the required sample size, which helps us determine how long we need to run an experiment for.

In this section, I want to do the following:

- Derive the formula for power from first principles.

- Discuss implications of the formula for a number of experiment design aspects.

- ...

## Deriving the sample size formula

In the context of online experiments, we usually fix the probabilities of making [Type I and Type II errors](hypothesis_testing$types_of_errors), $\alpha$ and $\beta$, define the smallest true effect we want to be able to detect, $\tau^*$, and calculate how many units we have to collect data for. [Assuming equal sample sizes and variances](stats_of_online_experiments#standard_error), the answer to this is given by:
$$
\begin{align}
n = 4(z_{\alpha/2} + z_{1 - \beta})^2\frac{s^2}{\tau^*},
\end{align}
$$ {#eq-sampsi}

where $n$ is total sample size, $s^2$ is the [pooled variance](stats_of_online_experiments#standard_error) of the treatment groups, and $z_{\alpha/2}$ and $z_{1 - \beta}$ are the critical values of the standard normal distribution associated with the upper-tail probabilities of the pre-specified levels of significance $\alpha$ and power $1-\beta$.

The power formula can be intimidating and abstract (all the more so, because there are many different versions floating around).

The goal of this section is to demystify the formula. The best way to do that is to derive it from first principles, which helps us understand where the formula comes from and why it makes sense. In addition, I'll also show two more heuristic approaches to deriving the formula, which are faster to use in practice (e.g. to explain where the formula comes from to colleagues or stakeholders).

### Derivation from first principles

Power is the probability that we reject the null hypothesis if there exists a true effect of size $\tau^*$. 

We thus have:
$$
\begin{align}
&H_0: \tau = 0 \\[5 pt]
&H_A: \tau = \tau^*.
\end{align}
$$

We test the null hypothesis by constructing the test statistic
$$
Z = 
\frac{\hat{\tau}^{\text{dm}}}
{SE\left(\hat{\tau}^{\text{dm}}\right)},
$$

and reject $H_0$ if if falls into the rejection region beyond the critical value $z_{\alpha/2}$. Because the standard normal distribution is symmetric, for a two-sided test we thus reject $Z$ if
$$
\begin{align}
|Z| &> z_{\alpha/2} \\[5pt]

\left|\frac{\hat{\tau}^{\text{dm}}}{SE\left(\hat{\tau}^{\text{dm}}\right)}\right| 
&> z_{\alpha/2} \\[5pt]

\left|\hat{\tau}^{\text{dm}}\right| 
&> z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right).
\end{align}
$$

The power $1-\beta$ of the test given that $\tau = \tau^*$ is the probability that the test statistic $Z$ falls into the rejection region, which is:
$$
1 - \beta = P\left[
\left|\hat{\tau}^{\text{dm}}\right| 
> z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right)
\>\middle|\> H_A
\right].
$$

The test statistic falling into the lower or upper rejection region are mutually exclusive events, so the above is equal to
$$
1 - \beta 
= P\left[
\hat{\tau}^{\text{dm}} 
> z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right)\>\middle|\> H_A
\right]
+ P\left[
\hat{\tau}^{\text{dm}} 
< -z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right)\>\middle|\> H_A
\right]
$$

We can calculate these probabilities by standardising, which, using the standard normal CDF, $\Phi(z)$,  gives us

$$
\begin{align}
1 - \beta 
&= P\left[
\frac{\hat{\tau}^{\text{dm}} - \tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
>
\frac{z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right) - \tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
\right]

+ P\left[
\frac{\hat{\tau}^{\text{dm}} - \tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
<
\frac{-z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right) - \tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
\right]

\\[5pt]

&=
P\left[Z > \frac{z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right) - \tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
\right]

+ P\left[Z < \frac{-z_{\alpha/2}SE\left(\hat{\tau}^{\text{dm}}\right) - \tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
\right]

\\[5pt]

&=
P\left[Z > z_{\alpha/2} - \frac{\tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
\right]

+ P\left[Z < - z_{\alpha/2} - \frac{\tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}
\right]

\\[5pt]

&=1 - \Phi\left(z_{\alpha/2} - \frac{\tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}\right)
+ \Phi\left(- z_{\alpha/2} - \frac{\tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}\right)
\end{align}
$$

The probability that we reject the null hypothesis for the wrong reason -- because the test statistic falls below the lower critical value for a true positive effect or above the upper critical value for a true negative effect -- is very small.[^type3error] Hence, as the true effect size deviates from zero, one of the two terms in the expression above becomes vanishingly small and can be ignored. For the rest of this chapter, I assume we have a true positive effect and omit the second of the two terms. We thus have:

$$
\begin{align}
1 - \beta 
= 
1 - \Phi\left(z_{\alpha/2} - \frac{\tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)}\right)
\end{align}
$$

Using the symmetry of the standard normal distribution, which implies that $1 - \Phi(k) = \Phi(-k)$, we can simplify this to

$$
\begin{align}
1 - \beta 
= 
\Phi\left(\frac{\tau^*}{SE\left(\hat{\tau}^{\text{dm}}\right)} - z_{\alpha/2}\right).
\end{align}
$$

Assuming equal treatment group variance, we can use the standard error from @eq-se-equal, which gives us

$$
\begin{align}
1 - \beta 
= 
\Phi\left(\frac{\tau^*}{\sqrt{\frac{2s^2}{n_v}}} - z_{\alpha/2}\right).
\end{align}
$$


Final steps: use below comments for explanation
$$
\begin{align}
1 - \beta 
&= 
\Phi\left(\frac{\tau^*}{\sqrt{\frac{2s^2}{n_v}}} - z_{\alpha/2}\right) \\[1em]
\Phi^{-1}(1 - \beta) 
&= 
\frac{\tau^*}{\sqrt{\frac{2s^2}{n_v}}} - z_{\alpha/2} \\[1em]
\Phi^{-1}(1 - \beta) + z_{\alpha/2} 
&= 
\frac{\tau^*}{\sqrt{\frac{2s^2}{n_v}}} \\[1em]
\sqrt{\frac{2s^2}{n_v}} 
&= 
\frac{\tau^*}{\Phi^{-1}(1 - \beta) + z_{\alpha/2}} \\[1em]
\frac{2s^2}{n_v} 
&= 
\left(\frac{\tau^*}{\Phi^{-1}(1 - \beta) + z_{\alpha/2}}\right)^2 \\[1em]
n_v 
&= 
\frac{2s^2}{\left(\frac{\tau^*}{\Phi^{-1}(1 - \beta) + z_{\alpha/2}}\right)^2} \\[1em]
n_v 
&= 
\frac{2s^2\left(\Phi^{-1}(1 - \beta) + z_{\alpha/2}\right)^2}{\tau^{*2}}
\end{align}
$$




$$
1 - \beta = \Phi\left(\frac{\te}{\sefep} - z_{\alpha/2}\right), 
$$

which, with a bit of algebra (using $1/\sqrt{1/x} = \sqrt{x}$),
we can rewrite as

$$
1 - \beta = \Phi\left(\frac{\te}{\sev}\sqrt{P(1-P)N} - z_{\alpha/2}\right).
$$ {#eq-power}

To calculate the required sample size for an experiment, we can rearrange
@eq-power and solve for $N$. To do this, we use the inverse of the CDS function $\Phi(z)$. $\Phi(z)$ takes z-values and returns probabilities, so its inverse, $\Phi(p)^{-1}$, takes probabilities and returns z-values. Using this, we get:

$$
\begin{align}
\Phi(1 - \beta)^{-1} &= \Phi\left(\Phi\left(\frac{\te}{\sev}\sqrt{P(1-P)N} - z_{\alpha/2}\right)\right)^{-1} \\
z_{1 - \beta} &= \frac{\te}{\sev}\sqrt{P(1-P)N} - z_{\alpha/2} \\
\sqrt{P(1-P)N} &= (z_{1 - \beta} + z_{\alpha/2})\left(\frac{\sev}{\te}\right) \\
N &= \frac{(z_{1 - \beta} + z_{\alpha/2})^2}{P(1-P)}\frac{\vpe}{\te^2}.
\end{align}
$$ {#eq-sample-size}



### Starting from Type I and Type II error conditions

Use @list2011so

### Starting from graphical illustration


lh curve ...this is sampling dist of tee, know shape from sampling theory
reject h0 if value larger than za
rhs is sampling distr under ha
what is zk? 
now derive bloom formula...

@bloom1995minimum introduces the notion of the MDE as a useful way to quantify
power. In the process, he also uses an intuitive way to derive the power formula
based on an illustration of a typical hypothesis-testing scenario.

![Source: @duflo2007using, based on
@bloom1995minimum.](../inputs/power.png){#fig-power}

Let's start by understanding @fig-power, which visualises the setup of a
one-sided hypothesis test where the true effect equals 0 under the null
hypothesis and some positive constant $\te$ under the alternative hypothesis. Note that the curves are *not* the standard normal distribution,
but the sampling distribution of our estimator $\tee$. This means that the standard
deviation of the curves is given by the standard error of $\tee$, which is
$\se$. Under the assumption of a homogenous treatment effect, the standard
error is identical under $\hn$ and $\ha$, which is why the two curves have the
same shape (see @sec-experiment-stats for details).

the distribution will be the same under both the null and the alternative
hypothesis, with the center of each distribution given by our hypothesised value
of $\te$ -- zero under $\hn$ and a positive constant under $\ha$.

We reject $\hn$ if $\tee$ is to the right of the critical value $\za$. Also,
for a given level of power $\beta$, 





## Effective sample size of test

### Effective Sample Size in a Two-Sample Test (Equal Variances Assumed)

When comparing two groups — treatment (size $N_T$) and control (size $N_C$) — and assuming equal variances, the effective sample size for estimating the variance of the difference in means is given by the harmonic mean:

$$
N_{\text{eff}} = \frac{1}{\frac{1}{N_T} + \frac{1}{N_C}}
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



## Implications of the formula

## Rules of thumb -- the big 16 vs 32 confusion

Blog post on 16 or 32 power confusion:
- Reliably looking posts who get it wrong: (https://towardsdatascience.com/probing-into-minimum-sample-size-formula-derivation-and-usage-8db9a556280b --- starts with the wrong std error with N for total instead of variant sample size), there is also Kohavi book or paper that gets it wrong

- There is another way to express the variance, which has led to massive
confusion.

- I'm pretty sure its the 1/N vs 1/(N/2) error that accounts for the wrong
result, and nobody seems to derive this from first principles to check.

- Is original wrong? Check in book -- access through WBS.

## Different types of metrics

## Power for quasi-experimental studies




## Useful rule of thumb



Popular experiment textbooks and countless sources on the internet often refer to the rule-of thumb for the total sample size calculation that is given by:

$$
\N \approx \frac{32\vpe}{\tee^2}.
$$

Using formula @eq-sample-size we can see that the rule of thumb straightforwardly results from using the default parameters.

Assuming equal sample size, so that $P=0.5$ gives us

$$
N = 4 (z_{1 - \beta} + z_{\alpha/2})^2\left(\frac{\sev}{\te}\right)^2.
$$

Setting the false positive rate to 5% and the false negative rate at 20% for a two-sided hypothesis test, as we commonly do, we get

$$
\begin{align}
N &= 4 (0.84 + 1.96)^2\left(\frac{\sev}{\te}\right)^2 \\
&\approx \left(\frac{32\sev}{\te}\right)^2
\end{align}
$$


Give also per variant, as this is more useful to calculate sample size for experiments with n arms. 




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




## Old notes

Largely based on @duflo2007randomization


Power basics

![title](../inputs/power.png)


$$
n = \frac{(f(\alpha) + f(\beta))}{\text{Sample allocation}}\frac{\sigma}{\delta}
$$


- In the simplest possible, we randomly draw a sample of size $N$ from an identical population, so that our observations can be assumed to be i.i.d, and we allocate a fraction $P$ of our sample to treatment. We can then estiamte the treatment effect using the OLS regression

$$ y = \alpha + \beta T + \epsilon$$

- where the standard error of $\beta$ is given by $\sqrt{\frac{1}{P(1-P)}\frac{\sigma^2}{N}}$.

- std error derivation (from standard variance result of two independent samples, using population fractions):

$$
std = \sqrt{\frac{\sigma^2}{N_t} + \frac{\sigma^2}{N_c}} = \sqrt{\frac{\sigma^2}{PN} + \frac{\sigma^2}{(1-P)N}} = ... = \sqrt{\frac{1}{P(1-P)}\frac{\sigma^2}{N}}
$$

- The distribution on the left hand side below shows the distribution of our effect size estimator $\hat{\beta}$ if the null hypothesis is true.

- We reject the null hypothesis if the estimated effect size is larger than the critical value $t_{\alpha}$, determined by the significance level $\alpha$. Hence, for this to happen we need $\hat{\beta} > t_{\alpha} * SE(\hat{\beta})$ (follows from rearranging the t-test formula).

- On the right is the distribution of $\hat{\beta}$ if the true effect size is $\beta$.

- The power of the test for a true effect size of $\beta$ is the area under this curve that falls to the right of $t_{\alpha}$. This is the probability that we reject the null hypothesis given that it is false.

- Hence, to attain a power of $\kappa$ it must be that $\beta > (t_a + t_{1-\kappa}) * SE(\hat{\beta})$, where $t_{1-\kappa}$ is the value from a t-distribution that has $1-\kappa$ of its probability mass to the left (for $\kappa = 0.8$, $t_{1-\kappa} = 0.84$).

- This means that the minimum detectable effect ($\delta$) is given by:

$$ \delta = (t_a + tq_{1-\kappa}) * \sqrt{\frac{1}{P(1-P)}\frac{\sigma^2}{N}} $$

- Rearranding for the minimum required sample size we get:

$$ N =  \frac{(t_a + t_{1-\kappa})^2}{P(1-P)}\left(\frac{\sigma}{\delta}\right)^2 $$

- So that the required sample size is inversely proportional to the minimal effect size we wish to detect. This makes sense, it means that the smaller an effect we want to detect, the larger the samle size we need. In particular, given that $N \propto \delta^{-2}$, to detect an effect of half the size we need a sample four times the size.

- SE($\beta$) also includes measurement error, so this is also a determinant of power.


## Measuring power

- Cohen (1977) proposes estimated effect size / standard deviation of outcome. This is useful to compare effects across studies and domains.

- Bloom (1985) proposes MDE, useful for within study/domain comparisons. More directly interpretable.

## What determines power

- Significance level

- Effect size

- Standard error

    - Sample size

    - Variant allocation proportion

    - Metric variance


## How to increase power

- Power can be increased trivially by lowering the significance level, which we often don't want to do, or by increasing sample size, which we're often trying to avoid.

- Increase effect size

  - Ensure that only users who are exposed to the change are in the data to avoid dilution of the effect

- Optimally allocate variance proportions

  - Usually equal for highest power

  - Show why with many treatment variants, higher share in control is better

- Reduce metric variance
  
  - Choose metric with low variance (e.g. indicator)
  
  - Use variance reduction technique
  
  - Only include triggered users


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

## Q&A

Questions:

1. Longer experiment duration generally increases power. Can you think of a scenario where this is not the case?

2. An online shopping site ranks products according to their average rating. Why might this be suboptimal? What could the site do instead?

Answers:

1. When using a cumulative metric such as number of likes, the variance of which will increase the longer the experiment runs, which will increase the standard error of our treatment effect estimate and lower our power. Remember that $SE(\hat{\tau}) = \sqrt{\frac{1}{P(1-P)}\frac{\sigma^2}{N}}$. So, whether this happens depends on what happens to $\frac{\sigma^2}{N}$, as experiment duration increases. A decrease in power is plausible -- likely, even! -- because $N$ will increase in a concave fashion over the course of the experiment duration (some users keep coming back), while $\sigma^2$ is likely to grow faster than linearly, which causes the ratio to increase and power to decrease. 

2. The approach is suboptimal because products with few ratings will have much more variance than products with many ratings, and their average rating is thus less reliable. The problem is akin to small US states having the highest *and* lowest rates of kidney cancer, or small schools having highest *and* lowest average pupil performance. Fundamentally, it's a problem of low power -- the sample size is too low to reliably detect a true effect. The solution is to use a shrinkage method: use a weighted average of the product average rating and some global product rating, with the weight of the product average rating being proportional to the number of ratings. This way, products with few ratings will be average, while products with many ratings will reflect their own rating.

[^type3error]: This kind of error is somtimes called a [Type III
    error](https://en.wikipedia.org/wiki/Type_III_error)

[^mutuallyexcl]: We can simply dd up the probability of the test statistic falling into the
upper and lower tail because the two events are independent.


