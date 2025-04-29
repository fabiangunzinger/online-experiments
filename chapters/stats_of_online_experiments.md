
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
&= \frac{1}{n}\sum_{i=1}^n \tau_i \\[5pt]
&= \frac{1}{n}\sum_{i=1}^n \left(Y_i(1) - Y_i(0)\right) \\[5pt]
&= \frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0) \\[5pt]
&= \overline{Y}(1) - \overline{Y}(0).
\end{align}
$$

Running an experiment with our $n$ units means that we randomly assign some units to treatment and some to control. We use the binary treatment indicator $W_i \in \{0, 1\}$ to indicate treatment exposure for unit $i$ and write $W_i = 1$ if they are in treatment and $W_i = 0$ if they are in control. We collect all unit-level treatment indicators in the $n \times 1$ vector $\mathbf{W}$. After treatment assignment, we have $n_t = \sum_{i=1}^n W_i$ units in treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ units in control. For each unit, we observe outcome $Y_i$.

To estimate $\tau$, we use the observed difference in means between the treatment and control units:

$$
\begin{align}
\hat{\tau}^{\text{dm}}
= \overline{Y}_t - \overline{Y}_c,
\end{align}
$$
where

$$
\begin{align}
\overline{Y}_t &= \frac{1}{n_t}\sum_{i:W_i=1}Y_i \\[5pt]
&= \frac{1}{n_t}\sum_{i=1}^nW_iY_i
\end{align}
$$
and
$$
\begin{align}
\overline{Y}_c &= \frac{1}{n_c}\sum_{i:W_i=0}Y_i \\[5pt]
&= \frac{1}{n_c}\sum_{i=1}^n(1-W_i)Y_i
\end{align}
$$

In the rest of this section we show that $\hat{\tau}^{\text{dm}}$ is an unbiased estimator of $\tau$ and calculate its variance.

### Unbiasedness of $\hat{\tau}^{\text{dm}}$

To show unbiasedness, we need to show that $\mathbb{E}[{\hat{\tau}^{\text{dm}}}] = \tau$.[^omitted_contidioning]

Hence, what we have to show is:

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

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^n W_iY_i\right] - \mathbb{E}\left[\frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\right]
&\text{}
\\[5pt]

&\overset{!}{=}
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0) \\[5pt]

&=
\overline{Y}(1) - \overline{Y}(0)
&\text{}
\\[5pt]

&=
\tau
\end{align}
$$

Which shows that for treatment outcomes we have to show that
$$
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^n W_iY_i\right]
\overset{!}{=}
\frac{1}{n}\sum_{i=1}^nY_i(1)
$$
and similar for control outcomes.

There are two pieces we need for this:

1) Relate observed outcomes to potential outcomes – to do this, we need an assumption, SUTVA
2) Get rid of the expectation and the assignment indicator $W_i$, and replace $\frac{1}{n_t}$ with $\frac{1}{n}$ – this we get from randomisation.

Let's tackle them in turn.

#### SUTVA links observed outcomes with potential outcomes

We want to learn something about unit-level differences in potential outcomes

$$
\tau_i = Y_i(1) - Y_i(0),
$$
where a potential outcome is a function unit-indexed function of the treatment level, with the treatment levels in our case simply being "treatment" and "control", captured by the assignment indicator $W_i \in {0, 1}$. 

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
The only way to make progress is to assume what we need: that potential outcomes for unit $i$ are a function only of the treatment level unit $i$ itself receives and independent of

1. the treatment assignments of all other units
2. different forms of the treatment level (they either aren't available, don't matter, or are random)
3. different ways of treatment administrations (they either aren't available, don't matter, or are random)

This is SUTVA, the Stable Unit Treatment Value Assumption. It ensures that the potential outcomes for each unit and each treatment level are well-defined functions of the unit index and the treatment level – "treatment value" in the assumption's name refers to potential outcome.

This allows us to write:

$$
Y_i = W_iY_i(1) + (1 - W_i)Y_i(0),
$$
and thus creates the link we need between observed and potential outcomes.


Todo:
- Link to two parts of assumption: no interference (1), no hidden variations of treatment (2, 3)
- Explain name
	- Check: Rubin, D. B. (1980). “Comment on D. Basu, 'Randomization Analysis of Experimental Data: The Fisher Randomization Test'.” _Journal of the American Statistical Association_ 75:591–593.
	- rubin1986which
- Why important: Remembering that what we want is the effect of a universal policy makes clear why this is important: we want to know what happened if we rolled out our policy to everyone compared to if we didn't roll it out to anyone. To have any hope of estimating this we can't have treatment level's vary over time or depending on circumstances, but need them to be pinned down for each unit. (In the context of Tech, this would mean that the experience of a feature for a given user is pinned down by, say, the size of their phone screen and the app version they use, which, by and large, is plausible.)
- Concrete example
	- E-commerce website change to checkout flow can depend on:
		- My partner treatment assignment
		- Accidental server-side bug that creates different background colours for button
		- Web-browser I use to view site
