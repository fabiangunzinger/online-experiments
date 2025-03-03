[[_Experimentation]]


Steps to victory
- [x] Setup
- [ ] CRE
	- [ ] Potential outcomes fixed
		- [ ] ding2023first approach
			- [ ] Write down proof of Lemma 4.1 which is important, use my definitiosn
			- [ ] Then use chat notes on lemma 2 ([here](https://chatgpt.com/share/67c14491-afb4-8005-a06c-b6634c6ab20f)) to apply to Neyman problem. Basic approach:
				- [ ] Start with var tau = var y1 bar + var y2 bar - 2cov y1bar, y2bar
				- [ ] I have these from lemma c2
				- [ ] Use relationship between cov and var tau_i from lemma 4.1 and substitute
			- [ ] Translate to my notation
		- [ ] Link to imbens2015causal approach in appendix
	- [ ] Potential outcomes random, condition on them
		- [ ] Pick up from footnote 1 on page 25 in ding2025first, which talks about conditioning and relation to BRE
		- [ ] Read wager2024causal for reference, too, to get clarity of how this all fits together
- [ ] BRE, see [[temp – BRE and IID]]
- [ ] SRS 
	- [ ] With replacement / IID approach
		- [ ] Same as BRE?
		- [ ] Relation to above approaches
		- [ ] Why do we need potential outcomes?
	- [ ] Without replacement
		- [ ] Same as CRE?
- [ ] Use [[stats_of_online_experiments]] to add context

Perspectives
- Two entities to view as either fixed or random:
	- Potential outcomes (Sampling perspective?)
	- Sample sizes (BRE vs CRE)



## Experiment setup

Could add super population here, with population statistics $\mu_1, \sigma_1^2$, etc.

...

Finite sample of $n$ units, each with potential outcomes $Y_i(1)$ and $Y_i(0)$.

Individual-level causal effects:
$$
\tau_i = Y_i(1) - Y_i(0)
$$
Finite sample average treatment effect:

$$
\tau_{\text{fs}}
= \frac{1}{n}\sum_{i=1}^n \tau_i
= \frac{1}{n}\sum_{i=1}^n \left(Y_i(1) - Y_i(0)\right)
= \overline{Y}(1) - \overline{Y}(0)
$$

Finite sample means and variances:

$$
\begin{align}
\overline{Y}(1) = \frac{1}{n}\sum_{i=1}^n Y_i(1),
\qquad
S_1^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - \overline{Y}(1)\right)^2
\\[5pt]
\overline{Y}(0) = \frac{1}{n}\sum_{i=1}^n Y_i(0),
\qquad
S_0^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(0) - \overline{Y}(0)\right)^2
\\[5pt]
\end{align}
$$
Variance of individual-level causal effects: 

$$
\begin{align}
S_{\tau_i}^2
&= \frac{1}{n-1}\sum_{i=1}^{n}\left(Y_i(1) - Y_i(0) 
- \left(\overline{Y}(1) - \overline{Y}(0)\right)\right)^2
\\[5pt]
&= \frac{1}{n-1}\sum_{i=1}^{n}\left(\tau_i - \tau_{\text{fs}}\right)^2 \\[5pt]
\end{align}
$$
Covariance of potential outcomes:

$$
\begin{align}
S_{0, 1} &= \frac{1}{n-1}\sum_{i=1}^{n}
\left(Y_i(1) - \overline{Y}(1)\right)
\left(Y_i(0) - \overline{Y}(0)\right)
\end{align}
$$

We have data from a randomised experiment with assignment vector $\mathbf{W} = \{W_1, ... W_n\}$ where $n_t = \sum_{i=1}^n W_i$ units are allocated to treatment and the remaining $n_c = \sum_{i=1}^n (1-W_i)$ to control. 

Observed outcomes are:
$$ 
Y_i = W_iY_i(1) + (1 - W_i)Y_i(0)
$$

We can estimate the finite sample statistics using:

$$
\begin{align}
\overline{Y}_t = \frac{1}{n_t}\sum_{i=1}^n W_iY_i,
\qquad
s_t^2 = \frac{1}{n_t-1}\sum_{i=1}^{n}W_i\left(Y_i - \overline{Y}_t\right)^2
\\[5pt]
\overline{Y}_c = \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i,
\qquad
s_c^2 = \frac{1}{n_c-1}\sum_{i=1}^{n}(1-W_i)\left(Y_i - \overline{Y}_c\right)^2
\end{align}
$$


