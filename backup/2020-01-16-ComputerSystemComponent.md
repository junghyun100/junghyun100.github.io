---
layout: post
title: "Computer System Component"
tags: [OS]
comments: true
---
 
해당 Post는 OS에 들어가기에 앞서 Computer System Component를 정리한 파일이다.

---

### Computer System Component<br>

#### 1.Processor(CPU = Central processing Unit)
 <img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C.png" width= "350px" height ="350px" alt="My Image">

중앙 처리 장치(CPU)는 명령어를 해석하는 컴퓨터의 한 부분이다.
마이크로프로세서(Microprocessor)는 마이크로컴퓨터의 한 부분이다.

#### 2.BUS
BUS는 프로세스를 비롯한 각 장치 간 또는 서스시스셈을 서로 연결하여 정보를 주고받을 수 있게 하는 통로
> 위치에 따라 내부, 외부 버스로 구분된다.

#### 3.Register

프로세서에 위치한 고속메모리, 프로세서가 바로 사용할 수 있는 데이터를 담는다.
특수한 값 하나를 저장하는 "기억공간"으로 사용된다.
 
> 구분
<ul>
<li>용도 - 전용레지스터, 범용레지스터</li>
<li>정보의 종류 - 데이터레지스터, 주소레지스터, 상태레지스터</li>
<li>정보변경 여부 - 사용자 가시 레지스터, 사용자 불가시 레지스터</li>
</ul>

##### 사용자 가시 레지스터
1. 데이터 레지스터 - 함수의 연산에 필요한 데이터 저장
2. 주소 레지스터 - 주소나 유효주소를 계산시에 필요한 주소의 일부 저장

##### 사용자 불가시 레지스터
1. 프로그램 카운터 - 프로그램을 수행할 때 명령어 실행 순서를 보관한다 = 다음에 실행할 명령어의 주소 저장
2. 명령어 레지스터 - 현재 수행하는 명령어를 저장한다
3. 프로그램 상태 레지스터 - 플러그와 같은 상태 정보를 저장한다
4. 메모리 주소 레지스터 = 접근하려는 메모리의 주소 저장
5. 메모리 버퍼 레지스터 - 메모리에서 정보를 읽을 떄 또는 정보를 저장할 때 사용한다

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/register.PNG" width= "600px" height ="440px" alt="My Image">
 

학부시절 마이크로프로세서 공부를 할때 배웠던 내용들이라 감회가 새롭습니다.<br>
당시에는 Operand나 Operator가 왜 그렇게 안 외워 졌는지 모르겠으나.. 포스팅을 위해서 다시 한번 공부중입니다.<br>
조금 더 공부후 레지스터간의 관계와 메모리에 대해서 포스팅 하겠습니다.<br>

---
