# The stats of online experiments

## Setup

We study a sample of $n$ units, indexed by $i = 1, \dots, n$, to learn about the effect of a binary treatment on these units.[^2] The sample of units might be all visitors to an e-commerce app and the treatment a new UX feature. The treatment is "binary" because we only consider two treatment conditions: a unit either experiences the active treatment and is exposed to the new feature or experiences the control treatment and is exposed to the status-quo. We often refer to the two treatment conditions simply as "treatment" and "control".

Each unit has two potential outcomes: $Y_i(1)$ is the outcome for unit $i$ if they are in treatment and $Y_i(0)$ is the outcome if they are in control. To simplify notation, we collect all unit-level potential outcomes in the $n \times 1$ vectors $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$. These outcomes are  "potential outcomes" because before the start of the experiment, each unit could be exposed to either treatment condition so that they can potentially experience either outcome. Once the experiment has started and units are assigned to treatment, only one of the two outcomes will be observed.

The causal effect of the treatment for unit $i$ is the difference between the two potential outcomes:[^unit_level_treatment_effects]
$$
\tau_i = Y_i(1) - Y_i(0).
$$
Because a unit can only ever be in either treatment or control, we can only ever observe one of the two potential outcomes, which means that directly *observing unit-level treatment effects* is impossible. This is the fundamental problem of causal inference [@holland1986statistics]. 

An experiment is one solution to the fundamental problem:[^scientific_solution] randomly assigning units from a population to either treatment or control allows us to *estimate average (unit-level) treatment effects*. In the words of @holland1986statistics [p. 947]:[^shortcut] 

> "The important point is that [an experiment] replaces the impossible-to-observe causal effect of [a treatment] on a specific unit with the possible-to-estimate *average* causal effect of [the treatment] over a population of units."

Hence, instead of trying to observe unit-level causal effects, the quantity of interest – the *estimand* – in an experiment is an average across a sample of units. In particular, we are usually interested in the effect of a universal policy, a comparison between a state of the world where everyone is exposed to the treatment and one where nobody is. While we can capture the difference between these two states of the world in many different ways, we typically focus on the difference in the averages of all these unit-level causal effects over the entire sample:

$$
\begin{align}
\tau
= \frac{1}{n}\sum_{i=1}^n \left(Y_i(1) - Y_i(0)\right)
= \frac{1}{n}\sum_{i=1}^n Y_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0).
\end{align}
$${#eq-estimand}

This is the estimand, the statistical quantity we are trying to estimate in our experiment.

Running an experiment with our $n$ units means that we randomly assign some units to treatment and some to control. We use the binary treatment indicator $W_i \in \{0, 1\}$ to indicate treatment exposure for unit $i$ and write $W_i = 1$ if they are in treatment and $W_i = 0$ if they are in control. We collect all unit-level treatment indicators in the $n \times 1$ vector $\mathbf{W} = (W_1, W_2, \dots, W_n)'$. At the end of the experiment, we have $n_t = \sum_{i=1}^n W_i$ units in treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ units in control. For each unit, we observe outcome $Y_i$.

To estimate $\tau$, we use the observed difference in means between the treatment and control units:[^1]

$$
\begin{align}
\hat{\tau}^{\text{dm}}
=
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\end{align}
$$

This is our estimator, the algorithm we use to produce estimates of the estimand. 

## Assignment mechanism

The procedure we use the allocate units to treatment conditions is the [assignment mechanism](experiment_setup.md#assignment-mechanism). In online experiments, we typically assign units to treatment conditions dynamically as they visit our site and use an assignment mechanism where the assignment of each unit is determined by a process that is equivalent to a coin-toss, such that $P(W_i) = q$, where $q \in [0, 1]$. Throughout, I'll focus on the most common case where $q=\frac{1}{2}$, so that we have:  

$$
P(W_i = 1) = P(W_i = 0) = \frac{1}{2}.
$$
Because of their coin-toss-like nature assignments follow a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution) and the type of experiment is called a Bernoulli Randomised Experiment. Formally, we have:
$$
\begin{align}
W_i &\sim \text{Bernoulli}(1/2) \\
\mathbb{E}[{W_i}] &= 1/2 \\
\end{align}
$$
and
$$
\begin{align}
n_t &\sim \text{Binomial}(n, 1/2) \\
\mathbb{E}[{n_t}] &= n(1/2).
\end{align}
$$


## Analysis

There are different approaches we could take to formally analyse our experiment.

Decisions:

- We could either take a superpopulation or fixed sample perspective. Because for all but the largest companies, most online experiments are eventually run on the entire population of interest, I focus on the latter. This means that the goal of our experiment is to estimate the average treatment effect of the treatment on our $n$ units, rather than using the estimate for our $n$ units to infer the average treatment effect on a larger population from which the $n$ units are drawn. I thus use a fully design-based approach (see @sec-experiment-setup for details)

- We can analyse the Bernoulli Randomised Experiment treating $n_t$ as a Binomial random variable or taking it as given. Because by the time of the analysis it is, in fact, given, I follow the latter approach. This approach also has the advantage of considerably simplifying the math.

Implications:

- Our sample of $n$ units and the associated potential outcomes $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$ are fixed (because units are non-random but determined by sample I have). I refer to the potential outcomes collectively as $\mathbf{Y(w)} = (\mathbf{Y(1)}, \mathbf{Y(0)})$.

- Once randomisation is complete the number of units in treatment and control, $n_t$ and $n_c$ are given. I refer to them collectively as $\mathbf{n} = (n_t, n_c)$.

- The assignment mechanism is also such that units treatment assignment is independent of the treatment assignment of all other units. 

In the next two sections we show that $\hat{\tau}^{\text{dm}}$ is an unbiased estimator of $\tau$ and calculate its variance. (My approach is based on @ding2023first. For an alternative, see Appendix 6.B. in @imbens2015causal.)

## Unbiasedness

An estimator is unbiased if its expected value equals the estimand. To show that the difference in means estimator is unbiased we thus have to show that:

$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]

&=
\tau.
\end{align}
$$
Given our definitions above this is means showing that

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

1) Link observed to potential outcomes so that $Y_i = Y_i(W_i)$. This requires the Stable Unit Treatment Value Assumption (SUTVA).

2) Link treatment group averages to sample averages so that $\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=w}Y_i(w)\right] = \frac{1}{n}\sum_{i=1}^{n}Y_i(w)$. This requires randomisation.

Let's tackle them in turn.

### SUTVA links observed outcomes to potential outcomes

We want to learn something about unit-level differences in potential outcomes
$$
\tau_i = Y_i(1) - Y_i(0),
$$
where a potential outcome is a unit-indexed function of the treatment level, with the treatment levels in our case simply being "treatment" and "control", captured by the assignment indicator $W_i \in {0, 1}$. 

Now say we run an experiment and unit $i$ is allocated to treatment. The $i$-th element of the assignment vector $\mathbf{W}$ of that experiment is thus $W_i = 1$, and we refer to this assignment vector as $\mathbf{W}^{(i=1)}$. 

We know we can't observe both $Y_i(0)$ and $Y_i(1)$, but given that $i$ is in treatment we can observe the potential outcome under treatment, $Y_i(1)$, which will help us estimate $\hat{\tau}$. So we need 

$$
Y_i = Y_i(1).
$$

What we directly observe, however, is
$$
Y_i = Y_i\left(\mathbf{W}^{(i=1)}\right).
$$
In words: the outcome we observe for unit $i$ in our experiment is not the potential outcome for $i$ under treatment, but the potential outcome for $i$ under the specific assignment vector of our experiment, $\mathbf{W}^{(i=1)}$. What does this mean concretely? It means that the observed outcome is a function not only of $i$'s treatment assignment but of:

1. The assignment of all units in the experiment
2. The precise form of the assigned treatment level received by $i$
3. The way in which said assigned treatment level is administered to $i$