## Estimand and estimator of interest

We are interested in the finite sample average treatment effect $\tau_{\text{fs}}$.

Given the observed data, a natural estimator is:

$$
\begin{align}
\hat{\tau}^{\text{dm}}
= \overline{Y}_t - \overline{Y}_c
\end{align}
$$
We analyse the properties of this estimators for different types of experiments (assignment mechanisms). In particular, we are interested in showing that the estimator is unbiased and to estimate its variance.

## CRE

A completely randomised experiment has an assignment mechanism that assigns a fixed number of units $n_t$ to treatment and a fixed number $n_c = n - n_t$ to control in a way such that each assignment vector $\mathbf{W}$ with $\sum_{i=1}^{n}W_i = n_t$ has equal probability of being observed. There are $\frac{n!}{n_t! (n-n_t)!} = \binom{n}{n_t}$ such assignment vectors, which means that:

$$
P(\mathbf{W} = \mathbf{w}) = \frac{1}{\binom{n}{n_t}}.
$$

The vectors of potential outcomes $\mathbf{Y(1)} = (Y_i(1), ..., Y_n(1))$ and $\mathbf{Y(0)} = (Y_i(0), ..., Y_n(0))$ are generally viewed as fixed in the above definition.

Alternatively, we can view them as random and condition on them, so that we have:
$$
P(\mathbf{W} = \mathbf{w} | \mathbf{Y(1)}, \mathbf{Y(0)}) = \frac{1}{\binom{n}{n_t}},
$$
because in a CRE, we have $\mathbf{W} \perp\!\!\!\perp \mathbf{Y(1)}, \mathbf{Y(0)}$.

To start with, we view the potential outcomes as fixed. This is the approach Neyman was interested in, and that is discussed in @imbens2015causal and @ding2023first. My derivations are based on the approach in @ding2015first. A step-by-step derivation of the approach used in @imbens2015causal is provided in the appendix [[notes_on_imbens2015causal]].

In a CRE, the treatment indicator has the following properties:

$$
\mathbb{E}(W_i) = \frac{n_t}{n}, \qquad \mathbb{V}(W_i) = \frac{n_t n_c}{n^2}, \qquad \text{Cov}(W_i, W_j) = -\frac{n_t n_c}{n^2(n-1)}
$$
Proofs are in [[notes_on_ding2023first]].

<details>
  <summary>Proofs</summary>
  {{< include cre_ass_indicator.md >}}
</details> 



We rewrite the estimator as

$$
\begin{align}
\hat{\tau}^{\text{dm}}

&=
\overline{Y}_t - \overline{Y}_c
&\text{}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n W_iY_i - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i
&\text{}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n W_iY_i(1) - \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i(0)
&\text{SUTVA}
\\[5pt]

\end{align}
$$


Unbiasedness of $\hat{\tau}^{\text{dm}}$:

$$
\begin{align}
\mathbb{E}\left[
\hat{\tau}^{\text{dm}}
\right]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^n W_iY_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n (1-W_i)Y_i(0)
\right]
&\text{}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n \mathbb{E}[W_i]Y_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n \mathbb{E}[(1-W_i)]Y_i(0)
&\text{}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^n \left(\frac{n_t}{n}\right)Y_i(1) 
- \frac{1}{n_c}\sum_{i=1}^n \left(\frac{n_c}{n}\right)Y_i(0)
&\text{}
\\[5pt]

&=
\frac{1}{n}\sum_{i=1}^n Y_i(1) 
- \frac{1}{n}\sum_{i=1}^n Y_i(0)
&\text{}
\\[5pt]

&=
\overline{Y}(1) - \overline{Y}(0)
&\text{}
\\[5pt]

&=
\hat{\tau}^{\text{dm}} 
\qquad\square
&\text{}
\\[5pt]

\end{align}
$$


Variance of tau dm:

...








Temp: used notation for easy copying:

$$
\begin{align}
\hat{\tau}^{\text{dm}}
\\[5pt]

\overline{Y}(1)
\\[5pt]

\end{align}
$$
