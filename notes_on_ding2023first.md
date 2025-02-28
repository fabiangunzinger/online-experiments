
Lemmas in appendix C proof useful results. The discussion is based on a simple random sample, yet is then used in the main text for the proofs of Neyman's theorem in the case of CRE.


## Applying ding notes to Neyman proof

Move this section to main chapter once It's complete.

Path to victory
- [ ] Write out proof of lemma C.2 from Moleskin
- [ ] Write down proof of Lemma 4.1 which is important
- [ ] Write out definitions for means and variances for neyman problem (use notes form imbens)
- [ ] Then use chat notes on lemma 2 ([here](https://chatgpt.com/share/67c14491-afb4-8005-a06c-b6634c6ab20f)) to apply to Neyman problem. Basic approach:
	- [ ] Start with var tau = var y1 bar + var y2 bar - 2cov y1bar, y2bar
	- [ ] I have these from lemma c2
	- [ ] Use relationship between cov and var tau_i from lemma 4.1 and substitute










## Notation

To be closer to my own notation, I make the following changes to the notation in the text:

- $n_1, n_o = n_t, n_c$
- $\bar{x} = \mu_x$
- $\hat{\bar{x}} = \bar{x}$
- $S_x^2 = \sigma_x^2$
- $S_{xy} = \sigma_{xy}$
- $\hat{S}_x^2 = \hat{\sigma}_x^2$

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


## Lemma F.1

These results are skipped over in Ding but are useful for a complete understanding of the proofs below.

We have 

$\mathbb{E}[W_i^2] = \mathbb{E}[W_i] = \frac{n_t}{n}$

and

$\mathbb{E}[W_i W_j] = \frac{n_t}{n}\frac{n_t-1}{n-1}.$

Proof:

Given that $W_i \sim \text{Bern} \left( \frac{n_t}{n} \right)$, which implies that $W_i \in \{0, 1\}$, we have:

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
$$Hence:

$\mathbb{E}[W_i^2] = \mathbb{E}[W_i] = \frac{n_t}{n}$.

Because $W_i$ is an indicator variable for which $E[W_i] = P(W_i = 1)$ and similar for $W_j$, we also have $E[W_i W_j] = P(W_i = 1, W_j = 1)$. 

Hence, what we are looking for is the joint probability of selecting both units $i$ and $j$ as part of a sample of size $n_t$ from a finite population of size $n$.

Number of ways to choose a sample of size $n_t$ from a finite population of size $n$:

$$
\left(
    \begin{array}{c}
      n \\
      n_t
    \end{array}
  \right) = \frac{n!}{n_t!(n-n_t)!}.
$$

Number of ways to choose a sample of size $n_t$ that contains both $i$ and $j$:
$$
\left(
    \begin{array}{c}
      n-2 \\
      n_t-2
    \end{array}
  \right) 
  = \frac{(n-2)!}{(n_t-2)!((n-2)-(n_t-2))!}
  = \frac{(n-2)!}{(n_t-2)!(n-n_t)!}.
$$

In general, the probability units $i$ and $j$ both being part of a sample of size $n_t$ is:

$$
P(W_i = 1, W_j = 1) 
= \frac{\text{Number of ways to choose a sample of size } n_t \text{ containing }i, j}{\text{Number of ways to choose any sample of size }n_t}
$$

Hence, we have
$$
\begin{align}
P(W_i = 1, W_j = 1) 
&= \frac{
\left(
	\begin{array}{c}
	  n-2 \\
	  n_t-2
	\end{array}
\right)}
{\left(
	\begin{array}{c}
	  n \\
	  n_t
	\end{array}
\right)} \\[5pt]
&= \frac{\frac{(n-2)!}{(n_t-2)!(n-n_t)!}}{\frac{n!}{n_t!(n-n_t)!}} \\[5pt]
& = \frac{(n-2)!}{(n_t-2)!}\frac{n_t!}{n!} \\[5pt]
& = \frac{(n-2)!}{(n_t-2)!}\frac{n_t(n_t-1)(n_t-2)!}{n(n-1)(n-2)!} \\[5pt]
& = \frac{n_t(n_t-1)}{n(n-1)}.
\end{align}
$$
There is also a more heuristic way to get the same result:

1. The probability of selecting $i$ is $P(W_i = 1) = \frac{n_t}{n}$
2. The probability of selecting $j$ given that $i$ is selected is $P(W_j = 1 | W_i = 1) = \frac{n_t - 1}{n-1}$
3. Using the multiplication rule for joint probabilities we have
$$
P(W_i = 1, W_j = 1) = P(W_i=1)P(W_j=1|W_j=1)=\frac{n_t}{n}\frac{n_t-1}{n-1}.
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

&=Cov\left[
\left(\frac{1}{n_t}\sum_{i=1}^{n}W_i c_i\right),
\left(\frac{1}{n_t}\sum_{j=1}^{n}W_j d_j\right)
\right]
&\text{}
\\[5pt]

&=Cov\left[
\left(\frac{1}{n_t}\sum_{i=1}^{n}W_i (c_i - \mu_c)\right),
\left(\frac{1}{n_t}\sum_{j=1}^{n}W_j (d_j - \mu_d)\right)
\right]
&\text{Invariance of addition}
\\[5pt]

&=
\frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{j=1}^{n}Cov\big(W_i(c_i - \mu_c), W_j(d_j - \mu_d)\big)
&\text{Bilinearity of } Cov
\\[5pt]

&=
\frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{j=1}^{n}Cov\big(W_i, W_j\big)(c_i - \mu_c)(d_j - \mu_d)
&\text{Multiplicative property of }Cov
\\[5pt]

&=
\frac{1}{n_t^2}\left[
\sum_{i=1}^{n}\mathbb{V}(W_i^2)(c_i - \mu_c)(d_j - \mu_d)
+ \sum_{i=1}^{n}\sum_{j \neq i}Cov(W_i, W_j)(c_i - \mu_c)(d_j - \mu_d)
\right]
&\text{Separating cases}
\\[5pt]

&=
\frac{1}{n_t^2}\left[
\sum_{i=1}^{n}\mathbb{V}(W_i)(c_i - \mu_c)(d_j - \mu_d)
+ \sum_{i=1}^{n}\sum_{j \neq i}Cov(W_i, W_j)(c_i - \mu_c)(d_j - \mu_d)
\right]
&\mathbb{V}(W_i^2) = \mathbb{V}(W_i)
\\[5pt]

&=
\frac{1}{n_t^2}\left[
\sum_{i=1}^{n}\left(\frac{n_tn_c}{n^2}\right)(c_i - \mu_c)(d_j - \mu_d)
- \sum_{i=1}^{n}\sum_{j \neq i}\left(\frac{n_tn_c}{n^2(n-1)}\right)(c_i - \mu_c)(d_j - \mu_d)
\right]
&\mathbb{V}(W_i^2) = \mathbb{V}(W_i)
\\[5pt]

&=
\frac{1}{n_t^2}\left[
\left(\frac{n_tn_c}{n^2}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
- \left(\frac{n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}\sum_{j \neq i}(c_i - \mu_c)(d_j - \mu_d)
\right]
&
\\[5pt]
\end{align}
$$

The sum of demeaned variables is zero, so that $\sum_{i=1}^{n}(c_i - \mu_c) = 0$ and  $\sum_{i=1}^{n}(d_i - \mu_d) = 0$. Hence:

$$
\begin{align}
0 
&= \sum_{i=1}^{n}(c_i - \mu_c)\sum_{j=1}^{n}(d_j - \mu_d) \\[5pt]
&= \sum_{i=1}^{n}\sum_{j=1}^{n}(c_i - \mu_c)(d_j - \mu_d) \\[5pt]
&= \sum_{i=1}^{n}(c_i - \mu_c)(d_i - \mu_d)
+ \sum_{i=1}^{n}\sum_{j \neq i}(c_i - \mu_c)(d_j - \mu_d) \\[5pt]
\sum_{i=1}^{n}\sum_{j \neq i}(c_i - \mu_c)(d_j - \mu_d) &= 
-\sum_{i=1}^{n}(c_i - \mu_c)(d_i - \mu_d)
\end{align}
$$

Substituting this in the above equation we get:

$$
\begin{align}
Cov(\bar{c}, \bar{d})

&=
\frac{1}{n_t^2}\left[
\left(\frac{n_tn_c}{n^2}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
- \left(\frac{n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}\sum_{j \neq i}(c_i - \mu_c)(d_j - \mu_d)
\right]
&\\[5pt]

&=
\frac{1}{n_t^2}\left[
\left(\frac{n_tn_c}{n^2}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
- \left(\frac{n_tn_c}{n^2(n-1)}\right)\left(-\sum_{i=1}^{n}(c_i - \mu_c)(d_i - \mu_d)\right)
\right]
&\\[5pt]

&=
\frac{1}{n_t^2}\left[
\left(\frac{n_tn_c}{n^2}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
+ \left(\frac{n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_i - \mu_d)
\right]
&\\[5pt]

&=
\frac{1}{n_t^2}
\left(\frac{n_tn_c}{n^2} + \frac{n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)

&\\[5pt]

&=
\frac{1}{n_t^2}
\left(\frac{n_tn_c(n-1) + n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
&\\[5pt]

&=
\frac{1}{n_t^2}
\left(\frac{nn_tn_c - n_tn_c + n_tn_c}{n^2(n-1)}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
&\\[5pt]

&=
\frac{1}{n_t^2}
\left(\frac{n_tn_c}{n(n-1)}\right)\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
&\\[5pt]

&=
\frac{n_c}{n_tn}\frac{1}{n-1}\sum_{i=1}^{n}(c_i - \mu_c)(d_j - \mu_d)
&\\[5pt]

&=
\frac{n_c}{n_tn}\sigma_{cd}
&\\[5pt]
\end{align}
$$


**I'm here**
- Complete path to victory above

## Lemma C.3

Gives the first moment of the sample variances and the covariance.


## Lemma C.4

Justifies the use of Wald-type confidence intervals.




**Backup**
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
+ \frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{j \neq i}W_i W_j c_id_j
\right] 
- \mu_c\mu_d
&\text{}
\\[5pt]

&=\frac{1}{n_t^2}\sum_{i=1}^{n}\mathbb{E}\left[W_i^2\right] c_id_i
+ \frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{j \neq i}\mathbb{E}\left[W_i W_j\right] c_id_j
- \mu_c\mu_d
&\text{}
\\[5pt]

&=\frac{1}{n_t^2}\sum_{i=1}^{n}\left(\frac{n_t}{n}\right) c_id_i
+ \frac{1}{n_t^2}\sum_{i=1}^{n}\sum_{j \neq i}\left(\frac{n_t(n_t-1)}{n(n-1)}\right) c_id_j
- \mu_c\mu_d
&\text{}
\\[5pt]

\end{align}
$$



