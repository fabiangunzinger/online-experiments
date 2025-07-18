# Hypothesis testing {#sec-hypothesis-testing}


## Sampling distribution

We have shown in @sec-unbiasedness that $\mathbb{E}\left[\hat{\tau}^{\text{dm}}\mid \mathbf{n}, \mathbf{Y(w)}\right]=\tau$ and in @sec-stderror that $\widehat{SE}= \sqrt{\frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}}$. Furthermore, @imbens2015causal and @ding2023first explain why a normal approximation to the  [sampling distribution](stats_foundations.md#sampling-distribution) of $\hat{\tau}^{\text{dm}}$ is justified. Hence, the sampling distribution of $\hat{\tau}^{\text{dm}}$ is given by:
$$
\hat{\tau}^{\text{dm}} \mid \mathbf{n}, \mathbf{Y(w)} \sim N\left(\tau, \frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}\right).
$${#eq-sampdist}

## Basic approach

To test whether the observed treatment effect is significantly different from zero, we test:
$$
\begin{align}
&H_0: \tau = 0 \\[5 pt]
&H_A: \tau \neq 0.
\end{align}
$$
and calculate the test-statistic
$$
t = \frac{\hat{\tau}^{\text{dm}}}
{\widehat{SE}}
$$
which, for large samples, follows approximately a standard normally distribution.[^1]

For a given significance level $\alpha$, the critical value $z_\alpha$ is the point on the standard normal distribution that has $\alpha$ of the probability mass to its right. In our two-sided test we thus reject reject $H_0$ if $|t| \geq z_{\alpha/2}$.

## Types of errors

With the above procedure, there are two types of mistakes we can make:

- We can reject $H_0$ when it is true. This is called a **Type I error** and constitutes a false positive. The probability of this error is denoted by $\alpha$, the **significance level**. It represents the false positive rate $\alpha = P(\text{significant result} | H_0\text{ true})$.

- We can fail to reject $H_0$ when it is false. This is called a **Type II error** and constitutes a false negative. The probability of this type of error is denoted by $\beta$, and it represents the false negative rate $\beta = P(\text{insignificant result} | H_A\text{ true})$.

The complements of these two errors are:

- The **confidence level**, the probability that we do not reject $H_0$ if it is true (the probability of a true negative) is given by $1 - \alpha = P(\text{insignificant result} | H_0\text{ true})$. 

- **Power**, the probability that we do reject $H_0$ if it is false (the probability of a true positive) is given by $1 - \beta = P(\text{significant result} | H_A\text{ true})$.

One of Neyman's key insights was that while we cannot control error rates for a single experiment, we can control them over the long-run over many experiments. In practice, this means that if we follow the approach consistently and run many experiments we know that in $\alpha$ percent of cases where a feature has no true effect we will erroneously find a significant result.

An interesting question then arises: out of all significant results we find, how many are false positives? The answer is given by the **false discovery rate**, which is $P(H_0\text{ true} | \text{significant result})$. Note that the fewer true effects are generated, the more significant results are due to false positives.


[^1]: See @imbens2015causal Section 6.6.2 and @ding2023first Section 4.2 for details.
