# Network experiments

- Network experiments are experiments in a context in which the outcome of some units is affected by the treatment status of other units, so that the treatment may not only change the behaviour of treated untis but also that of control units.

- This violates [SUTVA](neyman_rubin_causal_model.qmd## Stable unit treatment value assumption (SUTVA)) and leads to a bias in standard ATE estimators. 

- Fundamentally, we're dealing with a threat to external validity. In general, external validity is about the extent to which results from a scientific study can be generalised to contexts outside of the study. In the context of online experiments, where we typically experiment on a sample of the overall user population, external validity is about the extent to which results from our experiments translate to the entire population of users. Standard ATE estimators estimate the difference between two counterfactual states of the world: that where all units are treated and that which no units are treated. With interference, control units behave differently than they would have if nobody had been exposed to the treatment (and treatment of the treted units hadn't spilled over to them), and so the difference in treatment and control outcomes is no longer an unbiased estimate of the difference in outcomes of these two counterfactual states of the world.

- Older description: Spillovers are externalities of our treatment on non-treatment group units. They can be behavioural, informational, physical, and market-wide (general equilibrium effects). They are a problem because they reduce the quality of the counterfactual, and, if we don't take them into account, will lead us to overestimate (negative spillovers) or underestimate (positive spillovers) the effect. We can limit spillovers by identifying them properly (both positive and negative ones) and then randomising at the appropriate level and/or explicitly measure them by adjusting the design accordingly. To adjust our analysis, we need three groups: treatment, not treated but spillovers, not treated and no spillovers. We can then control for social network of geographical (distance) variables.



## Types of interferance

@larsen2023statistical differentiate between:

- Network interference. Suppose we have a network where all nodes are of the same type and where the behaviour of each node can affect the behaviour of the nodes it is directly connected to. An example is a social network, where all nodes are people, and where more activity of one person may plausibly lead to more activity among their connections, which may lead to more activity among their connections, and so on. Here, experimentation is difficult because if we randomly split units into treatment and control group, units in the treatment group that change their behaviour as a result of the treatment might interact with connections of theirs that are allocated to the control group and thus change their behaviour as a result. But now the units in the control group don't behave any longer as they would under the absence of the treatment, and comparing the differences in behaviour between treatment and control is not an accurate representation of the different outcomes in two counterfactual states of the world, one where everyone is treated and one where nobody is.

- Marketplace interference. Suppose we have a network composed of two of more types of nodes which interact with each other, and where, from the perspective of each type of node, the number of other nodes is finite, so that each type of node competes for interactions with a fixed number of nodes of the other type. Examples include ride hailing, where drivers compete for customers and customers for available rides; ads marketplaces, where marketers compete for slots and platforms with slots for marketers; or food delivery services, where riders compete for orders and customers who place orders for riders (food delivery services, such as Deliveroo, are three-sided marketplaces, managing the interaction of restaurants, riders, and customers). In such a marketplace experimentation is challenging because treatment and control groups compete for a fixed pool of resources, so that an increase in behaviour for the treatment group must lead to a decrease in the decreate of the behaviour for the control group. This "cannibalisation" effect exaggerates the actual treatment effect. (See @liu2020trustworthy for more.)


## Dealing with interference

- There are a number of different approaches (see, e.g. discussion in @larsen2023statistical)

- What they share in common is that they all address the fundamental problem, which is that under the presence of interaction effects, the difference in outcomes between the treatment and the control group is not an accurate representation of the two counterfactual states of the world where everyone is and isn't exposed to the treatment, which is what standard ATE estimators measure.

- There are two broad ways to tackle the problem: we can adjust the design to limit interference in the first place, or we can model interference and adjust the analysis.

### Design-based approaches

#### Split resources

- If shared resources cause the issue (e.g. budget for ads), then splitting the resources is an option (i.e. 50% of resources for a variance that gets 50% of the traffig)


#### Cluster-based randomisation approaches

