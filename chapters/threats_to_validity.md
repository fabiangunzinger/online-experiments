# Threats to validity {#sec-threats-to-validity}

todo:
- Integrate Shadish, Cook, and Campbell (2002) Experimental and quasi-experimental designs for causal inference... (cited in athey2017econometrics)

- For experiment results to be trustworthy, they need to be reliable, have high internal validity, and high external validity across time.

- Reliability is about reproducibility of our results and the extent to which a rerun of the exact same experiment would lead to the same result. It's about the stability of measurements and the replicability of the work.

- Internal validity is about the extent to which the treatment effect estimate of our experiment reflects the cause and effect relationship of interest instead of being influenced by other factors. This is all about the design of the experiment.

- External validity is about the extent to which our experiment results generalise both across different populations and across time. Results don't usually generalise across populations (a feature may work well in one country but not in another), which isn't a problem because we can (and usually should!) just run the experiment separately for different markets. Traditional experiments do, howerver, assume that experiments generalise over time, in that we think that the effect we estimate during the experiment period will be persistent -- i.e. we think that the long-term effect of a change will be the same as the short-term effect.

- In my mind, some of the threats listed below, such as learning effects, could be listed as threats to either internal or external validity. I list them where I think they make most sense given the definitions, but one could argue about that and some authors make different choices -- what matters is that you think about them when designing an experiment!

- Consider integrating @king2006dangers, section 3.2 onwards, types of biases:
1. Omitted variable bias
2. Posttreatment bias (including variables in X that are result of T)
3. Interpolation bias ()
4. Extrapolation bias



**Violations of SUTVA**

- SUTVA  will be violated if there are network effects: if the behaviour of units is mutually dependent. For instance: if my wife and I both use an online photo-sharing service and my wife sees a new feature that we both like while I'm in the control group, we might stil share the same number of family photos but start sharing them all on her account instead of mine. This creates an artificial treatment effect because if I had also had access to the new feature, we might not have changed our behaviour at all, while, during the experiment, her sharing volume went up while mine went down, suggesting the existent of a positive effect.

- Another case where the no interference assumption can be violated is in the form of general equilibrium effects. A classic example is the effect of further education: the effect of my doing a PhD in statistics on my earnings while nobody else changes their behaviour (the partial-equilibrium effect) is surely different from the outcome of my earnings if suddenly everyone decided to do a PhD in statistics (the general-equilibrium effect).

- The two violations capture the two different ways interference can lead to incorrect results: interference can happen and bias our results either during the experiment (threat to internal validity) or once the feature is fully rolled out (threat to external validity). In either case, the treatment of some unit has an externality on other units.

- Another way in which this aspect of SUTVA can be violated is when treatments are administered in different ways, e.g. by either giving patients medication or asking them to take the medication themselves.


### Selection bias

Common types of selection bias in data science:

- The vast search effect (using the data to answer many questions will eventually reveal something interesting by mere chance -- if 20,000 people flip a coin 10 times, some will have 10 straight heads)

- Nonrandom sampling

- Cherry-picking data

- Selecting specific time-intervals

- Stopping experiments prematurely

- Regression to the mean (occurs in settings where we measure outcomes repeatedly over time and where luck and skill combine to determine outcomes, since winners of one period will be less lucky next period and perform closer to the mean performer)


Ways to guard against selection bias:

- have one or many holdout datasets to confirm your results.



## Threats to reliability

## Threats to internal validity

- Internal validity refers to the ability of a study to measure the causal effect within the study population.

### Misc effects (some of them relevant only for field experiments)

- Hawthorne effect: the treatment group works harder than normal.

- John Henry effect: the comparison group competes with the treatment group.

- Resentment and demoralising effect: not getting the treatment changes behaviour negatively.

- Demand effect: treatment units more likely to do what they think is wanted from them.

- Anticipation effect: control group changes behaviour in anticipation of future treatment.

- Survey effect: being surveyed changes behaviour.

### Interference

- Basically, all violations to SUTVA

- Interference can happen due to

  - Network effects

  - Cannibalisation of resources in marketplaces

  - Shared resources (i.e. treatment slowing down site for everyone)

