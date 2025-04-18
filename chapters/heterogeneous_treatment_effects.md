# Heterogeneous treatment effects

todo: 
- Look at Athey and Wagner 2017 causal forrest approach: https://www.tandfonline.com/doi/full/10.1080/01621459.2017.1319839#d1e2336


- Focusing only on mean outcomes can be detrimental in the long-term, especially in contexts where metrics are driven heavily by a small subset of superusers [@bojinov2020avoid]

- In such cases, focusing on the average effectively builds products for superusers, which can be a missed opportunity because increasing the engagement of infrequent users and ensuring they don't churn is often one of the largest opportunities for growth.


- Solutions:

  - Use metrics and approaches that reflect the needs of different customer segments (e.g. specially designed metric, [interleaving](https://netflixtechblog.com/interleaving-in-online-experiments-at-netflix-a04ee392ec55))

  - Measure impact across different levels of technical access (e.g. slice experiment results based on type of device (latest or older), speed of internet connection (check percentiles))

  - Account for group-specific behaviour (slice experiment results by country, main individual characteristics, and track share of top-line metrics contributed by top 1% of users)

  - Segment key markets (e.g. build versions of the product suitable for particular needs -- e.g. LinkedIn Light built to cater for Indian users on slower 3G networks).


## Basic approach -- compare outcomes by subgroup

- Analyse results separately for pre-defined subgroups (e.g. platforms) using same approach as for stratified randomisation design (but instead of calculating weighted averages from groups, calculate ATEs)

- Once concern is that if there are mutliple subgroups, we have multiple testing problem (if there are 100 binary covariates, we'll expect to find 5 significant differences even if covariates are unrelated to treatment effects)

- Possible solutions:

  - Pre-analysis plans (for social science)

  - MHT correction

## Testing for treatment effect heterogeneity

- See athey2017econometrics 10.2

- Can also test, for each covariate, whether they are associated with treatment effect (e.g. different ATE for below and above median valuse). To do this correctly, though, need to pre-specify the set of hypothesis tests, which makes exploring all possible interactions between covariates and ways to discretise them difficult.



## Machine-learning approaches

- Discussed in @athey2017state: Two main approaches: causal trees and LASSO

- See QuantCo post on metalearners: https://tech.quantco.com/blog/metalearners

## Good segments

- Market or country
- Device or platform
- Time of day or day of week
- User type (new vs existing, high-use vs low-use)
- Account characteristics (single vs joint account)


## Issues to be aware of

- Users changing segment can lead to misleading results: if I make changes on feature F, and compare users of F with those who don't use F. Suppose avg of sessions per wk is 20 for users, 10 for non-users. Now suppose that my change makes everyone with 15 sessions per wk who previously used F to stop using F. Avg for users of F will increase, that for non-users will also increase. But this has nothing to do with overall avg sessions per wk, which could go in any direction. -- Solution: if possible, only look at segments that are fixed before the start of the experiment. If you cannot do that, then beware of this issue, and look at aggregate metrics.

- Simpson's paradox


## Simpson's paradox

todo:
- What are the necessary and sufficient conditions for the paradox to occur (the test for us understanding this will be to be able to reproduce a simple example on the spot – as is done brilliantly in youtube video linked below)

- Understand Wasserman's point that problem is imprecision in how we describe conditional statements with langauge

- see ding2023 p.11 for graphical representation






- Simpson's paradox basically happens due to a kind of omitted variable bias.

- It can occus if pooling data overlooks a confounding driver of the data generation that affects the data in specific ways.

- In particular, the underlying data needs to be such that:

  1. Outcomes differ by the level of the confounding variable
  2. The difference in outcome between these levels is larger than the differences in outcome within each level

- This is very abstract -- I'm aiming to rewrite this over time.

- The paradox has two classic manifestations: frequency Tables and correlations.

- For frequency Tables, classic examples are listed on [Wikipedia](https://en.wikipedia.org/wiki/Simpson%27s_paradox#Examples). Notice that in all the examples, the difference in outcomes between levels of the confounder (departments, stone size, and year) is much larger than the differences between the groups of interest (men and women, treatments, batters) at any given level of the confounder.

- For correlations, the below gif makes things very clear.

![Pace~svwiki, CC BY-SA 4.0 <https://creativecommons.org/licenses/by-sa/4.0>, via Wikimedia Commons](../inputs/simpsons_paradox.gif)

Useful resources
- Excellent small example [https://www.youtube.com/watch?v=ebEkn-BiW5k](https://www.youtube.com/watch?v=ebEkn-BiW5k)
- Relevant section in Wasserman's All of Statistics (where he points out that the key is really that language doesn't precisely express conditional statements)



## Resources

- athey2017econometrics (Chapter 10)
- imai2010heterogeneous for very succinct summary of athey2017econometrics content

- @bojinov2020avoid

