
# The stats of online experiments


#### The setup

We study a population of $n$ units, indexed by $i = 1, \dots, n$, to learn about the effect of a binary treatment. The population of units might be all visitors to our website and the treatment a new UX feature. The treatment is "binary" because we only consider two treatment conditions: a unit either experiences the active treatment and is exposed to the new feature or the control treatment and is exposed to the status-quo experience. We often refer to the two treatment conditions simply as "treatment" and "control".

We use the binary treatment indicator $W_i \in \{0, 1\}$ to indicate treatment exposure for unit $i$ and write $W_i = 1$ if they are in treatment and $W_i = 0$ if they are in control. We collect all unit-level treatment indicators in the $n \times 1$ vector $\mathbf{W} = (W_1, W_2, \dots, W_n)'$.

Each unit $i$ has two potential outcomes: $Y_i(1)$ is the outcome for unit $i$ if they are in treatment whereas $Y_i(0)$ is the outcome if they are in control. These outcomes are called "potential outcomes" because before the beginning of the experiment, each unit could potentially be exposed to either treatment condition. We collect all unit-level potential outcomes in the $n \times 1$ vectors $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$. 

The causal effect of the treatment for individual $i$ is a comparison of the two potential outcomes, such as the difference $Y_i(1) - Y_i(0)$ or the ratio $Y_i(1)/Y_i(0)$. In online experiments, we usually focus on the difference and so we define the unit-level treatment effect as

$$
\tau_i = Y_i(1) - Y_i(0).
$$
Because a unit can only ever be either in treatment in control but never both experiences at the same time, we can only ever observe one of the two potential outcomes, which means we can never directly observe $\tau_i$. This is the fundamental problem of causal inference $holland1986statistics. 

An experiment is one approach to deal with the fundamental problem.

#### The experiment setup

- An experiment helps us deal with the fundamental problem by focusing on entire population of users and estimating average tau_i

- At the population level, we then care about E[Y(1) - Y(0)]

- Two possible cases: super population or finite sample perspective. I'm gonna focus on finite sample. Why
	- That's often what we work with (exceptions are super large companies)
	- It removes a layer of complication that is worth very little because results, ultimately, are basically the same (see imbens rubin book). Have a footnote explaining the main difference.

- Given that we treat sample as population, we can rewrite E as sum
- Use the below...

- - -


- 

- Once treatment is assigned, we observe

$$
Y_i^{obs} = Y_i(W_i) = \begin{cases} 
   Y_i(1) & \text{if } W_i = 1 \\
   Y_i(0)       & \text{if } W_i = 0
  \end{cases}
$$

- We can only ever observe one potential outcomes, which means that drawing inferences about the causal effect is impossible without additional assumptions. @holland1986statistics called this the fundamental problem of causal inference.

- An experiment is one way to solve the fundamental problem.




## Experiment setup

Could add super population here, with population statistics $\mu_1, \sigma_1^2$, etc.

...

Finite sample of $n$ units, each with potential outcomes $Y_i(1)$ and $Y_i(0)$.

Individual-level causal effects:
$$
\tau_i = Y_i(1) - Y_i(0)
$$
Finite sample average treatment effect:

$$
\tau_{\text{fs}}
= \frac{1}{n}\sum_{i=1}^n \tau_i
= \frac{1}{n}\sum_{i=1}^n \left(Y_i(1) - Y_i(0)\right)
= \overline{Y}(1) - \overline{Y}(0)
$$

Finite sample means and variances:

$$
\begin{align}
\overline{Y}(1) = \frac{1}{n}\sum_{i=1}^n Y_i(1),
\qquad
S_1^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)^2
\\[5pt]
\overline{Y}(0) = \frac{1}{n}\sum_{i=1}^n Y_i(0),
\qquad
S_0^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(0) - \overline{Y}(0)\right)^2
\\[5pt]
\end{align}
$$
Variance of individual-level causal effects: 

