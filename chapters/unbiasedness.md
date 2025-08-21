# Unbiasedness {#sec-unbiasedness}

An estimator is unbiased if its expected value equals the estimand. In @eq-estimator we defined our difference-in-means estimator as:

$$
\begin{align}
\hat{\tau}^{\text{dm}}
=
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i,
\end{align}
$$

and in @eq-estimand we defined the estimand as:

$$
\begin{align}
\tau
= \frac{1}{n}\sum_{i=1}^n Y_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0).
\end{align}
$$

Given that we take sample sizes $\mathbf{n} = (n_t, n_c)$ and potential outcomes $\mathbf{Y(w)} = (\mathbf{Y(1)}, \mathbf{Y(0)})$ [as fixed](experiments.md), showing that our estimator is unbiased amounts to showing that:

$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\mid \mathbf{n}, \mathbf{Y(w)}
\right]
&=\tau
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\mid \mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i
\mid \mathbf{n}, \mathbf{Y(w)}
\right]
- \mathbb{E}\left[
\frac{1}{n_c}\sum_{W_i=0}Y_i
\mid \mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0).
\end{align}
$$

There are two pieces we need for this:

1) Link observed to potential outcomes so that $Y_i = Y_i(W_i)$. This requires the Stable Unit Treatment Value Assumption (SUTVA).

2) Link treatment group averages to sample averages so that $\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=w}Y_i(w)\right] = \frac{1}{n}\sum_{i=1}^{n}Y_i(w)$. This requires randomisation.

## SUTVA links observed outcomes to potential outcomes

Our aim is to estimate the average difference in individual-level potential outcomes:

$$
\begin{align}
\tau
= \frac{1}{n}\sum_{i=1}^n Y_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0).
= \frac{1}{n}\sum_{i=1}^n \big(Y_i(1) - Y_i(0)\big),
\end{align}
$$
and we have [seen before](setup.md) that for unit , the unit-level potential outcome is
$$
\tau_i = Y_i(1) - Y_i(0).
$$
A potential outcome, $Y_i(w)$, is a unit-indexed function of the treatment level, $w$. The challenge is that without any assumptions, potential outcomes are not only functions of the treatment level of each individual unit, but of the treatment levels of all units. 

To see this, say we run an experiment and unit $i$ is allocated to treatment, so that the $i$-th element of the assignment vector $\mathbf{W}$ of that experiment is $W_i = 1$. We refer to this assignment vector as $\mathbf{W}^{(i=1)}$. What we know for sure about $i$'s outcome in that experiment is that it is the outcome given that all treatment allocations are exactly the way they are, so that we have
$$
Y_i = Y_i\left(\mathbf{W}^{(i=1)}\right).
$$
What does this mean concretely? It means that the observed outcome is a function not only of $i$'s treatment assignment but of:

1. The assignment of all units in the experiment
2. The precise form of the assigned treatment level received by $i$ (and all other units)
3. The way in which said assigned treatment level is administered to $i$ (and all other units)

What we need is:
$$
Y_i = Y_i\left(\mathbf{W}^{(i=1)}\right) \overset{!}{=} Y_i(1).
$$
The only way to make progress is to assume what we need: that potential outcomes for unit $i$ are a function only of the treatment level unit $i$ itself receives and independent of (i) treatment assignment of other units and (ii) the form and administration of the treatment level. (i) above is referred to as "no interference" in the literature, (ii) as "no hidden variations of treatment".

That assumption is SUTVA, the Stable Unit Treatment Value Assumption. It ensures that the potential outcomes for each unit and each treatment level are well-defined functions of the unit index and the treatment level only – that for a given unit and treatment level, the potential outcome is well-defined and, thus, "stable" [@rubin1980randomization].  

Note that (ii) does not mean that a treatment level has to take the same form for all units, even though it is often misinterpreted to mean that. What we need is that $Y_i(W_i)$ is a clearly defined function for all $i$ and all possible treatment levels $W_i \in \{0, 1\}$. For this to be the case, it must be the case that there are no different forms of possible treatments, the potential outcome for each for is the same so that the differences are irrelevant, or that the experienced form is random so that the expected outcome across all units remains stable.

In the context of our e-commerce app experiment, the non-interference part of SUTVA means that potential outcomes for all customers are independent of treatment assignment of all other customers – very much including family members and friends. The no-hidden-variation part means that a customer's precise experience is pinned down by their mobile device and remains stable over time: there are no accidental server-side bugs that create different background colours and it is either clear whether the customer uses Android or iOS, or the experiences are identical. SUTVA does not require, however, that the active treatment looks exactly the same on iOS and Android.

Why is this important? We want to know what happened if we rolled out our change to all units compared to if we rolled it out to no units. To have any hope of estimating this we can't have treatment level's vary over time or depending on circumstances, but need them to be pinned down for each unit.

%% SUTVA is a strong assumption and can be violated in a number of ways. If it holds, however, then we I'll discuss these, together with solutions, in @sec-threats-to-validity.

If SUTVA holds we have:
 %%
