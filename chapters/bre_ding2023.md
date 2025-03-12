This note contains the proof for unbiasedness and the derivation of the variance for the difference-in-means estimator for a BRE, based on the ding2023first approach for a CRE.


Next:
- Pull all I have so far together so nothing gets lost
- Then go further, starting with Deng discussion on randomised experiments

Story:
- Online experiments are BREs
- Yet, at point of analysis, sample sizes are fixed and we can analyse as CREs
- See chat discussion
	- See here: https://alexdeng.github.io/causal/randomintro.html (exactly what I want)

- Generally, literature uses a potential outcome framework 
	- @larsen2023statistical
- Some vendors/literature assume fixed sample sizes
	- @nordin2024precision

- Others use an iid framework for motivation
	- zhou2023all

- Also discuss why not just use iid approach
- Write draft and run past colleagues









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


# Approach 1: condition on $n_t$ and $n_c$

Treat $n_t$ as fixed, in which case $q=\frac{n_t}{n}$. This should be equivalent to a BRE, though I wanna check. Why do that?
- It makes the math easier as it spares us from modelling $n_t$ as a Binomial random variable -- it seems that in the Bernoulli case, the variance isn't finite, while in the CRE case, it is (that's the case everyone works with).
- Second, by the time we analyse the data, we do know $n$, so the assumption isn't completely arbitrary

Notes:
- In some cases, I explicitly condition also on potential outcomes, which we take as fixed. Mention somewhere that I don't usually condition on them to lighten notation.


### Building blocks

### Lemma B1:

$$
\mathbb{E}[W \;|\; \{Y_i(1), Y_i(0)\}_{i=1}^{n}, \;n_t] = \frac{n_t}{n}
$$
Proof:

By assumption, we have:
$$
P(W_i=1 \;|\; \{Y_i(1), Y_i(0)\}_{i=1}^{n}, \;n_t) = \frac{n_t}{n}, 
\qquad i=1, \dots, n.
$$
Also, $W_i$ is Bernoulli and hence either 0 or 1, which means that:
$$
\begin{align}
\mathbb{E}[W \;|\; \{Y_i(1), Y_i(0)\}_{i=1}^{n}, \;n_t] 

&= 
1 \times  
P(W_i=1 \;|\; \{Y_i(1), Y_i(0)\}_{i=1}^{n}, \;n_t)
\\[5pt]
&\quad + 
0 \times
P(W_i=0 \;|\; \{Y_i(1), Y_i(0)\}_{i=1}^{n}, \;n_t)
\\[5pt]

&=   
P(W_i=1 \;|\; \{Y_i(1), Y_i(0)\}_{i=1}^{n}, \;n_t)
\\[5pt]

&=
\frac{n_t}{n}
\end{align}
$$

#### Lemma 1 {#sec-lemma1}

$\mathbb{E}[W_i^2] = \mathbb{E}[W_i] = \frac{n_t}{n}$

**Proof:**

Given that $W_i \sim \text{Bern} \left( \frac{n_t}{n} \right)$, $W_i \in \{0, 1\}$, which means that:

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
$$

Hence:

$\mathbb{E}[W_i^2] = \mathbb{E}[W_i] = \frac{n_t}{n}$.


#### Lemma 2 {#sec-lemma2}

$\mathbb{E}[W_i W_j] = \frac{n_t}{n}\frac{n_t-1}{n-1}.$

**Proof:**

Because $W_i$ is an indicator variable for which $E[W_i] = P(W_i = 1)$ and similar for $W_j$, we also have $E[W_i W_j] = P(W_i = 1, W_j = 1)$. 

Hence, what we are looking for is the joint probability of selecting both units $i$ and $j$ as part of a sample of size $n_t$ from a finite population of size $n$. In general, this is given by:

$$
P(W_i = 1, W_j = 1) 
= \frac{\text{Number of ways to choose a sample of size } n_t \text{ containing }i, j}{\text{Number of ways to choose any sample of size }n_t}
$$

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

Hence:

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

A more heuristic way to get the same result:

