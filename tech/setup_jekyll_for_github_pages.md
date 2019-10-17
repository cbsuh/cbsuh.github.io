---
layout: page
title: GitHub Pages를 내 컴퓨터에서 확인하기
permalink: /tech/setup_jekyll_for_github_pages.html
lang: ko
---

## 요약

- [GitHub Pages][github_pages]가 GitHub에 \<user>/\<user>.github.io repository를 web으로 publish해주는 서비스임.
- 내 컴퓨터에 [Jekyll][jekyll]을 설치해서, GitHub에 push를 하기 전에 layout등을 점검하자.

## 환경

- [Windows Subsystem for Linux (WSL)][wsl] 위의 ubuntu

## 설치

### Jekyll 설치

[Jekyll on Windows / Installation via Bash on Windows 10][jekyll_on_windows_wsl]를 따라 했음.

``` bash
sudo apt-get update -y && sudo apt-get upgrade -y
```

``` bash
sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
sudo apt-get install ruby2.5 ruby2.5-dev build-essential dh-autoreconf
```

``` bash
gem update
```

``` bash
gem install jekyll -v 3.8.5
gem install bundler
```

#### 주의 사항

Jekyll의 version을 `3.8.5`로 명시해서 설치했음.

- version을 명시하지 않으면, 최신 version인 `4.0.0`이 설치됨.
- [GitHub Pages: Dependency versions][github_pages_dependency_versions]에 따르면, GitHub은 `3.8.5` 사용.
- `4.0.0`은 [GitHub plugin][github_pages_jekyll_plugins]들이 지원하지 않음.

### GitHub Repository 만들기

[GitHub Help: Creating a GitHub Pages site with Jekyll / Creating a repository for your site][github_pages_create_repository]를 따라 했음.

1. `New repository` 선택

    ![New repository](https://help.github.com/assets/images/help/repository/repo-create.png)

1. Owner 선택

    ![Owner](https://help.github.com/assets/images/help/repository/create-repository-owner.png)

1. Repository 이름 입력

    - 계정이 xxx면 xxx.github.io로 만들어야 함.

    ![Repository name](https://help.github.com/assets/images/help/pages/create-repository-name-pages.png)

1. 공개 설정

    ![Public / Private](https://help.github.com/assets/images/help/repository/create-repository-public-private.png)

### GitHub의 Repository를 내 컴퓨터로 가져오기

``` bash
git clone git@github.com:xxx/xxx.github.io.git
```

### Jekyll Site 만들기

``` bash
cd <path-to-xxx.github.io>
jekyll new .
```

- folder에 file들이 존재하는 경우에 `jekyll new . --force` 필요.
- [GitHub Help: Creating a GitHub Pages site with Jekyll / Creating your site][github_pages_create_site]에는 `"jekyll VERSION new ."`가 된다는데, 에러 난다.
- `index.md`가 있었어도, jekyll이 무시하고 덮어쓴다. `4.0.0`은 기존의 `index.md`를 남겨놓고, 별도로 `index.markdown`을 만든다.

### `_config.yml` 수정

1. GitHub Plugin들을 추가.

    ``` yaml
    plugins:
      - jekyll-feed
      - jekyll-coffeescript
      - jekyll-default-layout
      - jekyll-gist
      - jekyll-github-metadata
      - jekyll-optional-front-matter
      - jekyll-paginate
      - jekyll-readme-index
      - jekyll-titles-from-headings
      - jekyll-relative-links
    ```

    - 목록은 [About GitHub Pages and Jekyll / Plugins][github_pages_jekyll_plugins] 참고.

1. `title`, `email`, `description`도 맞게 설정하자.

    ``` yaml
    title: ...
    email: ...
    description: >- # this means to ignore newlines until "baseurl:"
      ...
    ```

### `Gemfile` 수정

``` ruby
source "https://rubygems.org"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
#gem "jekyll", "~> 3.8.5"

# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.5.0"

# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
gem "github-pages", "~> 201", group: :jekyll_plugins

# If you have any plugins, put them here!
group :jekyll_plugins do
gem "jekyll-feed", "~> 0.11.0"
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.0" if Gem.win_platform?
```

- `gem "jekyll" ...` 제거
- `gem "github-pages" ...` 추가
- [GitHub Pages: Dependency versions][github_pages_dependency_versions]에 따르면, 현재 `github-pages`의 version은 `201`.
- `minima`의 version을 `2.5.0`으로 수정
- `jekyll-feed`의 version을 `0.11.0`으로 수정

### Jekyll 실행

``` bash
bundle update
```

``` bash
bundle exec jekyll serve
```

- `Auto-regeneration may not work on some Windows versions.`이라는 경고가 나오지만, `Auto-regeneration`도 잘 된다. `WSL` 덕분일까?

[github_pages]: https://pages.github.com/
[github_pages_dependency_versions]: https://pages.github.com/versions/
[github_pages_jekyll_plugins]: https://help.github.com/en/articles/about-github-pages-and-jekyll#plugins
[github_pages_create_repository]: https://help.github.com/en/articles/creating-a-github-pages-site-with-jekyll#creating-a-repository-for-your-site
[github_pages_create_site]: https://help.github.com/en/articles/creating-a-github-pages-site-with-jekyll#creating-your-site

[jekyll]: https://jekyllrb.com/
[jekyll_on_windows]: https://jekyllrb.com/docs/installation/windows/
[jekyll_on_windows_wsl]: https://jekyllrb.com/docs/installation/windows/#installation-via-bash-on-windows-10

[wsl]: https://docs.microsoft.com/en-us/windows/wsl/about
