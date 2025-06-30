Add to variance page once done

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
