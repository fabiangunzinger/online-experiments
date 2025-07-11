
## False positives

- Integrate: kohavi2024false, berman2021false, colquhoun2019false.pdf

## History of the P-value

- There are two schools of thought in statistical significance testing.

- R.A. Fisher thought of the p-value as a useful metric to determine how surprising a given set of data was.

- Jerzy Neyman and Egon Pearson realised that while it is not possible to eliminate false positives and negatives, it is possible to set up a process that guarantees that false positives occur only at a pre-defined rate, which they called $\alpha$.

- Unlike in Fisher's approach, the p-value in the Neyman-Pearson approach doesn't tell us anything about the strength of the evidence in any particular experiment besides whether or not to reject the null hypothesis, but guarantees that in the long run (over the course of many experiments), our false positive rate is not larger than $\alpha$. 

## The base-rate fallacy

Suppose you test 100 features out of which 10 have a true effect. If you have 80
percent power and use a 5 percent significance level. You can expect to
get a significant result for 8 of the 10 working features, but you also have a
false positive rate of 5%, so you'll also get a significant result for about 5
non-working features. You thus end up with 13 treatments that appear to work.
But we know that only 8 of these 13, or 62% of them work, while the remaining
5 are false discoveries. Thus, you have a false discovery rate of 38%.

You commit the base-rate fallacy when you confuse your p-value with your false
discovery rate. This would lead you to make statements such as "My p-value is
0.002, so there is only a 2 in a 1000 chance that I found my result by
chance!"

This confuses

$$
P(significant result | H_0),
$$

the p-value, with

$$
P(H_A | significant result)
$$



## Limitations of hypothesis testing

- p-values do not tell you anything about whether the result has any practical significance.

- Any intervention usually has *some* effect, you will always find a significant result with enough data. The question is whether the range of plausible effect sizes (captured by the confidence interval) is relevant.

- Arbitrary cutoff

- No appreciation for variation of coefficient -- focus on ci instead (see @romer2020praise, @imbens2021statistical)

- Multiple hypothesis testing (actual) -- report and apply MHT-correction

- Multiple hypothesis testing (potential [Gelman post](https://statmodeling.stat.columbia.edu/2016/03/07/29212/))

- MHT: family-wise error rate vs false discovery rate


## Confidence intervals

- Rely on confidence intervals: they provide the same information as a p-value (if they include the value of the null hypothesis then we cannot reject it) but the width of the interval also provides additional information on the uncertainty of the effect and its practical significance. (See [In Praise of Confidence Intervals](https://www.aeaweb.org/articles?id=10.1257/pandp.20201059) by David Romer and [Statistical Significance, p-Values, and the Reporting of Uncertainty](https://www.aeaweb.org/articles?id=10.1257/jep.35.3.157) by Guido Imbens for more).


## Testing and CI duality


## One-sides vs two-sided tests

- Instacard uses one-sided because they focus on improvements (hesterberg2024power section 2)

## Multiple hypothesis correction


<!--
## Approaches

### Bonferroni Correction

Key Idea: The Bonferroni method controls the family-wise error rate (FWER) by dividing the significance level (α) by the number of hypotheses (m). This ensures that the chance of making one or more Type I errors is minimized across all tests.

- Formula: Adjusted p-value = original p-value * m
- Advantages: Simple, robust, and widely applicable.
- Disadvantages: Highly conservative, especially with large numbers of hypotheses, leading to a loss of statistical power (more false negatives).

**Key Paper**: Dunn, O. J. (1961). Multiple Comparisons Among Means. _Journal of the American Statistical Association_.

---
### Holm-Bonferroni Method

Key Idea: A sequentially rejective version of the Bonferroni method, which adjusts p-values stepwise, starting with the smallest.

- Advantages: Less conservative than Bonferroni while still controlling FWER.
- Disadvantages: Still conservative with a large number of hypotheses, though less so than Bonferroni.

Key Paper: Holm, S. (1979). A Simple Sequentially Rejective Multiple Test Procedure. _Scandinavian Journal of Statistics_.

---

### Benjamini-Hochberg (BH) Procedure

Key Idea: Controls the expected proportion of false discoveries (false positives) among rejected hypotheses. It’s more relaxed than controlling the FWER, making it less conservative and better suited for large-scale testing.

- Advantages: Increased statistical power compared to Bonferroni, suitable for large datasets.
- Disadvantages: Less strict control of Type I errors, may allow for more false positives.

Key Paper: Benjamini, Y., & Hochberg, Y. (1995). Controlling the False Discovery Rate: A Practical and Powerful Approach to Multiple Testing. _Journal of the Royal Statistical Society_.

- See BIT blue book 4.3 for nice explanation of BH MHT correction


---

#### Benjamini-Yekutieli (BY) Procedure

Key Idea: An extension of the BH procedure that controls the FDR in scenarios with correlated tests, offering a more conservative approach than BH.

- Advantages: Suitable for dependent tests, as it accounts for correlations.
- Disadvantages: More conservative than the BH method, leading to reduced power in highly correlated tests.

Key Paper: Benjamini, Y., & Yekutieli, D. (2001). The Control of the False Discovery Rate in Multiple Testing under Dependency. _The Annals of Statistics_.

---

### More advanced methods

- Westfall-Young Permutation Method:
	- Key Idea: Uses resampling techniques to empirically calculate adjusted p-values. This non-parametric approach is ideal for complex datasets where traditional assumptions may not hold.
	- Advantages: Provides exact control over error rates, especially effective with small sample sizes.
	- Disadvantages: Computationally expensive, especially with large datasets or numerous hypotheses.
	- Key Paper: Westfall, P. H., & Young, S. S. (1993). Resampling-Based Multiple Testing: Examples and Methods for p-Value Adjustment. _Wiley_.
- Hierarchical Hypothesis Testing
	- Key Idea: Focuses on groups of hypotheses, adjusting for MHT within hierarchically structured families of tests. It starts with a global null hypothesis and proceeds to test smaller clusters of hypotheses based on the outcome.
	- Advantages: Balances FWER control with increased power in some specific areas of interest.
	- Disadvantages: Complex to design and interpret, depends heavily on correct hypothesis clustering.
	- Key Paper: Goeman, J. J., & Solari, A. (2011). Multiple testing for exploratory research. _Statistical Science_.


### Industry approaches

- Statsig provides optional MHT correction. For each experiment, users can apply either [Bonferroni](https://docs.statsig.com/stats-engine/methodologies/bonferroni-correction) or [Benjamini-Hochberg](https://docs.statsig.com/stats-engine/methodologies/benjamini%E2%80%93hochberg-procedure) corrections either per group or metric or both

![[Screenshot 2024-09-16 at 11.08.21.png|400]]
-->