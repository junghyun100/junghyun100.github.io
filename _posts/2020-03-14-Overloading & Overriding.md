---
layout: post
title: "Overloading & Overriding"
tags: [개념]
comments: true
---

오버로딩(Overloading)과 오버라이딩(Overriding)은 이름이 비슷해서 자주 혼동합니다.

그런 이유인지 면접에서도 가끔 물어보는 문제입니다.

오늘은 그것에 대해서 포스팅해보겠습니다. 

---

### 오버로딩(Overloading)
### 같은 이름의 메소드를 여러 개 가지면서 매개변수의 유형과 개수가 다르도록 하는 기술

```java
public int exampleOverload(int A) {...}
public int exampleOverload(int A, String B) {...}
public int exampleOverload(Double C) {...}
```
예제에서와 같이 test 라는 같은 이름의 메소드를 여러개 정의하고 매개변수만 변경하여 선언했을 때,

호출 매개변수에 따라 매칭되어 함수를 실행시킵니다.


### 오버라이딩(Overriding)
### 상위 클래스가 가지고 있는 메소드를 하위 클래스가 재정의 해서 사용합니다.

```java
public class Main {
  public static void main(String[] args) {
    Shape[] shapes = new Shape[2];
    Circle circle = new Circle();
    Ambiguous ambiguous = new Ambiguous();

    shapes[0] = circle;
    shapes[1] = ambiguous;

    for(Shape s : shapes) {
      s.printMe();
      System.out.println(s.computeArea());
    }
  }
}
```
<a href="https://gmlwjd9405.github.io/2018/08/09/java-overloading-vs-overriding.html">참고 : gmlwjd9405님 깃허브</a>
```java
public class Main {
  public static void main(String[] args) {
    Shape[] shapes = new Shape[2];
    Circle circle = new Circle();
    Ambiguous ambiguous = new Ambiguous();

    shapes[0] = circle;
    shapes[1] = ambiguous;

    for(Shape s : shapes) {
      s.printMe();
      System.out.println(s.computeArea());
    }
  }
}
```
<a href="https://gmlwjd9405.github.io/2018/08/09/java-overloading-vs-overriding.html">참고 : gmlwjd9405님 깃허브</a>

Circle에서 printMe() 메서드를 재정의한다.

오버라이딩을 할 때 개발자의 실수를 방지하기 위해 메서드 위에 @Override 를 관례적으로 적는다.

### 오버로딩(Overloading)과 오버라이딩(Overriding) 성립조건

| 구분 | 오버로딩 | 오버라이딩 |
|:-----|:----:|-----:|
| 메소드 이름  | 동일  | 동일 |
| 매개변수, 타입  | 다름  | 동일  |
| 리턴타입  | 상관없음  | 동일  |

