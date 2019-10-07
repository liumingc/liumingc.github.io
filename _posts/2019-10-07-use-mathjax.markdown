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

Footnote: Jekyll always failed with the error `can't find ffi-1.9.XX`, i tried to reinstall jekyll many times, and still can't fix that. Then i search `bundle gem`, seems gem use `Gemfile` to lock some lib's version. So i just remove the `Gemfile`, and re-run jekyll, works locally now. Cheers!