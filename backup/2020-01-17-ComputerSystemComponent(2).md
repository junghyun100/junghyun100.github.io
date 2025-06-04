---
layout: post
title: "Computer System Component(2)"
tags: [OS]
comments: true
---
 
해당 Post는 OS에 들어가기에 앞서 Computer System Component를 정리한 파일이다.

---

### Computer System Component(2)<br>

#### 4.Memory
 
 컴퓨터 성능과 밀접하므로 사용자는 당연히 크고 빠르며 비용이 저렴한 메모리를 요구한다.<br>
 But. 속도가 빠른 메모리는 가격이 비싸고.. 아래 그림과 같은 계층을 따른다.<br>
 
 
<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/memory.PNG" width= "450px" height ="200px" alt="My Image">
 
> MAIN Memory
고유주소를 가진 워드나 바이트로 구성된 대규모 배열<br>

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/memory1.PNG" width= "450px" height ="100px" alt="My Image">

메인 메모리는 다수의 셀로 구성되며, 각셀은 비트들로 구성된다.<br>
만약 셀이 K비트면 2^k 값을 저장할 수 있다.<br>

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/memory2.PNG" width= "300px" height ="400px" alt="My Image">

프로그래머는 물리적 주소를 직접 사용하지 않고 수식어나 변수를 사용한다.<br>
논리적 주소 = 컴파일러가 프로그램을 기계 명령어로 변환할 때 변수와 명령어에 할당되는 주소<br>


<strong> 메모리 속도</strong>
어떤 동작의 시작 -> 종료 사이의 경과시간(메모리 접근시간과 사이클 시간으로 표현)

<strong>메모리 접근시간</strong>
명령발생후 목표번지를 검색하여 데이터 쓰기(읽기)를 시작할 떄까지의 시간
> 읽기 제어 신호를 가한 후 데이터가 메모리 버퍼 레지스터까지 나오는데 걸리는 시간

<strong>메모리 사이클 시간</strong>
두번의 연속적인 메모리 동작 사이에 필요한 최소지연시간
> 읽기제어신호 -> 다음 읽기제어신호까지의 시간<br>
일반적으로 사이클이 접근시간보다 약간 크며 세부 구현방법에 따라 달라진다.<br>

메인메모리는 프로세서가 사용할 프로그램이나 데이터를 저장하는 작업장<br>
프로세서 - 메모리 - 디스크 상에서 발생하는 디스크 입출력 병목현상을 해결하는 역할을 한다.<br>

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/memory3.PNG" width= "450px" height ="300px" alt="My Image">


---
