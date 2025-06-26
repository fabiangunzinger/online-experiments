
## Variance

This section derives the of our [difference-in-means estimator](experiments.md). Because we use a [design-based approach](experiment.md) to analyse our experiment, the simple sampling-based results do not apply and the derivation is considerably more involved. It's really extremely boring to follow! But, as with all derivations, following it at least once helps with understanding what's going on. 

My approach is based on @ding2023first but fills in a lot of steps that are skipped there in the hope that this step-by-step approach makes the logic more accessible.[^approach]

## Definitions

We start with some definitions.

The sample means and variances of the [potential outcomes](setup.md) are:

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

## Variance derivation

We can now start with our derivation. The derivation is long and makes use of intermediary results. I provide more information on these intermediary results in collapsible boxes at the bottom of the derivation and refer to them as "Note #".

To not make the notation even more complex I do not explicitly condition the variance on $\mathbf{n}$ and $\mathbf{Y(w)}$ here as I did in the proof for [unbiasedness](unbiasedness.md), but doing so would look the same and I use the fact that these terms are fixed throughout.
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
&\text{Note 1}
\\[5pt]
&=
\mathbb{V}\left(
\frac{1}{n_t}\sum_{i=1}^n W_iY_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\right)
\\[5pt]
&\text{Note 2}
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



::: {.callout-note collapse=true title="Note 1"}
Given that $W_i=1$ for treatment units and $W_i=0$ for control units, we can calculate treatment group means by summing over all units and using $W_i$ to "pick out" the relevant units for each treatment group so that we have:
$$
\begin{align}
\hat{\tau}^{\text{dm}}
&=
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\\[5pt]
&= 
\frac{1}{n_t}\sum_{i=1}^{n}W_iY_i - \frac{1}{n_c}\sum_{i=1}^{n}(1-W_i)Y_i
\end{align}
$$
:::

::: {.callout-note collapse=true title="Note 2"}
This step uses SUTVA which – if it holds – implies that:
$$
Y_i = Y_i(W_i) = \begin{cases} 
   Y_i(1) & \text{if } W_i = 1 \\
   Y_i(0)       & \text{if } W_i = 0,
  \end{cases}
$$

See [here](unbiasedness.md#sutva-links-observed-outcomes-to-potential-outcomes) for a detailed discussion.
:::





This is the [sampling variance](stats_foundations.md#sampling-distribution) of $\hat{\tau}^{\text{dm}}$. It's a theoretical quantity we cannot directly observe. 

## Variance estimation

However, we can observe treatment group means:

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
It can be shown that the observed treatment group variances $s_t^2$ and $s_c^2$ are unbiased estimators of the sample variances $S_1^2$ and $S_0^2$ (see, for instance, Appendix A in Chapter 6 of @imbens2015causal). The last term in 

<!-- @eq-var,  -->

$S_{\tau_i}^2$, is the variance of unit-level treatment effects, which is impossible to observe.

As a result, the most widely used estimator in practice is:
$$
\hat{\mathbb{V}}
= \frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}.
$$
In our context, the main advantages of this estimator are:

1. If treatment effects are constant across units, then this is an unbiased estimator of the true sampling variance since in this case, $S^2_{\tau_i} = 0$.

2. If treatment effects are not constant, then this is a conservative estimator of the sampling variance (since $S_{\tau_i}^2$ is non-negative).

[^other]: The other main motivation was that I couldn't find a [step-by-step derivations of the sample size formula](power.md).

[^approach]: @imbens2015causal provide an alternative derivation in Appendix B of Chapter 6 that is fairly detailed but still skips a lot of steps that weren't immediately obvious to me, and they define a helper variable I don't find to help that much. I find the approach in @ding2023first more transparent. But the original source skips a lot of steps and is thus not very accessible.