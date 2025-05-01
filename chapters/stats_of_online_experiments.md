
Notation an conventions:
- Running example: "your" behaviour/experience on an e-commerce app

Questions I'm unsure about
- Shound I start with potential outcomes, individual effect, then average effect, then dm estimator, then proofs *or* start with dm (what we get in experiment) and then show what it is and why we do it? For now, do first way, which is traditional and what I need for myself. Rewrite as needed.




# The stats of online experiments

#### The fundamental problem of causal inference

We study a population of $n$ units, indexed by $i = 1, \dots, n$, to learn about the effect of a binary treatment. The population of units might be all visitors to an e-commerce app and the treatment a new UX feature. The treatment is "binary" because we only consider two treatment conditions: a unit either experiences the active treatment and is exposed to the new feature or experiences the control treatment and is exposed to the status-quo. We often refer to the two treatment conditions simply as "treatment" and "control".

Each unit has two potential outcomes: $Y_i(1)$ is the outcome for unit $i$ if they are in treatment whereas $Y_i(0)$ is the outcome if they are in control. These outcomes are  "potential outcomes" because before the start of the experiment, each unit could potentially be exposed to either treatment condition. We collect all unit-level potential outcomes in the $n \times 1$ vectors $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$. 

The causal effect of the treatment for unit $i$ is the difference between the two potential outcomes:[^unit_level_treatment_effects]
$$
\tau_i = Y_i(1) - Y_i(0).
$$
Because a unit can only ever be in either treatment or control, we can only ever observe one of the two potential outcomes, which means that directly *observing unit-level treatment effects* is impossible. This is the fundamental problem of causal inference [@holland1986statistics]. 


#### Experiments

An experiment is one solution to the fundamental problem:[^scientific_solution] randomly assigning units from a population to either treatment or control allows us to *estimate average (unit-level) treatment effects*. In the words of @holland1986statistics [p. 947]:[^shortcut] 

> "The important point is that [an experiment] replaces the impossible-to-observe causal effect of [a treatment] on a specific unit with the possible-to-estimate *average* causal effect of [the treatment] over a population of units."

Hence, instead of trying to observe unit-level causal effects, the quantity of interest – the *estimand* – in an experiment is an average across a population of units. In particular, we are usually interested in the effect of a universal policy, a comparison between a state of the world where everyone is exposed to the treatment and one where nobody is. While we can capture the difference between these two states of the world in many different ways, we typically focus on the difference in the averages of all these unit-level causal effects over the entire population:

$$
\begin{align}
\tau
:= \frac{1}{n}\sum_{i=1}^n \left(Y_i(1) - Y_i(0)\right)
= \frac{1}{n}\sum_{i=1}^n Y_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0).
\end{align}
$$

Running an experiment with our $n$ units means that we randomly assign some units to treatment and some to control. We use the binary treatment indicator $W_i \in \{0, 1\}$ to indicate treatment exposure for unit $i$ and write $W_i = 1$ if they are in treatment and $W_i = 0$ if they are in control. We collect all unit-level treatment indicators in the $n \times 1$ vector $\mathbf{W}$. After treatment assignment, we have $n_t = \sum_{i=1}^n W_i$ units in treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ units in control. For each unit, we observe outcome $Y_i$.

To estimate $\tau$, we use the observed difference in means between the treatment and control units:[^1]

$$
\begin{align}
\hat{\tau}^{\text{dm}}
:=
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\end{align}
$$
In the rest of this section we show that $\hat{\tau}^{\text{dm}}$ is an unbiased estimator of $\tau$ and calculate its variance.

### Context

There are different perspectives/approaches we could take (indicates choice here):
- Superpopulation vs finite sample (latter: easier, corresponds to original theory, corresponds to reality for most firms)
- CRE or BRE (latter: follows from real-time hash-based randomisation)
- Given that we have BRE, condition on n or treat n as binomial variable (former: easier and corresponds to situation at time of analysis)

Implications:

- Potential outcomes $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$ are fixed (because units are non-random but determined by sample I have). I refer to them collectively as $\mathbf{Y(w)} = (\mathbf{Y(1)}, \mathbf{Y(0)})$.

- The number of units in treatment and control, $n_t$ and $n_c$ are given. I refer to them collectively as $\mathbf{n} = (n_t, n_c)$.


todo:
- See chat discussion
	- See here: https://alexdeng.github.io/causal/randomintro.html (exactly what I want)

- Generally, literature uses a potential outcome framework 
	- @larsen2023statistical
- Some vendors/literature assume fixed sample sizes
	- @nordin2024precision

- Others use an iid framework for motivation
	- zhou2023all
### Unbiasedness of $\hat{\tau}^{\text{dm}}$

An estimator is unbiased if its expected value equals the estimand. To show that the difference in means estimator is unbiased we thus have to show that:
$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
\\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
- \mathbb{E}\left[
\frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
\\[5pt]

&
\enspace
\overset{!}{=}
\enspace
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]

