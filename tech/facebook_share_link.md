# Facebook: 링크 공유

cbsuh.github.io의 page를 facebook으로 공유할 때, image가 다른 site 처럼 예쁘게 나오지 않아서 조사를 함.

## 이미지 선정 기준

* [Open Graph](https://ogp.me/)로 설정된 이미지

또는

* 첫번째 이미지

## 이미지 크기

* `600 x 315`보다 작고, `200 x 200`보다 크면 작은 정사각형으로 표시
* `600 x 315`보다 크면, 옆으로 길쭉한 직사각형
* `1.91:1`에 가까우면 잘리지 않음
* `og:image:width`와 `og:image:height`는 원본 이미지의 크기를 변경하지는 못함

내 경우에는 알라딘의 책 이미지를 사용했으므로 500 x 707 이었음. 따라서, 위아래가 잘린 상태로 좌우로 길쭉하게 보여짐.

## 참고

* [facebook for developers: 웹 마스터용 공유 가이드](https://developers.facebook.com/docs/sharing/webmasters)
* [facebook for developers: 공유 디버거](https://developers.facebook.com/tools/debug/sharing/)
* [facebook for developers: 링크 공유 FAQ](https://developers.facebook.com/docs/sharing/webmasters/faq)
