{% include mathjax %}
Equation 1:

\\[ \frac{1}{x^2 + 1} \\]

Equation 2:

\\[ \sum_{0}^{\infty} \\]

Equation 3:

\\[
f(x) = \\left\\{ \begin{aligned} \
x & = & \cos(t) \\\\ \
y & = & \sin(t) \\\\ \
z & = & \frac{x}{y} \
\end{aligned} \
\right. \
\\]

## Arith Expr

This example is taken from TAPL:

\\[
\begin{gathered}
true \in \tau & false \in \tau & 0 \in \tau \\\\ \
\frac{t_1 \in \tau}{succ \ t_1 \in \tau} & \frac{t_1 \in \tau}{pred \ t_1 \in \tau} & \frac{t_1 \in \tau}{iszero \ t_1 \in \tau} \\\\ \
 & \frac{t_1 \in \tau \quad t_2 \in \tau \quad t_3 \in \tau}{if \ t_1 \ then \ t_2 \ else \ t_3 \in \tau}
\end{gathered}
\\]


Footnote: Jekyll always failed with the error `can't find ffi-1.9.XX`, i tried to reinstall jekyll many times, and still can't fix that. Then i search `bundle gem`, seems gem use `Gemfile` to lock some lib's version. So i just remove the `Gemfile`, and re-run jekyll, works locally now. Cheers!