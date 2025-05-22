
Observational causal inference requires additional assumptions to identify causal effects. This section briefly explains what they are and how they relate to the assumptions required in experiments.

### Excludability

- Another key assumption, related to no hidden treatment variation, is that *assignment* to treatment affects outcomes only through the effect of the *administration* of the treatment -- being part of the treatment group does not have an effect on outcomes other than through the treatment itself.

- This could be violated if treatment units were somehow treated differently from control units (e.g. data collection was different)

- The assumption is called "excludability" because it assumes that we can exclude from the potential outcome definition separate indicators for treatment assignment and administration. Instead, throughout, we use the indicator $w_i$, which captures whether unit $i$ was allocated to treatment, and assume that this perfectly corresponds to having been administered the treatment.


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



