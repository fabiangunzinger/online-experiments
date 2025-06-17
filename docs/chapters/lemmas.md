
# Lemmas {#sec-lemmas}

In math, lemmas are proofs of intermediary results. Below the ones I use in the text. Most of them are based on @ding2023first.

## Lemma 1 {#sec-lemma1}

Given that $W_i \in \{0, 1\} \sim \text{Bernoulli}(q)$, and given that we take potential outcomes $\mathbf{Y(w)} = (\mathbf{Y(1)}, \mathbf{Y(0)})$ and sample sizes $\mathbf{n} = (n, n_t, n_c)$ as given, we have:
$$
\mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}] 
=
\frac{n_t}{n}
$$

and

$$
\mathbb{E}[1-W_i\>|\>\mathbf{n}, \mathbf{Y(w)}] 
=
\frac{n_c}{n}.
$$
**Proof:**

Given that $W_i \in \{0, 1\} \sim \text{Bernoulli}(q)$ we have:
$$
P(W_i = 1 \>|\>\mathbf{n}, \mathbf{Y(w)}) 
=
\frac{n_t}{n}.
$$Hence:
$$
\begin{align}
\mathbb{E}&[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}] 
\\[5pt]

&=
1 \times P(W_i = 1 \>|\>\mathbf{n}, \mathbf{Y(w)})
+ 0 \times P(W_i = 0 \>|\>\mathbf{n}, \mathbf{Y(w)})
\\[5pt]

&=
P(W_i = 1 \>|\>\mathbf{n}, \mathbf{Y(w)})
\\[5pt]

&= \frac{n_t}{n}
\end{align}
$$
and
$$
\begin{align}
\mathbb{E}&[(1-W_i)\>|\>\mathbf{n}, \mathbf{Y(w)}] 
\\[5pt]

&=
1 - \mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]
\\[5pt]

&=
1 - \frac{n_t}{n}
\\[5pt]

&=
\frac{n - n_t}{n}
\\[5pt]

&=
\frac{n_c}{n}
\qquad\square
\end{align}
$$

## Lemma 2 {#sec-lemma2}

Given that $W_i \sim \text{Bernoulli} \left( \frac{n_t}{n} \right)$, we have:

$\mathbb{V}(W_i^2) = \mathbb{V}(W_i)$.

**Proof:**

Bernoulli distributed random variables take on either values 0 or 1, so $W_i \in \{0, 1\}$, which implies that:
$$
W_i^2 =
\begin{cases} 
1 & 
\text{if } W_i = 1\\[5pt]
0 & 
\text{if } W_i = 0\\[5pt]
\end{cases}
\qquad \implies
W_i^2 = W_i.
$$

Hence:

$\mathbb{V}(W_i^2) = \mathbb{V}(W_i)\qquad\square$

## Lemma 3 {#sec-lemma3}

$$
\mathbb{V}(W_i) = \frac{n_t n_c}{n^2}, \qquad 
\text{Cov}(W_i, W_j) = -\frac{n_t n_c}{n^2(n-1)}
$$
**Proof:**

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

\text{Cov}(W_i, W_j) &= -\frac{n_tn_c}{n^2(n-1)}& \text{}
\qquad\square
\end{align}
$$

## Lemma 4 {#sec-lemma4}

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
## Lemma 5 {#sec-lemma5}

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



# Old lemmas
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

