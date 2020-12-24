---
layout: post
title: "네트워크 - User Datagram Protocol"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# UDP

User Datagram Protocol의 약자로서 

어떤 부가적인 기능이 제공 되고 있지 않고, 

전송 계층 프로토콜의 가장 기본인 multiflexing, demultiflexing 기능만 

거의 제공하고 있습니다.

TCP와 달리 어떤 커넥션을 설정하고 보내는 데이터가 아니기 때문에 

`connectionless service`라고도 이야기를 합니다.

특별한 어떤 커넥션을 설정하고 거기에 연달아서 데이터를 보내는 형태가 

아니기 때문에 각각의 `UDP segment`들은 독립적으로 취급이 됩니다.

또한 비 신뢰적인 서비스입니다.

데이터가 없어 질 수도 있고, 에러가 포함 되어 있을 수도 있고 잘 도달한다 

하더라도 순서가 뒤 바뀔 수 있습니다.

전체적으로 보면 안 좋은 것 같은데,

한 가지 좋은 점은 속도가 빠르다는 점입니다.

UDP는 보통 에러가 포함이 되더라도 속도가 빨라야 되는,

대표적으로 멀티미디어 응용들에 많이 사용이 되고,

도메인 네임 시스템처럼 속도가 빨라야하는 경우 사용 됩니다.

어떤 연결을 설립하기 위해서 google.com과 같은

사이트 URL에 맞는 주소를 가져와야 되는데 속도가 중요하기 때문에 

DNS를 사용 합니다.

`Simple Network Management Protocol`이라고 해서 

네트워크 관리 프로토콜이 있는데 여기에도 역시 UDP를 사용 합니다.

만약에 UDP를 사용하는 어떤 애플리케이션에서 신뢰성이 필요하다고 하면,

애플리케이션(응용) 프로그래머가 직접 구현을 해야 됩니다.

UDP에서 제공하는 `segment`에 에러가 포함 되어 있다, 포함 되어 있지 않다 
 
이 정도 정보는 제공 할 수 있는데 그 정보를 기반으로 에러가 포함되어 있다면 

응용 프로그램이 직접적으로 재전송을 요구한다던지

거기에 맞는 어떤 에러 보정을 한다던지 프로그램의 신뢰성을 높일 수 있습니다.

## UDP Segment Header

<img src= "https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1124/UDP%20header.PNG">

UDP도 서비스를 제공하기 위해서는 정보가 있어야 되고, `sender`가 UDP segment를 만들어서 

데이터를 담아서 보낼 때 헤더에 그런 정보를 담아서 보내게 됩니다.

UDP는 매우 단순하고 간단한 헤더를 가집니다.

전송 계층에서의 가장 기본적인 기능은 multiflexing, demultiflexing 입니다.

호스트 내에 있는 프로세스를 구별 하기 위한 기본적인 정보가 포트 번호이고, 포트 번호가 맨 상단에 담겨 있습니다.

보낸 프로세스의 포트 번호 16 bit, 받는 프로세스의 포트 번호 16 bit로 32bit를 가지고,

세 번째는 lengh, 길이. 16 bit의 길이 정보가 담겨 있는데 이 길이는 헤더를 포함 한 길이 입니다.

헤더가 지금 위가 4 byte, 아래가 4 byte 총 8 byte이기 때문에 이 데이터 길이 더하기 8 byte가 length 정보에 담기는 것입니다.

Checksum은 다음 장에서 설명합니다.

## UDP의 장점

여러 가지가 써 있기는 simple한 점입니다. 

간단하다는 것은 그 만큼 처리 속도가 빨라질 수 있다는 이야기 입니다.

일단 속도가 빠르고 처리 속도도 빠르지만 전송 할 때 커넥션을 설립하지 않기 때문에 거기서 드는 지연 시간도 없다는 것이고,

하나 하나 segment를 처리 할 때 마다 간단하고 헤더 사이즈도 작습니다.

TCP와 비교 해 보면 TCP 헤더의 40% 미만의 사이즈를 갖는데, 이 헤더 사이즈가 작다는 것은 

내가 이 데이터를 보낼 때 마다 붙여야 되는 헤더의 사이즈가 작다는 뜻입니다.

그러면 전체적으로 내가 전송해야 될 segment의 크기가 작아지는 것이고, 네트워크에도 부담이 적어진다는 장점이 있습니다.

UDP의 또 하나의 장점은 

TCP에서는 `congestion control`이라는 것이 있는데 UDP는 없습니다.

`Congestion`이라는 것은 한 마디로 네트워크가 처리 할 수 없을 만큼의 데이터 부하가 걸려서 

네트워크가 제 기능을 하지 못하는 것을 뜻합니다.

각각의 TCP sender들한테 전송 속도를 줄이게 만들어서 네트워크의 congestion 문제를 해결합니다.

UDP는 그런 기능이 없기 때문에 응용 프로그램이 보내려고 하는 데이터가 있으면 눈치 보지 않고 원하는 만큼 보낼 수 있습니다.

## UDP CheckSum

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1124/UDP%20header%20Checksum.PNG">

checksum은 간단하게 말해서 전체 UDP segment에 에러가 포함되어 있는지, 있지 않은 지를 판단하는 근거를 제공하는 필드 입니다.

애초에 UDP는 비 신뢰적인 서비스를 제공한다고 하는데 굳이 그런 것이 필요하겠느냐 생각 할 수 있지만,

상위 응용프로그램이 그 에러에 대해서 어떤 작업을 할지 어떤 행동을 취할지 결정 할 수 있는 근거가 됩니다.

경우에 따라서는 checksum이 전체 중에 에러가 포함 되어 있다, 그런데 어디에 있는지 모르기 때문에

만약에 포트 번호에 에러가 있다면 엉뚱한 곳에 데이터가 전달 될 수 있습니다.

그래서 에러가 있는 경우라면 포트 번호가 잘못 된 것일 수도 있기 때문에 아예 데이터를 버릴 수도 있습니다.

checksum은 매우 간단한 정보이고 어떤 필드, 비트에 에러가 있는지 알 수는 없지만

checksum을 통해서 여러 가지 부가적인 기능을 제공 할 수도 있기 때문에 일단 만들어 두었습니다.

Checksum은 결국 16 bit의 크기를 갖는데 16 bit의 integer 값입니다.

어떤 규칙에 의해서 전체 정보를 기반으로 어떤 규칙에 의해서 checksum을 만들면

`receiver`는 수신 한 전체 segment를 기반으로, 똑같은 규칙으로 다시 checksum을 재 생성 합니다.

만약 둘 사이에 checksum 정보가 일치한다고 하면 전송 중에 이 segment에 문제가 발생하지 않았다는 뜻입니다.

그런데 만약에 `sender`와 `receiver`가 만들어 낸 checksum 사이에 차이가 있다, 일치하지 않는다고 하면

`sender`에서 `receiver`로 전달 되는 동안에 데이터의 일부라던지 헤더의 일부 정보가 변경되었다는 것입니다.

변경 되어서 에러가 있다는 것이므로, checksum이 서로 일치하지 않을 때 어떻게 해라 이런 것이 딱 명확하게 규정 되어 있는 것은 아니지만

그 정보를 이용해서 프로그램의 개발자가 다른 작업을 할 수는 있습니다.

---