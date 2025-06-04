---
layout: post
title: "운영체제 - System Structure & Program Execution(2)"
tags: [OS]
comments: true

---

OS를 공부하면서 정리를 한 내용들 입니다.<br>

이번 챕터는 운영체제를 설명하기에 앞서 하드웨어적인 부분을 배우는 챕터입니다.

---

## 동기식 입출력과 비동기식 입출력

### 동기식 입출력(Synchronous I/O)
<ul>
    <li>
        I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어갑니다.
    </li>
    <li>구현방법 1
        <ol>
        <li>
            I/O가 끝날 때까지 CPU를 낭비 시킵니다.
        </li>
        <li>
            매시점 하나의 I/O만 일어날 수 있습니다.
        </li>
        </ol>
    </li>
    <li>구현방법 2
            <ol>
            <li>
                I/O가 완료될 때까지 해당 프로그래램에게서 CPU를 빼았습니다.
            </li>
            <li>
                I/O 처리를 기다리는 줄에 그 프로그램을 기다리게 합니다.
            </li>
            <li>
                다른 프로그램에게 CPU를 넘겨줍니다.
            </li>
            </ol>
        </li>
</ul>

### 비동기식 입출력(Asynchronous I/O)

* I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 <br>제어가 사용자 프로그램에 즉시 넘어갑니다.

#### 두가지 경우 모두 I/O의 완료는 인터럽트로 알려줍니다.

<img src= "https://www.toolsqa.com/wp-content/gallery/cypress/2_1-.png">

<a href = "https://www.toolsqa.com/cypress/cypress-asynchronous-nature/">이미지 출처 </a>

그림을 통해서 보면 더 확실히 이해할 수 있습니다.

유저가 I/O 작업을 커널단에 작업을 시켜놓고 나서 

두가지 방식의 차이점을 보면 아래와 같습니다.

1. Synchronous는 일을 맡긴 후 감독을 하며 마칠때 까지 `감시를 한다.`

2. Asynchronous는 일을 맡긴 후 (CPU 제어권을 얻어서)`자기가 할일을 한다.`

### DMA(Direct Memory Access)

빠른 입출력 장치를 사용한다면 어떻게 될까요?

인터럽트를 더욱 빈번히 걸게 됩니다.

그러면 CPU가 너무 자주 인터럽트가 걸려서 문제가 생길 수 있습니다.

그것을 해결하기 위해 사용합니다.

* CPU의 중재 없이. device controller가 device의 buffer storage의 내용을<br> 메모리에 `block`단위로 직접 전송
* 바이트 단위가 아니라 `block`단위로 인터럽트를 발생시킴

이전에 Synchronous와 Asynchronous와 혼동하기 쉬운

`Blocking`과 `NonBlocking`에 대해서도 포스팅 해놓았으니 참고하면 좋을 듯 합니다.

<a href = "https://junghyun100.github.io/Blocking&NonBlocking/"> Blocking과 NonBlocking</a>

---
