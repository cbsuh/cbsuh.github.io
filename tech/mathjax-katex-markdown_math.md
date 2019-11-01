---
layout: page
title: GitHub Pages와 vscode에서 수식 보기
permalink: /tech/mathjax-katex-markdown_math
lang: ko
---

### mathJax, KaTex, Markdown + Math

* Liquid Tag이 들어간 source code를 보여주기 위해서, 이 문서에서는 Liquid Tag의 처리를 껐다. {% raw %}

## 목표

markdown에서 수식을 쓰자.

* [vscode][vscode]의 preview에서 수식 보기
* [GitHub Pages][github_pages]에서 수식 보기
* local [jekyll][jekyll]에서 수식 보기
  * [GitHub Pages][github_pages]가 [jekyll][jekyll]을 사용함. 따라서, [jekyll][jekyll]만 설정하면 됨.

math_engine은 [mathJax][mathjax]와 [KaTeX][katex]를 선택할 수 있도록 한다. 둘의 특징은 다음과 같다.

* [mathJax][mathjax]
  * [SVG - Scalable Vector Graphics][svg]로 출력
  * LaTeX의 모든 syntax 지원
  * 느리다는 주장이 있음
  * 다른 module과 dependancy가 많다고 함
  * SVG를 지원하지 않는 옛날 browser와의 호환성 문제가 있다고 함
* [KaTeX][katex]
  * 지원 안되는 syntax가 있음
  * 빠르다고 주장. 하지만, 개인적으로 mathJax도 느리진 않음 :)

개인적으로는 [KaTeX][katex]만 지원해도 될 것 같지만, [GitHub Pages][github_pages]가 [kramdown][kramdown]의 `math_engine`으로 [mathJax][mathjax]를 강제한다.

* [kramdown][kramdown]은 [KaTeX][katex] 지원한다. [kramdown: Math Engine KaTeX][kramdown-katex] 참고.
* 하지만, [GitHub Pages][github_pages]에서 `mathJax`로 항상 override. GitHub: [github/pages-gem/lib/github-pages/configuration.rb][github-pages-config] 참고
* [GitHub Pages][github_pages]에서 강제한다는 것은, [mathJax][mathjax]의 server-side rendering을 지원한다는 뜻일 수 있다. 나중에 조사해보자.
* [kramdown][kramdown]도 [KaTeX][katex]의 server-side rendering을 지원하는 것 같다. 하지만, [GitHub Pages][github_pages]에서 지원해줘야 쓸 수 있다.

[mathJax][mathjax]와 [KaTeX][katex]의 선택은 각 markdown의 front matter에서 설정한다.

* [mathJax][mathjax]

    ```markdown
    ---
    mathjax: true
    ---
    ```

* [KaTeX][katex]

    ```markdown
    ---
    katex: true
    ---
    ```

* 둘 다 설정할 수 있는 문제가 있으나, 큰 문제가 아니므로 일단 넘어간다.

[jekyll][jekyll]의 theme은 `minima`를 사용한다.

## [vscode][vscode]의 preview에서 수식 보기

1. [Markdown+Math][markdown_math] extension 설치.
1. [Markdown+Math][markdown_math] 설정에서 delimiter를 `kramdown`으로 변경.
1. `Reload Window`를 하면 변경된 설정이 적용된다.

[Markdown+Math][markdown_math] extension을 설치하기만 하면, [vscode][vscode]의 markdown preview에서 수식을 볼 수 있다.

[Markdown+Math][markdown_math]의 기본 수식 delimiter는 `inline`의 경우 `$...$`, `display`의 경우 `$$...$$`이다. [jekyll][jekyll]에서 [kramdown][kramdown]을 사용하니, 설정에서 `delimiter`를 `kramdown`으로 변경하자.

## [mathJax][mathjax] 지원

1. `_includes/mathjax.html` 추가

    ```html
    {% if page.mathjax %}
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
        inlineMath: [ ['$','$'], ["\\(","\\)"] ],
        processEscapes: true
        }
    });
    </script>
    <script
    type="text/javascript"
    charset="utf-8"
    src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
    >
    </script>
    <script
    type="text/javascript"
    charset="utf-8"
    src="https://vincenttam.github.io/javascripts/MathJaxLocal.js"
    >
    </script>
    {% endif %}
    ```

