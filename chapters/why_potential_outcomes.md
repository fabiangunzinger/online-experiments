
The context
- We – that is, you, me, and a few colleagues you'll meet along the way – work at an e-commerce company.
- We have learned (from prior data, user experience testing, customer feedback, etc.) that our users find it difficult to evaluate whether a product is right for them.
- Our team thinks that adding feature A to the PDP might alleviate the problem.
- So, we want to answer the question "does feature A help our customers better evaluate whether a product is right for them?". 
- To answer that question, we need to make it more precise:
	- How do we measure "ease of product evaluation"? Say we know that metric $Y$ is a good proxy for it.
- ... (formulate hypothesis...)




Why potential outcomes?
- Clarifies what precisely we are trying to estimate: average individual treatment effect
- Makes explicit the assumptions we need to make to do so: assuming that Y0 are the same in expectation (sth else?)
 Next:
 - Answer above question, and clearly write down outline of entire story including assumptions (I have that, just revisit and make sure it's complete) - have bre assuming n known as basis (which is basically CRE). For now that's fine.
	 - Assumptions needed
		 - SUTVA
			 - What we need is to be able to write Y-i = WY1 + (1-W)Y0. For this we need i) independence from other's assignment, and ii) clearly defined meaning of Wi =1 and Wi = 0, becauese if they are not clearly defined then Y1/Y0 might not be stable. SUTVA handles both of these.
		- Randomisation: to make sure that EY0 for W=1 equals 
		- EY0 W=0
 - Then read cunningham2021causal (chapter 4) and titiunik2020natural for neat setup and write my section based on this
 - Write up entire thing (i.e. unbiasedness and derivation of std)
 - Then get feedback from colleagues and determine whether it's worth doing BRE without assumption of fixed n, and whether it's worth doing superpopulation perspective.
 - At the same time, keep going with further sections: power, estimation, inference, ...

- [ ] With replacement / IID approach
	- [ ] Same as BRE?
	- [ ] Relation to above approaches
	- [ ] Why do we need potential outcomes?
- [ ] Without replacement
	- [ ] Same as CRE?
- [ ] Use [[stats_of_online_experiments]] to add context

Material
- ding2023first footnote 2 in chapter 4 and 




### Notes

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



# 



## List of assumptions (temp, to be moved)


### Ignorability

States that if treatment assignment is independent of potential outcomes conditional on covariates and observed outcomes, then the assignment mechanism can be ignored for the recovery of causal effects (instead of having to be modeled). Note: assignment doesn't have to be random for ignorability to hold, just to behave as if it were random given covariates and observed outcomes.

Section 3.2 in rubin1978bayesian actually suggests that for assignment mechanism to be ignorable, recording mechanism simply needs to record everything upon which assignment depends, which, I suppose, can always be captured by X and Yobs, so that the below notation is comprehensive but may not always be necessary in the sense that in some cases, we only need recording of X or yobs if assignment doesn't rely on the other of the two. For instance: "play-the-winner" example in paper relies on yobs only, whereas "balance patients characteristics" relies on X only.

This is needed to ignore the assignment mechanism in Bayesian posterior inference (see rubin1978bayesian for details).

Formally:
$$
P(W|X, Y_i(1), Y_i(0)) = P(W|X, Y_i^{obs}),
$$
where
$$
Y_i^{obs} = W_iY_i(1) + (1 - W_i)Y_i(0).
$$

### Unconfoundedness

Aka (strong ignorability). Treatment is independent of potential outcomes given covariates. In practice, the following are used interchangeably to refer to this:
- Ignorability 
- Unconfoundedness
- Selection on observables
- No unmeasured confounding

This is stronger than ignorability: it implies ignorability, but the reverse is not true.

Formally:
$$
P(W|X, Y_i(1), Y_i(0)) = P(W|X)
$$



Note:

As Rubin points out on page 43 in rubin1978bayesian, if we rely on simple random sampling and then conduct a completely randomised experiment, we have

$$
P(W|X, Y_i(1), Y_i(0)) = P(W),
$$
which implies both ignorability and confoundedness.

### Potential outcomes framework details

From holland

- The framework has three key features:

  1. Causal effects are associated with potential outcomes
  2. Studying causal effects required multiple units
  3. Central role of the assignment mechanism

Their role is somewhat different, however: the first one is axiomatic: it's the starting point for how we think about causal effects and intimately linked to the notion that causal effects are always relative to a different state (see holland1986statistics notes, as well as Rubin interview). The second is a corollary from the first if we are unwilling to take the scientific solution (in Holland's words) to the Fundamental Problem: it's the insight that leads to the statistical solution. The third is a corollary of the second: to make the statistical solution work, the assignment mechanism is central.