### Interaction effects

- Users can be simultaneously part of multiple experiments, so that what we measure for reach of them is really the effect of the interaction of all of them. This means that, if only some features are implemented, the results after roll out could be different from those observed during the experiment period.

- However, with large sample sizes, this should not generally be a problem because effects of different experiments average out between treatment and control group.

- While the above may be true statistically, interaction effects can still lead to extremely poor user experiences (blue background interacted with blue font), which is why mature platforms aim to avoid them.


### Non-representative users

Possible scenarios:

- Our marketing department launches an add campaign and attracts a lot of unusual users to the site temporarily.

- A competitor does the same and temporarily takes a ways users from our site.

- Heavy-user bias: heavy users are more likely to be bucketed in an experiment, biasing the results relative to the overall effect of a feature. Depending on the context, this can be an issue.

- Solution: run experiments for longer (thought this comes with opportunity costs, and will increase cookie churn)


### Survivor bias

- This is really just a version of the above: if you select only users that have used the product for some time, your sample is not representative of all users. The classic demonstration of survivor bias is [Abraham Wald's](https://en.wikipedia.org/wiki/Abraham_Wald) insight in WWII that you want to put extra armour where returning plans got hit the least, since it's presumable the planes that got hit there that didn't make it back.

<!-- ![Where would you put extra armour? By Martin Grandjean (vector), McGeddon (picture), Cameron Moll (concept) - Own work, CC BY-SA 4.0, https://commons.wikimedia.org/w/index.php?curid=102017718](../inputs/survivorship-bias.svg){width=50%} -->


### Novelty and learning effects

- Challenge: behaviour might change abruptly and temporarily in response to a new feature (novelty or "burn in" effect) or it might take a while for behaviour fully adapt to a new feature (learning effects). In both cases, the results from a relatively short experiment will not provide a representative picture of the long-run effects of a feature.

- Examples: Increasing number of adds shows on Google led to increase in add revenue initially but then decrease of clicks in the long term because it increased add blindness @hohnhold2015focusing

- Solutions:

  - Measure long-term effects (by running experiments for longer)

  - Have a "holdout" group of users that isn't exposed to any changes for a pre-set period of time (a month, a quarter), to measure long-term effects

  - Estimate dynamic treatment effects to see the evolution of the treatment effect


### Sample ratio mismatch


### Attrition

Attrition is when, for some reason, we cannot collect endline data on some units.

It's a problem because when it's systematic rather than random, treatment and control group are no longer comparable, which is a threat to internal validity. (Even if the same number of people drop out, if they are different type, the problem is the same.) Also, it reduces sample size and thus power.

We can limit it if we promise access to the program to everyone (phase-in design), change the level of randomisation, and improve data collection.

In our analysis we should 1. Report the extent of attrition, 2. Check for differential attrition (between groups, and within groups based on observable characteristics), and 3. Determine the range of estimates given attrition using a selection model or bounds.

One method is to use Heckman's selection model: we look at the characteristics of those who attrite, assume that they have the same outcomes as those with the same characteristics for which we have data, and then fill in their outcome variables accordingly.

Another method is to use bounds. There are two types.

Manski-Horowitz bounds: Lower bound: replace all missing values in treatment with the least favourable outcome value from the observed sample and replace all missing values in control with most favourable value in the observed sample. Upper bound created in a reverse way. Unless attrition is low and the outcome variable is tightly bounded, this tends to lead to very large bounds.

Lee bounds: We treat the estimate from the available data as an upper bound, and construct the lower bound by trimming from the sample with less attrition the observations that most contribute to the treatment effect.

### ## Imperfect compliance

Partial complience

- Partial compliance occurs if, for some reason, some people in the treatment group are not treated or some people in the control group are.

- It's problematic because it reduces the difference in treatment exposure between treatment and control group (in the extreme, if they are equal, we learn nothing), and because they might make treatment takeup non-random.

- A few things can help to limit the problem: make takeup easy and/or incentivise it, randomise at a higher level, and provide a treatment to everyone.

- We can adjust our analysis by calculating LATE, either using the Wald estimator (ITT / difference in take-up between treatment and control groups) or 2SLS where we instrument the behaviour we want to encourage with the treatment dummy (and possibly other covariates).

- Do not drop non-compliers or re-assign them to the control group -- compliers and non-compliers are different so that dropping or reclassifying non-compliers would re-introduce self-selection into out two samples, which defeats the whole point of the randomisation.


Defiers

- They are the opposite of compliers: they either take up the treatment because they were assigned to control or the other way around.

- They might occur in an encouragement design if they overestimated the benefit of treatment and got discouraged by the information provided in the treatment.

- They might make us significantly misinterpret the true effect (RRE page 303, or, better, MHE p 156).

- We can deal with them only if they form an identifiable subgroup, in which case we can estimate the treatment effect on defiers and compliers separately and calculate an average treatment effect.

In an experiment with one-sided non-compliance what does IV estimate 1. if there are no always-takers and 2. if there are not never-takers?

- In general, IV estimates LATE, the effect on compliers, and the treated consist of compliers and always-takers, while the non-treated consist of compliers and never-takers.

- If there are no always-takers, the population of compliers is the same as the population of the treated, so that LATE = ATET.

- If there are no never-takers, the population of compliers is the same as that of the untreated, so LATE = average treatment effect of the untreated.


## Treats to external validity

- External validity is concerned with whether the results from a study generalise to other contexts.

### Budget effects in Ads

- On an adds platform, a treatment might perform very well during an experiment in that it makes marketers launch more adds. But once scaled up may do less well because the increased traffic might exhaust marketer's budgets, leading them to reduce adds launched.

### Feedback loops from personalisation

- Treatments might behave differently during experimentation and once they are scaled up if the performance of a feature is a function of the size of the audience it is exposed to (an example could be a recommendation algorithm, which performs better and better as it is being used more).

### Day-of-week effects

- See below

### Seasonality

- Seasonality comes in many forms: day of week effects, week of year effects, season effects, holiday effects, etc.

- The challenge is that, potentially, user behaviour might differ on certain days or over certain time periods either because we get different users or because users change their behaviour.

- Whether it is really a problem depends on the context. One aspect that is often forgotten here is that seasonality, first and foremost, is about a shift in levels -- activity on LinkedIn might go down during the summer months. What we usually want to measure, however, is the difference between treatment and control units. Hence, if you don't have reason to believe that the effect of the treatment is different during a particular season (e.g. because you think it's additive), then seasonality might not be a problem for you.

- Having said that, it's actually quite likely that with either different users or different behaviour by the same users, users might react differently to featore on different days. So it really is a thread to external validity, and we thus should usually care about it.

- Solution: design your experiment so as to take seasonality into account. E.g. run your experiment for at least one week to account for day of week effects (that's generally a good idea), don't run crucial experiments during the holiday season or on major holidays or discard data from such periods, etc. 

- What to take into account depends on your context. So understand the relevant seasonality for you (if you're a travel app, consider seasonality of travel demand, if you're an e-commerce site, consider seasonality of shopping behaviour)

### Differences in time-to-action between users

- Some users may engage with a new features immediately, others might take a while and then react differently to it.

- When running experiments for a very short time, we might thus get a biased picture of the overall effect of the feature.

- With this one, one could argue that it's a threat to internal validity, too. But that depends on what whether we set out to measure the short or long-term causal effect in our study

### Consent

- If consent for experiment participation is required, then people who give consent may be different from those who don't. 


### Heterogeneous treatment effects

- Consent above is a specific manifestation of the underlying problem: heterogeneous treatment effects. Often, it is plausible that participants of an experiment are different from non-participants, since -- in real life -- sampling into the experiment population doesn't happen at random. So, clearly defining who the full population of interest is, what population we can make inferences about from our experiment population, and understanding hte difference between the two is crucial.



## Resources

- [Forbes article on when not to trust your A/B tests](https://www.forbes.com/sites/quora/2015/06/19/when-should-ab-testing-not-be-trusted-to-make-decisions/)

- [Dennis Meisner discussing threats to external validity](https://towardsdatascience.com/ab-testing-when-external-validity-messes-with-your-results-888197b6bc7b)