1. The probability of selecting $i$ is $P(W_i = 1) = \frac{n_t}{n}$
2. The probability of selecting $j$ given that $i$ is selected is $P(W_j = 1 | W_i = 1) = \frac{n_t - 1}{n-1}$
3. Using the multiplication rule for joint probabilities we have

$$
P(W_i = 1, W_j = 1) = P(W_i=1)P(W_j=1|W_j=1)=\frac{n_t}{n}\frac{n_t-1}{n-1}.
$$


#### Lemma 3 {#sec-lemma3}

These results give the first two moments of the assignment indicator $W_i$ and are based on Lemma C.2. in @ding2023first.

Under a CRE we have:

$$
\mathbb{E}(W_i) = \frac{n_t}{n}, \qquad 
\mathbb{E}(1-W_i) = \frac{n_t}{n}, \qquad
\mathbb{V}(W_i) = \frac{n_t n_c}{n^2}, \qquad 
\text{Cov}(W_i, W_j) = -\frac{n_t n_c}{n^2(n-1)}
$$
**Proof:**

$$
\begin{align}
n_t &= \sum_{i=1}^{n}W_i &\\[5pt]
\mathbb{E}[n_t] &= \mathbb{E}\left[\sum_{i=1}^{n}W_i\right] &\\[5pt]
\mathbb{E}[n_t] &= n\mathbb{E}[W_i] & \text{symmetry of }W_is\\[5pt]
n_t &= n\mathbb{E}[W_i] & n_t\text{ is constant}\\[5pt]
\mathbb{E}[W_i] &= \frac{n_t}{n} & \\[5pt]
\end{align}
$$
and
$$
\begin{align}
\mathbb{E}(1-W_i)
&= \mathbb{E}(1) - \mathbb{E}(W_i) \\[5pt]
&= 1 - \frac{n_t}{n} \\[5pt]
&= \frac{n-n_t}{n} \\[5pt]
&= \frac{n_c}{n} \\[5pt]
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

From the basic result that $\mathbb{V}(X + Y) = \mathbb{V}(X) + \mathbb{V}(Y) + 2\text{Cov}(X, Y)$, and the fact that symmetry implies that the variances and covariances of all $W_i$s are the same, we get:

$$
\begin{align}
\mathbb{V}\left(\sum_{i=1}^{n}W_i\right) 
&= \sum_{i=1}^{n}\mathbb{V}(W_i) + 2\sum_{i<j}^{n}\text{Cov}(W_i, W_j)
& \\[5pt]

\mathbb{V}\left(\sum_{i=1}^{n}W_i\right) 
&= n\mathbb{V}(W_i) + 2\frac{n(n-1)}{2}\text{Cov}(W_i, W_j)
& \text{symmetry} \\[5pt]

\mathbb{V}\left(n_t\right) 
&= n\mathbb{V}(W_i) + 2\frac{n(n-1)}{2}\text{Cov}(W_i, W_j)
& \text{Def of }n_t \\[5pt]

0 &= n\mathbb{V}(W_i) + 2\frac{n(n-1)}{2}\text{Cov}(W_i, W_j)
& n_t\text{ is constant} \\[5pt]

0 &= n\left(\frac{n_tn_c}{n^2}\right) + 2\frac{n(n-1)}{2}\text{Cov}(W_i, W_j)
& \text{Result for } \mathbb{V}(W_i) \\[5pt]

0 &= \left(\frac{n_tn_c}{n}\right) + n(n-1)\text{Cov}(W_i, W_j)
& \text{} \\[5pt]

\text{Cov}(W_i, W_j) &= -\frac{n_tn_c}{n^2(n-1)}& \text{} \\[5pt]

\end{align}
$$

#### Lemma 4 {#sec-lemma4}

$$
-\sum_{i=1}^{n}\sum_{j \neq i}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)^2
$$
**Proof:**

The sum of demeaned variables is zero. Hence, $\sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+) = 0$, which implies that:

$$
\begin{align}
0 
&= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)
\sum_{j=1}^{n}(Y_j^+ - \overline{Y}^+) 
\\[5pt]