&=
\tau
\end{align}
$$

There are two pieces we need for this:

1) Link observed to potential outcomes so that $Y_i = Y_i(W_i)$. This requires the Stable Unit Treatment Assumption (SUTVA).

2) Link treatment group averages to population averages so that $\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=w}Y_i(w)\right] = \frac{1}{n}\sum_{i=1}^{n}Y_i(w)$. This requires randomisation.

Let's tackle them in turn.

#### SUTVA links observed outcomes to potential outcomes

We want to learn something about unit-level differences in potential outcomes
$$
\tau_i = Y_i(1) - Y_i(0),
$$
where a potential outcome is a unit-indexed function of the treatment level, with the treatment levels in our case simply being "treatment" and "control", captured by the assignment indicator $W_i \in {0, 1}$. 

Now say we run an experiment and unit $i$ is allocated to treatment. The $i$-th element of the assignment vector $\mathbf{W}$ of that experiment is thus $W_i = 1$, and we refer to this assignment vector as $\mathbf{W}^{(i=1)}$. 

We know we can't observe $\tau_i$, but given that $i$ is in treatment we need to observe the potential outcome under treatment, $Y_i(1)$, to help us estimate $\hat{\tau}$. So we need 

$$
Y_i = Y_i(1).
$$

What we directly observe, however, is
$$
Y_i = Y_i\left(\mathbf{W}^{(i=1)}\right).
$$
In words: the outcome we observe for unit $i$ in our experiment is not the potential outcome for $i$ under treatment, but the potential outcome for $i$ under the specific assignment vector of our experiment, $\mathbf{W}^{(i=1)}$. What does this mean concretely? It means that the observed outcome is a function not only of $i$'s treatment assignment but of:

1. The assignment of all units in the experiment
2. The precise form of the assigned treatment level received by unit $i$
3. The way in which the treatment is administered to unit $i$

We need:
$$
Y_i = Y_i\left(\mathbf{W}^{(i=1)}\right) \overset{!}{=} Y_i(1).
$$
The only way to make progress is to assume what we need: that potential outcomes for unit $i$ are a function only of the treatment level unit $i$ itself receives and independent of (i) treatment assignment of other units and (ii) the form and administration of the treatment. (i) above is referred to as "no interference" in the literature, (ii) as "no hidden variations of treatment".

That assumption is SUTVA, the Stable Unit Treatment Value Assumption. It ensures that the potential outcomes for each unit and each treatment level are well-defined functions of the unit index and the treatment level – "treatment value" in the assumption's name refers to potential outcome. 

Note that (ii) does not mean that a treatment level has to take the same form for all units, even though it is often misinterpreted to mean that. What we need is that $Y_i(W_i)$ is a clearly defined function for all $i$ and all possible treatment levels $W_i \in \{0, 1\}$. For this to be the case, it must be the case that there are no different forms of possible treatments, the potential outcome for each for is the same so that the differences are irrelevant, or that the experienced form is random so that the expected outcome across all units remains stable.

[todo]
In the context of our e-commerce app experiment, SUTVA means that potential outcomes for all customers are independent of treatment assignment of their partners (no interference) and that there their precise experience is pinned down by their mobile device and remains stable over time.
Link to three elements above:
(i) My partner treatment assignment
(ii) Accidental server-side bug that creates different background colours for button
(iii) Web-browser I use to view site

Why is this important? We want to know what happened if we rolled out our policy to everyone compared to if we didn't roll it out to anyone. To have any hope of estimating this we can't have treatment level's vary over time or depending on circumstances, but need them to be pinned down for each unit. (In the context of Tech, this would mean that the experience of a feature for a given user is pinned down by, say, the size of their phone screen and the app version they use, which, by and large, is plausible.)

SUTVA is a strong assumption and can be violated in a number of ways. I'll discuss these, together with solutions, in @sec-threats-to-validity.


If SUTVA holds we have:

$$
Y_i = Y_i(W_i) = \begin{cases} 
   Y_i(1) & \text{if } W_i = 1 \\
   Y_i(0)       & \text{if } W_i = 0,
  \end{cases}
$$
or, more compactly:
$$
Y_i = W_iY_i(1) + (1 - W_i)Y_i(0).
$$

This is the link between observed and potential outcomes we need, and makes clear that we observe $Y_i(1)$ for units in treatment and $Y_i(0)$ for units in control.

In our unbiasedness proof, we now have

$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
\\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
- \mathbb{E}\left[
\frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
\\[5pt]

&\text{SUTVA}
\\[5pt]

&=
\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=1}Y_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
- \mathbb{E}\left[\frac{1}{n_c}\sum_{W_i=0}Y_i(0)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
\\[5pt]

&
\enspace
\overset{!}{=}
\enspace
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]

&=
\tau
\end{align}
$$


What remains is to show that $\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=w}Y_i(w)\right] = \frac{1}{n}\sum_{i=1}^{n}Y_i(w)$. This requires randomisation.

