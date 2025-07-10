
## Standard error {#sec-stderror}

The [standard error](stats_foundations.md#sampling-distribution) of an estimator is simply the square root of its sampling variance. Using our estimator of the sampling variance from @eq-var-est, the standard error is thus given by:
$$
\widehat{SE}
= \sqrt{\frac{s_t^2}{n_t} + \frac{s_c^2}{n_c}}.
$${#eq-se}

In online experiments it is sometimes convenient to assume that **sample sizes and sample variances are equal**. This is justified in contexts where sample sizes are large and treatment effects are small. In such a case we denote the sample size per variant as $n_v$, with $n_t = n_c = n_v$, and we refer to the common sample variance of the outcome variable as $s^2$, with $s_t^2 = s_c^2 = s^2$. The common variance $s^2$ is estimated by "pooling" the treatment group variances to create a [degrees-of-freedom-weighted](stats_foundations.md#degrees-of-freedom) estimator of the form:
$$
s^2 = \frac{(n_t - 1) s_t^2 + (n_c - 1) s_c^2}{n_t + n_c - 2}.
$$
Substituting in @eq-se we then have:
$$
\widehat{SE}^{\text{equal}}
= \sqrt{\frac{2s^2}{n_v}}.
$${#eq-se-equal}

Finally, for the purpose of experiment design it is sometimes useful to express the standard error **in terms of the proportion of units allocated to the treatment group**. Hence, instead of assuming equal sample sizes, we use $p$ to denote that proportion and $n$ to denote total sample size, while maintaining the assumption of equal variance. Again substituting in @eq-se we can then write:
$$
\widehat{SE}^{\text{prop}}
= \sqrt{\frac{s^2}{pn} + \frac{s^2}{(1-p)n}}
= \sqrt{\frac{s^2}{np(1-p)}}.
$${#eq-se-prop}

Note that for $p=0.5$ and when expressing sample size in terms of $n_v$, we get:
$$
\widehat{SE}^{\text{prop}}_{p=1/2}
= \sqrt{\frac{s^2}{\frac{2n_v}{4}}}
= \sqrt{\frac{2s^2}{n_v}}
= \widehat{SE}^{\text{equal}}
$$
as expected.

Next, we'll use the standard deviation for sample size calculation.



