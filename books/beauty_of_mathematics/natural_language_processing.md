---
layout: page
title: 자연어 처리
permalink: /tech/beauty_of_mathematics/natural_language_processing.html
lang: ko
katex: true
---

### 통계언어 모델 / n-gram model

* wikipedia: [Language model][wikipedia:language-model] 참고

한 문장 $$S$$가 $$m$$개의 단어 $$w_1, w_2, \cdots, w_m$$로 만들어졌을 때, 이 단어들이 비교 대상 문장들에 얼마나 나왔는지로, 문장 $$S$$가 맞는 문장인지 판단할 수 있다.

$$
\begin{aligned}
P(S) &= P(w_1, \cdots, w_m) \\
     &= P(w_1) \cdot P(w_2 | w_1) \cdot P(w_3 | w_1, w_2) \cdots P(w_m | w_1, ... w_{m-1}) \\
     &= \prod^{m}_{i=1}{P(w_i | w_1, \cdots, w_{i-1})}
\end{aligned}
$$

여기서, 모든 단어에 대한 조건부 확률을 따지기는 힘드니, 앞의 $$n$$개 단어에 대한 조건부 확률만 따지는 것이 n-gram model이다.

$$
P(S) \approx \prod^{m}_{i=1}{P(w_i | w_{i - (n-1)}, \cdots, w_{i-1})}
$$

그런데, 비교 대상 문장들이 충분히 많으면, 확률은 개수를 세어서 알 수 있다.

$$
P(w_i | w_{i - (n-1)}, \cdots, w_{i-1}) = \frac{count(w_{i - (n-1)}, \cdots, w_{i-1}, w_i)}{count(w_{i - (n-1)}, \cdots, w_{i-1})}
$$

즉, **학습 자료의 단어가 몇 개인지 세는 것만으로, 문장을 검사할 수 있다.**

* $$n = 1$$ : unigram model
* $$n = 2$$ : bigram model
* $$n = 3$$ : trigram model
* $$n = 4$$ : tetragram model
  * 복잡도가 높아서 잘 사용하지 않는다.
  * Google에서 사용

### n-gram model 구현의 문제

n-gram model을 구현할 때, 학습 자료에 없는 조합이 문제가 된다.

$$
P(S) = \begin{cases}
    0,          & count(w_{i - (n-1)}, \cdots, w_{i-1}, w_i) = 0 \\
    unknown,    & count(w_{i - (n-1)}, \cdots, w_{i-1}) = 0
\end{cases}
$$

따라서, [Good–Turing frequency estimation][wikipedia:good-turing-freq-estm]과 같이 0이 아닌 값으로 바꾼다.

$$N$$개로 이루어진 dataset에서

* r회 등장하는 단어: $$N_r$$개
* 안나오는 단어: $$N_0$$개

이면,

$$
N = \sum^{\infty}_{r=1}{r N_r}
$$

이다. 여기서,

$$
d_r = (r + 1) \frac{N_r + 1}{N}
$$

이면,

$$
N = \sum^{\infty}_{r=1}{d_r N_r}
$$

이 된다. 일반적으로 $$d_r < r$$이지만, $$d_0 > 0$$이므로, 안나오는 단어의 경우에도 계산이 가능해진다.

Bi-gram의 경우 다음과 같다.

$$
P(w_i | w_{i-1}) = \begin{cases}
f(w_i | w_{i-1}), & \phantom{0 < } \#(w_{i-1}, w_i) \ge T \\
f_{gt}(w_i | w_{i-1}), & 0 < \#(w_{i-1}, w_i) < T \\
Q(w_{i-1}) \cdot f(w_i).
\end{cases}
$$

여기서, 한계값 $$T$$는 $$8 \sim 10$$, $$f_{gt}$$는 Good-Turing estimation이다.

$$
Q(w_{i-1}) = \frac{1 - \sum_{w_i seen}{P(w_i | w_{i-1})}}{\sum_{w_i unseen}{f(w_i)}}
$$

이면,

$$
\sum_{w_i \in V}{P(w_i | w_{i-1})} = 1
$$

이다.

Tri-gram에서는 다음과 같다.

