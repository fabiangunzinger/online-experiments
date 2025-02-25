
Lemmas in appendix C proof useful results. The discussion is based on a simple random sample, yet is then used in the main text for the proofs of Neyman's theorem in the case of CRE.

## Context

A finite population of $n$ units with associated values $\{c_1, ..., c_n\}$, and $\{d_1, ..., d_n\}$, with

$$
\begin{align}
\mu_c = \frac{1}{n}\sum_{i=1}^{n}c_i, \qquad
\sigma_c^2 = \frac{1}{n-1}\sum_{i=1}^{n}(c_i - \mu_c)^2 \\[5pt]

\mu_d = \frac{1}{n}\sum_{i=1}^{n}d_i, \qquad
\sigma_d^2 = \frac{1}{n-1}\sum_{i=1}^{n}(d_i - \mu_d)^2 \\[5pt]
\end{align}
$$
and covariance:

$$
\sigma_{cd} = \frac{1}{n-1}\sum_{i=1}^{n}(c_i - \mu_c)(d_i - \mu_d).
$$
Under simple random sampling with inclusion indicator $W_i$, the sample means for a sample of size $n_t$ are:

$$
\begin{align}
\bar{c} = \frac{1}{n_t}\sum_{i=1}^{n}W_i c_i, \qquad
\hat{\sigma}_c^2 = \frac{1}{n_t-1}\sum_{i=1}^{n} W_i (c_i - \bar{c})^2 \\[5pt]

\bar{d} = \frac{1}{n_t}\sum_{i=1}^{n}W_i d_i, \qquad
\hat{\sigma}_d^2 = \frac{1}{n_t-1}\sum_{i=1}^{n} W_i (d_i - \bar{d})^2 \\[5pt]
\end{align}
$$
and covariance:

$$
\hat{\sigma}_{cd} = \frac{1}{n_t-1}\sum_{i=1}^{n} W_i (c_i - \bar{c})(d_i - \bar{d}).
$$

## Lemma C.1

Gives first two moments of sampling indicator $W_i$.

Under SRS we have:

$$
\mathbb{E}(W_i) = \frac{n_t}{n}, \qquad \mathbb{V}(W_i) = \frac{n_t n_c}{n^2}, \qquad Cov(W_i, W_j) = -\frac{n_t n_c}{n^2(n-1)}
$$
Proof:

$$
\begin{align}
n_t &= \sum_{i=1}^{n}W_i &\\[5pt]
\mathbb{E}[n_t] &= \mathbb{E}\left[\sum_{i=1}^{n}W_i\right] &\\[5pt]
\mathbb{E}[n_t] &= n\mathbb{E}[W_i] & \text{symmetry of }W_is\\[5pt]
n_t &= n\mathbb{E}[W_i] & n_t\text{ is constant}\\[5pt]
\mathbb{E}[W_i] &= \frac{n_t}{n} & \\[5pt]
\end{align}
$$


Given that 
$$
W_i \sim \text{Bern} \left( \frac{n_t}{n} \right),
$$

$$
\begin{align}
\mathbb{V}(W_i) &= \left(\frac{n_t}{n}\right) \left(1-\frac{n_t}{n}\right) \\[5pt]
\mathbb{V}(W_i) &= \left(\frac{n_t}{n}\right) \left(\frac{n-n_t}{n}\right) \\[5pt]
\mathbb{V}(W_i) &= \left(\frac{n_t}{n}\right) \left(\frac{n_c}{n}\right) \\[5pt]
\mathbb{V}(W_i) &= \frac{n_tn_c}{n^2} \\[5pt]
\end{align}
$$
Finally:

From the basic result that $\mathbb{V}(X + Y) = \mathbb{V}(X) + \mathbb{V}(Y) + 2Cov(X, Y)$, and the fact that symmetry implies that the variances and covariances of all $W_i$s are the same, we get:

$$
\begin{align}
\mathbb{V}\left(\sum_{i=1}^{n}W_i\right) 
&= \sum_{i=1}^{n}\mathbb{V}(W_i) + 2\sum_{i<j}^{n}Cov(W_i, W_j)
& \\[5pt]