- What SUTVA doesn't mean:
	- That everyone receives the same form of treatment, just that I can only receive a single one / or doesn't matter / or is random – all we need is well specified function for index and treatment. This is very often misunderstood.
	- Same for treatment administration.
- Link 2 and 3 to excludability
- SUTVA is a strong assumption and can be violated in a number of ways. I'll discuss these, together with solutions, in @sec-threats-to-validity.






Notice that, fundamentally, these assumptions all have the same intention:...

- Both parts of SUTVA ensure that the potential outcomes, $Y_i(W_i)$, are well defined for each individual (the "treatment value" in SUTVA refers to "potential outcomes"). The no interference part ensures that these outcomes do not depend on the assignment of other units, while the no hidden treatment variation ensures that the precise form of each treatment level that any given unit receives is clear, which then ensures that the potential outcome for that treatment is also well defined (in the aspirin example: if it weren't clear whether treatment meant weak or strong aspirin for unit $i$, then the value for $Y_i(1)$ may vary depending on which aspirin $i$ ends up receiving, which means that potential outcome isn't well defined).











#### Randomisation gives us unconditional potential outcomes of interest

- We start with WY_i(1). This is akin to Y_i(1) | W_i = 1. We need Y_i(1). Randomisation gives us that.
- It also gives us ATE instead of only ATT

...

#### Full proof


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




Question:
- You randomise at customer level. For analysis, you do the following: you calculate metrics at a restaurant level, then calculate variant level averages from the restaurant-level averages. Is there a problem? What is it? What assumptions are being violated? 





### Excludability

- Another key assumption, related to no hidden treatment variation, is that *assignment* to treatment affects outcomes only through the effect of the *administration* of the treatment -- being part of the treatment group does not have an effect on outcomes other than through the treatment itself.

- This could be violated if treatment units were somehow treated differently from control units (e.g. data collection was different)

- The assumption is called "excludability" because it assumes that we can exclude from the potential outcome definition separate indicators for treatment assignment and administration. Instead, throughout, we use the indicator $w_i$, which captures whether unit $i$ was allocated to treatment, and assume that this perfectly corresponds to having been administered the treatment.







We can calculate average outcomes for treatment and control units as:
$$
\begin{align}
\overline{Y}_t = \frac{1}{n_t}\sum_{i=1}^n W_iY_i
\qquad
\overline{Y}_c = \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i,
\end{align}
$$
and can calculate the difference-in-means estimator as the difference between treatment and control average as:

$$
\hat{\tau}^{\text{dm}} = \overline{Y}_t - \overline{Y}_c.
$$
What we want to know now is whether $\hat{\tau}^{\text{dm}}$ is a good estimator of $\tau$ in the sense that it is unbiased – whether, on average, we have $\hat{\tau}^{\text{dm}} = \tau$ – and what its variance is.



  
  















## References


## Footnotes

[^unit_level_treatment_effects]: In principle, the unit-level level causal effect can be any comparison between the potential outcomes, such as the difference $Y_i(1) - Y_i(0)$ or the ratio $Y_i(1)/Y_i(0)$. In online experiments, we usually focus on the difference.

[^scientific_solution]: @holland1986statistics discusses two solutions to the Fundamental Problem: one is the *statistical solution*, which relies on estimating average treatment effects across a large population of units while the other is the *scientific solution*, which uses homogeneity or invariance assumptions. The scientific solution works as follows: say we have one measurement of a units outcome under treatment from today and another measurement of their outcome under control from yesterday. If we are prepared to assume that control measurements are homogenous and invariant to time – that yesterday's control measurement equals the control measurement we would have taken today – then we can calculate the individual level causal effect by comparing the two measurements taken at different points in time. Our assumption is untestable, of course, but in lab experiments it is sometimes possible to make a strong case that it is plausible. It is also the approach we informally use in daily life, whenever we conclude that taking Paracetamol helps against headaches or that going to sleep early makes us feel better the next morning.

[^shortcut]: I have taken a slight shortcut here by treating experiments as being synonymous with the statistical solution (see previous footnote) even though observational studies can serve the same purpose (albeit with additional assumptions). I do this because my focus here is on experiments. See, for instance, @imbens2015causal for an extensive discussion of experimental and observational approaches. 

[^omitted_contidioning]: We treat each unit's potential outcomes as given and $n_t$ and $n_c$ as fixed, so the expectation is conditional on these factors: $\mathbb{E}[{\hat{\tau}^{\text{dm}}} | \mathbf{Y(1)}, \mathbf{Y(0)}, n_t, n_c] = \tau$. I omit this explicit conditioning to simplify the notation.
