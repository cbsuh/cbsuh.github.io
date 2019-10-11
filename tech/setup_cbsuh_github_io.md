# cbsuh.github.io 만들기

[GitHub Pages][github_pages]가 xxx.github.io repository를 publish해주는 기능임.

1. Jekyll 설치
    - WSL을 사용
    - [jekyll: Jekyll on Windows][jekyll_on_windows] | `Installation via Bash on Windows 10` 참고
    - 특이 사항
        - `jekyll 4.0.0` 설치됨

## 기타

### Theme

- GitHub에 있는 [minima][github_jekyll_theme_minima] 사용 예정
- [`settings | Choose a theme`][github_theme_chooser]에는 minima가 나오지 않음
  - 기본으로 나오는게 minima인가?
  - default만 변경하면 되나?
    - [jekyll: Themes | Overriding theme defaultsPermalink][jekyll_themes_overriding_defaults] 참고
- chooser에서 고른 theme은 front matter가 없어도 markdown에는 자동 적용
  - html에는 front matter가 있어야 함

### Plugins

GitHub은 다음 plugin들을 항상 사용. local을 위해서 설정해야 하는가?

``` text
jekyll-coffeescript
jekyll-default-layout
jekyll-gist
jekyll-github-metadata
jekyll-optional-front-matter
jekyll-paginate
jekyll-readme-index
jekyll-titles-from-headings
jekyll-relative-links
```

[github_pages]: <https://pages.github.com/>
[jekyll_on_windows]: <https://jekyllrb.com/docs/installation/windows>
[github_jekyll_theme_minima]: <https://github.com/jekyll/minima>
[github_theme_chooser]: <https://help.github.com/en/articles/adding-a-theme-to-your-github-pages-site-with-the-theme-chooser>
[jekyll_themes_overriding_defaults]: <https://jekyllrb.com/docs/themes/#overriding-theme-defaults>
