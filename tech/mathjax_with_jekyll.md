---
layout: page
title: GitHub Pages에 수식 넣기 - MathJax
permalink: /tech/mathjax_with_jekyll.html
lang: ko
mathjax: true
---

[MathJax][mathjax_site]가 TeX, LaTex, MathML, AsciiMath를 모두 web browser로 볼 수 있게 해준다.

## 설치

[Adding MathJax to a GitHub Pages Jekyll Blog][adding_mathjax_to_jekyll]를 따라함.

* 설명과 달리, [minima][jekyll_minima] theme의 default.html을 수정했음. post를 사용하지 않는 page가 많아서임.

## Test

In N-dimensional simplex noise, the squared kernel summation radius $r^2$ is $\frac 1 2$
for all values of N. This is because the edge length of the N-simplex $s = \sqrt {\frac {N} {N + 1}}$
divides out of the N-simplex height $h = s \sqrt {\frac {N + 1} {2N}}$.
The kerel summation radius $r$ is equal to the N-simplex height $h$.

$$ r = h = \sqrt{\frac {1} {2}} = \sqrt{\frac {N} {N+1}} \sqrt{\frac {N+1} {2N}} $$

## 참고

* [MathJax][mathjax_site]
  * [MathJax Documentation][mathjax_doc]

[mathjax_site]: https://www.mathjax.org/
[mathjax_doc]: https://docs.mathjax.org/en/latest/index.html
[jekyll_minima]: https://github.com/jekyll/minima
[adding_mathjax_to_jekyll]: http://sgeos.github.io/github/jekyll/2016/08/21/adding_mathjax_to_a_jekyll_github_pages_blog.html
