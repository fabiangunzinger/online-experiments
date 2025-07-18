
# Variance {#sec-variance}

This section derives the [sampling variance](stats_foundations.md#sampling-distribution) of our [difference-in-means estimator](experiments.md). Because we use a [design-based approach](experiment.md) to analyse our experiment, the simple sampling-based results do not apply and the derivation is considerably more involved. It's really extremely boring to follow! But, as with all derivations, following it at least once helps with understanding what's going on. 

## Sampling variance

My approach is based on @ding2023first but fills in a lot of steps that are skipped there in the hope that this step-by-step approach makes the logic more accessible.[^approach] The derivation uses intermediary results labeled *lemmas*; their derivations appear at the bottom of the page and are linked from inside the main derivation.

To keep the notation simpler, I omit explicit conditioning on $\mathbf{n}$ and $\mathbf{Y(w)}$, though the conditioning is implicit throughout. Throughout, I define the sample means of the [potential outcomes](setup.md) as
$$
\overline{Y}(1) = \frac{1}{n}\sum_{i=1}^n Y_i(1),
\qquad
\overline{Y}(0) = \frac{1}{n}\sum_{i=1}^n Y_i(0),
$$
and use the shorthands:
$$
Y_i^+ \coloneqq \frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c}, 
\qquad \overline{Y}^+ \coloneqq \frac{\overline{Y}(1)}{n_t} + \frac{\overline{Y}(0)}{n_c}.
$$

Starting from the definition of our difference-in-means estimator, we get:

<!-- Jump back up anchor below -->
<a id="anchor-1"></a>
$$
\begin{align}
\mathbb{V}\left(\hat{\tau}^{\text{dm}}\right)
&= \mathbb{V}\left(\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i\right) \\[6pt]
&\href{#lemma-1}{=} \mathbb{V}\left(\frac{1}{n_t}\sum_{i=1}^n W_i Y_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i) Y_i\right) \\[6pt]
&\href{#lemma-2}{=} \mathbb{V}\left(\frac{1}{n_t}\sum_{i=1}^n W_i Y_i(1) - \frac{1}{n_c}\sum_{i=1}^n (1-W_i) Y_i(0)\right) \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i \frac{Y_i(1)}{n_t} + W_i \frac{Y_i(0)}{n_c} - \frac{Y_i(0)}{n_c}\right) \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i Y_i^+ - \sum_{i=1}^n \frac{Y_i(0)}{n_c}\right) \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i Y_i^+\right) \quad \text{(drop constant)} \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i (Y_i^+ - \overline{Y}^+)\right) \quad \text{(demean)} \\[6pt]
\end{align}
$$

Expressing the right-hand side in terms of the covariance:
$$
\begin{align}
\mathbb{V}\left(\sum_{i=1}^n W_i (Y_i^+ - \overline{Y}^+)\right)
&= \text{Cov}\left(
\sum_{i=1}^n W_i \left(Y_i^+ - \overline{Y}^+\right),
\sum_{j=1}^n W_j \left(Y_j^+ - \overline{Y}^+\right)
\right)
\\[6pt]
&= 
\sum_{i=1}^n \sum_{j=1}^n
\text{Cov}\left(
W_i \left(Y_i^+ - \overline{Y}^+\right),
W_j \left(Y_j^+ - \overline{Y}^+\right)
\right) \\[6pt]
&=
\sum_{i=1}^n \sum_{j=1}^n
\text{Cov}\left(W_i, W_j \right)
\left(Y_i^+ - \overline{Y}^+\right)
\left(Y_j^+ - \overline{Y}^+\right)
\end{align}
$$
Splitting the double sum into cases where $i=j$ and $j \neq j$:
<!-- Jump back up anchor below -->
<a id="anchor-2"></a>
$$
\begin{align}
\sum_{i=1}^n \sum_{j=1}^n
&
\text{Cov}\left(W_i, W_j \right)
\left(Y_i^+ - \overline{Y}^+\right)
\left(Y_j^+ - \overline{Y}^+\right)
\\[6pt]
&= 
\sum_{i=1}^n
\mathbb{V}\left(W_i^2\right)
\left(Y_i^+ - \overline{Y}^+\right)^2
+
\sum_{i \neq j}
\text{Cov}\left(W_i, W_j \right)
\left(Y_i^+ - \overline{Y}^+\right)
\left(Y_j^+ - \overline{Y}^+\right)
&\text{}
\\[6pt]
&\href{#lemma-3}{=} 
\sum_{i=1}^n
\mathbb{V}\left(W_i\right)
\left(Y_i^+ - \overline{Y}^+\right)^2
+
\sum_{i \neq j}
\text{Cov}\left(W_i, W_j \right)
\left(Y_i^+ - \overline{Y}^+\right)
\left(Y_j^+ - \overline{Y}^+\right)
\\[6pt]
&\href{#lemma-4}{=}
\sum_{i=1}^{n}\left(\frac{n_tn_c}{n^2}\right)
\left(Y_i^+ - \overline{Y}^+\right)^2
- \sum_{i \neq j}\left(\frac{n_tn_c}{n^2(n-1)}\right)
\left(Y_i^+ - \overline{Y}^+\right)\left(Y_j^+ - \overline{Y}^+\right)
\\[6pt]
&=
\left(\frac{n_tn_c}{n^2}\right)
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
- \left(\frac{n_tn_c}{n^2(n-1)}\right)\sum_{i \neq j}
\left(Y_i^+ - \overline{Y}^+\right)\left(Y_j^+ - \overline{Y}^+\right)
\\[6pt]
&\href{#lemma-5}{=}
\left(\frac{n_tn_c}{n^2}\right)
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
+ \left(\frac{n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}
\left(Y_i^+ - \overline{Y}^+\right)^2
\\[6pt]
&=
\frac{n_tn_c}{n(n-1)}
\sum_{i=1}^{n}\left(Y_i^+ - \overline{Y}^+\right)^2
\end{align}
$$

Reverting to full notation and expanding square term
$$
\begin{align}
\frac{n_tn_c}{n(n-1)}&
\sum_{i=1}^{n}\left[\frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c} - \left(\frac{\overline{Y}(1)}{n_t} + \frac{\overline{Y}(0)}{n_c}\right)\right]^2
\\[6pt]
&=
\frac{n_tn_c}{n(n-1)}
\sum_{i=1}^{n}\left[
\frac{1}{n_t}\left(Y_i(1) - \overline{Y}(1)\right)
+ \frac{1}{n_c}\left(Y_i(0) - \overline{Y}(0)\right)
\right]^2
\\[6pt]
&=
\frac{n_tn_c}{n(n-1)}
\sum_{i=1}^{n}\Biggl[
\frac{1}{n_t^2}\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \frac{1}{n_c^2}\left(Y_i(0) - \overline{Y}(0)\right)^2
\\[6pt]
&\qquad
+ \frac{2}{n_t n_c}\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)\Biggr]
\\[6pt]
&=
\frac{n_c}{n n_t}\frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \frac{n_t}{n n_c}\frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(0) - \overline{Y}(0)\right)^2
\\[6pt]
&\qquad
+ \frac{2}{n}\frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
\end{align}
$$

Defining the sample variances of the potential outcomes as:
$$
S_1^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)^2,
\qquad
S_0^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(0) - \overline{Y}(0)\right)^2,
$$
the variance of the individual-level causal effects as:
$$
S_{\tau_i}^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - Y_i(0) 
- \left(\overline{Y}(1) - \overline{Y}(0)\right)\right)^2,
$$
and the covariance of potential outcomes as:
$$
S_{0, 1} = \frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)
\left(Y_i(0) - \overline{Y}(0)\right),
$$
we thus have:
<!-- Jump back up anchor below -->
<a id="anchor-3"></a>
$$
\begin{align}
\mathbb{V}\left(\hat{\tau}^{\text{dm}}\right)
&= 
\frac{n_c}{n n_t}S_1^2
+ \frac{n_t}{n n_c}S_0^2
+ \frac{2}{n}S_{0,1}
\\[6pt]
&\href{#lemma-6}{=}
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
\\[6pt]
&=
\frac{n_c + n_t}{n n_t} S_1^2
+ \frac{n_t + n_c}{n n_c} S_0^2
- \frac{S_{\tau_i}^2}{n}
&\text{}
\\[6pt]
&=
\frac{S_1^2}{n_t}
+ \frac{S_0^2}{n_c} 
- \frac{S_{\tau_i}^2}{n}
\end{align}
$${#eq-var}

This is the sampling variance of $\hat{\tau}^{\text{dm}}$. It's a theoretical quantity we cannot directly observe. 

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
It can be shown that the observed treatment group variances $s_t^2$ and $s_c^2$ are unbiased estimators of the sample variances $S_1^2$ and $S_0^2$ (see, for instance, Appendix A in Chapter 6 of @imbens2015causal). The last term in @eq-var is the variance of unit-level treatment effects, which is impossible to observe. As a result, the most widely used estimator in practice is:
$$
\hat{\mathbb{V}}
= \frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}.
$${#eq-var-est}

In our context, the main advantages of this estimator are:

1. If treatment effects are constant across units, then this is an unbiased estimator of the true sampling variance since in this case, $S^2_{\tau_i} = 0$.

2. If treatment effects are not constant, then this is a conservative estimator of the sampling variance (since $S_{\tau_i}^2$ is non-negative).

This is the estimator of the sampling variance of $\hat{\tau}^{\text{dm}}$ based on which we'll derive the standard derivation in the next chapter.

## Lemmas
### Lemma 1 {#lemma-1}
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
[↩️](#anchor-1)

### Lemma 2 {#lemma-2} 
This step uses [SUTVA](unbiasedness.md#sutva-links-observed-outcomes-to-potential-outcomes) which – if it holds – implies that:
$$
Y_i = Y_i(W_i) = \begin{cases} 
   Y_i(1) & \text{if } W_i = 1 \\
   Y_i(0)       & \text{if } W_i = 0
  \end{cases}
$$[↩️](#anchor-1)

### Lemma 3 {#lemma-3}
In @sec-experiments we define $W_i \sim \text{Bernoulli}\left(\frac{n_t}{n}\right)$. Hence, $W_i$ takes on values 0 (control group) or 1 (treatment group), which implies that:
$$
W_i^2 =
\begin{cases} 
1 & 
\text{if } W_i = 1\\[5pt]
0 & 
\text{if } W_i = 0\\[5pt]
\end{cases}
\qquad \implies
W_i^2 = W_i
\qquad \implies
\mathbb{V}(W_i^2) = \mathbb{V}(W_i).
$$
[↩️](#anchor-2)

### Lemma 4 {#lemma-4}
Given that $W_i \sim \text{Bernoulli} \left( \frac{n_t}{n} \right)$ (see @sec-experiments) we have:
$$
\begin{align}
\mathbb{V}(W_i) &= \left(\frac{n_t}{n}\right) \left(1-\frac{n_t}{n}\right) \\[5pt]
\mathbb{V}(W_i) &= \left(\frac{n_t}{n}\right) \left(\frac{n-n_t}{n}\right) \\[5pt]
\mathbb{V}(W_i) &= \left(\frac{n_t}{n}\right) \left(\frac{n_c}{n}\right) \\[5pt]
\mathbb{V}(W_i) &= \frac{n_tn_c}{n^2} \\[5pt]
\end{align}
$$
From the basic result that $\mathbb{V}(X + Y) = \mathbb{V}(X) + \mathbb{V}(Y) + 2\text{Cov}(X, Y)$, and the fact that symmetry implies that the variances and covariances of all $W_i$s are the same, we get:
$$
\begin{align}
\mathbb{V}\left(\sum_{i=1}^{n}W_i\right) 
&= \sum_{i=1}^{n}\mathbb{V}(W_i) + 2\sum_{i<j}\text{Cov}(W_i, W_j)
& \\[6pt]
\mathbb{V}\left(\sum_{i=1}^{n}W_i\right) 
&= n\mathbb{V}(W_i) + 2\frac{n(n-1)}{2}\text{Cov}(W_i, W_j)
& \text{symmetry} \\[6pt]
\mathbb{V}\left(n_t\right) 
&= n\mathbb{V}(W_i) + n(n-1)\text{Cov}(W_i, W_j)
& \text{Def of }n_t \\[6pt]
0 &= n\mathbb{V}(W_i) + n(n-1)\text{Cov}(W_i, W_j)
& n_t\text{ is constant} \\[6pt]
0 &= n\left(\frac{n_tn_c}{n^2}\right) + n(n-1)\text{Cov}(W_i, W_j)
& \text{Result for } \mathbb{V}(W_i) \\[6pt]
0 &= \frac{n_tn_c}{n} + n(n-1)\text{Cov}(W_i, W_j)
& \text{} \\[6pt]
\text{Cov}(W_i, W_j) &= -\frac{n_tn_c}{n^2(n-1)}& \text{}
\end{align}
$$
[↩️](#anchor-2)

### Lemma 5 {#lemma-5}
We use the fact that:
$$
-\sum_{i \neq j}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)^2
$$
To see why this is true, remember that that sum of demeaned variables is zero. Hence, $\sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+) = 0$. This implies that:
$$
\begin{align}
0 
&= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)
\sum_{j=1}^{n}(Y_j^+ - \overline{Y}^+) 
\\[6pt]
&= \sum_{i=1}^{n}\sum_{j=1}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
\\[6pt]
&= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)^2
+\sum_{i \neq j}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
\\[6pt]
-\sum_{i \neq j}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
&= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)^2.
\end{align}
$$
[↩️](#anchor-2)

### Lemma 6 {#lemma-6}
We use the fact that 
$$
2S_{0,1} = S^2_1 + S^2_0 - S^2_{\tau_i}
$$
This holds because:
$$
\begin{align}
S_{\tau_i}^2
&= \frac{1}{n-1}\sum_{i=1}^{n}
\left[
Y_i(1) - Y_i(0)
- \left(\overline{Y}(1) - \overline{Y}(0)\right)
\right]^2
\\[6pt]
&= \frac{1}{n-1}\sum_{i=1}^{n}
\left[
\left(Y_i(1) - \overline{Y}(1)\right)
- \left(Y_i(0) - \overline{Y}(0)\right)
\right]^2
\\[6pt]
&= \frac{1}{n-1}\sum_{i=1}^{n}
\Bigl[
\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \left(Y_i(0) - \overline{Y}(0)\right)^2
\\[6pt]
&\qquad
- \frac{2}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
\Bigr]
\\[6pt]
&= 
\frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(0) - \overline{Y}(0)\right)^2
\\[6pt]
&\qquad
- 2\frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
\\[6pt]
&= 
S^2_1 + S^2_0 - 2S_{0, 1}
\end{align}
$$
Rearranging gives the desired result.
[↩️](#anchor-3)


[^other]: The other main motivation was that I couldn't find a [step-by-step derivations of the sample size formula](sample_size.md).

[^approach]: @imbens2015causal provide an alternative derivation in Appendix B of Chapter 6 that is fairly detailed but still skips a lot of steps that weren't immediately obvious to me, and they define a helper variable I don't find to help that much. I find the approach in @ding2023first more transparent. But the original source skips a lot of steps and is thus not very accessible.