$$
P(w_i | w_{i-2}, w_{i-1}) = \begin{cases}
f(w_i | w_{i-2}, w_{i-1}), & \phantom{0 < } \#(w_{i-2}, w_{i-1}, w_i) \ge T \\
f_{gt}(w_i | w_{i-2}, w_{i-1}), & 0 < \#(w_{i-2}, w_{i-1}, w_i) < T \\
Q(w_{i-2}, w_{i-1}) \cdot P(w_i | w_{i-1}).
\end{cases}
$$

n-gram model들을 interpolation해서 쓰기도 한다.

$$
\begin{aligned}
P(w_i | w_{i-2}, w_{i-1}) & = \lambda(w_{i-2}, w_{i-1}) f(w_i | w_{i-2}, w_{i-1}) \\
& + \lambda(w_{i-1}) f(w_i | w_{i-1}) \\
& + \lambda f(w_i)
\end{aligned}
$$

### 형태소 분석

형태소 분석은 중국어 분석에 중요하다. 영어도 필기체는 공백이 불확실해서, 필기체 인식에 형태소 분석 기술이 사용된다. 형태소 분석에도 통계언어 모델이 사용된다. 한 문장 $$S$$가 다음과 같이 다르게 끊을 수 있다고 하자.

$$
A_1, A_2, \cdots, A_k \\
B_1, B_2, \cdots, B_m \\
C_1, C_2, \cdots, C_n
$$

이 때, $$A_1, A_2, \cdots, A_k$$가 최적이면,

$$
\begin{aligned}
P(A_1, A_2, \cdots, A_k) &> P(B_1, B_2, \cdots, B_m) \\
P(A_1, A_2, \cdots, A_k) &> P(C_1, C_2, \cdots, C_n)
\end{aligned}
$$

이다.

분야에 따라, 유리한 끊는 길이가 다르다. 번역은 긴게 유리하고, 검색은 짧은게 유리하다. `칭화`만 입력해도 `칭화대학교`를 찾아주는 예를 보면 알 수 있다.

#### Hidden Markov Model

통신은 수신한 $$O_1, O_2, \cdots$$에서, 송신된 $$S_1, S_2, \cdots$$를 알아내는 기술이다.

$$ 정보 \xrightarrow{S_1, S_2, \cdots} 채널로 전달 \xrightarrow{O_1, O_2, \cdots} 수신 $$

이 때, 추정할 수 있는 여러 $$S_1, S_2, \cdots$$ 중 가장 가능성이 높은 경우를 찾아야 한다.

$$
S_1, S_2, \cdots = arg_{all S_1, S_2, \cdots} max P(S_1, S_2, \cdots | O_1, O_2, \cdots)
$$

