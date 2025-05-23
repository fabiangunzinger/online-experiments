# Statistics foundations


## Other assumptions in casusal inference

## Modes of inference

- see abadie2020sampling 
### From athey2017econometrics (my notes)

Sampling based:
- Also model-based (why?)
- This is the classical mode of inference in statistics and observational studies 
- Randomness results from random sampling.
- We assume that the population at hand is a random subsample of a (much) larger population, so that individual values are random.
- For instance, in $\bar{X} = \frac{1}{n}\sum_{i=1}^{n}X_i$ we treat each $X_i$ as a random variable.
- In the context of experimentation, where we can write the treatment group mean as $\bar{Y}_t = \frac{1}{n_t}\sum_{i=1}^{n}W_iY_i$, we treat the $W_i$s as fixed and $Y_i$s as random ("for each reserved slot in the treatment group, what is the value of the unit we randomly selected for that slot?")
Randomisation based:

### Good chat GPT explanation
*Written by ChatGPT (OpenAI), March 2025.*

Why Do the Terms *Design-Based* and *Model-Based* Make Sense?

- Great question — the terms **design-based** and **model-based** refer to **where the uncertainty comes from** and **what justifies your inference** in a causal or statistical analysis. The names make intuitive sense once you understand their core logic:

**Design-Based Inference**: Uncertainty from the Design

