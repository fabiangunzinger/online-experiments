
# Lemmas {#sec-lemmas}

In math, lemmas are proofs of intermediary results. Below the ones I use in the text. Most of them are based on @ding2023first.


## Lemma 2 {#sec-lemma2}


## Lemma 3 {#sec-lemma3}


## Lemma 4 {#sec-lemma4}

## Lemma 5 {#sec-lemma5}





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