$$
\begin{align}
S_{\tau_i}^2
&= \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - Y_i(0) 
- \left(\overline{Y}(1) - \overline{Y}(0)\right)\right)^2
\\[5pt]
&= \frac{1}{n-1}\sum_{i=1}^{n}\left(\tau_i - \tau_{\text{fs}}\right)^2 \\[5pt]
\end{align}
$$
Covariance of potential outcomes:

$$
\begin{align}
S_{0, 1} &= \frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)
\left(Y_i(0) - \overline{Y}(0)\right)
\end{align}
$$

We have data from a randomised experiment with assignment vector $\mathbf{W} = \{W_1, ... W_n\}$ where $n_t = \sum_{i=1}^n W_i$ units are allocated to treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ units are allocated to control. 

Observed outcomes are:
$$ 
Y_i = W_iY_i(1) + (1 - W_i)Y_i(0)
$$

We can estimate the finite sample statistics using the observed treatment group means:

$$
\begin{align}
\overline{Y}_t = \frac{1}{n_t}\sum_{i=1}^n W_iY_i
\qquad
\overline{Y}_c = \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\end{align}
$$
and observed treatment group variances:

$$
\begin{align}
s_t^2 = \frac{1}{n_t-1}\sum_{i=1}^{n}W_i\left(Y_i - \overline{Y}_t\right)^2
\qquad
s_c^2 = \frac{1}{n_c-1}\sum_{i=1}^{n}(1-W_i)\left(Y_i - \overline{Y}_c\right)^2
\end{align}
$$

## Estimand and estimator of interest

We are interested in the finite sample average treatment effect $\tau_{\text{fs}}$.

Given the observed data, a natural estimator is:

$$
\begin{align}
\hat{\tau}^{\text{dm}}
= \overline{Y}_t - \overline{Y}_c
\end{align}
$$
We analyse the properties of this estimators for different types of experiments (assignment mechanisms). In particular, we are interested in showing that the estimator is unbiased and to estimate its variance.









## Experiment setup

- The statistical solution to the Fundamental Problem is to rely on information from the entire experiment population and focus on the average treatment effect $\tau = \mathbb{E}[Y(1) - Y(0)] = \mathbb{E}[Y(1)] - \mathbb{E}[Y(0)]$.[^scientific_solution].

- The last two terms can be estimated from an experiment where some units are exposed to treatment and others to control. The key insight here is that the statistical approach makes it possible to replace the *impossible to observe* individual-level causal effect with the *possible to estimate* average causal effect over a population of units.

- We have a population of $i = 1, \dots, n$ units.

- The assignment indicator is now an $n$-vector $\mathbf{W}$ of assignments, with typical element $W_i$.

- In principle, the potential outcomes can depend on the treatments for all units, so that for each unit, we have $2^n$ potential outcomes $Y_i(\mathbf{W})$.

### SUTVA

- Without further assumptions, an experiment would tell us the difference in the average outcomes of units in treatment and control, *given that precise assignment of the experiment*, which not help us estimate the effect of our universal policy.

- To make progress, we need the Stable Unit Treatment Value Assumption. SUTVA has two components: no interference, and no hidden variations of treatments.

  - The no interference assumption states that a unit's potential outcomes are independent of the treatment assignment of all other units.

  - The no hidden treatment variation states that a unit receiving a specific treatment level cannot receive different forms of that treatment level. This does *not* mean that the form of the treatment level has to be the same for each unit, but only that a given treatment level is well specified for a given unit. To use Imbens and Rubin's aspirin example: suppose we test the effect of aspirin on reducing headaches but have old and new aspirins which vary in strength, so that we effectively have three possible treatment statuses: no aspirin (control), weak aspirin, and strong aspirin. SUTVA does *not* require that all treatment units either get the weak or the strong aspirin, but requires that each unit can only receive one or the other in case they are treated, so that there is no ambiguity what form of the treatment a given unit will receive in case it is treated. (It would be permissible to have the treatment be randomly weak or strong, but this is not relevant in my world.) Remembering that what we want is the effect of a universal policy makes clear why this is important: we want to know what happened if we rolled out our policy to everyone compared to if we didn't roll it out to anyone. To have any hope of estimating this we can't have treatment level's vary over time or depending on circumstances, but need them to be pinned down for each unit. (In the context of Tech, this would mean that the experience of a feature for a given user is pinned down by, say, the size of their phone screen and the app version they use, which, by and large, is plausible.)

