---
layout: post
title: "네트워크 - Error Detection & Correction(3)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# CRC Principle : Receiver

<a href="https://junghyun100.github.io/Error-Detection-&-Correction(2)/">CRC Principle: Sender 관련 포스트</a>

이전 포스트에서는 `Sender` 측에서 `CRC`를 추가한 데이터를 보냈습니다.

이번에는 이 데이터를 전달 받은 `receiver`는 이제 어떻게 하는지 살펴봅니다.

  ![CRC Principle_Receiver](/images/2021년/0115/CRC Principle_Receiver.PNG)

그림의 오른쪽을 예로 봅니다.

## For Example

`101110011`이라고 되어 있는 데이터를 서로 약속 하고 있는 `1001(G)`로 나눕니다.

쭉 계산을 하고나면 나머지가 `000`이 나온다는 것을 알 수 있습니다.

이런 경우는 `bit`가 프레임이 제대로 전송 되었다고 하며,

이 중 `1 bit`라도 바뀌게 되면 나머지가 `0`이 나오지 않으므로 

해당 프레임에는 문제가 있다고 판단 할 수 있습니다.

그래서 이 `CRC` 기술은 매우 간단하면서도 강력하기 때문에 

이더넷이나 와이파이 같은데 널리 쓰이고 있습니다.

그러나 `CRC`는 <strong>에러를 감지할 수는 있지만,</strong> 

<strong>어떤 bit가 바뀌었는지는 알 수가 없습니다.</strong>

# Error Correction

  ![Error Correction](/images/2021년/0115/Error Correction.PNG)

그래서 에러를 실제로 수정 할 수 있는 방법이 있지 않을까 고민을 했고

그 결과 나온 기술을 통틀어서 <strong>FEC(Forward error correction)</strong>라고 하거나

`channel coding 기술`을 써서 `FEC 서비스를 제공`한다고 말을 합니다.

<strong>FEC 기술</strong>에서는 다 `block code`라는 것을 쓰게 되는데 

이 코드에 사용하는 몇 가지 공통적인 용어들이 있습니다.

## For example

일단 예를 들어서 `k`라는 <strong>길이의 데이터를</strong> 보내고자 합니다.

여기에 `Detection`이나 `Correction`을 위해 `redundant한 데이터`를 붙여,

`sender`가 전송하는 데이터를 `코드워드(codeword)` 라고 합니다.

<strong>코드워드의 길이</strong>를 `n` 이라고 할 때,

<strong>redundancy</strong>는 `(n-k)/k`

즉, 전체 내가 실제 `k bit`의 데이터를 보내기 위해서는 

`n-k`라는 길이의 `redundant`한 데이터를 붙여야 된다는 뜻 입니다.

그래서 전체 <strong>code rate</strong>이라는 것은 `k/n`으로 볼 수 있는데

예를 들어 `1 bit`의 데이터를 보내기 위해서 

`1 bit`의 부가 정보를 붙여서 보내야 된다면 `code rate`는 `1/2`이 됩니다.

`code rate`의 값이 작으면 작을 수록, 

`redundant`한 데이터가 많이 들어가고 그 만큼 데이터를 받았을 때 

수정 해 낼 수 있는 가능성은 높아지게 됩니다.

그러나 그만큼 오버헤드가 커지기 때문에 그런 부분을 잘 고려해야 합니다.

# FEC : Hamming Code

  ![FEC : Hamming Code](/images/2021년/0115/FEC_Hamming Code.PNG)

<strong>FEC 기술</strong> 중에서 또 많이 쓰이는 기술이 <strong>hamming code</strong>라는 기술입니다.

예를 들어 그림에서와 같은 시스템을 가정해봅시다.

`k`라는 메시지의 길이는 `2 bi`t, `n`라는 물리적으로 전송해야 되는 `bit`는 `5`인

`Code rate`는 `2/5`가 됩니다.

`5 bit` 중에서 `2 bit`만 유효한 데이터이고,

나머지 `3 bit`는 `redundant`한 데이터라는 뜻입니다.

보내려고 하는 데이터가 실제로 `2 bit`씩 전송 한다면 경우는 `4가지` 입니다.

```
`00`을 전송 해야 될 때 `00`을 전송 하는게 아니라 `00000` 을 전송 하고

`01`을 전송 하려고 할 때는 `01`만 전송 하는게 아니라 `00111` 을 전송 하고

`10`을 전송 하려고 할때, `11`을 전송 하려고 할 때 각각 표의 워드코드를 보낸다는 겁니다.
```

그러면 어떤 경우가 생길 수 있냐 하면 `receiver`가 어떤 패턴을 받았는데 

받은게 `00100`일 때, `00100`은 이 워드코드에 없기 때문에 잘못된 데이터입니다.

그러면 여기서 이 `00100`을 유효한 워드코드 네 가지 하고

각 워드코드들과 비교해서 <strong>bit의 개수가 얼마나 차이가 나는지</strong> 확인합니다.

`FEC`가 에러를 수정한다고 해서 

항상 정확하게 수정하는 것은 아니고 `확률적으로 수정`하는데 

이 중에서 원래 어떤 데이터였을 확률이 가장 높은가를 생각 해 보는 겁니다.

그러므로 `4가지 코드` 중 가장 가까운 숫자인 `00000`로 수정을 하게 됩니다.

이렇게 수정된 코드 자체도 물론 틀렸을 수 있습니다.

그 경우 상위 계층인 네트워크 계층으로 올라 갔을 때 `checksum`에서 걸리게 됩니다.

> hamming distance : 두 개의 코드워드 사이 bit의 차이

# FEC : Hamming Code(2)

  ![FEC : Hamming Code2](/images/2021년/0115/FEC_Hamming Code2.PNG)

`5 bit`를 보냈을 때, `5 bit`로 표현 할 수 있는 코드워드는 `2의 5승`으로 `32가지` 입니다.

`32가지` 중에 어떻게 해서 네 개의 코드워드를 선택 했느냐 살펴 보면

네 개의 코드워드 사이에 `hamming distance를 비교 해 보면 

`최소 3의 distance`가 있다는 겁니다.

`distance`의 차이가 1씩 난다면 에러가 생긴건지 생기지 않은 건지 

알기가 어렵기 때문에 <strong>서로간의 hamming distance가 가장 먼 4가지</strong>를 골라낸 겁니다.

만약 `1 bit`가 바뀌더라도 `2 bit`의 차이가 나니까

에러를 수정 할 때는 <strong>"가장 가까운 코드워드가 `sender`에서 전송 되었던 코드일 것이다."</strong>

`2 bit` 에러가 발생하면 `detection`까지는 할 수 있게 됩니다.

---
