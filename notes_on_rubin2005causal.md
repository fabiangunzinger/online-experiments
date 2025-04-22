Notes on classic Rubin papers

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

- All randomized experiments are assignment mechanisms with two critically important properties:

	- **Ignorability**, which states that if treatment assignment is independent of potential outcomes conditional on covariates and observed outcomes, then the assignment mechanism can be ignored for the recovery of causal effects (instead of having to be modeled). Note: assignment doesn't have to be random for ignorability to hold, just to behave as if it were random given covariates and observed outcomes. (Section 3.2 in rubin1978bayesian actually suggests that for assignment mechanism to be ignorable, recording mechanism simply needs to record everything upon which assignment depends, which, I suppose, can always be captured by X and Yobs, so that the below notation is comprehensive but may not always be necessary in the sense that in some cases, we only need recording of X or yobs if assignment doesn't rely on the other of the two. For instance: "play-the-winner" example in paper relies on yobs only, whereas "balance patients characteristics" relies on X only) Formally:
$$
P(W|X, Y_i(1), Y_i(0)) = P(W|X, Y_i^{obs}),
$$
where
$$
Y_i^{obs} = W_iY_i(1) + (1 - W_i)Y_i(0).
$$
- 
	- The unit-level probabilities of treatment assignment – the propensities – are between zero and one.

- Historically, before formal modeling of assignment mechanism, people have used approaches consistent with above. For classical randomised experiments, they relied on **unconfoundedness**, 
$$
P(W|X, Y_i(1), Y_i(0)) = P(W|X)
$$

- See Rosenbaum and Rubin papers from the 1980s on propensity score to understand how it fits here: it's an approach to model the assignment mechanism.

- Understand what precisely the science is and why it is called that (it's X, and potential outcomes, but why call it that?)
- Then understand difference between modeling ass mech vs science (as discussed in last paragraph before section 5).

- Approaches to inference:
	- Fisher
	- Neyman
	- Model-based (Rubin's preferred approach), which is the Bayesian approach to modeling unobserved potential outcomes – to modeling nature, in words of Rubin 
- Seems like Rubin says in rubin1999teaching that choice of approach should be based on problem at hand – read more on this


little2000causal
- Very good and concise overview of the RCM

rubin1999teaching
- Text-based overview of RCM
- Discusses structure of Harvard causal inference course and its rationale
- Definition of causal effect using potential outcomes
- Approach labelled Rubin Causal Model (RCM), though acknowledges that formal notation goes back to Neyman (1923) and intuitive idea goes back much further. Rubin's contribution is extension of framework beyond randomised experiments and beyond randomisation-based inference
- Assumptions (non-interference, more encompassing SUTVA) ar eneeded – without them, causal inference is impossible.

Rubin comment in dawid2000causal
- Separation between the assignment mechanism (a model for treatment assignment given controls and potential outcomes) and a theory for nature (a model for the potential outcomes given controls). The former we can control at times (e.g. experiment) while the latter we cannot.

rubin1974estimating:
- Seminal article using Neyman's potential outcome notation for nonrandomised studies

rubin1975bayesian

rubin1976inference:

rubin1978bayesian:
- First article to precisely state the conditions under which causal effects can be estimated from data
