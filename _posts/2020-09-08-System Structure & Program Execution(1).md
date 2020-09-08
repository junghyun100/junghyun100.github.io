---
layout: post
title: "운영체제 - System Structure & Program Execution"
tags: [OS]
comments: true

---

OS를 공부하면서 정리를 한 내용들 입니다.<br>

이번 챕터는 운영체제를 설명하기에 앞서 하드웨어적인 부분을 배우는 챕터입니다.

---

CPU는 매 클럭 사이클마다 Memory에서 인스트럭션(기계어)를 하나씩 읽어내면서 실행하게 됩니다.

이와 비슷하게 각각의 IO Device에는 해당 Device 를 전담하는 작은 CPU같은 것이 붙어있습니다.

이것을 Device Controller라고 합니다.

## Device Controller

* 해당 IO 장치 유형을 관리하는 일종의 작은 CPU
* 제어 정보를 위해 control register, status register를 가짐

Disk에서 어떤 데이터를 읽을지 내부를 통제하는 것은 CPU가 아닌 Disk Controller가 하게 됩니다. 

메인 CPU에도 작업공간인 메모리가 있듯이 Device Controller에도 그들의 작업공간이 있습니다.

이것을  local buffer 라고 합니다. 

* IO는 실제 Device 와 local buffer 사이에서 일어납니다.
* Device Controller는 IO가 끝났을 경우 Interrupt로 CPU에 그 사실을 알립니다.

## Timer
* 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킵니다.
* 타이머는 매 클럭 틱 마다 1씩 감소합니다.
* 타이머 값이 0이 되면 인터럽트가 발생하므로 CPU를 특정 프로그램이 독점하는 것을 막습니다.

## 입출력의 수행
* 모든 입출력 명령은 특권 명령이다.
*  사용자 프로그램은 어떻게 IO를 하는가?

1. 시스템콜 : 사용자 프로그램은 운영체제에게 IO를 요청합니다.
2. trap을 사용하여 인터럽트 벡터의 특정 위치로 이동합니다.
3. 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동합니다.
4. 올바른 IO 요청인지 확인 후 IO 수행합니다.
5. IO 완료 시 제어권을 시스템콜 다음 명령으로 옮깁니다. 

> 시스템 콜: 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것

## Interrupt
* 인터럽트 당한 시점의 레지스터와 PC를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘깁니다.

### 넓은 의미의 인터럽트

하트웨어 인터럽트 : 하드웨어가 발생시킨 인터럽트
Trap(SW 인터럽트) : Exception(프로그램이 오류를 범한 경우), System Call(프로그램이 커널 함수를 호출하는 경우)


---
