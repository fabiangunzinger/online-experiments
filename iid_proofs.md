Collect here material on the classic two-sample case. Discuss why using this in online experiments is incorrect. What error are we making?

- [ ] Discuss why we need causal framework at all (what if we just do naive derivations treating t and c as independent samples)? What error are we making? (start with rubin1974estimating)
- [ ] With replacement / IID approach
	- [ ] Same as BRE?
	- [ ] Relation to above approaches
	- [ ] Why do we need potential outcomes?
- [ ] Without replacement
	- [ ] Same as CRE?
- [ ] Use [[stats_of_online_experiments]] to add context

Material
- ding2023first footnote 2 in chapter 4



In the classic two-sample problem, the outcomes in treatment are assumed to be **IID draws** from a distribution with mean $\mu_t$ and variance $\sigma_t^2$ and similar for control, and the variance of the  difference in means estimator is given by:

$$
\mathbb{V}(\hat{\tau}) = \frac{\sigma_t^2}{n_t} + \frac{\sigma_c^2}{n_c}.
$$
That is, there is no third term for the variance of the individual-level potential outcomes.

Here, the variance is taken over the randomness of the outcomes because uncertainty is sampling based, whereas in the potential outcomes framework, where potential outcomes are fixed, the variance is taken over the randomisation distribution. 

Difference to BRE: there is no correlation between values in two groups, whereas in potential outcomes framework there is, unless Y1 and Y0 are cuncorrelated for each unit.

