---
layout: post
title: "네트워크 - Error Detection & Correction"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Error Detection & Correction Principles

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0113/Error%20Detection%20and%20Correction%20Principles.PNG">

먼저 에러 감지와 수정에 대한 어떤 원칙이 있는지 알아봅니다.

에러 감지, 수정을 하는 방법도 여러 가지가 있지만 

어떠한 방법이든 한 가지 공통적인 것이 있습니다.

바로 <strong>redundancy</strong>를 이용 한다는 점입니다.

> redundant : "중복하다" 는 뜻

데이터를 `5 bit`를 보내는 상황을 가정할 때,

`5 bit` 만을 보내서는 어디서 에러가 발생 한지 모르기 때문에.

`5 bit`를 두 번 연달아 보내거나, 

`5 bit`에 `부가적인 정보`를 좀 더 추가를 해서 

`부가 정보`를 보고 원 데이터가 에러가 있었는지를 알아 냅니다.

그래서 그림에서 처럼 데이터그램에 `원 데이터`를 담고

부가적으로 <strong>EDC(Error Detection and Correction bit)</strong>를 함께 전송합니다.

이렇게 함으로써 원래 전송하려고 하는 데이터보다 

더 많은 비트의 데이터를 전송 함으로써 오버헤드 되었지만

목적지에 도달 했을 때 데이터에 에러가 있었는지를 검사 할 수 있습니다.

그런데 어떠한 <strong>detection, correction 방법</strong>이라도 완벽하지는 않습니다. 

그래서 완벽하게 하려면 더 많은 `부가 정보`를 붙이거나 반복해야합니다.

그러나 부가 정보가 크면 클 수록 정확성이 높아지기는 하겠지만 

그만큼 오버 헤드가 커지기 때문에 

둘 사이에서 데이터 `전송의 정확성`과 데이터 `전송의 오버헤드` 사이에서 

트레이드 오프를 생각 해서 결정을 해야합니다.

# Parity Checking

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0113/Parity%20Checking.PNG">

에러 감지와 에러 수정을 위한 가장 기본적인 방법은 <strong>parity checking</strong>입니다.

## Single bit parity

보내는 쪽과 받는 쪽에서 항상 약속을 해서 

보내는 데이터의 `1`의 개수가 짝수든 홀수가 되게 한 bit를 붙입니다.

그래서 예를 들어봅시다.

### For example

```

예로 나와 있는 데이터 영역을 보면 1이 아홉 개가 있습니다.

`sender`과 `receiver`가 서로 약속을 해서 보내는 패킷의 1의 개수를 맞춥니다.

만약에 `sender`와 `receiver`가 `odd parity`를 쓰기로 했다면

`sender`는 전송 할 데이터를 살펴 보고 

1의 개수가 이미 홀수 개이기 때문에 

마지막 `parity bit`를 0을 붙여서 보냅니다.

`receiver`가 데이터를 받았을 때 원래 1이었던 bit가 0이 된다던지,

그 반대가 된 경우 프레임이 전송 중에 에러가 난 것을 알 수 있게 됩니다.

```

> 홀수가 되도록 하면 odd parity

> 짝수가 되도록 하면 even parity

전체 데이터 길이가 얼마가 되던 간에 

`odd parity`와 `even parity`를 만드는 것은 딱 `한 bit`만 있으면 되기 때문에

오버헤드는 굉장히 낮지만 에러가 `두 bit` 이상이 동시에 발생 했을 때는 

에러를 감지 하지 못하므로 성능이 많이 떨어지는 방식입니다.

## Two-dimensional bit parity

`parity`를 좀 더 다르게 쓰는 방법들도 있습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0113/Parity%20Checking.PNG">

예시로 밑에 나와 있는 <strong>2차원 형태의 bit parity</strong>를 사용하는 방식입니다.

데이터들을 `행렬 형식`으로 두고 `row parity` 행 쪽으로 parity를 취하고

`column parity` 열 쪽으로도 parity 값을 취하게 이렇게 했습니다.

그래서 이 경우에는 `even parity`를 썼는데. 

예를 들어서 보내려고 하는 데이터가 `15 bit` 라면

`5 bit`씩 세 줄로 나눈 다음,

행 쪽으로 `row parity`와 세로 쪽으로 `column parity`를 

1의 개수가 짝수가 되게끔 만듭니다.

이렇게 전송을 했을 때 `receiver`가 중간의 1이었던 bit가 

전송 중에 0으로 바뀌어서 에러가 발생 했면 찾아낼 수 있습니다.

이 방식도 `bit` 에러가 `한 bit` 정도 일 때 

에러가 발생 했는지를 감지 할 수 있습니다.

## 결론

`parity`는 매우 간단하고 오버헤드도 굉장히 적은 기술이지만

감지를 할 수 있는 에러의 형태는 매우 제약적입니다.

---
