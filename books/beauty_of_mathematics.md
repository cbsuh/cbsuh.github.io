---
layout: page
title: 우쥔. 수학의 아름다움.
permalink: /books/beauty_of_mathematics.html
lang: ko
katex: true
auto_ids: true
---

* 원제: 數學之美 (2014년)
* ISBN: 9788984077546

[![알라딘 커버 이미지](https://image.aladin.co.kr/product/17971/15/cover500/8984077542_1.jpg)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=179711500)

책을 읽다 보니, 자꾸 공부하듯이 필기를 하게 된다. 내가 이해할 수 있는 수준으로 책이 쓰여 있어서 그런가보다.

책은 xxx 부분으로 나뉘어 있다. 각 부분은 해당 분야의 거장에 대한 소개로 끝난다.

* 컴퓨터의 자연어 처리
  * 통계언어 모델
  * 형태소 분석
  * hidden Markov model


정보를 언어로 encoding하는 것과 채널에 encoding하는 통신의 유사성을 파악하고, 통신에서 사용되던 hidden Markov model을 자연어 처리에 도입한 젤리넥의 통찰력은 놀랍다. 학교에서 hidden Markov model을 배울땐, 어디에 쓸 지 몰랐다는 저자의 말도 와닿는다.

정보가 있어야 더 나은 추정이 가능하다. 정보없이 어떤 공식등에 의존하는 것은 거짓말이다.



문제를 해결할 때, 데이타를 수집하고, 데이타를 기반으로 모델을 만든다.
데이타가 없이 직관에 의존하면 안됨.

1. 데이타 수집
1. 모델 만들기
1. 모델 optimize



꼭 알아야할 것: 조건부 확률 계산 - 베이즈 공식?

엔지니어가 잘 모르는 분야를 직관에만 의존해서 해결하려는 것은 나쁜 버릇 - 데이타가 필요



* [코사인 법칙과 뉴스의 분류](/beauty_of_mathematics/코사인_법칙과_뉴스의_분류)
* [행렬 연산과 텍스트 처리의 두 가지 분류 문제](/beauty_of_mathematics/행렬_연산과_텍스트_처리의_두_가지_분류_문제)
* [정보 지문과 응용](/beauty_of_mathematics/정보_지문과_응용)
* [중국 드라마 `암산`에서 떠올린 암호학의 수학 원리](/beauty_of_mathematics/중국_드라마_암산에서_떠올린_암호학의_수학_원리)
* [반짝인다고 다 금은 아니다](/beauty_of_mathematics/반짝인다고_다_금은_아니다)

### 지은이 추천 책/다큐

* 조지 가모프. [1 2 3 그리고 무한](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=16407165).
* 스티븐 호킹. [시간의 역사](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=636131).
* [Through the Wormhole](https://en.wikipedia.org/wiki/Through_the_Wormhole)




#### 컴퓨터의 자연어 처리

언어는 정보를 encoding/decoding한다.

$$ 정보 \xrightarrow{encoding} 언어 \xrightarrow{decoding} 정보 $$

이는 통신의 과정과 유사하다. 통신의 이론을 가져온 통계언어 모델(Statistical Language Model, SLM)의 결과가 좋은 이유다.

과거에는 컴퓨터로 자연어를 처리할 때, 문법에 기반한 구문 분석을 사용했다. 하지만, 자연어는 문맥에 따라 뜻이 달라지는 경우가 많다. 많은 문법 규칙을 추가해도, 구문 분석의 결과는 좋지 않았다.

통계언어 모델(Statistical Language Model, SLM)에서는 한 문장안의 연이은 단어들이, 실제 문장에서는 얼마나 등장했는지로, 이 문장이 맞을 확률을 계산한다.

$$
\begin{aligned}
P(S) &= P(w_1, w_2, \cdots, w_n) \\
     &= P(w_1) \cdot P(w_2 | w_1) \cdot P(w_3 | w_1, w_2) \cdots P(w_n | w_1, w_2, ... w_{n-1})
\end{aligned}
$$

만약, 앞의 한 단어와 같이 나오는 경우만 따지는 bi-gram model이면, (앞의 N 단어까지 고려하면, [N-gram Model](n-gram-model))

$$
\begin{aligned}
P(S) &= P(w_1) \cdot P(w_2 | w_1) \cdot P(w_3 | w_2) \cdots P(w_n | w_{n-1}) \\
     &\approx \frac{\#(w_1)}{\#} \cdot \frac{\#(w_1, w_2)}{\#(w_1)} \cdot \frac{\#(w_2, w_3)}{\#(w_2)} \cdots  \frac{\#(w_{n-1}, w_n)}{\#(w_{n-1})}
\end{aligned}
$$

가 되어서, 학습에 사용되는 문장들에서 단어들의 등장 회수를 세기만 하면, 계산할 수 있다. ([두 단어의 조건부 확률 근사](#두-단어의-조건부-확률-근사) 참고)

