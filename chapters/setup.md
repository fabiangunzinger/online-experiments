# Setup {#sec-setup}

We study a sample of $n$ units to learn about the effect of a binary treatment on these units. We index units as $i = 1, \dots, n$.

The sample of units might be all visitors to an e-commerce app and the treatment a new UX feature. The treatment is "binary" because we only consider two treatment conditions: a unit either experiences the active treatment and is exposed to the new feature or experiences the control treatment and is exposed to the status-quo. We often refer to the two treatment conditions simply as "treatment" and "control".

Each unit has two **potential outcomes**: $Y_i(1)$ is the outcome for unit $i$ if they are in treatment and $Y_i(0)$ is the outcome if they are in control. To simplify notation, we collect all unit-level potential outcomes in the $n \times 1$ vectors $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$. These outcomes are  "potential outcomes" because before the start of the experiment, each unit could be exposed to either treatment condition so that they can potentially experience either outcome. Once the experiment has started and units are assigned to treatment, only one of the two outcomes will be observed.

The causal effect of the treatment for unit $i$ is the difference between the two potential outcomes:

$$
\tau_i = Y_i(1) - Y_i(0).
$$

Because a unit can only ever be in either treatment or control, we can only ever observe one of the two potential outcomes, which means that directly observing unit-level treatment effects is impossible. This is **the fundamental problem of causal inference** [@holland1986statistics]. 

An experiment is one solution to the fundamental problem:[^scientific_solution] randomly assigning units from a population to either treatment or control allows us to estimate average (unit-level) treatment effects. In the words of @holland1986statistics [p. 947]:[^shortcut] 

> "The important point is that [an experiment] replaces the impossible-to-observe causal effect of [a treatment] on a specific unit with the possible-to-estimate *average* causal effect of [the treatment] over a population of units."

Hence, instead of trying to observe unit-level causal effects, the quantity of interest – the **estimand** – in an experiment is an average across a sample of units. We are usually interested in the effect of a universal policy, a comparison between a state of the world where everyone is exposed to the treatment and one where nobody is. While we can capture the difference between these two states of the world in many different ways, we typically focus on the difference in the averages of all these unit-level causal effects over the entire sample:

$$
\begin{align}
\tau
= \frac{1}{n}\sum_{i=1}^n \left(Y_i(1) - Y_i(0)\right)
= \frac{1}{n}\sum_{i=1}^n Y_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0).
\end{align}
$${#eq-estimand}

This is the estimand, the statistical quantity we are trying to estimate in our experiment. In the next section we show how an experiment helps us estimate its true value.


[^scientific_solution]: @holland1986statistics discusses two solutions to the Fundamental Problem: one is the *statistical solution*, which relies on estimating average treatment effects across a large population of units while the other is the *scientific solution*, which uses homogeneity or invariance assumptions. The scientific solution works as follows: say we have one measurement of a units outcome under treatment from today and another measurement of their outcome under control from yesterday. If we are prepared to assume that control measurements are homogenous and invariant to time – that yesterday's control measurement equals the control measurement we would have taken today – then we can calculate the individual level causal effect by comparing the two measurements taken at different points in time. Our assumption is untestable, of course, but in lab experiments it is sometimes possible to make a strong case that it is plausible. It is also the approach we informally use in daily life, whenever we conclude that taking Paracetamol helps against headaches or that going to sleep early makes us feel better the next morning.

[^shortcut]: I have taken a slight shortcut here by treating experiments as being synonymous with the statistical solution because my focus here is on experiments. In principle, however, observational studies can serve the same purpose (albeit with additional assumptions). See, for instance, @imbens2015causal for an extensive discussion of experimental and observational approaches.