Why “design-based”?
- Because **inference is justified by the experimental design itself** — specifically, the **randomization**.
- The potential outcomes are assumed **fixed** (part of the “science” in Rubin's terms).
- The only randomness comes from **how the treatment was assigned**, which is determined by the **experimental design** (e.g., flipping a fair coin).

> 📌 The inference relies solely on the known **design of the experiment**, not on any model of the outcomes.

Analogy:
- You’re not modeling anything — you’re just using the fact that “we randomly assigned treatment, so any observed differences must be causally interpretable.”

**Model-Based Inference**: Uncertainty from a Statistical Model

Why “model-based”?
- Because **inference is justified by a statistical model** for the data-generating process — not just the design.
- Units are thought of as drawn **randomly from a population**.
- Potential outcomes are **random variables**.
- You specify models for those outcomes: \( Y_i(1), Y_i(0) \sim \text{some distribution} \)

> 📌 The validity of your inference depends on the **correctness of the model**, not just the study design.

Analogy:
- You’re saying: “Let me make assumptions about how the data are generated, and then use probability to estimate causal effects based on those assumptions.”

Summary Table

| Term             | Why It Makes Sense                                     | Where Uncertainty Comes From | Key Assumption              |
| ---------------- | ------------------------------------------------------ | ---------------------------- | --------------------------- |
| **Design-Based** | Relies on **experimental design** (e.g. randomization) | Assignment mechanism         | Randomized treatment        |
| **Model-Based**  | Relies on **statistical models** of outcomes           | Sampling or model error      | Correct model specification |

Why These Terms Are Intuitive
- **Design-based**: You get inference "for free" from the **structure of your design**, without modeling outcomes.
- **Model-based**: You infer causal effects **by assuming a model** — inference comes from the model, not the design.


### Other chat conversation: What *precisely* makes the approach in the main text **design-based**?

The key is that your derivation conditions on the **randomization mechanism** — that is, the known process by which treatment is assigned.

### 🔹 1. Expectation taken over assignment mechanism

You compute:

$$
\mathbb{E}\left[
\hat{\tau}^{\text{dm}} \mid \mathbf{n}, \mathbf{Y(w)}
\right]
$$

- The randomness here is over the **assignment vector** \( \mathbf{W} \) — i.e., how units are assigned to treatment or control.
- The potential outcomes \( \mathbf{Y(w)} \) are treated as **fixed** (non-random).
- This reflects the design of the experiment: if we re-ran the randomization, we’d get a different estimate \( \hat{\tau}^{\text{dm}} \), but the outcomes under treatment and control for each unit would remain the same.

> **This is the hallmark of the design-based approach**:  
> It treats assignment as the source of randomness, and all other aspects (units, outcomes, covariates) as fixed.

---

### 🔹 2. Unbiasedness is shown by averaging over random assignments

You show:

$$
\mathbb{E}[\hat{\tau}^{\text{dm}} \mid \mathbf{n}, \mathbf{Y(w)}] = \tau
$$

This tells us that:
- The estimator is **unbiased** *because of how the assignment was randomized*, not because of any assumptions about functional forms, ignorability, or data-generating processes.
- That is: **randomization justifies the inference**, not a model of selection or outcome behavior.

---

### 🔹 3. No assumptions about ignorability or conditional independence

You do **not** assume:
- \( W_i \perp (Y_i(0), Y_i(1)) \mid X_i \) (as in observational methods),
- or any structural model of selection or behavior.

Instead, your inference relies solely on the **known randomization scheme** — e.g., Bernoulli or complete random assignment.

---

### 🧭 In contrast: Identification-based (observational) approach

- Takes the data as generated by an **unknown, possibly non-random assignment**.
- Makes assumptions (e.g., unconfoundedness) to justify:

$$
\mathbb{E}[Y_i(1) - Y_i(0)] = \mathbb{E}[Y_i \mid W_i = 1] - \mathbb{E}[Y_i \mid W_i = 0]
$$

- Assumes assignment is as good as random **after controlling for covariates**.

---

### 📌 Summary

> The approach in the main text is **design-based** because it:
> - Treats potential outcomes as fixed,  
> - Takes expectations **only over the random assignment**,  
> - Shows unbiasedness of the estimator purely from the **known experimental design**,  
> - Requires **no modeling of selection**.

This is fundamentally different from the identification-based approach, which models or assumes ignorability to equate observed and counterfactual outcomes.



## Sampling 

- We rely on a sample to learn about a larger population.

- We thus need to make sure that the sampling procedure is free of bias, so that units in the sample are representative of those in the population.

- While representativeness cannot be achieved perfectly, it's important to ensure that non-representativeness is due to random error and not due to systematic bias.

- Random errors produce deviations that vary over repeated samples, while systematic bias persists. Such selection bias can lead to misleading and ephemeral conclusions.

- Sampling procedures:
	- [[Simple random sampling]]
	- [[Completely random sampling]]
	- [[Stratified random sampling]]
  - Randomly select $n_s$ from each stratum $S$ of a population of $N$
  - On stratification: why does it reduce variance? Imagine an extreme case, where the number of strata were equal to the number of different units in the sample. In this case, the variance would be zero. Number of diff units here needs be individuals, but groups of units that share all relevant characteristics
  - The mean outcome of the sample is denoted $\bar{x}$; that of the population, $\mu$.

- Repeated sampling creates a [[Sampling distribution]]

## Selection bias

Common types of selection bias in data science:
- The vast search effect (using the data to answer many questions will eventually reveal something interesting by mere chance -- if 20,000 people flip a coin 10 times, some will have 10 straight heads)
- Nonrandom sampling
- Cherry-picking data
- Selecting specific time-intervals
- Stopping experiments prematurely
- Regression to the mean (occurs in settings where we measure outcomes repeatedly over time and where luck and skill combine to determine outcomes, since winners of one period will be less lucky next period and perform closer to the mean performer)

Ways to guard against selection bias:
- have one or many holdout datasets to confirm your results. 


### Sampling distribution

- A sampling distribution is the distribution of a statistic (e.g. the mean) over many repeated samples. Classical statistics is much concerned with making inferences from samples about the population based on such statistics.  

- When we measure an attribute of the population based on a sample using a statistic, the result will vary over repeated samples. To capture by how much it varies, we are concerned with the sampling variability.

- Key distinctions:

	- The data distribution is the distribution of the data in the sample, and its spread is measured by the variance or its square root, the standard deviation.

	- The sampling distribution is the distribution of the sample statistic, and its spread is measured by the sampling variance or its square root, the standard error.

- Plots below show that:
  - Data distribution has larger spread than sampling distributions (each data point is a special case of a sample with n = 1)
  - The spread of sampling distributions decreases with increasing sample size

```
rng = np.random.default_rng(2312)


def means(data, sample_size, num_means=1000):
    return rng.choice(data, (sample_size, num_means)).mean(0)


# Create dataset with population and sample data
data = pd.DataFrame({"Population": rng.normal(size=1_000_000)})
for n in [10, 100, 1000]:
    data = data.join(
        pd.Series(means(data.Population, n), name=f"Means of samples of {n}")
    )
data = data.melt()


g = sns.FacetGrid(data, col="variable")
g.map(sns.histplot, "value", bins=40, stat="percent")
g.set_axis_labels("Value", "Count")
g.set_titles("{col_name}");
```

### Variance and standard error

- The standard error is a measure for the variability of the sampling distribution.

- It is related to the standard deviation of the observations, $\sigma$ and the sample size $n$ in the following way:

$$
se = \frac{\sigma}{\sqrt{n}}
$$
- The relationship between sample size and se is sometimes called the "Square-root of n rule", since reducing the $se$ by a factor of 2 requires an increase in the sample size by a factor of 4.

Derivation:

The sum of a sequence of independent random variables is:
$$
T = (x_1 + x_2 + ... + x_n)
$$

Which has variance

$$
Var(T) = Var(x_1) + Var(x_2) + ... + Var(x_n) = n\sigma^2
$$

and mean

$$
\bar{x} = T/n.
$$

The variance of $\bar{x}$ is then given by:

$$
Var(\bar{x}) = Var\left(\frac{T}{n}\right) = \frac{1}{n^2}Var(T) = \frac{1}{n^2}n\sigma^2 = \frac{\sigma^2}{n}.
$$

The standard error is defined as the standard deviation of $\bar{x}$, and is thus

$$
se(\bar{x}) = \sqrt{Var(\bar{x})} = \frac{\sigma}{\sqrt{n}}.
$$


### Example code

```python

rng = np.random.default_rng(2312)

def means(data, sample_size, num_means=1000):
	return rng.choice(data, (sample_size, num_means)).mean(0)

  
# Create dataset with population and sample data

data = pd.DataFrame({"Population": rng.normal(size=1_000_000)})

for n in [10, 100, 1000]:
	data = data.join(
		pd.Series(means(data.Population, n),
		name=f"Means of samples of {n}")
	)

data = data.melt()

g = sns.FacetGrid(data, col="variable")
g.map(sns.histplot, "value", bins=40, stat="percent")
g.set_axis_labels("Value", "Count")
g.set_titles("{col_name}");
```

Figure shows that:

- The spread of sampling distributions decreases with increasing sample size

- Data distribution has larger spread than sampling distributions (each data point is a special case of a sample with n = 1)


## Law of large numbers and central limit theorems

- Suppose that we have a sequence of independent and identically distributed (iid) random variables $\{x_1, ..., x_n\}$ drawn from a distribution with expected value $\mu$ and finite variance $\sigma^2$, and we are interested in the mean value $\bar{x} = \frac{x_1 + ... + x_n}{n}$.  

- The law or large numbers states that $\bar{x}$ converges to $\mu$ as we increase the sample size. Formally:

$$
\bar{x} \rightarrow \mu \text{ as } n \rightarrow \infty.
$$

- The (classical, Lindeberg-Lévy) central limit theorem describes the spread of the sampling distribution of $\bar{x}$ around $\mu$ during this convergence. In particular, it implies that for large enough $n$, the distribution of $\bar{x}$ will be close to a normal distribution with mean $\mu$ and variance $\sigma^2/n$. The above figures are a visual representation of this. Formally:

$$
\lim _{n\to\infty} \sqrt{n}(\bar{x} - \mu) \rightarrow \mathcal{N}\left(0,\sigma ^{2}\right).
$$

- This is useful because it means that irrespective of the underlying distribution (i.e. the distribution of the values in our sequence above), we can use the normal distribution and approximations to it (such as the t-distribution) to calculate sampling distributions when we do inference. Because of this, the CLT is at the heart of the theory of hypothesis testing and confidence intervals, and thus of much of classical statistics.

- For experiments, this means that our estiamted treatment effect is normally distributed, which is what allows us to draw inferences from our experimental setting ot the population as a whole. The CLT is thus at the heart of the experimental approach.

- The CLT also explains the prevalence of the normal distribution in the natural world. Many characteristics of living things we observe and measure are the sum of the additive effects of many genetic and environmental factors, so their distribution tends to be normal.


## Degrees of freedom

In statistics, degrees of freedom generally refers to the number of values in a calculation that can vary freely.

Examples:

- Variance calculation: given that we have a mean, once we know all but one value, we also know final value, since sum of mean deviations has to be zero.

- Covariance calculation: given the two means, once we know the values for all but one x and y pair, we also know the values of the final pair. Hence, we loose one df (not clear to me why not two, given that both x and y are determined -- because we treat their product as a single value? but that seems arbitrary)

- Also, why no correction when we have popultion means? See wikipedia article on variance for section on bias correction

- There is lots of confusion out there when it comes to df. For instance, you sometimes hear people say that df is the number of parameters you had to calculate on route. But this is wrong. It happens to come to the same when calculating variance, but not if you calcualte covariance (where you calculate two means beforehand, but only loose one df).


## The bootstrap

- In practice, we often use the bootstrap to calculate standard errors of model parameters or statistics.

- Conceptually, the bootstrap works as follows:

1) we draw an original sample and calculate our statistic