todo:
- Why called SUTVA, see @rubin1980randomization
- Link 2 and 3 to excludability

#### Randomisation links treatment groups to the population

In math therms, it gives us unconditional potential outcomes.



todo:
- We start with WY_i(1). This is akin to Y_i(1) | W_i = 1. We need Y_i(1). Randomisation gives us that.
- It also gives us ATE instead of only ATT
- Link last line of below to Ding lemma 3


Notes:


$$
\begin{align}
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i(1) 
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^{n}W_iY_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right] 
\quad&\text{Definition of }W_i
\\[5pt]

=
\frac{1}{n_t}\sum_{i=1}^{n}
\mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]
Y_i(1)
\quad&\text{}
\\[5pt]

\end{align}
$$


What is $\mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]$?


#### Full proof for reference


$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\right]

&=
\mathbb{E}\left[
\overline{Y}_t - \overline{Y}_c
\right]
&\text{}
\\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^n W_iY_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\right]
&\text{}
\\[5pt]

&\text{SUTVA}
\\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^n W_iY_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i(0)
\right]
&\text{}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n \mathbb{E}[W_i]Y_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n \mathbb{E}[(1-W_i)]Y_i(0)
&\text{}
\\[5pt]

&\text{Randomisation}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n \left(\frac{n_t}{n}\right)Y_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n \left(\frac{n_c}{n}\right)Y_i(0)
\\[5pt]

&=
\frac{1}{n}\sum_{i=1}^n Y_i(1) 
- \frac{1}{n}\sum_{i=1}^n Y_i(0)
&\text{}
\\[5pt]

&=
\overline{Y}(1) - \overline{Y}(0)
&\text{}
\\[5pt]

&=
\tau
\qquad\square
&\text{}
\\[5pt]
\end{align}
$$







## Q&A

Questions

1. Why do we need potential outcomes at all? Can't we interpret the difference from a simple comparison of averages as the causal effect?

Answer

1. 
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




# Old notes

### Assignment mechanism: randomisation

- In order for these estimates to be valid estimates of the two quantities we need, it is critically important how units receive the treatment they receive.

- This is the role of the assignment mechanism: the mechanism that determines how units are allocated into different treatment conditions.

- In particular, allocation to treatment has to be random because if treatment allocation is random, then we have:



Appendix material: how does my approach above relate to the below?

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







## References


## Footnotes

[^unit_level_treatment_effects]: In principle, the unit-level level causal effect can be any comparison between the potential outcomes, such as the difference $Y_i(1) - Y_i(0)$ or the ratio $Y_i(1)/Y_i(0)$. In online experiments, we usually focus on the difference.

[^scientific_solution]: @holland1986statistics discusses two solutions to the Fundamental Problem: one is the *statistical solution*, which relies on estimating average treatment effects across a large population of units while the other is the *scientific solution*, which uses homogeneity or invariance assumptions. The scientific solution works as follows: say we have one measurement of a units outcome under treatment from today and another measurement of their outcome under control from yesterday. If we are prepared to assume that control measurements are homogenous and invariant to time – that yesterday's control measurement equals the control measurement we would have taken today – then we can calculate the individual level causal effect by comparing the two measurements taken at different points in time. Our assumption is untestable, of course, but in lab experiments it is sometimes possible to make a strong case that it is plausible. It is also the approach we informally use in daily life, whenever we conclude that taking Paracetamol helps against headaches or that going to sleep early makes us feel better the next morning.

[^shortcut]: I have taken a slight shortcut here by treating experiments as being synonymous with the statistical solution (see previous footnote) even though observational studies can serve the same purpose (albeit with additional assumptions). I do this because my focus here is on experiments. See, for instance, @imbens2015causal for an extensive discussion of experimental and observational approaches. 

[^omitted_contidioning]: We treat each unit's potential outcomes as given and $n_t$ and $n_c$ as fixed, so the expectation is conditional on these factors: $\mathbb{E}[{\hat{\tau}^{\text{dm}}} | \mathbf{Y(1)}, \mathbf{Y(0)}, n_t, n_c] = \tau$. I omit this explicit conditioning to simplify the notation.

[^1]: To denote the sum over all units in a given treatment group $w$ I use $\sum_{W_i=w}$ as a shorthand for $\sum_{i:W_i=w}$ to keep the notation compact.



## List of assumptions (temp, to be moved)


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

## Potential outcomes framework details (tmp)

From holland

- The framework has three key features:

  1. Causal effects are associated with potential outcomes
  2. Studying causal effects required multiple units
  3. Central role of the assignment mechanism

Their role is somewhat different, however: the first one is axiomatic: it's the starting point for how we think about causal effects and intimately linked to the notion that causal effects are always relative to a different state (see holland1986statistics notes, as well as Rubin interview). The second is a corollary from the first if we are unwilling to take the scientific solution (in Holland's words) to the Fundamental Problem: it's the insight that leads to the statistical solution. The third is a corollary of the second: to make the statistical solution work, the assignment mechanism is central.