---
layout: post
title: "L3 Switch vs Router의 차이점"
tags: [Network]
comments: true

---

이번 포스팅에서는 L3 Switch vs Router의 차이점을 간단히 정리했습니다..

---

## Switch와 Router란?

#### Switch
네트워크 단위들을 연결하는 통신 장비로서 소규모 통신을 위한 허브보다는 전송 속도가 개선된 것이다.<br>
> 종류로는 MAC 브리지, 스위칭 허브, 포트 스위칭 허브등이 있다.<br>

데이터를 필요로 하는 컴퓨터에 전송하기 위해 컴퓨터의 고유한 MAC주소를 기억하고 있어야한다.<br>
이렇게 저장하기 위한 공간을 MAC Address Table이라고 한다.<br>
Switch가 가지고 있는 MAC주소와 Port번호가 저장된다.<br>

#### Router
패킷의 위치를 추출하여, 그 위치에 대한 최적의 경로를 지정하며,<br>
이 경로에 따라 데이터 패킷을 다음 장치로 전향시키는 장치이다.<br>
> 간단히 말해 서로 다른 네트워크 간에 중계 역할을 해준다.

Router에서도 데이터를 다른 컴퓨터에 전송하기 위해서는<br>
컴퓨터의 고유한 Ip주소를 기억하고 있어야한다.<br>
이렇게 저장하기 위한 공간을 Routing Address Table이라고 한다.<br>
패킷의 Destination 주소를 NetMask을 적용하여 Network Destination을 결정한 후<br>
적당한 Interface로 보낸다.<br>



### 그래서 차이점은?

이번 포스팅의 핵심이다.<br>

<strong>스위치</strong>는 일반적으로 데이터링크 계층(L2)에서, <strong>라우터</strong>는 네트워크 계층(L3)에서<br>
사용된다고 학교에서는 배운다. 물론 나도 통신프로토콜 교과목 당시 교수님께서 그렇게 배웠다.<br> 
그러나 Swtich는 L3뿐만아니라 다른 계층에서 동작하는 제품도 존재한다는 사실을 알았을 때<br>
이부분에 대해서 질문하러 찾아갔던 기억이 있다.<br>

| 주요 특징 | Router | L3 Switch |
|:-----|:----:|-----:|
| 주요 수행 OSI Layer  | Layer 3  | Layer 3 |
| Routing 수행 방법  | Software (CPU + Software) | Hardware(ASIC chip) |
| forwarding performance  | Slow(CPU성능과 가격에 따라 다름)  | Fast |

간단하게 정리한 사항은 위와 같다.<br>
가장 큰 차이점은 <strong>"ASIC chip을 통한 성능적인 측면에서 차이가 난다."</strong>라고 생각하면 된다. 

<br>


---
