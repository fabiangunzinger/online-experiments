
# Proofs
de
fff


## Proof 1 {#sec-proof1}
The estimator \( \hat{\theta} \) is unbiased if:

\[
E[\hat{\theta}] = \theta
\]

To show this, we start by noting that:

\[
E[\hat{\theta}] = E\left[\frac{1}{n} \sum_{i=1}^{n} X_i\right]
\]

By the linearity of expectation, we have:

\[
E[\hat{\theta}] = \frac{1}{n} \sum_{i=1}^{n} E[X_i] = \frac{1}{n} \cdot n \cdot \theta = \theta
\]

Therefore, the estimator is unbiased.
