# Experiments {#sec-experiments}

Running an experiment with our $n$ units means that we randomly assign some units to treatment and some to control. We use the binary treatment indicator $W_i \in \{0, 1\}$ to indicate treatment exposure for unit $i$ and write $W_i = 1$ if they are in treatment and $W_i = 0$ if they are in control. We collect all unit-level treatment indicators in the $n \times 1$ vector $\mathbf{W} = (W_1, W_2, \dots, W_n)'$. At the end of the experiment, we have $n_t = \sum_{i=1}^n W_i$ units in treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ units in control. For each unit, we observe outcome $Y_i$.

To estimate $\tau$ from @eq-estimand, we use the observed difference in means between the treatment and control units:[^1]
$$
\begin{align}
\hat{\tau}^{\text{dm}}
=
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i.
\end{align}
$${#eq-estimator}

This is our **estimator**, the method we use to produce estimates of [the estimand](setup.md). 

The procedure we use the allocate units to treatment conditions is the **assignment mechanism**. In online experiments, we typically assign units to treatment conditions dynamically as they visit our site. The assignment mechanism is such that the assignment of each unit is determined by a process that is equivalent to a coin-toss, such that $P(W_i=1) = q$, where $q \in [0, 1]$. Because of their coin-toss-like nature assignments follow a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution) and the type of experiment is called a Bernoulli Randomised Experiment. Formally, we have $W_i \sim \text{Bernoulli}(q)$.

There are different approaches we could take to formally analyse our experiment. First, we have to decide how to think of our sample of $n$ units. We could either view these units as the population of interest or treat them as a sample from a larger population and use results from the sample to make inferences about the population. Because in all but the largest companies experiments tend to run on all traffic, the first perspective is more natural for online experiments and in what follows we treat our $n$ units as the population of interest. In this approach, where the sample of $n$ units is taken as fixed (rather than being treated as a random sample from a larger population), the only source of randomness comes from treatment assignments – from the design of our experiment – and the approach is thus naturally called "design-based".[^design] Given that our sample is fixed, the two vectors of potential outcomes, $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$ are also fixed.

Second, we have to decide how to treat sample sizes. We can analyse the Bernoulli Randomised Experiment treating $n_t$ and $n_c$ as Binomial random variables or we can take them, too, as given. I follow the latter approach for two reasons: it considerably simplifies the math, and it more naturally aligns with the situation we face in online experiments given that, by the time of the analysis, sample sizes are, indeed, given. Taking sample sizes and potential outcomes as fixed implies that:

$$
W_i \sim \text{Bernoulli}\left(\frac{n_t}{n}\right),
$$
or, equivalently,

$$
P(W_i = 1 \>|\>\mathbf{n}, \mathbf{Y(w)}) = \frac{n_t}{n},
$${#eq-prob}

where $\mathbf{n} = (n_t, n_c)$. That is: given sample sizes and potential outcomes, each unit's probability of being assigned to the treatment group simply equals the proportion of units assigned to the treatment group. Importantly, assignments are independent, meaning that user's probabilities do not depend on how many other users are already part of the treatment group.

This setup will play an important role in the next section, where we show that $\hat{\tau}^{\text{dm}}$ is an unbiased estimator of $\tau$ – that, on average, our estimator helps us find the true value of the estimand.

[^design]: For more on the difference between sampling vs design-based inference, see @abadie2020sampling, @athey2017econometrics, and @imbens2015causal.

[^1]:  I use $\sum_{W_i=w}$ as a shorthand for $\sum_{i:W_i=w}$ to denote the sum over all units in a given treatment group $w$.

