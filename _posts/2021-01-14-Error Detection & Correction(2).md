---
layout: post
title: "네트워크 - Error Detection & Correction(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

이전에 포스트 했던 <strong>Parity checking</strong>은 

간단한 편이고 오버헤드는 적은 기술이지만,

실제로 감지를 할 수 있는 에러의 형태는 매우 제약되었습니다.

그래서 사용되는 방식이 아래의 <strong>CRC 방식</strong>입니다.

# Cyclic Redundancy Check(CRC)

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0114/Cyclic%20Redundancy%20Check.PNG">

<strong>CRC</strong>는 여러 가지 에러 감지 기술중에서 가장 널리 쓰이는 기술입니다.

에러 감지나 수정하는 기술은 항상 

뒤에 `EDC, error detection과 connection`을 위한

`redundant`한 데이터가 붙습니다.

그림에서 보면 원래 보내려고 하는 데이터가 `D`라고 표현 된 이 부분이고,

에러 체크를 위한 `CRC 비트`를 뒤에 붙이는 겁니다.

`redundant`한 데이터의 크기에 따라서, 

크면 클 수록 더 많은 동시 비트 에러를 감지 할 수 있지만

무작정 크게 하기엔 전체적인 오버헤드가 있기 때문에 잘 결정을 해야 합니다.

## 어떻게 redundant한 데이터를 붙이는가?

일단 이 `CRC 기술`에서는 모든 데이터를 비트열로 봅니다.

여기서 알아야할 점은 `sender`와 `receiver` 사이에 

서로 약속 된 `G`라는 코드가 있다는 겁니다.

`CRC의 길이`를 `r 비트`라고 하고 싶다면 `G`는 `r+1 비트`의 패턴이 되어야 되고,

`G`가 서로 약속이 되면 `sender`는 `데이터(D)`에다가 `CRC(R)비트`를 붙이는데

이 때, 어떻게 해서 붙이냐면 이 전체를 `receriver`가 받아서 `G`로 나누었을 때

딱 나누어 떨어지도록 `R`을 뒤에 붙입니다.

그래서 `G`로 나누어서 나누어 떨어지면 에러가 없는 것이고, 

나누어 떨어지지 않으면 에러가 있는 것이다고 판단합니다.

# CRC Principle: Sender

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0114/CRC%20principle_Sender.PNG">

뒤에 붙이는 `R`이라는 데이터를 붙여서 `receiver`가 받았을 때

`G`로 나누어서 전체가 나누어 떨어지도록 어떻게 만드는지 알아봅시다.

먼저 `D`라는 데이터가 있다고 하면 그것에 2의 `r`승을 곱합니다.

2의 `r`승을 곱한다는 것은 

뒤에 `r 비트` 만큼의 공간을 남기고 

데이터를 좌측 `r 비트` 만큼 shift를 한다는 것 입니다.

그리고 생성된 공간에 `CRC 코드`를 붙일 수 있도록 마련합니다.

`CRC 코드`를 `XOR(exclusiveOR)` 하게 되면은 

공간안에 `R(CRC 코드)`이 들어가는 형태가 됩니다.

우리가 하고 싶은 것은 `receiver`가 이것을 받아서 `G`로 나누었을때 

딱 나누어 떨어지게끔하는 만들고 싶다는 겁니다.

그러기 위해서 이 첫번 째 식의 양 변에 `exclusiveOR R`을 합니다.

이것은 `XOR R`을 또 한 것이기 때문에 0이 되고,

```
D * 2^r = nG XOR R
```

위의 형태가 됩니다.

이것을 양 변을 `G`로 나눠 보면은 이 부분은 나누어 떨어질테고 

`R`은 `D`에 `2의 r승`을 곱한 이것을 `G`로 나누었을 때 나머지가 되도록 `R`을 붙이면

`receiver`가 받아서 `G`로 나누었을때 나누어 떨어지게 됩니다.

---