We need:
$$
Y_i = Y_i\left(\mathbf{W}^{(i=1)}\right) \overset{!}{=} Y_i(1).
$$
The only way to make progress is to assume what we need: that potential outcomes for unit $i$ are a function only of the treatment level unit $i$ itself receives and independent of (i) treatment assignment of other units and (ii) the form and administration of the treatment level. (i) above is referred to as "no interference" in the literature, (ii) as "no hidden variations of treatment".

That assumption is SUTVA, the Stable Unit Treatment Value Assumption. It ensures that the potential outcomes for each unit and each treatment level are well-defined functions of the unit index and the treatment level only – that for a given unit and treatment level, the potential outcome is well-defined and, thus, "stable" [@rubin1980randomization].  

Note that (ii) does not mean that a treatment level has to take the same form for all units, even though it is often misinterpreted to mean that. What we need is that $Y_i(W_i)$ is a clearly defined function for all $i$ and all possible treatment levels $W_i \in \{0, 1\}$. For this to be the case, it must be the case that there are no different forms of possible treatments, the potential outcome for each for is the same so that the differences are irrelevant, or that the experienced form is random so that the expected outcome across all units remains stable.

In the context of our e-commerce app experiment, the non-interference part of SUTVA means that potential outcomes for all customers are independent of treatment assignment of all other customers – very much including family members and friends and the no-hidden-variation part means that there their precise experience is pinned down by their mobile device and remains stable over time: there are no accidental server-side bugs that creates different background colours and the it is either clear whether a customer uses an Android or iOS app (or the experience is identical). But, again, SUTVA does not require that the active treatment looks exactly the same on iOS and Android apps just that for each unit in our experiment, their experience if they are part of the active treatment is pinned down.

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

### Randomisation links treatment groups to the population

Steps:

- We use the definition of $W_i$ to write  $\sum_{W_i=1}Y_i(1)$ as $\sum_{i=1}^{n} W_iY_i(1)$ and similarly for control units.

- We use the linearity of $\mathbb{E}$ to move $\mathbb{E}$ inside the summation, where the only random element is $W_i$.

- Use [Lemma 1](lemmas.md#lemma-1) (randomisation) to replace expectations with expected values.

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

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^{n}W_iY_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
-
\mathbb{E}\left[
\frac{1}{n_c}\sum_{i=1}^{n}(1-W_i)Y_i(0)
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^{n}
\mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]
Y_i(1)
- 
\frac{1}{n_c}\sum_{i=1}^{n}
\mathbb{E}[1-W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]
Y_i(0)
\\[5pt]

&\text{Randomisation (Lemma 1)}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^{n}
\left(\frac{n_t}{n}\right)
Y_i(1)
-
\frac{1}{n_c}\sum_{i=1}^{n}
\left(\frac{n_c}{n}\right)
Y_i(0)
\\[5pt]

&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]

&=
\tau
\end{align}
$$

## Variance

For the variance calculation below we need a few more definitions. We can define sample means and variances of the potential outcomes as:

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

We have already defined the sample average treatment effect in @eq-estimand. I rewrite it here for convenience and expand the definition using the above expressions:

$$
\begin{align}
\tau 
&= \frac{1}{n}\sum_{i=1}^n \tau_i
\\[5pt]&= \frac{1}{n}\sum_{i=1}^n \left(Y_i(1) - Y_i(0)\right)
\\[5pt]&= \frac{1}{n}\sum_{i=1}^n Y_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]&= \overline{Y}(1) - \overline{Y}(0).
\end{align}
$$


The variance of the individual-level causal effects is:

$$
\begin{align}
S_{\tau_i}^2
&= \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - Y_i(0) 
- \left(\overline{Y}(1) - \overline{Y}(0)\right)\right)^2
\\[5pt]
&= \frac{1}{n-1}\sum_{i=1}^{n}\left(\tau_i - \tau\right)^2 \\[5pt]
\end{align}
$$
The covariance of potential outcomes is:
$$
\begin{align}
S_{0, 1} &= \frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)
\left(Y_i(0) - \overline{Y}(0)\right)
\end{align}
$$

