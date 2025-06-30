
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

We now begin the derivation, which uses intermediary results labeled *lemmas*; their derivations appear at the bottom of the page and are linked from inside the main derivation.

To keep the notation simpler, I omit explicit conditioning on $\mathbf{n}$ and $\mathbf{Y(w)}$, though the conditioning is implicit throughout. I also define throughout
$$
Y_i^+ \coloneqq \frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c}, 
\quad \overline{Y}^+ \coloneqq \frac{\overline{Y}(1)}{n_t} + \frac{\overline{Y}(0)}{n_c}.
$$

Starting from the definition of our difference-in-means estimator, we get:

<!-- Jump back up anchor below -->
<a id="main-derivation"></a>
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

Expressing the variance in terms of the covariance we get:
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
Splitting the double sum into cases where $i=j$ and where $j \neq j$ we get:


**I'm here**




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
[↩️](#main-derivation)

### Lemma 2 {#lemma-2} 
This step uses [SUTVA](unbiasedness.md#sutva-links-observed-outcomes-to-potential-outcomes) which – if it holds – implies that:
$$
Y_i = Y_i(W_i) = \begin{cases} 
   Y_i(1) & \text{if } W_i = 1 \\
   Y_i(0)       & \text{if } W_i = 0
  \end{cases}
$$[↩️](#main-derivation)

### Lemma 3 {#lemma-3}

Step-by-step, we have:
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
[↩️](#main-derivation)





[^other]: The other main motivation was that I couldn't find a [step-by-step derivations of the sample size formula](power.md).

[^approach]: @imbens2015causal provide an alternative derivation in Appendix B of Chapter 6 that is fairly detailed but still skips a lot of steps that weren't immediately obvious to me, and they define a helper variable I don't find to help that much. I find the approach in @ding2023first more transparent. But the original source skips a lot of steps and is thus not very accessible.