&= \sum_{i=1}^{n}\sum_{j=1}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
\\[5pt]

&= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)^2
+\sum_{i=1}^{n}\sum_{j \neq i}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
\\[5pt]

-\sum_{i=1}^{n}\sum_{j \neq i}^{n}
(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+)
&= \sum_{i=1}^{n}(Y_i^+ - \overline{Y}^+)^2
\end{align}
$$
#### Lemma 5 {#sec-lemma5}

This lemma is lemma 4.1 in @ding2023first, and states the relationship between variances and covariance of potential outcomes:
$$
2S_{0,1} = S^2_1 + S^2_0 - S^2_{\tau_i}
$$
**Proof:**

$$
\begin{align}
S_{\tau_i}^2

&= \frac{1}{n-1}\sum_{i=1}^{n}
\left(
Y_i(1) - Y_i(0)
- \left(\overline{Y}(1) - \overline{Y}(0)\right)
\right)^2
\\[5pt]

&= \frac{1}{n-1}\sum_{i=1}^{n}
\left(
\left(Y_i(1) - \overline{Y}(1)\right)
- \left(Y_i(0) - \overline{Y}(0)\right)
\right)^2
\\[5pt]

&= \frac{1}{n-1}\sum_{i=1}^{n}
\left(
\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \left(Y_i(0) - \overline{Y}(0)\right)^2
- 2\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
\right)
\\[5pt]

&= 
\frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)^2
+ \frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(0) - \overline{Y}(0)\right)^2
- 2\frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)\left(Y_i(0) - \overline{Y}(0)\right)
\\[5pt]

&= 
S^2_1 + S^2_0 - 2S_{0, 1}
\\[5pt]

2S_{0, 1} &= S^2_1 + S^2_0 - S^2_{\tau_i}
\end{align}
$$


### Unbiasedness of $\hat{\tau}^{\text{dm}}$

To show unbiasedness, we need to show that $\mathbb{E}[{\hat{\tau}^{\text{dm}}}] = \tau$.

Note: we implicitly condition the expectation here on potential outcomes and on $n_t$.

$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\right]

&=
\mathbb{E}\left[
\overline{Y}_t - \overline{Y}_c
\right]
&\text{}
\\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^n W_iY_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\right]
&\text{}
\\[5pt]

&\text{SUTVA}
\\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^n W_iY_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i(0)
\right]
&\text{}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n \mathbb{E}[W_i]Y_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n \mathbb{E}[(1-W_i)]Y_i(0)
&\text{}
\\[5pt]

&\text{Lemma 3}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n \left(\frac{n_t}{n}\right)Y_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n \left(\frac{n_c}{n}\right)Y_i(0)
\\[5pt]

&=
\frac{1}{n}\sum_{i=1}^n Y_i(1) 
- \frac{1}{n}\sum_{i=1}^n Y_i(0)
&\text{}
\\[5pt]

&=
\overline{Y}(1) - \overline{Y}(0)
&\text{}
\\[5pt]

&=
\tau
\qquad\square
&\text{}
\\[5pt]
\end{align}
$$

### Variance of of $\hat{\tau}^{\text{dm}}$

$$
\begin{align}

\mathbb{V}\left(
\hat{\tau}^{\text{dm}}
\right)

&=
\mathbb{V}\left(
\overline{Y}_t - \overline{Y}_c
\right)
\\[5pt]

&=
\mathbb{V}\left(
\frac{1}{n_t}\sum_{i=1}^n W_iY_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
\right)
\\[5pt]

&\text{SUTVA}
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
\sum_{i=1}^n W_i \left(\frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c}\right)
- \frac{1}{n_c}\sum_{i=1}^n Y_i(0)
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

&\text{Substituting from Lemma 4}
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
+ \frac{2}{n}S_{0,1}
&\text{}
\\[5pt]

&\text{Using Lemma 5}
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
$$



## Approach 2: treat $n_t$ as Bernoulli

See problem 4.7 in ding


## Useful notation

$$
\{Y_i(1), Y_i(0)\}_{i=1}^{n}
$$