All lemmas referred to below are [here](lemmas.md).

We can then calculate the variance as:

$$
\begin{align}

\mathbb{V}\left(
\hat{\tau}^{\text{dm}}
\right)

&=
\mathbb{V}\left(
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\right)
\\[5pt]

&=
\mathbb{V}\left(
\frac{1}{n_t}\sum_{i=1}^n W_iY_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\right)
\\[5pt]

&\text{SUTVA}
\\[5pt]

&=
\mathbb{V}\left(
\frac{1}{n_t}\sum_{i=1}^n W_iY_i(1) - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i(0)
\right)
\\[5pt]

&=
\mathbb{V}\left(
\frac{1}{n_t}\sum_{i=1}^n W_iY_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n Y_i(0)
+ \frac{1}{n_c}\sum_{i=1}^n W_iY_i(0)
\right)
\\[5pt]

&=
\mathbb{V}\left(
\sum_{i=1}^n W_i\frac{Y_i(1)}{n_t} 
- \sum_{i=1}^n \frac{Y_i(0)}{n_c}
+ \sum_{i=1}^n W_i\frac{Y_i(0)}{n_c}
\right)
\\[5pt]

&=
\mathbb{V}\left(
\sum_{i=1}^n W_i \left(\frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c}\right)
- \sum_{i=1}^n \frac{Y_i(0)}{n_c}
\right)
\\[5pt]

&\text{Dropping constant term}
\\[5pt]

&=
\mathbb{V}\left(
\sum_{i=1}^n W_i \left(\frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c}\right)
\right)
\\[5pt]

&\text{Demeaning (leaves variance unchanged)}
\\[5pt]

&= 
\mathbb{V}\left(\sum_{i=1}^n W_i \left(\frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c} - \left(\frac{\overline{Y}(1)}{n_t} - \frac{\overline{Y}(0)}{n_c}\right)
\right)\right)
&\text{}
\\[5pt]

&\text{Using shorthands } Y_i^+ = Y_i(1)/n_t + Y_i(0)/n_c \text{ and } \overline{Y}^+ = \overline{Y}(1)/n_t - \overline{Y}(0)/n_c
\\[5pt]

&= 
\mathbb{V}\left(\sum_{i=1}^n W_i \left(Y_i^+ - \overline{Y}^+
\right)\right)
&\text{}
\\[5pt]

&\text{Rewriting variance in terms of covariance}
\\[5pt]

&= 
\text{Cov}\left(
\sum_{i=1}^n W_i \left(Y_i^+ - \overline{Y}^+\right),
\sum_{j=1}^n W_j \left(Y_j^+ - \overline{Y}^+\right)
\right)
&\text{}
\\[5pt]

&= 
\sum_{i=1}^n \sum_{j=1}^n
\text{Cov}\left(
W_i \left(Y_i^+ - \overline{Y}^+\right),
W_j \left(Y_j^+ - \overline{Y}^+\right)
\right)
&\text{}
\\[5pt]

&= 
\sum_{i=1}^n \sum_{j=1}^n
\text{Cov}\left(W_i, W_j \right)
\left(Y_i^+ - \overline{Y}^+\right)
\left(Y_j^+ - \overline{Y}^+\right)
&\text{}
\\[5pt]

&= 
\sum_{i=1}^n
\mathbb{V}\left(W_i^2\right)
\left(Y_i^+ - \overline{Y}^+\right)^2
+
\sum_{i=1}^n \sum_{j \neq i}
\text{Cov}\left(W_i, W_j \right)
\left(Y_i^+ - \overline{Y}^+\right)
\left(Y_j^+ - \overline{Y}^+\right)
&\text{}
\\[5pt]

&\text{Lemma 2}
\\[5pt]