2) we then create a blown-up version of that sample by duplicating it many times

3) we then draw repeated samples from the large sample, recalculate our statistic, and calculate the standard deviation of these statistics to get the standard error.

- To achieve this easily, we can skip step 2) by simply sampling with replacement from the original distribution in step 3).

- The full procedure makes clear what the bootstrap results tell us, however: they tell us how lots of additional samples would behave if they were drawn from a population like our original sample.

- Hence, if the original sample is not representative of the population of interest, then bootstrap results are not informative about that population either.

- The bootstrap can also be used to improve the performance of classification or regression trees by fitting multiple trees on bootstrapped sample and then averaging their predictions. This is called "bagging", short for "bootstrap aggregating".

- We can use to boostrap also to calculate CIs following this algorithm:

1) Draw a large number of bootstrap samples and calculate the statistic of interest

2) Trim [(100-x)/2] percent of the bootstrap results on either end of the distribution

3) The trim points are the end point of the CI.

```{python}

from sklearn.utils import resample

  

rng = np.random.default_rng(2312)

  

population = rng.normal(3, 5, 1_000_000)

sample = rng.choice(population, 1000)

resample_means = pd.Series(resample(sample).mean() for _ in range(1000))

  

print(f"{'Population mean:':20} {np.mean(population):.3f}")

print(f"{'Sample mean:':20} {np.mean(sample):.3f}")

print(f"{'Bootstrap mean:':20} {np.mean(resample_means.mean()):.3f}")

print(f"{'Bootstrap se:':20} {np.mean(resample_means.std()):.3f}")

```

