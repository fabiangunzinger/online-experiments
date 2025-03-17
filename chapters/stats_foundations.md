# Statistics foundations

## Degrees of freedom

In statistics, degrees of freedom generally refers to the number of values in a calculation that can vary freely.

Examples:

- Variance calculation: given that we have a mean, once we know all but one value, we also know final value, since sum of mean deviations has to be zero.

- Covariance calculation: given the two means, once we know the values for all but one x and y pair, we also know the values of the final pair. Hence, we loose one df (not clear to me why not two, given that both x and y are determined -- because we treat their product as a single value? but that seems arbitrary)

- Also, why no correction when we have popultion means? See wikipedia article on variance for section on bias correction

- There is lots of confusion out there when it comes to df. For instance, you sometimes hear people say that df is the number of parameters you had to calculate on route. But this is wrong. It happens to come to the same when calculating variance, but not if you calcualte covariance (where you calculate two means beforehand, but only loose one df).


## The bootstrap

- In practice, we often use the bootstrap to calculate standard errors of model parameters or statistics.

- Conceptually, the bootstrap works as follows:

1) we draw an original sample and calculate our statistic

2) we then create a blown-up version of that sample by duplicating it many times

3) we then draw repeated samples from the large sample, recalculate our statistic, and calculate the standard deviation of these statistics to get the standard error.

- To achieve this easily, we can skip step 2) by simply sampling with replacement from the original distribution in step 3).

- The full procedure makes clear what the bootstrap results tell us, however: they tell us how lots of additional samples would behave if they were drawn from a population like our original sample.

- Hence, if the original sample is not representative of the population of interest, then bootstrap results are not informative about that population either.

- The bootstrap can also be used to improve the performance of classification or regression trees by fitting multiple trees on bootstrapped sample and then averaging their predictions. This is called "bagging", short for "bootstrap aggregating".

- We can use to boostrap also to calculate CIs following this algorithm:

1) Draw a large number of bootstrap samples and calculate the statistic of interest

2) Trim [(100-x)/2] percent of the bootstrap results on either end of the distribution

3) The trim points are the end point of the CI.


<!--

```{python}

from sklearn.utils import resample

  

rng = np.random.default_rng(2312)

  

population = rng.normal(3, 5, 1_000_000)

sample = rng.choice(population, 1000)

resample_means = pd.Series(resample(sample).mean() for _ in range(1000))

  

print(f"{'Population mean:':20} {np.mean(population):.3f}")

print(f"{'Sample mean:':20} {np.mean(sample):.3f}")

print(f"{'Bootstrap mean:':20} {np.mean(resample_means.mean()):.3f}")

print(f"{'Bootstrap se:':20} {np.mean(resample_means.std()):.3f}")

``` -->


## Combination vs Permutation

**Permutation (Order Matters)**

If we have $n$ items and we want to pick $r$ in a specific order, the formula is:  

$$
P(n, r) = \frac{n!}{(n - r)!}
$$
Example:
Arranging 3 letters (A, B, C) in 2 positions.  
- Possible orders: AB, AC, BA, BC, CA, CB → 6 ways  
- Formula:  
  $$
  P(3,2) = \frac{3!}{(3 - 2)!} = \frac{3!}{1!} = \frac{3 \times 2 \times 1}{1} = 6
  $$

**Combination (Order Doesn't Matter)**

If we have $n$ items and we want to pick $r$, but order does not matter, the formula is:

$$
C(n, r) = \frac{n!}{r!(n - r)!}
$$

Example:

Choosing 2 letters from (A, B, C), where order does not matter.  
- Possible groups: {A, B}, {A, C}, {B, C} → 3 ways  
- Formula:  
  $$
  C(3,2) = \frac{3!}{2!(3 - 2)!} = \frac{3!}{(2! \times 1!)} = \frac{3 \times 2 \times 1}{(2 \times 1) \times 1} = 3
  $$

**Key Difference:**
- **Permutation:** ABC and BAC are different  
- **Combination:** ABC and BAC are the same  

**Shortcut:**  
$$
P(n, r) = C(n, r) \times r!
$$  
(Since for every combination, there are $r!$ ways to arrange it.)



## Variance and covariance properties

Building blocks for advanced manipulations.


  
$$
\begin{align}
Var(X + c) &= Var(X) \\
Var(X + Y + c) &= Var(X + Y)
\end{align}
$$


Add here covariance properties that show that

cov(X + a, Y + b) = cov(X ,Y)


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
\end{align}
$$

In general:

$$
\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y] + \text{Cov}(X, Y)
$$