[Bayes' theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem)를 적용하면, $$P(O_1, O_2, \cdots)$$가 상수이므로 (수신후에 변하지 않으므로)

$$
\begin{aligned}
P(S_1, S_2, \cdots | O_1, O_2, \cdots) &= \frac{P(O_1, O_2, \cdots | S_1, S_2, \cdots) \cdot P(S_1, S_2, \cdots)}{P(O_1, O_2, \cdots)} \\
&\approx P(O_1, O_2, \cdots | S_1, S_2, \cdots) \cdot P(S_1, S_2, \cdots)
\end{aligned}
$$

가 된다. 여기서,

* $$P(O_1, O_2, \cdots \| S_1, S_2, \cdots)$$ : $$S_1, S_2, \cdots$$을 encoding하면 $$O_1, O_2, \cdots$$일 확률
* $$P(S_1, S_2, \cdots)$$ : decoding했을 때, 말이 되는가.

이다.

Markov는 $$S_t$$의 확률분포는 그 직전인 $$S_{t-1}$$에만 영향을 받는다고 가정했다.

$$
P(S_t | S_1, S_2, \cdots, S_{t-1}) = P(S_t | S_{t-1})
$$

다음과 같은 Markov 연쇄가 있을 때,

* *$$m_1 \xrightarrow{1.0} m_2$$*
* *$$m_2 \xrightarrow{0.6} m_3$$*
* *$$m_2 \xrightarrow{0.4} m_4$$*
* *$$m_3 \xrightarrow{0.7} m_3$$*
* *$$m_4 \xrightarrow{0.3} m_4$$*

$$
\begin{aligned}
P(S_{t+1} = m_3 | S_t = m_2) &= 0.6 \\
P(S_{t+1} = m_4 | S_t = m_2) &= 0.4
\end{aligned}
$$

이다. 어떤, Markov 연쇄가 $T$ 시간동안 동작해서 $$S_1, S_2, \cdots, S_T$$ 상태를 만들면,

$$
\frac{\#(m_i, m_j)}{\#(m_i)}
$$

와 같이 개수를 세서, 조건부 확률을 알 수 있다.

Hidden Markov model은 markov 연쇄에서 결과인 $$O_1, O_2, \cdots$$는 알지만, $$S_1, S_2, \cdots$$는 모르는 상태다. 이 때, 다음과 같다.

$$
\begin{aligned}
P(S_1, S_2, \cdots, O_1, O_2, \cdots) &= \prod_t{P(S_t | S_{t-1}) \cdot P(O_t | S_t)} \\
P(O_1, O_2, \cdots | S_1, S_2, \cdots) &= \prod_t{P(O_t | S_t)} \\
P(S_1, S_2, \cdots) &= \prod_t{P(S_t | S_{t-1})}
\end{aligned}
$$

여기서

* 추이 확률 $$P(S_t \| S_{t-1})$$
* 생성 확률 $$P(O_t \| S_t)$$

를 매개 변수라 한다. 이 매개 변수를 알아내는 것이 모델의 학습이다.

생성 확률은

$$
\begin{aligned}
P(O_t | S_t) &= \frac{P(O_t, S_t)}{P(S_t)} \\
             &= \frac{\#(O_t, S_t)}{\#(S_t)}
\end{aligned}
$$

이므로, 수작업으로 marking된 data가 많으면, 셀 수 있다. 이것을 지도학습이라 한다.

추이 확률은 통계언어 모델의 경우와 같으므로,

$$
\begin{aligned}
P(S_t | S_{t-1}) &= \frac{P(S_{t-1}, S_t)}{P(S_{t-1})} \\
                 &\approx \frac{\#(S_{t-1}, S_t)}{\#(S_{t-1})}
\end{aligned}
$$

가 된다.

Data에 marking하는 것은 비용이 많이 들어서, 대량으로 관측한 신호 $$O_1, O_2, \cdots$$ 만으로 $$P(S_t \| S_{t-1})$$과 $$P(O_t, S_t)$$를 추산하기도 하는데, 이를 unsupervised learning이라고 한다.

#### 바움-웰치 알고리즘

매개변수 $$\theta_1, \theta_2$$가 2개의 모델 $$M_{\theta_1}, M_{\theta_2}$$를 구성할 때, 같은 출력 $$O_1, O_2, \cdots$$이 나올 수 있음. 하지만, 이 중 한 model의 생성 확률이 높을 것임. 이런 model $$M_{\hat{\theta}}$$를 찾는 방법이 바움-웰치 알고리즘이다.

1. *$$O_1, O_2, \cdots$$* 이 생성되는 $$\theta_0$$를 찾는다. 이 매개변수로 초기모델 $M_{\theta_0}$을 만든다.
1. *$$P(O \| M_{\theta_0})$$* 를 바탕으로 더 좋은 모델을 찾는다.
1. *$$P(O \| M_{\theta_1}) > P(O \| M_{\theta_0})$$* 인 $$\theta_1$$을 찾는다.

* EM process: expectation - maximization

바움-웰치 알고리즘의 약점: local maxima는 찾을 수 있으나, convex function이 아니면, global maxima는 못 찾을 수도 있다. 따라서, 비지도학습의 결과가 지도학습보다 좀 후지다.

학습 알고리즘과 디코딩 알고리즘이 필요.

* 바움-웰치 알고리즘 - 학습 알고리즘
* 비터비 알고리즘 - 디코딩 알고리즘

#### 정보 엔트로피

* Wikipedia: [Entropy (information theory)][wikipedia:entropy-information-theory] 참고

1948년 클로드 섀넌의 논문 "A Mathematic Theory of Communication"에 등장.

$$
H(x) = - \sum_{x \in{X}}{P(x) log_2{P(x)}}
$$

예) 32개 팀 중 우승팀을 맞추는 문제의 정보 엔트로피는 다음과 같다.

$$
H = -(p_1 \cdot log_2{p_1} + p_2 \cdot log_2{p_2} + \cdots + p_{32} \cdot log_2{p_{32}})
$$

여기서, 각 팀의 우승 확률 $$p_i$$가 동일하면 32개 팀이므로 $$p_i = \frac{1}{32}$$이고, $$H$$는 다음과 같다.

$$
\begin{aligned}
H &= -32 \cdot log_2{\frac{1}{32}} \\
  &= 5
\end{aligned}
$$

섀넌은 이걸 5bit라고 불렀고, $$H \le 5$$이다.

불확실성은 정보만 제거할 수 있다. 검색에서 어떤 공식이나 머신러닝을 활용하는 것은 추가 정보가 없으므로 더 나쁜 결과가 나온다.

정보 엔트로피를 사용하면 통계언어 모델의 장단점을 측정할 수 있다. $Y$를 알 때, 조건부 확률 $$H(X \| Y)$$는 다음과 같다.

$$
H(X | Y) = - \sum{x \in{X}, y \in{Y}}{P(x, y) \cdot log{P(x, y)}}
$$

따라서, $$H(x) \ge H(X \| Y)$$이므로, unigram보다 bigram이 불확실성이 적다. 같은 식으로 $$Y, Z$$를 알 때,

$$
H(X | Y, Z) = - \sum{x \in{X}, y \in{Y}, z \in{Z}}{P(x, y, z) \cdot log{P(x, y, z)}}
$$

이므로, $$H(X \| Y) \ge H(X \| Y, Z)$$가 되고, trigram이 bigram보다 유리하다.

추가 정보가 불확실성을 전혀 제거할 수 없을 때, $$=$$가 성립한다.

상호정보량(mutual information)은 사건 $$X, Y$$에 대해,

$$
\begin{aligned}
I(X; Y) &= \sum_{x\in{X}, y\in{Y}}{P(x,y) \cdot \log{\frac{P(x, y)}{P(x)P(y)}}} \\
I(X; Y) &= H(X) - H(X | Y)
\end{aligned}
$$

이고, $$0 \le I(X; Y) \le 1$$이다. X와 Y가 무관할 때 0, 100% 연관될 때 1이다. 여기서, $$P(x,y) \cdot \log{\frac{P(x, y)}{P(x)P(y)}}$$은 쉽게 계산할 수 있다.

기계 번역에서 문장안의 단어들의 상호정보량으로 번역의 중의성 문제를 해결한다.
예) bush가 대통령인지, 덤불인지 구분

변수의 상호정보량과 비슷하게 함수의 상대 엔트로피(relative entropy, cross entropy, Kullback-Leibler divergence)는 다음과 같다.

$$KL(f \parallel g) = \sum_{x\in{X}}{f(x) \cdot log{\frac{f(x)}{g(x)}}}$$

상대 엔트로피는 다음과 같은 특징을 갖는다.

1. *$$f \equiv g \iff KL(f \parallel g) = 0$$*
1. 상대 엔트로피가 크면 두 함수의 차이도 크고, 작으면 차이도 작다.
1. 확률분포 또는 확률 밀도 함수가 취하는 값이 0이면, 상대 엔트로피로 두 확률 분포의 차이성을 계량할 수 있다. (?)

$$KL(f \parallel g) \ne KL(g \parallel f)$$

여서 다루기 까다로와서,

$$JS(f \parallel g) = \frac{1}{2}[KL(f \parallel g) + KL(g \parallel f)]$$

가 사용된다.

#### 추천 도서

* Thomas Cover. Elements of Information Theory
  * 교보문고나 알라딘은 하드커버만 파는데, 너무 비싸다.

#### 참고

* 우쥔. [수학의 아름다움](/books/beauty_of_mathematics.html).
* Wikipedia: [Language model][wikipedia:language-model]
* Wikipedia: [Good–Turing frequency estimation](https://en.wikipedia.org/wiki/Good%E2%80%93Turing_frequency_estimation)
* Wikipedia: [Frederick Jelinek](https://en.wikipedia.org/wiki/Frederick_Jelinek)

[wikipedia:language-model]: https://en.wikipedia.org/wiki/Language_model
[wikipedia:good-turing-freq-estm]: https://en.wikipedia.org/wiki/Good%E2%80%93Turing_frequency_estimation
[wikipedia:entropy-information-theory]: https://en.wikipedia.org/wiki/Entropy_(information_theory)
