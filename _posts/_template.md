---
layout: post
title: Template
subtitle: _template
---
<script>
MathJax = {
  loader: {load: ['[tex]/upgreek', '[tex]/mathtools']},
  tex: {
    packages: {'[+]': ['upgreek'], '[+]': ['mathtools']},
    inlineMath: [['$', '$'], ['\\(', '\\)']]
  },
  svg: {
    fontCache: 'global'
  }
};
</script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js">
</script>

reference[^1]

{::options parse_block_html="true" /}

<details><summary markdown="span">summ $\mathbf W$</summary>
$$a\coloneqq b$$
</details>

{::options parse_block_html="false" /}

<div style="border: 1px solid black;margin: 0 auto;padding: 15px 15px 15px 25px;">
<b>Theorem 1 (attractive case)</b>: Assume $\forall x\in\Lambda\colon U_x\leq0$ and  $N$ even. Then:
<ol><li>Among the ground states of $\mathbf H$ given by $(1)$, there exists one with vanishing total spin $S = 0$;</li>
    <li>Furthermore, if $\forall x\colon U_x < 0$, the ground state is unique (and by part 1., of total spin $S = 0$).</li></ol>
test additional
</div>

<div style="border: 1px solid black;margin: 0 auto;padding: 15px 15px 5px 25px;">
<b>Theorem 1 (attractive case)</b>: Assume $\forall x\in\Lambda\colon U_x\leq0$ and  $N$ even. Then:
<ol><li>Among the ground states of $\mathbf H$ given by $(1)$, there exists one with vanishing total spin $S = 0$;</li>
    <li>Furthermore, if $\forall x\colon U_x < 0$, the ground state is unique (and by part 1., of total spin $S = 0$).</li></ol>
</div>

\\| for vertical bar\
\\* for asterisk (when inline)\
\\_ for underscore (when inline)\
\\{ for braces (when inline)

[^1]: [E. H. Lieb. _Two Theorems on the Hubbard Model_. Physical Review Letters **62**. 1989.](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.62.1201)