- This approache first splits the units into clusters that are disjoint (or as disjoint as possible). Randomisation then happens at the cluster level, and all units within a cluster receive the same treatment.

- Because units will interact only (or mostly) with units that have the same treatment status as they themselves, this design provides a better counterfacturals for the all-treated and all-controlled alternatives.

- One challenge is creating clusters: effectively, you need to find the clusters between which there is no interactions, so that externalities don't spill across cluster boundaries. Examples:

  - Geographical clustering:

    - to experiment on currier behaviour in a food delivery network: randomise at a level at which curriers don't interact (such as a part of a large city, or even between cities).

    - In a online-dating network, clusters may be geographical regions between which there is little interaction between users (i.e. locations to far away for people to wanting to find partners)

  - social/interaction-graph-clustering:

    - To test a feature in a social-network app: create graphs of user interactions, and partition the graph such that there is maximal within-group interaction and minimal between group interaction

- Another challenge is ensuring you have enough effective sample size: if the outcomes of users within a group are correlated, then randomising at a level higher than the unit reduces effective sample size. In the extreme case, where within group correlation is perfect, your effective sample size is the number of groups, not the number of units.

- Effectively, we face a variance-bias tradeoff: the fewer clusters we have the larger and more isolated they are, which leads to low bias but large variance and vice versa. 

Implications for analysis

- How to adjust standard errors? See @abadie2023should and @de2024level, also check
middleton2015unbiased

- If we have randomised at the group level: either analyse the data at the group level, or, if analysing the data at the individual level, use clustered standard errors.

Effect on power

  - Reduces power because observations within clusters are almost surely correlated. Intuition:
  
    1. It effectively reduces sample size (if all observations within a cluster are identical, then our number of observations equals the number of clusters).
  
    2. It makes the sample less diversified and hence less representative of the population so that different estimates based on different samples will vary more widely, which leads to a larger standard error.

  - The effect depends on the number of clusters (the more the more power we have) and on the intra-cluster correlation (the less correlation the higher the power).


#### Ego-clusters

- Instead of traditional clusters, create much smaller, unit-based clusters. See @saint2019using

- Basically: you create clusters based on a central node (the ego) and its direct links (the alters), and randomise egos and alters seprately.

- If your user-base is large enough, then this has the advantage that you can create many more clusters than with traditional clustering (e.g. city-level clusters), which means you can limit spillovers without suffering from low power.


#### Switchback experiments

- An approach often used to deal with marketplace interferance, whereby all units are frequently switched from being in control to being in treatment.

- There could be carry-over issues with this, but this can be dealt with by using "burn-in" periods.

- Another issue is that, like cluster-randomisation, switchbacks also suffer from lower power.


### Analysis-based approaches

- Analysis based approaches model the interference and then adjust the analysis accordingly. See last paragraph of section 6 in @larsen2023statistical for details. Also, @bojinov2023design, in their introduction, differentiate between approaches that rely on game theoretic, Markov Chain, and network approachs to model interference.


## Measuring interaction effects

Overall approach: if there are no interaction effects, different estimands should be the same

Design side

- run clauster and unit-level experiment side-by-side. Different results imply presence of interaction effects

- We can design differences in treatment density by varying the allocation fraction for different groups.

- We can use a multistage randomisation to allocate treatment first at the group level and then, within the group, at the individual level. This creates a treatment group (the treated in the treatment group), a spillover control group (the untreated in the treatment group), and a pure control group (those in the nontreatment group). 

Analysis side

- exposure modelling / reweighting
- use of focal units
- We can exploit natural variation in treatment density (number of children eligible for deworming program within a geographic area).



## Useful resources

- @larsen2023statistical, in section 6, provides a recent review of the literature.

- @karrer2021network describe network experimentation at Meta.


## Q&A

1. You randomise at customer level. For analysis, you do the following: you calculate metrics at a restaurant level, then calculate variant level averages from the restaurant-level averages. Is there a problem? What is it? What assumptions are being violated? 

