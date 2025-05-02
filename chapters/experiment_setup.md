
# Experiment setup {#sec-experiment-setup}


In the main text, I refer to this file three times:
1) Decision between finite sample vs super population
2) Decision between CRE  or BRE – settled by how we randomise in online experiments
3) Decision how to analyse BRE – condition on n or treat n as binomial variable (former: easier and corresponds to situation at time of analysis


todo:
- See chat discussion
	- See here: https://alexdeng.github.io/causal/randomintro.html (exactly what I want)
- Generally, literature uses a potential outcome framework 
	- @larsen2023statistical
- Some vendors/literature assume fixed sample sizes
	- @nordin2024precision
- Others use an iid framework for motivation
	- zhou2023all

## Finite sample vs superpopulation analysis

- First, we decide whether the $n$ units in our experiment sample are the population of interest, or whether that population of interest is instead a larger super-population of size $N$ of which the $n$ units in the experiment sample are a random sample. Following @imbens2015causal, I refer to the former as the finite sample perspective and the later as the super population perspective.

- Online experiments typically go through a ramp-up phase: they include only a small fraction of users at the start and all users at the end.  

Below is from an old version – I don't actually consider both. Just discuss difference here. Use material from imbens2015causal to point out that, ultimately, the difference is irrelevant in practice.

- Statistically, this means we have to consider two different cases. In the first case, during the early stages of an experiment, we use a sample of the entire user population to learn something about that population as a whole; in this case, the sample we work with and the sample we want to learn something about are different. In the second case, when the entire user population is part of the experiment, the sample we work with and the sample we want to learn something about are the same. Following @imbens2015causal, I refer to the first approach as the super-population perspective and the second case the finite sample perspective.

- We have a super population of $N$ users. In the example I'm going to use throughout, these are all of our iOS-app users in the UK.

- From this super population, we sample $n$ users to be part of the experiment, and then allocate $n_t$ users to treatment and $n_c$ users to control.

- The way we sample users into the experiment and allocate them to treatment has implications for our statistical analysis, so let's have a look at the details.

- Online experiments typically use the following **sampling** approach to determine whether a user is part of an experiment:

	1. Create a hash string unique for each user in the experiment such as `<user_id><experiment_id><market>`.  
	2. Feed the hash string into a hash algorithm (often MD5) and receive a hash value.
	3. Use the hash value to determine whether a user is part of the experiment. Say we allocate 10% of traffic to the experiment. We then include the user only if their hash value falls within the bottom (or top) 10% of possible hash values.

- With this approach, the probability that a user is sampled into the experiment is independent of the sampling decisions of all other users.

- We write $R_i = 1$ if user $i$ is part of the experiment sample and $R_i = 0$ if they aren't.

- If we sample $n$ users into the experiment, then each of our $N$ users is part of the experiment experiment with probability $n/N$. 

- For each user in the super population, being part of the experiment sample is a [Bernoulli trial](https://en.wikipedia.org/wiki/Bernoulli_trial) with $R_i \sim \text{Bernoulli}(n/N)$ and, hence, $\mathbb{E}[R_i] = n/N$ and $\mathbb{V}(R_i) = n/N(1-n/N)$. I assume here that we know $n$. The reason for doing so is twofold. First, it makes the math easier as it spares us from modeling $n$ as a Binomial random variable. Second, by the time we analyse the data, we do know $n$.


## Assignment mechanism

### General discussion

- See [[notes_on_imbens2015causal]] for background on assignment mechanism




### BRE

BRE in online experiments, but CRE used in literature for analysis. Apart from those, many others.

- Second, regardless of which of these two perspectives we adopt, we decide how to allocate the $n$ units in the experiment sample into a treatment and control group. The procedure for performing this allocation is called the assignment mechanism, and there are a number of different such mechanisms.


**Treatment assignment** for units in the experiment sample then works in the same way:

4. We again create a unique -- but different -- hash string for each unit (e.g. `<user_id><experiment_id><market>` with the first bit flipped).

Why another hash? Suppose we used the same hash strings instead, and that we sampled units with hash values that fall into the bottom 10% of possible hash values. What would happen if we now allocated all units with hash values in the bottom 50% of possible values to treatment and all others to control? How many units would be in the control group? (None! Since the hash values of all units in the sample would fall into the bottom 10% of possible hash values so that all units would be allocated to treatment.) A simple way to create a different hash value is to flip the first bit of the hash string. Because of the [avalanche effect](https://en.wikipedia.org/wiki/Avalanche_effect), this would result in a completely different hash value.

5. We use the hash algorithm to generate a hash value

6. If we wanted to allocate units equally to a control and treatment group, we could allocate all units with hash values within the bottom 50% of possible values to the treatment group and all others to the control group.

- Hashed sampling and treatment assignment resembles sampling without replacement in that we can only include a given user in the experiment once, and resembles sampling with replacement in that **sampling and treatment assignment are independent** across units -- sampling any one unit does not alter the chance of being sampled for any other unit.


- In online experiments, as least, this is not an assumption if we properly test the randomisation proceedure (see discussion of SRM in @sec-threats-to-validity)

- In online experiments, the assignment mechanism is usually a BRE. The  assignment mechanism of a BRE is individualistic, probabilistic, and unconfounded. In the simplest case without stratification, it is also independent of covariates. In all cases, the assignment mechanism is fully under our control. For probability of treatment assignment $q$, we thus have:

$$
P(\mathbf{W} | \mathbf{X}, \mathbf{Y}(1), \mathbf{Y}(0)) = P(\mathbf{W}) = q^{n_t} (1-q)^{n_c}
$$





## Approaches to analysing BREs

- Sampling vs randomisation based – see [[notes_on_athey2017econometrics]]

- Reading Wager, it seems there are two relevant factors for inference: he just conditions on n to get a CRE, and then there is the question of whether to take a finite sample or super-population perspective. 

- The additional assumption (discussed in population asymptotics) of random sampling from super population holds for online experiments.

- He does use Bernoulli sampling, which is what I need for online experiments. I just don'f fully understand how his perspective fits into the Imbens Rubin book / Athey Imbens one. 

- See Ding book





  
