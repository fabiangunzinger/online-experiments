# FAQs

## Q1 

**Why do we need potential outcomes at all? Can't we interpret the difference from a simple comparison of averages as the causal effect?**

Why potential outcomes?
- Clarifies what precisely we are trying to estimate: average individual treatment effect
- Makes explicit the assumptions we need to make to do so:
	 - SUTVA: What we need is to be able to write Y-i = WY1 + (1-W)Y0. For this we need i) independence from other's assignment, and ii) clearly defined meaning of Wi =1 and Wi = 0, because if they are not clearly defined then Y1/Y0 might not be stable. SUTVA handles both of these.
	- Randomisation: to make sure that EY0 for W=1 equals EY0 W=0
Material
- ding2023first footnote 2 in chapter 4 and 

- What is definition of causal effect in suggested comparison?
- What is source of randomisation?
- Discuss textbook iid approach and why it's not a good model for our purpose.
- Show that in practice, variance is the same

In the classic two-sample problem, observations in the treatment group {y1s} and control group {y0s} are assumed to be IID draws from **two separate** distributions. Treatment observations are assumed to be **IID draws** from a distribution with mean $\mu_t$ and variance $\sigma_t^2$ and similar for control, and the variance of the  difference in means estimator is given by:

$$
\mathbb{V}(\hat{\tau}) = \frac{\sigma_t^2}{n_t} + \frac{\sigma_c^2}{n_c}.
$$
That is, there is no third term for the variance of the individual-level potential outcomes.

In contrast, Rubin points out that for proper causal inference, {y1, y0} pairs are from the same distribution but we observe only one item of the pair.

Differences:
- Sampling based vs randomisation based variation: makes sense given that in IID case we are assumed to sample from population, whereas in FS case we are assumed to have all units, but randomise which item of the PO pair is observed.
- Hence: the variance in IID is taken over the randomness of the outcomes because uncertainty is sampling based, whereas in the potential outcomes framework, where potential outcomes are fixed, the variance is taken over the randomisation distribution. 
- As a result: there is no correlation between two groups in IID case (covar = 0) and hence no third term, whereas in FS case there is – why precisely? Because there is correlation between y1s and y2s – if there isn't, then the third term vanishes. See ding derivation. However, ultimately it's because there is heterogeneity in individual-level treatment effects. Why is that? Is that the same as PO correlation at individual level?
- Weird, though, that Ding lemmas seem to be based on IID case!

## Q2

**Why do randomised trials not require the excludability assumption in order to lead to unbiased results?**

## Q3

**a) A student is asked 12 true-or-false questions and gets 3 of them wrong. What is the probability that the student guessed randomly?**

**b) The student above would actually have been asked questions until they got 3 wrong. What is the probability that they guessed randomly?**

**c)What do you conclude from a) and b)?**


a) Calculate p-value for getting no more than observed number of errors

```python
p = stats.binom.cdf(k=3, n=12, p=0.5)
print(f"p-value is: {p:.3f}")
```




## Q4

**Consider the outcome of a large sample size (e.g. 10K subjects) A/B test that yielded tiny (e.g. odds ratio of 1.0008) but statistically significant (e.g. p < 0.001) results. Would you deploy the change to production. Why or why not?**

## Q5

**A researcher runs an experiment, gets a p-value of 0.033, and concludes that his false positive rate is 3.3 percent. Is that correct? Why or why not?**

No. An individual experiment doesn't have a false positive rate. The false positive rate is determined by the proceedure used for experiments over the long run. For example, if you consistently use a p-value of 0.05, then you are guaranteed to have a false positive rate of 5 percent in the long run. This is the main insight underlying the Neyman-Pearson hypothesis testing framework.

## Q6

**Why don't we include MDE in our hypothesis statement (i.e. "a will increase b by at least MDE for target population")? Wouldn't that be a good idea in a business context where we strongly care about an MDE? Don't we just not do that because academics, who developed the methods, don't usually care about an effect being of a certain size but only about learning what the effect is?**

Including the MDE's in the hypothesis would be incorrect. Why? Because what we formulate is the alternative hypothesis. Without altering the null hypothesis, which traditionally states the effect is zero, our two hypotheses would not be complete (H0 testing effect different from 0, HA asserting that effect is at least MDE means rejecting H0 would not imply support of HA). So why not adapt H0, too? Because the hypothesis is the thing we test when we calculate our test statistic (duh!), and so if included the MDE in the hypothesis formulation, then we'd have to include it in the calculation of the test statistic (subtract it from the observed difference), which would be incorrect. Why? The question confused the MDE with the effect size under the Null hypothesis, which are conceptually different things: the MDE is the smallest effect size we want to detect -- we are interested in effects that are as large or larger than the MDES, we're not testing whether the observed difference is significantly different from it, which is what the null hypothesis value is.

## Q7

**The 95% CI for control and treatment overlap. Does this imply treatment is not significant?**
...

## Q8

**Longer experiment duration generally increases power. Can you think of a scenario where this is not the case?**

When using a cumulative metric such as number of likes, the variance of which will increase the longer the experiment runs, which will increase the standard error of our treatment effect estimate and lower our power. Remember that $SE(\hat{\tau}) = \sqrt{\frac{1}{P(1-P)}\frac{\sigma^2}{N}}$. So, whether this happens depends on what happens to $\frac{\sigma^2}{N}$, as experiment duration increases. A decrease in power is plausible -- likely, even! -- because $N$ will increase in a concave fashion over the course of the experiment duration (some users keep coming back), while $\sigma^2$ is likely to grow faster than linearly, which causes the ratio to increase and power to decrease. 


## Q9

**An online shopping site ranks products according to their average rating. Why might this be suboptimal? What could the site do instead?**

The approach is suboptimal because products with few ratings will have much more variance than products with many ratings, and their average rating is thus less reliable. The problem is akin to small US states having the highest *and* lowest rates of kidney cancer, or small schools having highest *and* lowest average pupil performance. Fundamentally, it's a problem of low power -- the sample size is too low to reliably detect a true effect. The solution is to use a shrinkage method: use a weighted average of the product average rating and some global product rating, with the weight of the product average rating being proportional to the number of ratings. This way, products with few ratings will be average, while products with many ratings will reflect their own rating.