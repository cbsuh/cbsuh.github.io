---
layout: page
title: 게임 개발에 관한 생각들
permalink: /game-dev/
lang: ko
---

## Item Transaction

과거의 구현 방식

* Client에서 시작된 item과 관련된 action은 전 과정에서 관련 item 정보를 갖고 다닌다.
  * 장점: 서버가 정보를 cache하지 않으므로, 구조가 단순.
  * 단점: 모든 packet에 관련 정보를 추가해야 해서, coding자체가 귀찮아짐.

각 주체별로 한번에 하나의 action만 가능하게 하는게 맞지 않을까?

* client에서 item 관련 action은 한번에 하나만 허용?
* character와 player의 action 구분?
  * 이 경우에는 character의 action은 server에서 시작되는 것이 맞지 않을까?

모든 action에는 initiator가 있어야 함.

* debugging, logging에 필요.
* player action이 server에서 timer action을 만드는 경우는 어떻게?

## Server의 Update 단위

* 상호 작용이 필요한 actor들의 partition 별로 update
  * 이미 이런 식의 시도가 많았는데, 엄청 큰 partition이 나오면 의미가 없음.

## 자동 사냥에 대해서

* 무식한 수집퀘 / 닥사는 자동 사냥이 좋음.
* mobile 환경에서는 어쩔 수 없이 지원해야 하는지도.
* 자동 사냥으로 개발되면, bot으로 test 환경 구축도 가능.
