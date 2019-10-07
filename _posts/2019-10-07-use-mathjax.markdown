<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>
<script type="text/x-mathjax-config">
let isMathjaxConfig = false; // initialize once

const initMathjaxConfig = function() {
  if (!window.MathJax) {
    return;
  }
  window.MathJax.Hub.Config({
    showProcessingMessages: false, //关闭js加载过程信息
    messageStyle: "none", //不显示信息
    jax: ["input/TeX", "output/HTML-CSS"],
    tex2jax: {
      inlineMath: [["$", "$"], ["\\(", "\\)"]], // inline selector
      displayMath: [["$$", "$$"], ["\\[", "\\]"]], // paragraph selector
      skipTags: ["script", "noscript", "style", "textarea", "pre", "code", "a"]
    },
    "HTML-CSS": {
      availableFonts: ["STIX", "TeX"], //可选字体
      showMathMenu: false //关闭右击菜单显示
    }
  });
  isMathjaxConfig = true;
};


if (isMathjaxConfig == false) { // if not configure yet...
  initMathjaxConfig();
}
</script>

Equation 1:

$$ \frac{1}{x^2 + 1} $$

Equation 2 (inline):
$ \sum_0^\infty $

Equation 3:

\[ f(x)=\left\{
\begin{aligned}
x & = & \cos(t) \\
y & = & \sin(t) \\
z & = & \frac xy
\end{aligned}
\right.
\]