## Combination vs Permutation

**Permutation (Order Matters)**

If we have $n$ items and we want to pick $r$ in a specific order, the formula is:  

$$
P(n, r) = \frac{n!}{(n - r)!}
$$
Example:
Arranging 3 letters (A, B, C) in 2 positions.  
- Possible orders: AB, AC, BA, BC, CA, CB → 6 ways  
- Formula:  
  $$
  P(3,2) = \frac{3!}{(3 - 2)!} = \frac{3!}{1!} = \frac{3 \times 2 \times 1}{1} = 6
  $$

**Combination (Order Doesn't Matter)**

If we have $n$ items and we want to pick $r$, but order does not matter, the formula is:

$$
C(n, r) = \frac{n!}{r!(n - r)!}
$$

Example:

Choosing 2 letters from (A, B, C), where order does not matter.  
- Possible groups: {A, B}, {A, C}, {B, C} → 3 ways  
- Formula:  
  $$
  C(3,2) = \frac{3!}{2!(3 - 2)!} = \frac{3!}{(2! \times 1!)} = \frac{3 \times 2 \times 1}{(2 \times 1) \times 1} = 3
  $$

**Key Difference:**
- **Permutation:** ABC and BAC are different  
- **Combination:** ABC and BAC are the same  

**Shortcut:**  
$$
P(n, r) = C(n, r) \times r!
$$  
(Since for every combination, there are $r!$ ways to arrange it.)


## Moments of random variables

In general, the kth uncentered moment of a discrete random variable X is defined by

$$
E(X^k) = \sum_{i=1}^n p(x_i)x_i^k,
$$

and the kth centered moment as

$$
E\left(X-E(X)\right)^k = \sum_{i=1}^n p(x_i)(x_i - \mu)^k,
$$

Hence, the mean of a random variable is the first uncentered momement, and the variance is the second centered moment.



## Variance and covariance properties

Building blocks for advanced manipulations.


  
$$
\begin{align}
Var(X + c) &= Var(X) \\
Var(X + Y + c) &= Var(X + Y)
\end{align}
$$


Add here covariance properties that show that

cov(X + a, Y + b) = cov(X ,Y)


$$
\begin{align}
Cov(\bar{c}, \bar{d})

&=\mathbb{E}\left[(\bar{c} - \mathbb{E}[\bar{c}])
(\bar{d} - \mathbb{E}[\bar{d}])\right]
&\text{}
\\[5pt]

&=\mathbb{E}\left[
\bar{c}\bar{d}
- \mathbb{E}[\bar{d}]\bar{c}
- \mathbb{E}[\bar{c}]\bar{d}
+ \mathbb{E}[\bar{c}]\mathbb{E}[\bar{d}]
\right]
&\text{}
\\[5pt]

&=
\mathbb{E}\left[\bar{c}\bar{d}\right]
- \mathbb{E}[\bar{d}]\mathbb{E}[\bar{c}]
- \mathbb{E}[\bar{c}]\mathbb{E}[\bar{d}]
+ \mathbb{E}[\bar{c}]\mathbb{E}[\bar{d}]
&\text{}
\\[5pt]

&=\mathbb{E}\left[\bar{c}\bar{d}\right] - \mathbb{E}[\bar{c}]\mathbb{E}[\bar{d}]
&\text{}
\\[5pt]
\end{align}
$$

In general:

$$
\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y] + \text{Cov}(X, Y)
$$



