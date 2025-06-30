## Variance Derivation

We now derive the variance of the difference-in-means estimator. Throughout, we define:

$$
Y_i^+ \coloneqq \frac{Y_i(1)}{n_t} + \frac{Y_i(0)}{n_c}, \quad 
\overline{Y}^+ \coloneqq \frac{\overline{Y}(1)}{n_t} + \frac{\overline{Y}(0)}{n_c}, \quad
c \coloneqq \frac{n_t n_c}{n(n-1)}.
$$

---

$$
\begin{align}
\mathbb{V}\left(\hat{\tau}^{\text{dm}}\right)
&= \mathbb{V}\left(\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i\right) \\[6pt]
&\href{#lemma-1}{=} \mathbb{V}\left(\frac{1}{n_t}\sum_{i=1}^n W_i Y_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i) Y_i\right) \\[6pt]
&\href{#lemma-2}{=} \mathbb{V}\left(\frac{1}{n_t}\sum_{i=1}^n W_i Y_i(1) - \frac{1}{n_c}\sum_{i=1}^n (1-W_i) Y_i(0)\right) \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i \frac{Y_i(1)}{n_t} + W_i \frac{Y_i(0)}{n_c} - \frac{Y_i(0)}{n_c}\right) \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i Y_i^+ - \sum_{i=1}^n \frac{Y_i(0)}{n_c}\right) \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i Y_i^+\right) \quad \text{(drop const)} \\[6pt]
&= \mathbb{V}\left(\sum_{i=1}^n W_i (Y_i^+ - \overline{Y}^+)\right) \quad \text{(demean)} \\[6pt]
&= \sum_{i=1}^n \sum_{j=1}^n \text{Cov}(W_i, W_j) (Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+) \\[6pt]
&= \sum_i \mathbb{V}(W_i)(Y_i^+ - \overline{Y}^+)^2 + \sum_{i \neq j} \text{Cov}(W_i, W_j)(Y_i^+ - \overline{Y}^+)(Y_j^+ - \overline{Y}^+) \\[6pt]
&\href{#lemma-3}{=} c \sum_{i=1}^n (Y_i^+ - \overline{Y}^+)^2.
\end{align}
$$

---

We now expand the squared term:

$$
\begin{align}
(Y_i^+ - \overline{Y}^+)^2
&= \left(\frac{Y_i(1) - \overline{Y}(1)}{n_t} + \frac{Y_i(0) - \overline{Y}(0)}{n_c}\right)^2 \\[6pt]
&= \frac{(Y_i(1) - \overline{Y}(1))^2}{n_t^2} + \frac{(Y_i(0) - \overline{Y}(0))^2}{n_c^2} + \frac{2(Y_i(1) - \overline{Y}(1))(Y_i(0) - \overline{Y}(0))}{n_t n_c}.
\end{align}
$$

Summing over $i$:

$$
\sum_{i=1}^n (Y_i^+ - \overline{Y}^+)^2 
= \frac{1}{n_t^2}\sum_{i=1}^n (Y_i(1) - \overline{Y}(1))^2 
+ \frac{1}{n_c^2}\sum_{i=1}^n (Y_i(0) - \overline{Y}(0))^2 
+ \frac{2}{n_t n_c}\sum_{i=1}^n (Y_i(1) - \overline{Y}(1))(Y_i(0) - \overline{Y}(0)).
$$

---

Define:

$$
S_1^2 \coloneqq \frac{1}{n-1}\sum_{i=1}^n (Y_i(1) - \overline{Y}(1))^2,\quad
S_0^2 \coloneqq \frac{1}{n-1}\sum_{i=1}^n (Y_i(0) - \overline{Y}(0))^2,
$$

$$
S_{0,1} \coloneqq \frac{1}{n-1}\sum_{i=1}^n (Y_i(1) - \overline{Y}(1))(Y_i(0) - \overline{Y}(0)).
$$

---

Therefore:

$$
\mathbb{V}(\hat{\tau}^{\text{dm}})
= c(n-1) \left( \frac{S_1^2}{n_t^2} + \frac{S_0^2}{n_c^2} + \frac{2S_{0,1}}{n_t n_c} \right).
$$

Finally, substituting $c = \frac{n_t n_c}{n(n-1)}$:

$$
= \frac{n_c}{n n_t} S_1^2 + \frac{n_t}{n n_c} S_0^2 + \frac{2}{n} S_{0,1}.
$$

Or, applying Lemma 5:

$$
= \frac{S_1^2}{n_t} + \frac{S_0^2}{n_c} - \frac{S_{\tau_i}^2}{n}.
$$