1. `_includes/head.html` 수정

    ```html
    <head>
        ...
        {% include mathjax.html %}
    </head>
    ```

1. 수식을 쓸 page의 front matter 설정

    ```markdown
    ---
    ...
    mathjax: true
    ---
    ```

[Adding MathJax to a GitHub Pages Jekyll Blog][adding_mathjax_to_jekyll]를 따라했다. 단, 설명과 달리, [minima][jekyll_minima] theme의 head.html을 수정해서 모든 page에 적용했다.

## [KaTeX][katex] 지원

1. `_config.yml` 수정

    ```yml
    ...
    kramdown:
      math_engine: mathjax
    ...
    ```

    [GitHub Pages][github_pages]에서 강제하지만, local [jekyll][jekyll]에서 혼란이 없도록 적어준다.

1. `_includes/katex_head.html` 추가

    ```html
    {% if page.katex %}
    <!-- Load jQuery -->
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>

    <!-- Load KaTeX -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.css" integrity="sha384-zB1R0rpPzHqg7Kpt0Aljp8JPLqbXI3bhnPWROx27a9N0Ll6ZP/+DiW/UqRcLbRjq" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.js" integrity="sha384-y23I5Q6l+B6vatafAwxRu/0oK/79VlbSz7Q9aiSZUvyWYIYsd+qj+o24G5ZU2zJz" crossorigin="anonymous"></script>

    <!-- KaTeX fonts -->
    <script>
        window.WebFontConfig = {
        custom: {
            families: ['KaTeX_AMS', 'KaTeX_Caligraphic:n4,n7', 'KaTeX_Fraktur:n4,n7',
            'KaTeX_Main:n4,n7,i4,i7', 'KaTeX_Math:i4,i7', 'KaTeX_Script',
            'KaTeX_SansSerif:n4,n7,i4', 'KaTeX_Size1', 'KaTeX_Size2', 'KaTeX_Size3',
            'KaTeX_Size4', 'KaTeX_Typewriter'],
        },
        };
    </script>
    <script defer src="https://cdn.jsdelivr.net/npm/webfontloader@1.6.28/webfontloader.js" integrity="sha256-4O4pS1SH31ZqrSO2A/2QJTVjTPqVe+jnYgOWUVr7EEc=" crossorigin="anonymous"></script>
    {% endif %}
    ```

1. `_includes/katex_tail.html` 추가

    ```html
    {% if page.katex %}
    <script type="text/javascript">
    $("script[type='math/tex']").replaceWith(
        function(){
        var tex = $(this).text();
        return "<span class=\"inline-equation\">" + 
                katex.renderToString(tex) +
                "</span>";
    });
    
    $("script[type='math/tex; mode=display']").replaceWith(
        function(){
        var tex = $(this).text();
        return "<div class=\"equation\">" + 
                katex.renderToString("\\displaystyle "+tex, {displayMode:true}) +
                "</div>";
    });
    </script>
    {% endif %}
    ```

1. `_includes/head.html` 수정

    ```html
    <head>
        ...
        {% include katex_head.html %}
    </head>
    ```

1. `_layouts/default.html` 수정

    ```html
    <html>
    ...
    </html>

    {%- include katex_tail.html -%}
    ```

1. 수식을 쓸 page의 front matter 설정

    ```markdown
    ---
    ...
    katex: true
    ---
    ```

[jekyll][jekyll]에서 [KaTeX][katex]를 사용하는 설정은 Cheng Xu의 blog [Rendering LaTeX using KaTeX and Jekyll][latex-katex-jekyll]을 기반으로 했다. jQuery로 `<script type="math/tex">`를 바꿔치기하는 일종의 꽁수인데, 잘 동작한다. [KaTeX][katex] 문서에 나온 설정은 잘 동작하지 않아서 포기했다.

## Markdown에 수식을 쓸 때 주의할 것

### 수식의 delimiter는 `$$...$$`만 사용

