# Facebook: 내 정보 다운로드

Facebook에 내가 올린 글이나, 댓글등을 HTML이나 JSON으로 download할 수 있음.

## 요청

> 설정 > 내 Facebook 정보 > 내 정보 다운로드 | 보기

여기서 선택을 한 후 `파일 만들기`를 누르면 요청됨.

## 받기

얼마 후에 email로 download link가 전송됨.

## JSON string encoding

JSON으로 받은 경우에 string이 \uXXXX 형태로 encoding이 되어 있음.

### 방식

1. unicode로 저장
1. unicode를 UTF-8으로 변환
1. UTF-8 code를 1 byte씩 \u00xx 형태로 출력

영문 font만 있는 영어권 개발자들을 위해서 UTF-8을 직접 출력하지 않았다고 추정. 하지만, 그런 목적이면, unicode 자체를 \uXXXX 형태로 출력하는 것이 맞다고 생각됨.

### 예

``` text
각
```

은 unicode로

``` unicode
AC01 = 1010 1100 0000 0001
```

이고, UTF-8로 변환하면

``` text
*1110* 1010 = EA
*10*11 0000 = B0
*10*00 0001 = 81
```

이것이 JSON에는

``` json
\u00ea\u00b0\u0081
```
로 적힘.

### 의견

Facebook의 encoding은 bug로 봐야함. JSON spec에서 \uXXXX에서 XXXX를 `4 hexadecimal digits`라고 정의했고, `Basic Multilingual Plane (U+0000 through U+FFFF)`에 해당되는 경우에 하나의 \uXXXX로 표시될 수 있다고 설명한 것으로 보아, \u00XX는 XX 1 byte가 아니라 00 XX의 2 byte로 변환되는게 맞음.
따라서, fix_fbjson같은 프로그램이 필요함.

### 참고

- [Wikipedia: UTF-8](https://en.wikipedia.org/wiki/UTF-8)
- [Unicode: Hangul Syllables / Range: AC00–D7AF](https://www.unicode.org/charts/PDF/UAC00.pdf)
- [The JSON Data Interchange Syntax](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)
