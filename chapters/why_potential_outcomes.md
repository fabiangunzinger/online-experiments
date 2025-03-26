
### Sources

Notes on [Randomised Experiment chapter](https://alexdeng.github.io/causal/randomintro.html):

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

Why potential outcomes, then?


# Notes on classic Rubin papers

rubin2005causal:
- Summary and overview paper of history of potential outcomes framework. 
- Assumptions are always needed for causal inference and the big value of potential outcomes is that it makes them transparently. Half of potential outcomes are missing and assumptions about them matter. Having both PO in framework helps make these assumptions explicit (e.g. such as, for randomisation, the fact that (Y0 | w=1)= (Y0 | w=0).
- Key contributions of Neyman (1923)
	- Introduce potential outcomes notation
	- First use of randomised experiments
	- Shows that difference-in-means estimator is unbiased
	- Shows that commonly used variance estimator for DIM estimator is positively biased unless there is homogeneity in individual-level treatment effects
- First application of potential outcomes notation introduced by Neyman to nonrandomised studies in rubin1974estimating

- "An assignment mechanism must be posited for probabilistic causal inference and in certain circumstances, such as randomized experiments, it is the only model that needs to be posited to make inferential progress. That is, no model on the science may be needed beyond SUTVA."
- Stuff to unpack about the above:
	- What is probabilistic causal inference (what are alternatives)? – see discussion below
	- Why do randomisation and SUTVA suffice in randomised experiments? – See writeup in stats of online experiments section where, based on holland1986statistical I walk through the argument step-by-step.

- Probabilistic causal inference: (From ChatGPT (OpenAI), March 2025.) Probabilistic causal inference refers to drawing conclusions about **causal effects** (e.g., treatment → outcome) while **explicitly modeling or incorporating randomness** — either in the **assignment** of treatments (design based inference) or in the **uncertainty** about outcomes (model-based inference). 
	- In this view:
		- The **assignment mechanism** (e.g., randomization) is treated probabilistically.
		- The **causal effect** is a **fixed but unobserved quantity**, and statistical tools are used to estimate it **with uncertainty** (e.g., confidence intervals, posterior distributions).
	- Example: In a randomized experiment:
		- A treatment is assigned with probability 0.5.
		- You observe \( Y_i(1) \) or \( Y_i(0) \), but never both.
		- You estimate the **average treatment effect** (ATE) and calculate a **confidence interval** around it.
	- This is **probabilistic causal inference** — inference **under uncertainty**.

- Design-based vs model-based inference – see modes of inference subsection in stats foundations section






rubin1974estimating:
- Seminal article using Neyman's potential outcome notation for nonrandomised studies

rubin1975bayesian

rubin1976inference:

rubin1978bayesian:
- First article to precisely state the conditions under which causal effects can be estimated from data
