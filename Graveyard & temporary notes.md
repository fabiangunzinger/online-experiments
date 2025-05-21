

Impact of randomisation for Y1
$$
\begin{align}
\mathbb{E}&\left[
\frac{1}{n_t}\sum_{W_i=1}Y_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right] \\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_t}\sum_{i=1}^{n}W_iY_i(1)
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right] 
\quad&\text{Definition of }W_i
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^{n}
\mathbb{E}[W_i\>|\>\mathbf{n}, \mathbf{Y(1)}]
Y_i(1)
\quad&\text{}
\\[5pt]

&=
\frac{1}{n_t}\sum_{i=1}^{n}
\left(\frac{n_t}{n}\right)
Y_i(1)
\quad&\text{Lemma 1}
\\[5pt]

&=
\frac{1}{n}\sum_{i=1}^{n}
Y_i(1)
\quad&\text{}

\end{align}
$$
Similarly:

$$
\begin{align}
\mathbb{E}&\left[
\frac{1}{n_c}\sum_{W_i=1}Y_i(0)
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right] \\[5pt]

&=
\mathbb{E}\left[
\frac{1}{n_c}\sum_{i=1}^{n}(1-W_i)Y_i(0)
\>|\>\mathbf{n}, \mathbf{Y(w)}
\right] 
\quad&\text{Definition of }W_i
\\[5pt]

&=
\frac{1}{n_c}\sum_{i=1}^{n}
\mathbb{E}[(1-W_i)\>|\>\mathbf{n}, \mathbf{Y(1)}]
Y_i(0)
\quad&\text{}
\\[5pt]

&=
\frac{1}{n_c}\sum_{i=1}^{n}
\left(\frac{n_c}{n}\right)
Y_i(0)
\quad&\text{Lemma 2}
\\[5pt]

&=
\frac{1}{n}\sum_{i=1}^{n}
Y_i(0)
\quad&\text{}

\end{align}
$$
