# Introduction


## Aim of stats part

- A/B tests are used throughout industry to make decisions that allocate millions of dollars and that affect millions of customers.
- My goal is to provide a complete overview of the statistical theory on which all this decision-making rests.


## Why causal inference?

- Because many thinks we think work don't (look at list of refuted claims from Kohavi [here](https://experimentguide.com/refuted_observational_studies/))

The purpose:

- Create a valid counterfactual outcome - the outcome of the treated if they had not been treated - in order to eliminate selection bias when estimating the treatment effect.


## Why experiments

1. The randomisation of treatment eliminates selection bias.

2. The presence of a control group provides a counterfactual of what would have happened without treatment.


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



## Fundamental problem of causal inference


## Thinking about causality


- [Milton Friedman thermostat example](https://www.mercatus.org/macro-musings/nick-rowe-monetary-basics-milton-friedmans-thermostat-and-more)

## Interpretation

- We often think of experiment as answering: "Does the intervention work?", but what it really answers is "What is the average effect of the intervention with this specific implementation for this particular population at this time?"

- When we have multiple treatments or analyse subgroups separately, we need to test whether different estimates are significantly different from each other (can't just look at whether some are significantly different from zero while some aren't) 


## Conventions used throughout

- I use users instead of the more generic units

- I discuss the case with one treatment and one control group.