\mathbb{V}\left(\sum_{i=1}^{n}W_i\right) 
&= n\mathbb{V}(W_i) + 2\frac{n(n-1)}{2}Cov(W_i, W_j)
& \text{symmetry} \\[5pt]

\mathbb{V}\left(n_t\right) 
&= n\mathbb{V}(W_i) + 2\frac{n(n-1)}{2}Cov(W_i, W_j)
& \text{Def of }n_t \\[5pt]

0 &= n\mathbb{V}(W_i) + 2\frac{n(n-1)}{2}Cov(W_i, W_j)
& n_t\text{ is constant} \\[5pt]

0 &= n\left(\frac{n_tn_c}{n^2}\right) + 2\frac{n(n-1)}{2}Cov(W_i, W_j)
& \text{Result for } \mathbb{V}(W_i) \\[5pt]

0 &= \left(\frac{n_tn_c}{n}\right) + n(n-1)Cov(W_i, W_j)
& \text{} \\[5pt]

Cov(W_i, W_j) &= -\frac{n_tn_c}{n^2(n-1)}& \text{} \\[5pt]

\end{align}
$$


## Lemma C.2

Gives the first two moments of the sample means.

Claim:

The sample means have expectations
$$
\mathbb{E}[\bar{c}] = \mu_c \qquad \mathbb{E}[\bar{d}] = \mu_d,
$$
variances
$$
\mathbb{V}(\bar{c}) = \frac{n_c}{n n_t}\sigma_c^2
\qquad
\mathbb{V}(\bar{d}) = \frac{n_c}{n n_t}\sigma_d^2,
$$
and covariance
$$
Cov(\bar{c},\bar{d}) = \frac{n_c}{n n_t}\sigma_{cd}.
$$
Proof:

The expectations follow from the linearity of the expectation operator and the expectation of the inclusion operator:

$$
\begin{align}
\mathbb{E}[\bar{c}] 
&= \mathbb{E}\left[\frac{1}{n_t}\sum_{i=1}^{n}W_i c_i\right]
&\text{}
\\[5pt]
&= \frac{1}{n_t}\sum_{i=1}^{n}\mathbb{E}[W_i] c_i
&\text{Linearity of }\mathbb{E}
\\[5pt]
&= \frac{1}{n_t}\sum_{i=1}^{n}\frac{n_t}{n} c_i
&\text{}
\\[5pt]
&= \frac{1}{n}\sum_{i=1}^{n} c_i
&\text{}
\\[5pt]
&=\mu_c
\end{align}
$$


The covariance 

$$
\begin{align}
Cov(\bar{c}, \bar{d})

&=\mathbb{E}\left[\bar{c}\bar{d}\right] - \mathbb{E}[\bar{c}]\mathbb{E}[\bar{d}]
&\text{Definition of covariance}
\\[5pt]

&=\mathbb{E}\left[\left(\frac{1}{n_t}\sum_{i=1}^{n}W_i c_i\right)\left(\frac{1}{n_t}\sum_{j=1}^{n}W_j d_j\right)\right] 
- \mu_c\mu_d
&\text{}
\\[5pt]

&=\mathbb{E}\left[\frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{j=1}^{n}W_i W_j c_id_j\right] 
- \mu_c\mu_d
&\text{}
\\[5pt]

&=\mathbb{E}\left[
\frac{1}{n_t^2}\sum_{i=1}^{n}W_i^2 c_id_i
+ \frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{i \neq j}W_i W_j c_id_j
\right] 
- \mu_c\mu_d
&\text{}
\\[5pt]

&=\frac{1}{n_t^2}\sum_{i=1}^{n}\mathbb{E}\left[W_i^2\right] c_id_i
+ \frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{i \neq j}\mathbb{E}\left[W_i W_j\right] c_id_j
- \mu_c\mu_d
&\text{}
\\[5pt]

&=\frac{1}{n_t^2}\sum_{i=1}^{n}\mathbb{E}\left[W_i^2\right] c_id_i
+ \frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{i \neq j}\mathbb{E}\left[W_i W_j\right] c_id_j
- \mu_c\mu_d
&\text{}
\\[5pt]


\end{align}
$$


** I'm here **
- Need to know above expectations –




## Lemma C.3

Gives the first moment of the sample variances and the covariance.


## Lemma C.4

Justifies the use of Wald-type confidence intervals.