SUTVA is a strong assumption and can be violated in a number of ways. If it holds, however, then we have:

$$
Y_i = Y_i(W_i) = \begin{cases} 
   Y_i(1) & \text{if } W_i = 1 \\
   Y_i(0)       & \text{if } W_i = 0,
  \end{cases}
$$
or, more compactly:
$$
Y_i = W_iY_i(1) + (1 - W_i)Y_i(0).
$$

This is the link between observed and potential outcomes we need, and makes clear that we observe $Y_i(1)$ for units in treatment and $Y_i(0)$ for units in control.

In our unbiasedness proof, we now have

$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=\tau
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
- \mathbb{E}\left[
\frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
&\text{SUTVA}
\\[5pt]
\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=1}Y_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
- \mathbb{E}\left[\frac{1}{n_c}\sum_{W_i=0}Y_i(0)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\end{align}
$$

What remains is to show that $\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=w}Y_i(w)\right] = \frac{1}{n}\sum_{i=1}^{n}Y_i(w)$. This requires randomisation.

## Randomisation links treatment groups to the population

We start by rewriting the last equation slightly: we use the [definition of $W_i$](experiments.md) to write $\sum_{W_i=1}Y_i(1)$ as $\sum_{i=1}^{n} W_iY_i(1)$ for treatment units and use the corresponding expression for control units; we use the linearity of the expectation operator to move it inside the summation; and we move $Y_i(w)$, [which is fixed](experiments.md), out of the expectation. We now have:


$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=\tau
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
- \mathbb{E}\left[
\frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
&\text{SUTVA}
\\[5pt]
\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=1}Y_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
- \mathbb{E}\left[\frac{1}{n_c}\sum_{W_i=0}Y_i(0)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
\frac{1}{n_t}\sum_{i=1}^{n}\mathbb{E}\left[W_i
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]Y_i(1)
- \frac{1}{n_c}\sum_{i=1}^{n}\mathbb{E}\left[1-W_i
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]Y_i(0)
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0).
\end{align}
$$

This expression makes transparent that $W_i$ is the only random element. $\mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]$ and $\mathbb{E}[1-W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]$ are determined by the assignment mechanism. Given that we have [defined](experiments.md) $W_i$ as a Bernoulli random variable, and given that we take potential outcomes $\mathbf{Y(w)}$ and sample sizes $\mathbf{n}$ as given, we have established in @eq-prob that:
$$
P(W_i = 1 \>|\>\mathbf{n}, \mathbf{Y(w)}) 
=
\frac{n_t}{n}.
$$Hence:
$$
\begin{align}
\mathbb{E}&[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}] 
\\[5pt]
&=
1 \times P(W_i = 1 \>|\>\mathbf{n}, \mathbf{Y(w)})
+ 0 \times P(W_i = 0 \>|\>\mathbf{n}, \mathbf{Y(w)})
\\[5pt]
&=
P(W_i = 1 \>|\>\mathbf{n}, \mathbf{Y(w)})
\\[5pt]
&= \frac{n_t}{n}.
\end{align}
$$
Similarly:
$$
\begin{align}
\mathbb{E}&[(1-W_i)\>|\>\mathbf{n}, \mathbf{Y(w)}] 
\\[5pt]
&=
1 - \mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(w)}]
\\[5pt]
&=
1 - \frac{n_t}{n}
\\[5pt]
&=
\frac{n - n_t}{n}
\\[5pt]
&=
\frac{n_c}{n}.
\end{align}
$$

Plugging these two expressions gives us the final result:

$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=\tau
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i - \frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
\mathbb{E}\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
- \mathbb{E}\left[
\frac{1}{n_c}\sum_{W_i=0}Y_i
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
&\text{SUTVA}
\\[5pt]
\mathbb{E}\left[\frac{1}{n_t}\sum_{W_i=1}Y_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
- \mathbb{E}\left[\frac{1}{n_c}\sum_{W_i=0}Y_i(0)
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
\frac{1}{n_t}\sum_{i=1}^{n}\mathbb{E}\left[W_i
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]Y_i(1)
- \frac{1}{n_c}\sum_{i=1}^{n}\mathbb{E}\left[1-W_i
\>|\>\mathbf{n}, \mathbf{Y(w)}\right]Y_i(0)
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
&\text{Randomisation}
\\[5pt]
\frac{1}{n_t}\sum_{i=1}^{n}\left(\frac{n_t}{n}\right)Y_i(1)
- \frac{1}{n_c}\sum_{i=1}^{n}\left(\frac{n_c}{n}\right)Y_i(0)
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\\[5pt]
\frac{1}{n}\sum_{i=1}^{n}Y_i(1)
- \frac{1}{n}\sum_{i=1}^{n}Y_i(0)
&=
\frac{1}{n}\sum_{i=1}^nY_i(1) - \frac{1}{n}\sum_{i=1}^nY_i(0)
\end{align}
$$

This completes our proof of unbiasedness of our difference in means estimator.

In the next section, we'll calculate the estimator's variance.