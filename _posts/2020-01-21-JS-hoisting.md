---
layout: post
title: "자바스크립트 Hoisting"
tags: [Javascript]
comments: true
---
 
해당 Post는 Javascript의 Hoisting 기능에 대해 정리한 파일입니다.

---

#### Hoisting이란?

자바 스크립트의 파서가 "함수가 실행이 되기 전에 함수에 대한 코드를 한번 딱 훑는다" 라고 생각하면 됩니다.

변수의 범위가 전역범위인지 함수 범위인지에 따라 다르게 수행될 수 있습니다.

함수 범위의 변수는 해당 함수의 최상위로, 전역 범위의 변수는 스크립트 단위의 최상위로 끌어올려진다.

실제로 코드가 끌어올려지는 것은 아니다.

파서 내부적으로 끌어올려서 처리하며, 실제 메모리는 변화가 없다.

#### for Example

```javascript

// 함수의 호출.
function printName(firstname) {

    var inner = function(){
    return "inner name";
  }

  var result = inner();
   console.log("name is " + result);
  }
  printName();

```

지금 상태에서 

```javascript

// 함수의 호출.
function printName(firstname) {

  var result = inner();
   console.log("name is " + result);
  
  var inner = function(){ // 위치가 아래로 내려옴
    return "inner name";
  }
   
  }
  printName();

```  
해당 코드를 실행하면 "inner is not a function" Error가 나오게 됩니다.

inner가 undefined로 인식이 되는 것 입니다.

그러나!

```javascript

// 함수의 호출.
function printName(firstname) {

  var result = inner();
   console.log("name is " + result);
  
  function inner(){ // 함수 선언문으로
    return "inner name";
  }
   
  }
  printName();

```  

이렇게 작성을 한다면 정상적으로 동작하게 됩니다.

> 조금 더 자세히..

호이스팅이란 "함수가 실행이 되기 전에 함수에 대한 코드를 한번 딱 훑는다" 라고 이야기 했듯이

마지막 예제 소스코드를 Hoisting 한 모습을 보이면


```javascript

// 함수의 호출.
function printName(firstname) {

  function inner(){ // 끌어올려진 상태
    return "inner name";
  }
  
  var result = inner();
   console.log("name is " + result);
  
  /*function inner(){ // 위로 이동
    return "inner name";
  }*/
   
  }
  printName();

``` 

위 소스코드 처럼 끌어 올려진다고 생각하면 됩니다.

<a href = "https://www.edwith.org/boostcourse-web/lecture/16695/"> 참고 : 부스트코스 강좌</a>

---
