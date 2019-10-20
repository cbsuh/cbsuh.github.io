# CBSUH의 낙서장

Facebook에 끄적이던 것들을 GitHub Pages로 옮겨본다.

## 작업 방식

* 작업은 별도의 branch에서 한다.
* 작업이 끝나면 먼저 branch에 push 한다.
* branch에 push된 작업 중 publish할 것을 master에 merge 한다.

## Local Computer에서 확인하기

* Jekyll을 설치하면 됨. [GitHub Pages를 내 컴퓨터에서 확인하기](https://cbsuh.github.io/tech/setup_jekyll_for_github_pages.html) 참고.
* `--host`, `--port` option 지원.

    ``` bash
    bundle exec jekyll serve --host 192.168.0.2 --port 80
    ```