- Both parts of SUTVA ensure that the potential outcomes, $Y_i(W_i)$, are well defined for each individual (the "treatment value" in SUTVA refers to "potential outcomes"). The no interference part ensures that these outcomes do not depend on the assignment of other units, while the no hidden treatment variation ensures that the precise form of each treatment level that any given unit receives is clear, which then ensures that the potential outcome for that treatment is also well defined (in the aspirin example: if it weren't clear whether treatment meant weak or strong aspirin for unit $i$, then the value for $Y_i(1)$ may vary depending on which aspirin $i$ ends up receiving, which means that potential outcome isn't well defined).

- SUTVA is a strong assumption and can be violated in a number of ways. I'll discuss these, together with solutions, in @sec-threats-to-validity.

- If SUTVA holds, however, then instead of $Y_i = Y_i(\mathbf{W})$ we have $Y_i = Y_i(W_i)$, which allows us to write:

$$ Y_i^{obs} = W_iY_i(1) + (1 - W_i)Y_i(0)
$$ {#eq-yi}

- This is progress because now each unit's potential outcome is a function only of the unit's treatment assignment. As a result, the outcome we observe once the unit has been assigned, too, is a function of that unit's treatment assignment only.

- This is the precondition that allows us to compare outcomes of treated and untreated units to estimate the two quantities needed for the statistical solution to the Fundamental Problem: $\mathbb{E}[Y(1)] - \mathbb{E}[Y(0)]$.

### Assignment mechanism: randomisation

- In order for these estimates to be valid estimates of the two quantities we need, it is critically important how units receive the treatment they receive.

- This is the role of the assignment mechanism: the mechanism that determines how units are allocated into different treatment conditions.

- In particular, allocation to treatment has to be random because if treatment allocation is random, then we have:

Source: mostly harmless econometrics

(Population) average treatment effect:
$$
ATE = \mathbb{E}[Y_i(1) - Y_i(0)]
$$
Average treatment effect on the treated:
$$
ATT = \mathbb{E}[Y_i(1) - Y_i(0) \,|\, W_i=1]
$$

The observed difference is the sum of ATET and selection bias:

$$
\begin{align}

\underbrace{
\mathbb{E}[Y_i \,|\, W_i=1] - \mathbb{E}[Y_i \,|\, W_i=0]
}_{\text{Observed difference}}

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]
&\quad\quad\, 
+\mathbb{E}[Y_i(0) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=1]
\\[5pt]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=1]
\\[5pt]
&\quad\quad\, 
+
\mathbb{E}[Y_i(0) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]

\quad&=\quad
\underbrace{
\mathbb{E}[Y_i(1) - Y_i(0) \,|\, W_i=1]
}_{\text{ATT}}
\\[5pt]
&\quad\quad\, 
+
\underbrace{
\mathbb{E}[Y_i(0) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
}_{\text{Selection bias}}
\\[5pt]

\end{align}
$$
Randomisation ensures that $\mathbb{E}[Y_i(0) \,|\, W_i=0] = \mathbb{E}[Y_i(0) \,|\, W_i=1]$ and thus solves the selection problem and also allows us to estimate the ATE (show this rigorously if needed):
$$
\begin{align}
\mathbb{E}[Y_i \,|\, W_i=1] - \mathbb{E}[Y_i \,|\, W_i=0]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=1]
\\[5pt]

\quad&=\quad
\underbrace{
\mathbb{E}[Y_i(1) - Y_i(0) \,|\, W_i=1]
}_{\text{ATT}}
\\[5pt]

\quad&=\quad
\underbrace{
\mathbb{E}[Y_i(1) - Y_i(0)]
}_{\text{ATE}}
\\[5pt]
\end{align}
$$



- In online experiments, as least, this is not an assumption if we properly test the randomisation proceedure (see discussion of SRM in @sec-threats-to-validity)

