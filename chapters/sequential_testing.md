# Sequential testing

- kohavi2023peeking is a good starting point into the literature.



## Deliveroo 

Based on [this](https://deliveroo.engineering/2018/10/22/how-to-experiment-rapidly-without-losing-rigour.html) blog post from deliveroo engineering and [this](https://www.analytics-toolkit.com/pdf/Efficient_AB_Testing_in_Conversion_Rate_Optimization_-_The_AGILE_Statistical_Method_2017.pdf) paper referenced therein.

- In practice, the above has two main consequences: detecting small effects requires large samples and thus long experiments (if we can increase sample size by running the experiment for longer), and the minimal detectable effect we decide to callibrate the experiment for is crucially important, even if we might not have a good sense of what the effect of the treatment is.
- Sequential testing allows us to check for results along the way and stop the experiment early in case the effect is much larger (or smaller) than expected while accounting for multiple comparisons.

- The main idea is to use the idea of a family-wise error-rate (the error rate of concucting a family rather than a single hyopthesis test) and applying it to conducting hypothesis tests at different points in time.