---
layout: post
title: "Blocking과 NonBlocking의 차이점"
tags: [Network]
comments: true

---

이번 포스팅에서는 Blocking과 NonBlocking의 차이점을 간단히 정리했습니다..

---

<img src="https://www.ibm.com/developerworks/library/l-async/figure1.gif" alt="My Image">
출처 https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/<br>


## Blocking과 NonBlocking이란?

알아보기전에 Input/Output에 대해서 먼저 알고 있어야합니다.

파일의 입출력을 다룰 때, 주로 네트워크에서 볼 수 있습니다.

이 I/O의 가장 기본적인 모델로 <strong>Blocking</strong>이 있습니다.

### Blocking

호출된 함수의 I/O작업이 진행되는 동안에는 작업을 모두 끝낼 때까지 제어권을 꽉 쥐고 있기 때문에

결과가 올 때까지 대기하도록 하는 방식을 말합니다.

이 I/O의 가장 기본적인 모델로 <strong>Blocking</strong>이 있습니다.

호출된 함수의 I/O작업이 진행되는 동안에는 작업을 모두 끝낼 때까지 제어권을 꽉 쥐고 있기 때문에

결과가 올 때까지 대기하도록 하는 방식을 말합니다.

1. 유저가 커널측으로 Read 작업을 요청하면

2. 데이터가 입력 될 때까지 대기!

3. 그리고 커널측에서 유저측으로 return되어 값이 넘어오게 되고

4. 다시 수행하던 작업을 이어서 진행

이러한 비효율적인것을 해결하기 위해서 고안된 것이 <strong>NonBlocking</strong>입니다.

### NonBlocking

위에서 설명한 비효율적인 방식을 해결하기 위해서 어떤 방법을 취했는가?

I/O작업이 진행되는 동안 유저의 프로세스 작업을 중단시키지 않는 방법을 고안해 냈습니다.

1. 유저가 커널측으로 Read 작업을 요청하면

2. 그 즉시, 결과값을 반환! (입력데이터가 없을 시 데이터가 없다는 결과를 반환합니다.)

3. 1과 2번을 계속 반복하다가 입력데이터가 들어온다?

3-1. 커널측에서 유저측으로 return되어 값이 넘어옵니다.

간단하게 요약하자면 호출된 함수가 바로 리턴해서 호출한 함수에게 제어권을 넘겨주고,

호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있으면 NonBlocking입니다.

그렇지 않고 호출된 함수가 자신의 작업을 모두 마칠 때까지 호출한 함수에게 제어권을 넘겨주지 않고 대기하게 만든다면 Blocking입니다.

---
