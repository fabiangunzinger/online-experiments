
## Standard error

The [standard error](stats_foundations.md#sampling-distribution) of an estimator is simply the square root of its sampling variance. From 

<!-- @eq-var  -->

we thus have:

$$
\widehat{SE}
= \sqrt{\frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}}.
$${#eq-se}

Because in online experiments sample sizes are large and treatment effects are usually small, it is sometimes convenient to assume equal sample sizes, so that the sample size for each variant is $n_t = n_c = n_v$, and equal variances, so that $s_t^2 = s_c^2 = s^2$. The common variance $s^2$ is estimated by "pooling" the treatment group variances to create a [degrees-of-freedom-weighted](stats_foundations.md#degrees-of-freedom) estimator of the form:
$$
s^2 = \frac{(n_t - 1) s_t^2 + (n_c - 1) s_c^2}{n_t + n_c - 2}.
$$
Substituting in @eq-se we then have:
$$
\widehat{SE}^{\text{equal}}
= \sqrt{\frac{s^2}{n_v} + \frac{s^2}{n_v}}
= \sqrt{\frac{2s^2}{n_v}}.
$${#eq-se-equal}

Finally, for the purpose of experiment design it is sometimes useful to express the standard error in terms of the proportion of units allocated to the treatment group. Hence, instead of assuming equal sample sizes, we use $p$ to denote that proportion and $n$ to denote total sample size, while maintaining the assumption of equal variance. Again substituting in @eq-se we can then write:
$$
\widehat{SE}^{\text{prop}}
= \sqrt{\frac{s^2}{pn} + \frac{s^2}{(1-p)n}}
= \sqrt{\frac{s^2}{np(1-p)}}.
$${#eq-se-prop}

For $p=0.5$, this formulation is equivalent to @eq-se-equal as expected.

