[[_Experimentation]]


Eppo's terminology is useful here:

A **fact** is a statistic collected at the level of the unit of randomisation, so usually a user. It could be, for instance, the number of orders or the number of sessions.

A **metric** is defined at the variant level and is comprised of one or two facts, each together with an aggregation method, expressed in per user terms. There are simple and ratio metrics.

A **simple metric** is a single fact and aggregation metric. For instance, the sum of the number of orders in the treatment group, defined as:

$$
\frac{\sum_{i:W_i=1}o_i}{n_t}
$$

A **ratio metric** is the ratio of two facts and their aggregation methods (still implicitly, expressed in per user terms, but the denominator common to the numerator and denominator of the ratio drops out). For instance, number of orders per session defined as:

$$
\frac{\frac{\sum_{i:W_i=1}o_i}{n_t}}{\frac{\sum_{i:W_i=1}s_i}{n_t}}
= \frac{\sum_{i:W_i=1}o_i}{\sum_{i:W_i=1}s_i}
$$

Another way to calculate ratio metrics is using the double-average approach, whereby we first calculate user-level averages and then take the average across all users, which gives us:

$$
\frac{\sum_{i:W_i=1}\frac{o_i}{s_i}}{n_t}
$$
This approach gives equal weight to each user, which makes it more robust to outliers. But it is still a ratio metric.