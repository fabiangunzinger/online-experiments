
Notes on [Randomised Experiment chapter](https://alexdeng.github.io/causal/randomintro.html):



From mostly harmless (alsousing brave and true for [reference](https://matheusfacure.github.io/python-causality-handbook/01-Introduction-To-Causality.html)):

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
