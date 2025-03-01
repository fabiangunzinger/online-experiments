
## BRE

A Bernoulli randomised experiment has an assignment mechanism where the assignment of each unit is determined by a process that is equivalent to a coin toss, such that $P(W_i = 1) = q$ and $P(W_i = 0) = 1-q$ where $q \in [0, 1]$. Hence, the probability of observing any given assignment vector $\mathbf{W}$ is:

$$
P(\mathbf{W} = \mathbf{w}) = q^{n_t} (1-q)^{n_c},
$$
where $n_t = \sum_{i=1}^{n}W_i$ and $n_c = \sum_{i=1}^{n}(1 - W_i)$.

We thus have:
$$
\begin{align}
W_i &\sim \text{Bernoulli}(q) \\
\mathbb{E}[{W_i}] &= q \\
\mathbb{V}({W_i}) &= q(1-q)
\end{align}
$$
and

$$
\begin{align}
n_t &\sim \text{Binomial}(n, q) \\
\mathbb{E}[{n_t}] &= nq \\
\mathbb{V}({n_t}) &= nq(1-q)
\end{align}
$$




Case where we assume fixed ns – todo:

- Sometimes, people assume that $n$ is known, in which case $q = n/N$. This turns the setup into a CRE ([[Backup of Neyman's ATE estimator (CRE, imbens2015causal proof)]]). Why do that?
	- It makes the math easier as it spares us from modelling $n$ as a Binomial random variable -- it seems that in the Bernoulli case, the variance isn't finite, while in the CRE case, it is (that's the case everyone works with).
	- Second, by the time we analyse the data, we do know $n$, so the assumption isn't completely arbitrary




## Comparison to simple two-sample case

In the classic two-sample problem, the outcomes in treatment are assumed to be **IID draws** from a distribution with mean $\mu_t$ and variance $\sigma_t^2$ and similar for control, and the variance of the  difference in means estimator is given by:

$$
\mathbb{V}(\hat{\tau}) = \frac{\sigma_t^2}{n_t} + \frac{\sigma_c^2}{n_c}.
$$
That is, there is no third term for the variance of the individual-level potential outcomes.

Here, the variance is taken over the randomness of the outcomes because uncertainty is sampling based, whereas in the potential outcomes framework, where potential outcomes are fixed, the variance is taken over the randomisation distribution. 

Difference to BRE: there is no correlation between values in two groups, whereas in potential outcomes framework there is, unless Y1 and Y0 are cuncorrelated for each unit.