[kramdown][kramdown]이 `inline`이건 `display`건 `$$...$$`를 사용한다.  [kramdown][kramdown]은 수식의 위치에 따라서, `inline`과 `display`를 구분하므로, delimiter가 같아도 문제가 되지 않는다.

* 문장 중간에 오는 `$$...$$`

    ```html
    <script type="math/tex">...</script>
    ```

* 별도 paragraph의 `$$...$$`

    ```html
    <script type="math/tex; mode=display">...</script>
    ```

### 여러 수식의 정렬은 `\begin{aligned}...\end{aligned}`만 사용

[mathJax][mathjax]와 [KaTeX][katex]를 둘 다 지원하려면, 여러 수식을 정렬할 때, `\begin{aligned}...\end{aligned}`만 사용해야 한다.

[WIKIBOOK: LaTeX/Advanced Mathematics][latex_adv_math]에 따르면, 'aligned'는

* `align`과 비슷
* `다른 mathematics environment 안에서 사용`

이어서, 기본은 `align`인 것 같은데,

* [mathJax][mathjax] - `align`, `aligned` 둘 다 지원
* [KaTeX][katex] - `aligned`만 지원

이므로, markdown에서 `aligned`만 사용해야 한다.

GitHub: jgm/pandoc issue #3953 "[KaTeX can now support the align environment][katex-align-issue]"에 관련 언급이 있는데, 자세히 보지는 않았다. :)

[KaTeX][katex]에서 `\begin{equation}...\end{equation}` 지원되지 않는 것을 볼 때, [KaTeX][katex]는 수식 모드만 구현했다고 생각해도 될 것 같다.

## Test Page

같은 markdown을 front matter에서 `katex: true`, `mathjax: true`만 바꿔서 2개의 page를 만들었다. 개인적으로는 KaTeX의 출력이 좀 더 마음에 든다. 둘 사이의 큰 차이는 없다.

* [KaTeX test](/test/math_engine-katex)

    ```markdown
    ---
    layout: page
    title: Test KaTeX
    permalink: /test/math_engine-katex
    lang: ko
    katex: true
    ---

    문장 중에 오는 경우: $$H(x) = - \sum_{x \in{X}}{P(x) log_2{P(x)}}$$

    문장 밖에 오는 경우

    $$
    H(x) = - \sum_{x \in{X}}{P(x) log_2{P(x)}}
    $$
    ```

* [mathJax test](/test/math_engine-mathjax)

    ```markdown
    ---
    layout: page
    title: Test mathJax
    permalink: /test/math_engine-mathjax
    lang: ko
    mathjax: true
    ---

    문장 중에 오는 경우: $$H(x) = - \sum_{x \in{X}}{P(x) log_2{P(x)}}$$

    문장 밖에 오는 경우

    $$
    H(x) = - \sum_{x \in{X}}{P(x) log_2{P(x)}}
    $$
    ```

## 기타

* [Google Trends - mathJax, KaTeX 비교][google_trends-mathjax-katex]

{% endraw %}

[vscode]: https://code.visualstudio.com/
[github_pages]: https://pages.github.com/
[jekyll]: https://jekyllrb.com/
[mathjax]: https://www.mathjax.org/
[katex]: https://katex.org/
[kramdown]: https://kramdown.gettalong.org/
[kramdown-katex]: https://kramdown.gettalong.org/math_engine/katex.html
[markdown_math]: https://marketplace.visualstudio.com/items?itemName=goessner.mdmath
[latex_adv_math]: https://en.wikibooks.org/wiki/LaTeX/Advanced_Mathematics
[svg]: https://en.wikipedia.org/wiki/Scalable_Vector_Graphics
[katex-align-issue]: https://github.com/jgm/pandoc/issues/3953
[github-pages-config]: https://github.com/github/pages-gem/blob/master/lib/github-pages/configuration.rb#L50-L55
[google_trends-mathjax-katex]: https://trends.google.com/trends/explore?date=today%205-y&q=mathJax,KaTeX
[adding_mathjax_to_jekyll]: http://sgeos.github.io/github/jekyll/2016/08/21/adding_mathjax_to_a_jekyll_github_pages_blog.html
[latex-katex-jekyll]: https://xuc.me/blog/katex-and-jekyll/
