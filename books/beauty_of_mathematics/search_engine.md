---
layout: page
title: 자연어 처리
permalink: /books/beauty_of_mathematics/natural_language_processing
lang: ko
mathjax: true
---

우쥔에 따르면, 검색은 3가지 요소로 구현된다.

* 웹페이지 다운로드
* 색인 만들기
* 배열 (이건 무슨 의미인지 아직 모르겠음)

### Page Rank

> 한 페이지의 가중치는 그 페이지를 가리키는 페이지들의 가중치로 결정된다. 그런데, 처음에 각 페이지의 가중치가 동일하다고 가정해도, 페이지들간의 link의 개수를 계속 곱하면, 언젠가 실제 가중치에 수렴한다.

* 웹 페이지사이의 link 개수 $$A$$
  * 웹페이지 $$m$$에서 $$n$$으로의 link 개수 : $$a_{m,n}$$

  $$
  A = \begin{pmatrix}
    a_{1,1} & \cdots & a_{1,n} & \cdots & a_{1,M}   \\
    \vdots  & \ddots & \vdots  & \ddots & \vdots    \\
    a_{m,1} & \cdots & a_{m,n} & \cdots & a_{m,M}   \\
    \vdots  & \ddots & \vdots  & \ddots & \vdots    \\
    a_{M,1} & \cdots & a_{M,n} & \cdots & a_{M,M}
  \end{pmatrix}
  $$

* Page rank $$B$$
  * 웹페이지 : $$N$$개
  * 각 page의 rank : $$b_i$$

  $$ B = (b_1, b_2, \cdots, b_N)^T $$

모든 page의 rank가 같다고 하면 $$B_0$$은 다음과 같다.

$$ B_0 = (\frac{1}{N}, \cdots, \frac{1}{N})^T $$

이제, 다음의 공식으로 $$B_1, B_2, \cdots$$를 계산하면, $$B_i$$가 $$B$$에 가까와진다.

$$ B_i = A \cdot B_{i-1} $$

따라서, $$B_i$$와 $$B_{i-1}$$의 차이가 적당히 작아질 때까지만 계산을 반복하면 된다.

> Google에서 이 작업은 sparse matrix를 사용해서 반자동, 반수동으로 진행되다가, 2003년 map reduce가 개발되면서 완전 자동화 되었다.

실제 상황에서는 link의 개수가 0인 경우가 많아서 다음과 같이 작은 상수 $$\alpha$$로 보정한다.

$$ B_i = \Big[ \frac{\alpha}{N} \cdot I + (1 - \alpha)A \Big] \cdot B_{i-1} $$

학습 set에 등장하지 않는 경우를 확률 계산에 포함시키는 방법과 유사하다 :)

### 웹페이지와 질문의 관련성 - TF-IDF

검색어가 "원자력의 응용"인 경우, 각 가중치는 다음과 같아야 한다.

* 원자력
  * 전문용어
  * 가중치가 커야 함
* 의
  * 불용어
  * 가중치가 0이어야 함.
* 응용
  * 통용어
  * 가중치가 전문용어보다 작아야 함.

> TF - Term Frequency : 한 페이지에서 한 단어의 개수를 전체 단어의 개수로 나눈 수

질문이 $$N$$개의 keyword $$w_1, \cdots, w_N$$일 때, 이 질문과 한 페이지의 관련성은 다음과 같다.

$$ TF_1 + \cdots + TF_N $$

Keyword $$w$$가 $$D_w$$개의 페이지에 등장할 때, $$D_w$$가 클수록 $$w$$의 가중치가 높다고 볼 수 있다.

> IDF - Inverse Document Frequency

전체 웹페이지의 개수가 $$D$$일 때, IDF는 다음과 같다.

$$ IDF = \log_2{\frac{D}{D_w}} $$

10억개의 웹페이지가 있으면 $$D = 10억$$이고

* `의`가 모든 페이지에 나오면, $$D_w = 10억$$

    $$
    \begin{aligned}
    IDF &= \log_2{\frac{10억}{10억}}  \\
        &= \log_2{1}                  \\
        &= 0
    \end{aligned}
    $$

* `원자력`은 전문용어이니, $$D_w = 200만$$정도 라고 보면,

    $$
    \begin{aligned}
    IDF &= \log_2{\frac{10억}{200만}} \\
        &= \log_2{500}                \\
        &= 8.96
    \end{aligned}
    $$

* `응용`은 통용어이니 좀 많이 나와서, 5억개가 나온다면,

    $$
    \begin{aligned}
    IDF &= \log_2{\frac{10억}{5억}}   \\
        &= \log_2{2}                  \\
        &= 1
    \end{aligned}
    $$

따라서, IDF가 앞의 가중치 조건을 만족한다.

> TF-IDF: Term Frequency - Inverse Document Frequency

위를 바탕으로 관련성을 다음과 같이 정의한다.

$$ TF_1 \cdot IDF_1 + \cdots + TF_N \cdot IDF_N $$

* TF-IDF는 특정 조건에서 키워드 확률분포의 쿨백-라이블러 발산임 (?)

#### TF-IDF의 정보이론적 근거

코퍼스(?)가 $$N$$개일 때, $$w$$의 정보량은 다음과 같다.

$$
\begin{aligned}
I(w) &= -P(w) \log{ P(w) }                          \\
     &= -\frac{TF(w)}{N} \log{ \frac{TF(w)}{N} }    \\
     &= \frac{TF(w)}{N} \log{ \frac{N}{TF(w)} }     \\
\end{aligned}
$$

여기서, $$N$$이 상수이므로 생략하면, 다음과 같다.

$$
I(w) = TF(w) \log{ \frac{N}{TF(w)} }
$$

추가 조건

* 문헌의 크기가 $$M$$개 단어로 동일하다.

    $$
    \begin{aligned}
    M &= \frac{N}{D}    \\
    &= \frac{ \sum_w{TF(w)} }{D}
    \end{aligned}
    $$

* keyword가 문헌에 등장하면, 공헌도가 같다. 그러면 다음과 같다.

    $$
    c(w) = \begin{cases}
        \frac{TF(w)}{D(w)},     \\
        0.
    \end{cases}
    $$

여기서 $$c(w) < M$$이면,

$$
\begin{aligned}
I(w) &= TF(w) \log{ \frac{N}{TF(w)} }                   \\
     &= TF(w) \log{ \frac{M \cdot D}{c(w) \cdot D(w)} } \\
     &= TF(w) \log{ \Big( \frac{D}{D(w)} \cdot \frac{M}{c(w)} \Big)}
\end{aligned}
$$

따라서, 다음과 같다.

$$
TF-IDF(w) = I(w) - TF(w) \cdot \log{ \frac{M}{c(w)} }
$$

* 한 단어의 정보량 - $$I(w)$$이 클수록 TF-IDF가 커진다.
* 한 단어 $$w$$가 명중하는 문헌에서 $$w$$의 평균 등장 회수가 많을수록 TF-IDF가 커진다.

### 참고 자료

* [우쥔. 수학의 아름다움](/books/beauty_of_mathematics)
