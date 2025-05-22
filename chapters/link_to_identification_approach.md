The approach in main text to show unbiasedness is *design-based* (link to @abadie2020sampling).

In the main derivation above, we use a **design-based approach**:
- We assume **random assignment** of treatment.
- We show directly, using the assignment mechanism and properties of expectations, that:

$$
\mathbb{E}[\hat{\tau}^{\text{dm}} \mid \mathbf{n}, \mathbf{Y(w)}] = \tau
$$

That is, the **difference-in-means estimator is unbiased** for the **average treatment effect (ATE)**.

---

This connects to the **observational / identification framework** commonly used in econometrics texts like *Angrist & Pischke*, which begins by noting:

- The observed difference in outcomes:

$$
\mathbb{E}[Y_i \mid W_i = 1] - \mathbb{E}[Y_i \mid W_i = 0]
$$

can be decomposed as:

$$
\underbrace{\mathbb{E}[Y_i(1) - Y_i(0) \mid W_i = 1]}_{\text{ATT}}
\;+\;
\underbrace{\mathbb{E}[Y_i(0) \mid W_i = 1] - \mathbb{E}[Y_i(0) \mid W_i = 0]}_{\text{Selection Bias}}
$$

---

**Random assignment implies:**

$$
W_i \perp (Y_i(1), Y_i(0))
$$

which gives:

$$
\mathbb{E}[Y_i(0) \mid W_i = 1] = \mathbb{E}[Y_i(0) \mid W_i = 0]
\quad \text{and} \quad
\mathbb{E}[Y_i(1) \mid W_i = 1] = \mathbb{E}[Y_i(1)]
$$

So:

$$
\text{ATT} = \text{ATE}
\quad \text{and} \quad
\text{Selection Bias} = 0
$$

Therefore:

$$
\mathbb{E}[Y_i \mid W_i = 1] - \mathbb{E}[Y_i \mid W_i = 0] = \mathbb{E}[Y_i(1) - Y_i(0)] = \text{ATE}
$$

Full derivation:

(Population) average treatment effect:
$$
ATE = \mathbb{E}[Y_i(1) - Y_i(0)]
$$
Average treatment effect on the treated:
$$
ATT = \mathbb{E}[Y_i(1) - Y_i(0) \,|\, W_i=1]
$$

The observed difference is the sum of ATET and selection bias:

$$
\begin{align}
\underbrace{
\mathbb{E}[Y_i \,|\, W_i=1] - \mathbb{E}[Y_i \,|\, W_i=0]
}_{\text{Observed difference}}

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]
&\quad\quad\, 
+\mathbb{E}[Y_i(0) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=1]
\\[5pt]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=1]
\\[5pt]
&\quad\quad\, 
+
\mathbb{E}[Y_i(0) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]

\quad&=\quad
\underbrace{
\mathbb{E}[Y_i(1) - Y_i(0) \,|\, W_i=1]
}_{\text{ATT}}
\\[5pt]
&\quad\quad\, 
+
\underbrace{
\mathbb{E}[Y_i(0) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
}_{\text{Selection bias}}
\\[5pt]

\end{align}
$$
Randomisation ensures that $\mathbb{E}[Y_i(0) \,|\, W_i=0] = \mathbb{E}[Y_i(0) \,|\, W_i=1]$ and thus solves the selection problem and also allows us to estimate the ATE (show this rigorously if needed):
$$
\begin{align}
\mathbb{E}[Y_i \,|\, W_i=1] - \mathbb{E}[Y_i \,|\, W_i=0]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=0]
\\[5pt]

\quad&=\quad
\mathbb{E}[Y_i(1) \,|\, W_i=1] - \mathbb{E}[Y_i(0) \,|\, W_i=1]
\\[5pt]

\quad&=\quad
\underbrace{
\mathbb{E}[Y_i(1) - Y_i(0) \,|\, W_i=1]
}_{\text{ATT}}
\\[5pt]

\quad&=\quad
\underbrace{
\mathbb{E}[Y_i(1) - Y_i(0)]
}_{\text{ATE}}
\\[5pt]
\end{align}
$$
---

**Connection**

- The **design-based proof** shows unbiasedness of the estimator using random assignment directly.
- The **identification-based decomposition** explains how randomization ensures that ATT = ATE and removes selection bias.
- Both frameworks **converge under random assignment**:
  - One via potential outcomes and independence logic,
  - The other via expectations over the experimental design.

This shows how the design-based approach justifies the assumptions often taken as given in identification-based frameworks.