&= 
\sum_{i=1}^n
\mathbb{V}\left(W_i\right)
\left(Y_i^+ - \overline{Y}^+\right)^2
+
\sum_{i=1}^n \sum_{j \neq i}
\text{Cov}\left(W_i, W_j \right)
\left(Y_i^+ - \overline{Y}^+\right)
\left(Y_j^+ - \overline{Y}^+\right)
&\text{}
\\[5pt]

&\text{Lemma 3}
\\[5pt]

&=
\sum_{i=1}^{n}\left(\frac{n_tn_c}{n^2}\right)
\left(Y_i^+ - \overline{Y}^+\right)^2
- \sum_{i=1}^{n}\sum_{j \neq i}\left(\frac{n_tn_c}{n^2(n-1)}\right)
\left(Y_i^+ - \overline{Y}^+\right)\left(Y_j^+ - \overline{Y}^+\right)
\\[5pt]

&=
\left(\frac{n_tn_c}{n^2}\right)
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
- \left(\frac{n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}\sum_{j \neq i}
\left(Y_i^+ - \overline{Y}^+\right)\left(Y_j^+ - \overline{Y}^+\right)
\\[5pt]

&\text{Lemma 4}
\\[5pt]

&=
\left(\frac{n_tn_c}{n^2}\right)
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
+ \left(\frac{n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}
\left(Y_i^+ - \overline{Y}^+\right)^2
\\[5pt]

&=
\left(\frac{n_tn_c}{n^2} + \frac{n_tn_c}{n^2(n-1)}\right)
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
&\\[5pt]

&=
\frac{n_tn_c(n-1) + n_tn_c}{n^2(n-1)}
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
&\\[5pt]

&=
\frac{nn_tn_c - n_tn_c + n_tn_c}{n^2(n-1)}
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
&\\[5pt]

&=
\frac{n_tn_c}{n(n-1)}
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
&\\[5pt]

&\text{Reverting to full notation and expanding square term}
\\[5pt]

&=
\frac{n_tn_c}{n(n-1)}
\sum_{i=1}^{n}\left(\frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c} 
- \frac{\overline{Y}(1)}{n_t} - \frac{\overline{Y}(0)}{n_c}\right)^2
&\text{}
\\[5pt]

&=
\frac{n_tn_c}{n(n-1)}
\sum_{i=1}^{n}\left(
\left(\frac{Y_i(1)}{n_t} - \frac{\overline{Y}(1)}{n_t}\right)
+ \left(\frac{Y_i(0)}{n_c} - \frac{\overline{Y}(0)}{n_c}\right)
\right)^2
&\text{}
\\[5pt]

&=
\frac{n_tn_c}{n(n-1)}
\sum_{i=1}^{n}\left(
\frac{1}{n_t}\left(Y_i(1) - \overline{Y}(1)\right)
+ \frac{1}{n_c}\left(Y_i(0) - \overline{Y}(0)\right)
\right)^2
&\text{}
\\[5pt]

&=
\frac{n_tn_c}{n(n-1)}\left[
\sum_{i=1}^{n}\left(
\frac{1}{n_t^2}\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \frac{1}{n_c^2}\left(Y_i(0) - \overline{Y}(0)\right)^2
+ \frac{2}{n_t n_c}\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
\right)
\right]
&\text{}
\\[5pt]

&=
\frac{n_tn_c}{n(n-1)}\left[
\frac{1}{n_t^2}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \frac{1}{n_c^2}\sum_{i=1}^{n}\left(Y_i(0) - \overline{Y}(0)\right)^2
+ \frac{2}{n_t n_c}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
\right]
&\text{}
\\[5pt]

&=
\frac{n_c}{n n_t}\frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \frac{n_t}{n n_c}\frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(0) - \overline{Y}(0)\right)^2
+ \frac{2}{n}\frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
&\text{}
\\[5pt]

&=
\frac{n_c}{n n_t}S_1^2
+ \frac{n_t}{n n_c}S_0^2
+ \frac{1}{n}2S_{0,1}
&\text{}
\\[5pt]

