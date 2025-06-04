---
layout: post
title: "Computer System Component(3)"
tags: [OS]
comments: true
---
 
해당 Post는 OS에 들어가기에 앞서 Computer System Component를 정리한 파일이다.

---

### Computer System Component(3)<br>

#### Cache
 
 캐시는 처리속도가 빠른 프로세서와 상대적으로 느린 메인 메모리 사이에서 데이터나 정보를 저장하는 고속 버퍼이다.<br>
 메모리 + 캐시 = 캐시 메모리<br>
 위의 구성으로 메모리 가격과 성능을 절충하여 느린 메모리 때문에 발생하는 성능 저하를 줄여준다.
 
 
<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/%EC%BA%90%EC%8B%9C1.PNG" width= "450px" height ="450px" alt="My Image">

<strong>캐시</strong>는 메인메모리에서 일정 블록의 데이터를 가져와 워드단위로 프로세서에 전달하여 정보를 빠르게 제공한다.<br>
또한 데이터가 이동할 수 있는 통로를 확대하여 프로세서와 메인메모리의 속도차이를 줄여주는 역할을 한다.<br>

<strong>프로세서</strong>는 메인 메모리 접근이 필요하면<br>
1. 캐시를 조사한다.<br>
2. 캐시태그와 메모리 주소의 태그영역을 비교한다.<br>
  > 만약 일기 연산이고 캐시가 데이터를 보관하고 있다?<br>
  캐시 -> 프로세서 데이터 전송 = Cache Hit(캐시 적중이라고 한다.)
  
  > 만약 위의 상황이 아니라면?<br>
  Cache Miss가 발생<br>
  캐시제어기는 메인 메모리에서 해당 블록을 읽어 캐시에 넣고 프로세서에 전송한다.<br>

#### 컴퓨터 시스템의 동작과정
<ol>
<li>입력 장치를 통해 정보를 입력받아 메모리에 저장</li>
<li>메모리에 저장한 정보를 프로그램의 제어에 따라 인출하여 산술장치나 논리장치에서 처리</li>
<li>처리한 정보를 출력 장치에 표시하거나 디스크에 저장</li>
</ol>

여기서 입력되는 정보는 명령어와 데이터로 구분된다.<br>
명령어의 구성은 <br>
프로세서가 실행할 연산을 나타내는 <strong>연산코드(Operation Code)</strong><br>
명령어가 처리할 데이터나 데이터가 저장된 주소에 관한 정보(레지스터, 메모리)를 기술하는 <strong>오퍼랜드(Operand)</strong><br>

명령어는 실행 전에 메인 메모리에 저장되고 한번에 하나씩 프로세서에 전송되어 해석되고 실행된다.<br>

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/%EC%BA%90%EC%8B%9C2.PNG" width= "360px" height ="150px" alt="My Image">

오퍼랜드 번지 부분이 오퍼랜드의 내용이 담긴 메모리 번지를 가리키면 직접주소<br>
장소의 번지를 저장한 곳을 가리키면 간접주소라 부른다.<br>


<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/%EB%AA%85%EB%A0%B9%EC%96%B4%ED%98%95%EC%8B%9D.PNG" width= "600px" height ="400px" alt="My Image">


#### 명령어 실행 사이클

명령어는 일반적으로 다음과 같은 과정으로 실행된다.<br>
<ol>
<li>명령어 인출(메모리 -> 명령어Register)</li>
<li>명령어 해석(Program counter 변경)</li>
<li>오퍼랜드 인출</li>
<li>명령어 실행</li>
<li>결과 저장</li>
<li>다음명령어 이후 1번의 과정으로 간다.</li>
</ol>

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/%EB%AA%85%EB%A0%B9%EC%96%B4%EC%8B%A4%ED%96%89%EC%82%AC%EC%9D%B4%ED%81%B4.PNG" width= "180px" height ="250px" alt="My Image">

> 인터럽트 : 현재 진행중인 프로그램의 수행을 연기하고 다른 프로그램의 수행을 요구하는 명령이다.<br>
시스템 처리 효율을 향상시키기 위해 사용되며 프로그램이 실행 순서를 바꿔가면서 처리되기 떄문에 다중 프로그래밍에 사용된다.<br>

---
