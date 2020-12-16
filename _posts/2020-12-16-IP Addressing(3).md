---
layout: post
title: "네트워크 - IP Addressing(3)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Subnet

<strong>Sub(아래를 뜻하는 접두사)</strong>와 <strong>net(네트워크)</strong>의 합성어 입니다.

<strong>네트워크를 좀 더 잘게 나눈 개념</strong>으로 생각하면 편합니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1215/Subnet.PNG">

실제로 IP 네트워크는 class A,B,C로 나눕니다.

`class B`의 네트워크 ID를 가지는 기관이라고 하면 

밑에 `2의 16승`개의 호스트를 서비스 할 수 있습니다.

인터넷에서의 데이터그램 포워딩은 라우터에 의해서 수행이 되고,

라우터는 네트워크 ID를 보고 데이터들을 전달해 줍니다.

외부에서 어떤 데이터가 기관 네트워크의 라우터에 도달 했을 때,

라우터는 자신의 하위에 있는 목적지 네트워크에 데이터를 전달 해 주어야 합니다.

여기서 많은 기관들에서 class B 주소를 받았다 하더라도

class B에 65,000개 정도의 호스트들 하나의 네트워크로 관리 하는 것은 

<strong>maintenance 하는데 너무 비효율적</strong>이라 판단했습니다.

그래서 이것을 나누는 방식을 사용했습니다.

실제 `ICANN`에서 정한 주소 체계에는 맞지 않지만 

임의로 나누어서 학교를 예로 들면 engineering 빌딩에 해당하는 네트워크,

manufacturing을 위한 빌딩 또는 재무, 경영 담당 등으로 

각각의 별도의 네트워크 인 것 처럼 논리적으로 나눕니다.

그것이 <strong>서브 네트워킹</strong>입니다. 

이것이 마치 분할하고 합해진다 해서 <strong>Divide and conquer</strong>하다고 이야기 하기도 합니다.

# Subnet Mask

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1215/Subnet%20Mask.PNG">

서브 네트워크를 한다는 것은 기관에서 부여 받은 `네트워크 ID`가 있고,

네트워크 ID에 따라서 사용 할 수 있는 호스트 넘버를 `Subnet Number`와 `Host Number`로 나눕니다.

`Subnet Number`는 `Subnet ID`로 간주를 해서 `Host Number`의 공통의 값을 갖는 네트워크들은

하나의 동일한 서브 네트워크로 간주를 하고 다시 `Host Number`로 구별하게끔 사용 합니다.

그럼 몇 bit 까지를 서브넷 넘버로 사용하는지 어떻게 알아내는지 알아봅시다.

예를 들어 어떤 기관에서는 `class B` 주소를 받았습니다.

그러면 총 16 bit가 호스트 넘버인데 이 중에 앞의 `8 bit`를 서브넷 네트워크 넘버로 쓰고 싶습니다.

이것은 기관에서는 하나의 서브 네트워크 안에는 256개 이하의 컴퓨터만이 

이 서브넷에 도착 할 수 있게끔 서브넷을 잘게 나누고 싶다는 뜻입니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1215/Subnet%20Mask%20-%20%EB%B3%B5%EC%82%AC%EB%B3%B8.PNG">

> 주소를 class B 주소로 가정합니다.

그림에서 처럼 서브넷 마스크에 `255.255.255.0` 으로 적혀있다면 

세번째 `255`부분은 호스트 부분으로 8 bit 중에 앞의 8 bit를 `11111111`로 씀으로 인해서

이것을 `서브넷 ID`로 쓰겠다는 뜻입니다. 

만약에 7 bit만 쓰겠다 하면 11111111에서 마지막은 `11111110`으로 바꿉니다.

그럼 `255`가 아니라 `254`가 되고, `255.255.254.0`는 해당 기관에서는 

앞의 7 bit만을 `서브넷 ID`로 쓰겠다는 뜻이 됩니다.

그리고 뒤의 9 bit를 호스트 넘버로 쓰겠다는 것 입니다.

하나의 서브넷에서 2의 8승 개 까지 밖에 호스트를 두지 못하지만 

만약에 앞의 7 bit를 서브넷 ID로 쓴다고 하면 2의 9승 개 까지의 호스트를 

하나의 서브넷 안에 둘 수 있게 됩니다.

그럼 <strong>512대의 컴퓨터를 하나의 서브넷에서 관리</strong> 할 수 있는 것입니다.

# Subnetworking Example

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1215/Subnet%20Working%20Example.PNG">

만약에 기관에서는 154.71로 class B에 해당 하는 주소를 사용합니다.

이곳을 한 서브넷 내에 2,000대의 컴퓨터는 지원 하는 서브넷으로 나누고 싶습니다.

그러기 위해서는 `11 bit(약 2000대의 컴퓨터)`정도를 호스트 ID로 두어야합니다.

2,048대의 컴퓨터를 하나의 서브넷에 둘 수 있고 앞의 상위 5 bit는 서브넷 ID로 쓴다 그러면

서브넷 마스크를 `11111111.11111111.11111000.00000000` = `255.255.248.0` 로 사용합니다.

이렇게 설정한다면 하나의 서브넷 내에 2,048대의 컴퓨터까지 지원 할 수 있게 되는 것이고,

기관 내에 서브넷의 개수는 `2의 5승(32개)`의 서브넷이 존재하게 됩니다.

# Subnetworking Example2

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1215/Subnet%20Working%20Example%20classC.PNG">

class C의 경우, 표준 자체가 3 byte가 네트워크 ID 입니다.

이 경우에는 서브넷 마스크가 있다 하더라도 class C 네트워크 자체가 

하나의 서브넷 마스크로 사용 되는 형태입니다. 

이렇게 실제 `네트워크 ID가 서브넷 ID가 구별 되지 않는 경우`도 있을 수 있습니다.

---