&\text{Lemma 5}
\\[5pt]

&=
\frac{n_c}{n n_t}S_1^2
+ \frac{n_t}{n n_c}S_0^2
+ \frac{1}{n}\left(S_1^2 + S_0^2 - S_{\tau_i}^2\right)
&\text{}
\\[5pt]

&=
\left(\frac{n_c}{n n_t} + \frac{1}{n}\right)S_1^2
+ \left(\frac{n_t}{n n_c} + \frac{1}{n}\right) S_0^2
- \frac{S_{\tau_i}^2}{n}
&\text{}
\\[5pt]

&=
\left(\frac{n_c}{n n_t} + \frac{1}{n}\right)S_1^2
+ \left(\frac{n_t}{n n_c} + \frac{1}{n}\right) S_0^2
- \frac{S_{\tau_i}^2}{n}
&\text{}
\\[5pt]

&=
\frac{n_c + n_t}{n n_t} S_1^2
+ \frac{n_t + n_c}{n n_c} S_0^2
- \frac{S_{\tau_i}^2}{n}
&\text{}
\\[5pt]

&=
\frac{S_1^2}{n_t}
+ \frac{S_0^2}{n_c} 
- \frac{S_{\tau_i}^2}{n}
&\text{}
\\[5pt]
\end{align}
$${#eq-var}

This is the [sampling variance](stats_foundations.md#sampling-distribution) of $\hat{\tau}^{\text{dm}}$. It's a theoretical quantity we cannot directly observe. However, we can observe treatment group means:

$$
\begin{align}
\overline{Y}_t = \frac{1}{n_t}\sum_{i=1}^n W_iY_i
\qquad
\overline{Y}_c = \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\end{align}
$$
and treatment group variances:

$$
\begin{align}
s_t^2 = \frac{1}{n_t-1}\sum_{i=1}^{n}W_i\left(Y_i - \overline{Y}_t\right)^2
\qquad
s_c^2 = \frac{1}{n_c-1}\sum_{i=1}^{n}(1-W_i)\left(Y_i - \overline{Y}_c\right)^2.
\end{align}
$$
It can be shown that the observed treatment group variances $s_t^2$ and $s_c^2$ are unbiased estimators of the sample variances $S_1^2$ and $S_0^2$ (see, for instance, Appendix A in Chapter 6 of @imbens2015causal). The last term in @eq-var, $S_{\tau_i}^2$, is the variance of unit-level treatment effects, which is impossible to observe. As a result, the most widely used estimator in practice is:
$$
\hat{\mathbb{V}}\left(\hat{\tau}^{\text{dm}}\right)
= \frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}.
$$
In our context, the main advantages of this estimator are:

1. If treatment effects are constant across units, then this is an unbiased estimator of the true sampling variance since in this case, $S^2_{\tau_i} = 0$.

2. If treatment effects are not constant, then this is a conservative estimator of the sampling variance (since $S_{\tau_i}^2$ is non-negative).

## Standard error

The [standard error](stats_fundamentals.md#sampling-distribution) of an estimator is simply the square root of its sampling variance. From @eq-var we thus have:

$$
SE\left(\hat{\tau}^{\text{dm}}\right)
= \sqrt{\frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}}.
$${#eq-se}

Because in online experiments sample sizes are large and treatment effects are usually small, it is sometimes convenient to assume equal sample sizes, so that $n_t = n_c = n/2$, and equal variances, so that $s_t^2 = s_c^2 = s^2$. The common variance $s^2$ is estimated by "pooling" the treatment group variances to create a [degrees-of-freedom-weighted](stats_foundations.md#degrees-of-freedom) estimator of the form:
$$
s^2 = \frac{(n_t - 1) s_t^2 + (n_c - 1) s_c^2}{n_t + n_c - 2}.
$$
Substituting in @eq-se we then have:
$$
SE\left(\hat{\tau}^{\text{dm}}\right)
= \sqrt{\frac{s^2}{\frac{n}{2}} + \frac{s^2}{\frac{n}{2}}}
= \sqrt{\frac{4s^2}{n}}.
$${#eq-se-equal}

Finally, for the purpose of experiment design it is sometimes useful to express the standard error in terms of the proportion of units allocated to the treatment group. Hence, instead of assuming equal sample sizes, we use $p$ to denote that proportion and $n$ to denote total sample size, while maintaining the assumption of equal variance. Again substituting in @eq-se we can then write:
$$
SE\left(\hat{\tau}^{\text{dm}}\right)
= \sqrt{\frac{s^2}{pn} + \frac{s^2}{(1-p)n}}
= \sqrt{\frac{s^2}{np(1-p)}}.
$${#eq-se-prop}

For $p=0.5$, this formulation is equivalent to @eq-se-equal as expected.




## Q&A

##### Question 1

Why do we need potential outcomes at all? Can't we interpret the difference from a simple comparison of averages as the causal effect?

##### Question 2

Why do randomised trials not require the excludability assumption in order to lead to unbiased results?



##### Answer 1


Why potential outcomes?
- Clarifies what precisely we are trying to estimate: average individual treatment effect
- Makes explicit the assumptions we need to make to do so: assuming that Y0 are the same in expectation (sth else?)
 - Assumptions needed
	 - SUTVA
		 - What we need is to be able to write Y-i = WY1 + (1-W)Y0. For this we need i) independence from other's assignment, and ii) clearly defined meaning of Wi =1 and Wi = 0, becauese if they are not clearly defined then Y1/Y0 might not be stable. SUTVA handles both of these.
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

##### Answer 2
...


[^unit_level_treatment_effects]: In principle, the unit-level level causal effect can be any comparison between the potential outcomes, such as the difference $Y_i(1) - Y_i(0)$ or the ratio $Y_i(1)/Y_i(0)$. In online experiments, we usually focus on the difference.

[^scientific_solution]: @holland1986statistics discusses two solutions to the Fundamental Problem: one is the *statistical solution*, which relies on estimating average treatment effects across a large population of units while the other is the *scientific solution*, which uses homogeneity or invariance assumptions. The scientific solution works as follows: say we have one measurement of a units outcome under treatment from today and another measurement of their outcome under control from yesterday. If we are prepared to assume that control measurements are homogenous and invariant to time – that yesterday's control measurement equals the control measurement we would have taken today – then we can calculate the individual level causal effect by comparing the two measurements taken at different points in time. Our assumption is untestable, of course, but in lab experiments it is sometimes possible to make a strong case that it is plausible. It is also the approach we informally use in daily life, whenever we conclude that taking Paracetamol helps against headaches or that going to sleep early makes us feel better the next morning.

[^shortcut]: I have taken a slight shortcut here by treating experiments as being synonymous with the statistical solution (see previous footnote) even though observational studies can serve the same purpose (albeit with additional assumptions). I do this because my focus here is on experiments. See, for instance, @imbens2015causal for an extensive discussion of experimental and observational approaches. 

[^omitted_contidioning]: We treat each unit's potential outcomes as given and $n_t$ and $n_c$ as fixed, so the expectation is conditional on these factors: $\mathbb{E}[{\hat{\tau}^{\text{dm}}} | \mathbf{Y(1)}, \mathbf{Y(0)}, n_t, n_c] = \tau$. I omit this explicit conditioning to simplify the notation.

[^1]: To denote the sum over all units in a given treatment group $w$ I use $\sum_{W_i=w}$ as a shorthand for $\sum_{i:W_i=w}$ to keep the notation compact.

[^2]: Our $n$ units are not a sample of a larger population that we hope to learn about but the entire population of units of interest. We thus use a finite sample rather than a super-population approach. For a discussion on the difference between these approaches see @sec-experiment-setup.

[^3]: See @sec-experimen-setup for a more detailed discussion of the treatment assignment mechanism and its implications.


## References
