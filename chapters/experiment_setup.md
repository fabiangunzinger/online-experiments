todo
- Combine as needed


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

We have data from a randomised experiment with assignment vector $\mathbf{W} = \{W_1, ... W_n\}$ where $n_t = \sum_{i=1}^n W_i$ units are allocated to treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ to control. 

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










# Old notes

## Experiment sampling note

Online experiments usually use the following **sampling** approach to determine whether a user is part of an experiment:

1. Create a hash string unique to each user. For example: `<user_id><experiment_id>`.

2. Feed the hash string into a hash algorithm (often MD5) and receive a hash value.

3. Use the hash value to determine whether a user is part of the experiment. Say we allocate 10% of traffic to the experiment. We then include the user only if their hash value falls within the bottom (or top) 10% of possible hash values.

With this approach, the probability that a user is sampled into the experiment is independent of the sampling decisions of all other users. 

We write $R_i = 1$ if user $i$ is part of the experiment sample and $R_i = 0$ if they aren't.

If we sample $n$ users into the experiment, then each of our $N$ users is part of the experiment experiment with probability $n/N$. [^conditioning_on_n]
  

[^conditioning_on_n]: For each user in the super population, being part of the experiment sample is a [Bernoulli trial](https://en.wikipedia.org/wiki/Bernoulli_trial) with $R_i \sim \text{Bernoulli}(n/N)$ and, hence, $\E{R_i} = n/N$ and $\V{R_i} = n/N(1-n/N)$. I assume here that we know $n$. The reason for doing so is twofold. First, it makes the math easier as it spares us from modeling $n$ as a Binomial random variable. Second, by the time we analyse the data, we do know $n$.

## Setup notes 1

- When deciding on the experiment setup we make two types of decisions.

- First, we decide whether the $n$ units in our experiment sample are the population of interest, or whether that population of interest is instead a larger super-population of size $N$ of which the $n$ units in the experiment sample are a random sample. Following @imbens2015causal, I refer to the former as the finite sample perspective and the later as the super population perspective. See more in [[Finite sample vs super-population perspectives]]

- Second, regardless of which of these two perspectives we adopt, we decide how to allocate the $n$ units in the experiment sample into a treatment and control group. The procedure for performing this allocation is called the [[Assignment mechanism]], and there are a number of different such mechanisms.

- Technically, this type of experiment is called a [[Bernoulli randomised experiment]].

- If we are treating $n$ as known, then the setting becomes equivalent to a [[Backup of Neyman's ATE estimator (CRE, imbens2015causal proof)]] (see discussion in [[Bernoulli randomised experiment]] for more detail)


## Setup notes 2 (online experiments)

[[_Experimentation]]

- Online experiments typically go through a ramp-up phase: they include only a small fraction of users at the start and all users at the end.  

- Statistically, this means we have to consider two different cases. In the first case, during the early stages of an experiment, we use a sample of the entire user population to learn something about that population as a whole; in this case, the sample we work with and the sample we want to learn something about are different. In the second case, when the entire user population is part of the experiment, the sample we work with and the sample we want to learn something about are the same. Following @imbens2015causal, I refer to the first approach as the super-population perspective and the second case the finite sample perspective.

- We have a (super) population of $N$ users. In the example I'm going to use throughout, these are all of our iOS-app users in the UK.

- From this super population, we sample $n$ users to be part of the experiment, and then allocate $n_t$ users to treatment and $n_c$ users to control.

- The way we sample users into the experiment and allocate them to treatment has implications for our statistical analysis, so let's have a look at the details.

- Online experiments usually use the following **sampling** approach to determine whether a user is part of an experiment:

	1. Create a hash string unique to each user such as `<user_id><experiment_id><market>`.  
	2. Feed the hash string into a hash algorithm (often MD5) and receive a hash value.
	3. Use the hash value to determine whether a user is part of the experiment. Say we allocate 10% of traffic to the experiment. We then include the user only if their hash value falls within the bottom (or top) 10% of possible hash values.

- With this approach, the probability that a user is sampled into the experiment is independent of the sampling decisions of all other users.


**Treatment assignment** for units in the experiment sample then works in the same way:

4. We again create a unique -- but different[^different_hash] -- hash string for each unit.

5. We use the hash algorithm to generate a hash value

6. If we wanted to allocate units equally to a control and treatment group, we could allocate all units with hash values within the bottom 50% of possible values to the treatment group and all others to the control group.

- Hashed sampling and treatment assignment resembles sampling without replacement in that we can only include a given user in the experiment once, and resembles sampling with replacement in that **sampling and treatment assignment are independent** across units -- sampling any one unit does not alter the chance of being sampled for any other unit.[^dependent_sampling]

[^different_hash]: Suppose we used the same hash strings instead, and that we sampled units with hash values that fall into the bottom 10% of possible hash values. What would happen if we now allocated all units with hash values in the bottom 50% of possible values to treatment and all others to control? How many units would be in the control group? (None! Since the hash values of all units in the sample would fall into the bottom 10% of possible hash values so that all units would be allocated to treatment.) A simple way to create a different hash value is to flip the first bit of the hash string. Because of the [avalanche effect](https://en.wikipedia.org/wiki/Avalanche_effect), this would result in a completely different hash value.


## Setup notes 3







  
