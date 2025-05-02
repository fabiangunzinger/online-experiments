# Introduction

## Why causal inference?

- Because many things we think work don't (look at list of refuted claims from Kohavi [here](https://experimentguide.com/refuted_observational_studies/))

The purpose:

- Create a valid counterfactual outcome - the outcome of the treated if they had not been treated - in order to eliminate selection bias when estimating the treatment effect.

- [Milton Friedman thermostat example](https://www.mercatus.org/macro-musings/nick-rowe-monetary-basics-milton-friedmans-thermostat-and-more)

Comparison to other types of data analysis

- Descriptive analysis describes the data. We simply turn data into meaningfull summary measures presented in tables or -- better, usually! -- figures. The aim here is to simply describe the data as it is, highlighting aspects that are particularly interesting or relevant to the task at hand.

- Predictive analysis predicts unobserved metric values based on observed ones. We build a model that captures the data generating process of the metric we aim to predict based on training data we observe, so that we can then predict metric values we don't observe.

- Causal inference makes statements about what would happen to outcomes if we changed the world in a particular way.

Causal inference vs prediction

- Prediction is about finding the most likely outcome based on a set of (existing) covariates. Causal inference is about finding the effect of a change in a covariate on the outcome.

- The difference is profound: when predicting, you take the features as given and predict outcomes based on them -- you're asking: "given existing features, what outcome can I expect?". When you perform causal inference, you want to know what would happen if you were to change one of the covariates -- you're asking "if I were to change one covariate in a certain way, what outcome could I expect?". (In prediction, different units have different covariate values. In causal inference, we ask what would happen if we changed covariate values for some or all units.)

- Causal inference is about manipulating covariates -- to paraphrase Donald Rubin: there is no causality without manipulation.

- Technically, what this really comes down to is that in prediction, you don't care about selection bias, whereas in causal inference that's the main thing you care about.

- This also means that the role of goodness of fit is very different: for prediction, it's obviously very important -- if your model explains only a very small part of the variation in the outcome, it won't be very good at predicting outcomes. In causal inference, goodness of fit doesn't matter because your aim is not to predict, but to know how the outcome changes if you change a covariate. So, you can have very low goodness of fit (lots of things outside the model predicting outcomes), but if you can precisely estimate your treatment effect, that's very valuable (you learn that regardless of all the many other factors that determine the outcome, changing a covariate in a certain way tends to change outcomes in a certain way.)

- In a business context, they capture two sets of problems:
	- Prediction is about making statements in domains where the future is expected to be similar to the past (i.e. revenue given sales)
	- Causal inference is about answering "what if" questions (what happens to sales if we change X)


## Why experiments

We have built a new feature for our platform and want to know whether the platform is the better or worse for it. How can we know?

First, let's make the question more precise. What we care about is a comparison of two states of the world. We want a comparison of the state of the world with and without our new feature. So, how do we do that? 

Ideally, we'd clone the world and release the feature in one world and not the other, keep everything else the same, and compare outcomes. We can't do that. What can we do?

We could compare outcomes from before and after the release of the feature. If nothing has changed in the world but the release of our feature, then this gives us the same result as our ideal scenario above. But nothing never changes in the world. If we find that outcomes after the release differ from outcomes from before we can never know whether that is because of our feature or whether the observed difference would have occurred regardless – because of a change in season, a big piece of world news, a change in customer preferences, or any of many other possible reasons or a mix thereof. What else can we do?

We can show the feature to one group of customers and withhold it from another group and compare outcomes between these two groups. If nothing but exposure to the feature is different between the two groups then this approach, too, gives us the same answer as our infeasible ideal approach above. Whether or not there *is* a difference between the two groups of customers depends on how we select customers for those groups: do we put high-value customers in one group and occasional customers in another? Or customers from one region in one and customers from another region in the other group? All such manual approaches raise concerns – some more obvious than others. An experiment is the best tool we have to maximise our chance that the two groups are, indeed the same: the randomisation of treatment eliminates selection bias, and the presence of a control group provides a counterfactual of what would have happened without treatment.


## Is experimentation worth it in business?

- In some business organisations there is a prevailing wisdom that "science" often in the form of A/B testing (which is the digital version of controlled experiments) makes making decisions harder by slowing them down, which wastes important time.

- One of the main jobs of an experimentation specialist in an organisation is to help people understand why running experiments is crucial. In this space, I want to gather my evolving thoughts on this topic with the aim of crafting an increasingly convincing argument.

Relevant thoughts:

- Epistemic argument about how we know what works: human behaviour is heterogeneous and largely unpredictable, so to know what works you need to test things, and the easiest (certainly for most tech companies) and most rigorous way to test things is to run experiments.

- Experiments do not slow decision making down, but are one of the most powerful tools we have to ensure that the decisions we make are good ones.

- Discuss when not to experiment (for there clearly are cases where you either don't have the time or where there really is no need).

- Andrew Gelman argues (in [this](https://statmodeling.stat.columbia.edu/2023/11/24/what-are-the-important-parts-of-statistics/) blog post) that i) "to reduce, control, or adjust for bias and variation in measurements" and ii) "to systematically gather data on multiple cases" are the two most important parts of statistics. This is a good way of emphasising why running experiments in a standardised way are important even if you feel you have data about your feature: it ensures that you collect data from a representative sample of users and it ensures you are judging the success of your feature by pre-defined metrics defined in a standardised way (at least ideally!).

- How do we define identity of executives or product managers: not around knowing what is true or what works or what customers want, but around coming up with good hypotheses, being willing to learn from data, and strong at iterating on features based on data and past experience.

Is experimentation worth it?

- @king2020power on how Pinterest was able to build a machine-learning system that is 20% better at detecting policy-violating content thanks to incremental testing that experimentation allows.


## Interpretation

- We often think of experiment as answering: "Does the intervention work?", but what it really answers is "What is the average effect of the intervention with this specific implementation for this particular population at this time?"

- When we have multiple treatments or analyse subgroups separately, we need to test whether different estimates are significantly different from each other (can't just look at whether some are significantly different from zero while some aren't) 


## Conventions used throughout

- I use users instead of the more generic units

- I discuss the case with one treatment and one control group.
