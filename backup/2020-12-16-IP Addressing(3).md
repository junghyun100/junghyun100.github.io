---
layout: post
title: "네트워크 - IP Addressing(3)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Classless Inter-Domain Routing(CIDR)

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1216/CIDR.PNG">

서브넷하고 반대되는 개념인 <strong>CIDR</strong>은 <strong>Classless Inter-Domain Routing</strong>의 약자입니다.

이것은 서브넷하고 반대인 <strong>supernetting</strong> 이라는 이름으로 부르기도 합니다. 

### 왜 사용을 하는가?

`Class B`의 경우에는 앞에 16 bit가 네트워크 ID로 사용 됩니다. 

앞의 2 bit가 1,0 이어야 하고, 나머지 14 bit를 가지고 네트워크 ID를 구별하므로, 

`16,000개` 정도의 네트워크가 존재 할 수 있습니다.

인터넷이 전 세계로 민간으로 퍼지고 상업용, 교육용도로 퍼지면서 

모든 회사, 학교 이런 기관을 생각하다 보니까 

`class B` 주소를 필요로 하는 기관이 16,000개를 훨씬 초과했습니다.

그렇다고 `class C`는 호스트 ID가 2의 8승의 갯수를 가지기 떄문에

어떤 하나의 기관이 네트워크 주소를 하나 받는다고 해도

그 안에 2의 8승 개 밖에 지원을 못 하는 단점이 있었습니다.

그래서 서브넷의 반대로 <strong>수퍼넷(CIDR)</strong>라는 것으로 `Class C` 주소를 뭉쳐서, 

몇 개를 뭉쳐서 `Class B`에 가까운 형태로 쓸 수 있지 않을까라는 생각을 한 것 입니다.

### 예를 들어봅시다. 

`192.168`이라는 숫자는 `class A, B, C` 기준으로 보면 `class C`에 해당 하는 것입니다.

`192.168.1`, `192.168.2` 그리고 `192.168.3`도 네트워크 ID인 것입니다.

이것을 이렇게 숫자들을 쭉 실제 bit로 써 놓고 보면

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1216/CIDR2.PNG">

<strong>22 bit</strong>까지 겹치는 것을 볼 수 있습니다.

`subnet Part`가 겹치고 `host ID`가 다른 네 개의 연속 된 `class C`를 

하나의 기관에 할당 하고 이 앞의 <strong>22 bit를 네트워크 ID인 것 처럼 간주</strong>하는 것 입니다.

라우터들은 `Address format`의 뒤에 있는 `x`라는 이 숫자를 가지고 

어느 bit 까지를 네트워크 ID로 간주 해 주는지 파악합니다.

그러면 결국은 이 네 개의 연속된 `class C` 주소를 받은 기관은 

2의 8승이 아니라 2의 10승 개의 호스트를 사용하는 것 같은 효과를 얻을 수 있습니다.

## 결론

<strong>CIDR</strong>, <strong>supernetting</strong> 이라는 것은 더 낮은 레벨의 더 작은 네트워크 여러 개를, 

연속 된 네트워크를 모아서 하나의 네트워크처럼 보이게 하는 것 입니다.

---
