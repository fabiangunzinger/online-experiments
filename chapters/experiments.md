# Experiments

Running an experiment with our $n$ units means that we randomly assign some units to treatment and some to control. We use the binary treatment indicator $W_i \in \{0, 1\}$ to indicate treatment exposure for unit $i$ and write $W_i = 1$ if they are in treatment and $W_i = 0$ if they are in control. We collect all unit-level treatment indicators in the $n \times 1$ vector $\mathbf{W} = (W_1, W_2, \dots, W_n)'$. At the end of the experiment, we have $n_t = \sum_{i=1}^n W_i$ units in treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ units in control. For each unit, we observe outcome $Y_i$.

To estimate $\tau$, we use the observed difference in means between the treatment and control units:[^1]

$$
\begin{align}
\hat{\tau}^{\text{dm}}
=
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\end{align}
$$

This is our **estimator**, the method we use to produce estimates of the estimand. 

The procedure we use the allocate units to treatment conditions is the **assignment mechanism**. In online experiments, we typically assign units to treatment conditions dynamically as they visit our site and use an assignment mechanism where the assignment of each unit is determined by a process that is equivalent to a coin-toss, such that $P(W_i=1) = q$, where $q \in [0, 1]$. Throughout, I'll focus on the most common case of equal sample sizes, where $q=\frac{1}{2}$, so that we have:  

$$
P(W_i = 1) = P(W_i = 0) = \frac{1}{2}.
$$

Because of their coin-toss-like nature assignments follow a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution) and the type of experiment is called a Bernoulli Randomised Experiment. There are different approaches we could take to formally analyse our experiment.

First, we have to decide how to think of our sample of $n$ units. We could either view these units as the population of interest or treat them as a sample from a larger population and use results from the sample to make inferences about the population. Because in all but the largest companies experiments usually run on all traffic, the first perspective is more natural for online experiments and in what follows we treat our $n$ units as the population of interest. In this approach, where the sample of $n$ units is taken as fixed (rather than being treated as a random sample from a larger population), the only source of randomness comes from treatment assignments – from the design of our experiment – and the approach is thus naturally called "design-based".[^design] Given that our sample is fixed, the two vectors of potential outcomes, $\mathbf{Y(1)}$ and $\mathbf{Y(0)}$ are also fixed.

Second, we have to decide how to treat the treatment condition sample sizes. We can analyse the Bernoulli Randomised Experiment treating $n_t$ and $n_c$ as Binomial random variables or taking them as given. I follow the latter approach for two reasons: it considerably simplifies the math, and it more naturally aligns with the situation we face in online experiments given that, by the time of the analysis, sample sizes are, indeed, given.

In the next two sections we show that $\hat{\tau}^{\text{dm}}$ is an unbiased estimator of $\tau$ – that is that, on average, our estimator helps us find the true value of the estimand.

[^design]: For more on the difference between sampling vs design-based inference, see @abadie2020sampling, @athey2017econometrics, @imbens2015causal  for discussions on sampling vs design-based inference.