## Confidence intervals

- A CI is another way to learn about the variability of a test statistic. 

- It can be calculated using the (standard) normal distribution or the t-distribution (if sample sizes are small).

- But for data science purposes we can compute an x-percent CI from the bootstrap, following this algorithm: 1) Draw a large number of bootstrap samples and calculate the statistic of interest, 2) Trim [(100-x)/2] percent of the bootstrap results on either end of the distribution, 3) the trim points are the end point of the CI.

## Commonly used distributions

from [here](https://en.wikipedia.org/wiki/Variance)

### Commonly used probability distributions

The following table lists the variance for some commonly used probability distributions.

| Name of the probability distribution | Probability distribution function | Mean | Variance |

|--------------------------------------|-----------------------------------|------|----------|

| Binomial distribution | $\Pr\,(X=k) = \binom{n}{k}p^k(1 - p)^{n-k}$ | $np$ | $np(1 - p)$ |

| Geometric distribution | $\Pr\,(X=k) = (1 - p)^{k-1}p$ | $\frac{1}{p}$ | $\frac{1 - p}{p^2}$ |

| Normal distribution | $f(x \mid \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x - \mu)^2}{2\sigma^2}}$ | $\mu$ | $\sigma^2$ |

| Uniform distribution (continuous) | $f(x \mid a, b) = \begin{cases} \frac{1}{b - a} & \text{for } a \le x \le b, \\[3pt] 0 & \text{for } x < a \text{ or } x > b \end{cases}$ | $\frac{a + b}{2}$ | $\frac{(b - a)^2}{12}$ |

| Exponential distribution | $f(x \mid \lambda) = \lambda e^{-\lambda x}$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ |

| Poisson distribution | $f(k \mid \lambda) = \frac{e^{-\lambda}\lambda^{k}}{k!}$ | $\lambda$ | $\lambda$ |