- In online experiments, the assignment mechanism is usually a BRE. The  assignment mechanism of a BRE is individualistic, probabilistic, and unconfounded. In the simplest case without stratification, it is also independent of covariates. In all cases, the assignment mechanism is fully under our control. For probability of treatment assignment $q$, we thus have:

$$
P(\mathbf{W} | \mathbf{X}, \mathbf{Y}(1), \mathbf{Y}(0)) = P(\mathbf{W}) = q^{n_t} (1-q)^{n_c}
$$

****I'm here****

- Reading Wager, it seems there are two relevant factors for inference: he just conditions on n to get a CRE, and then there is the question of whether to take a finite sample or super-population perspective. 

- The additional assumption (discussed in population asymptotics) of random sampling from super population holds for online experiments.

- He does use Bernoulli sampling, which is what I need for online experiments. I just don'f fully understand how his perspective fits into the Imbens Rubin book / Athey Imbens one. 


### Other related assumptions (not needed in online experiments, but discuss briefly)
- Ignorability
- Excludability
	- Another key assumption, related to no hidden treatment variation, is that *assignment* to treatment affects outcomes only through the effect of the *administration* of the treatment -- being part of the treatment group does not have an effect on outcomes other than through the treatment itself.
	
	- This could be violated if treatment units were somehow treated differently from control units (e.g. data collection was different)
	
	- The assumption is called "excludability" because it assumes that we can exclude from the potential outcome definition separate indicators for treatment assignment and administration. Instead, throughout, we use the indicator $w_i$, which captures whether unit $i$ was allocated to treatment, and assume that this perfectly corresponds to having been administered the treatment.

Check:
- See Wagner notes for 4 assumptions: https://web.stanford.edu/~swager/stats361.pdf 
- duflo2006randomizationd
- Mostly harmless metrics
- Field experiments book
- Kohavi papers/book
- Imbens and Rubins

Question:
- You randomise at customer level. For analysis, you do the following: you calculate metrics at a restaurant level, then calculate variant level averages from the restaurant-level averages. Is there a problem? What is it? What assumptions are being violated? 


## Estimand

- We are generally interested in the effect of a universal policy -- a comparison between a state of the world where everyone is exposed to the treatment and one where nobody is. Also, while we can capture the difference between these two states of the world in many different ways, we typically focus on the difference in average outcomes.

- Hence, the estimand of interest (the theoretical quantity we try to estimate) is:

$$
\tau = \bar{Y}(1) - \bar{Y}(0),
$$

  where ...



## Commonly used estimator for sampling variance

A commonly used estimator (recommended in practice by IR!) is:

$$
\hat{V}^{neyman} = \frac{s_t^2}{N_t} + \frac{s_c^2}{N_c},
$$

where $s_t^2$ and $s_c^2$ are unbiased estimators of $S_t^2$ and $S_c^2$. This estimator is popular for a few reasons:

1. If treatment effects are constant across units, then this is an unbiased estimator of the true sampling variance of $\bar{Y}_t^{obs} - \bar{Y}_c^{obs}$.

2. If treatment effects are not constant, then this is a conservative estimator of the sampling variance (since $S_{ct}^2$ is non-negative).

3. It is always unbiased for $\hat{\tau}^{dif}$ as an estimator of the infinite super-population average treatment effect (see below).

There are other options (see Section 6.5 in the IR book).


****


## References


## Footnotes

[^scientific_solution]: @holland1986statistics discusses two solutions to the Fundamental Problem: one is the statistical solution described in the main text, the other is the scientific solution, which uses homogeneity or invariance assumptions. Say we measure a unit's outcome under treatment now, and have measured their outcome under control beforehand. If we're prepared to assume that control measurements are homogenous and invariant to time -- that the earlier control measurement equals the control measurement we would have taken now had we not exposed the unit to treatment -- then we can calculate the individual level causal effect by comparing the two measurements taken at different points in time. Our assumption is untestable, of course, but in lab experiments it is sometimes possible to make a strong case that it is plausible. It is also the approach we informally use in daily life, whenever we conclude that taking Paracetamol helps against headaches or that going to sleep early makes us feel better the next morning.



