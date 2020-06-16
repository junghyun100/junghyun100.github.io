---
layout: post
title: "Singleton Pattern"
tags: [Singleton Pattern]
comments: true
---
 
해당 Post는 Singleton Pattern과 싱글톤 패턴에 대해 정리한 파일입니다.

### 싱글톤 패턴이란? 

애플리케이션이 시작될 때, 어떤 클래스가 최초 한 번만 메모리를 할당(static)하고 해당 메모리에 인스턴스를 만들어 사용하는 패턴

---

<a href= "https://junghyun100.github.io/Design-Pattern/">이전 포스트 : 디자인패턴</a>

## 멀티스레드 환경에서 안전한 싱글톤 만드는 법

### 1. Lazy Initialization (게으른 초기화)

```java
public class ThreadSafe_Lazy_Initialization{
 
    private static ThreadSafe_Lazy_Initialization instance;
 
    private ThreadSafe_Lazy_Initialization(){}
     
    public static synchronized ThreadSafe_Lazy_Initialization getInstance(){
        if(instance == null){
            instance = new ThreadSafe_Lazy_Initialization();
        }
        return instance;
    }
 
}
```

* private static으로 인스턴스 변수 만듭니다.

* private으로 생성자를 만들어 외부에서의 생성을 막습니다.

* synchronized 동기화를 활용해 스레드를 안전하게 만듭니다.

> 하지만, synchronized는 큰 성능저하를 발생시키므로 권장하지 않는 방법

이를 해결하기 위한 방안으로 Double-checked Locking을 추가한 방법이 있습니다.

### 2. Lazy Initialization + Double-checked Locking

```java
public class ThreadSafe_Lazy_Initialization{
    private volatile static ThreadSafe_Lazy_Initialization instance;

    private ThreadSafe_Lazy_Initialization(){}

    public static ThreadSafe_Lazy_Initialization getInstance(){
    	if(instance == null) {
        	synchronized (ThreadSafe_Lazy_Initialization.class){
                if(instance == null){
                    instance = new ThreadSafe_Lazy_Initialization();
                }
            }
        }
        return instance;
    }
}
```

1번의 성능저하를 완화시키는 방법으로 1번과는 달리, 

먼저 조건문으로 인스턴스의 존재 여부를 확인한 다음 두번째 조건문에서 synchronized를 통해 동기화를 시켜 인스턴스를 생성하는 방법

스레드를 안전하게 만들면서, 처음 생성 이후에는 synchronized를 실행하지 않기 때문에 성능저하 완화가 가능한 방법입니다.

그래도 아직은 완벽한 방법은 아닙니다. 그렇기 떄문에 다음의 방법이 나타났습니다.

### 3. Initialization on demand holder idiom (holder에 의한 초기화)

클래스 안에 클래스(holder)를 두어 JVM의 클래스 로더 매커니즘과 클래스가 로드되는 시점을 이용한 방법

```java
public class Something {
    private Something() {
    }
 
    private static class LazyHolder {
        public static final Something INSTANCE = new Something();
    }
 
    public static Something getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```
2번처럼 동기화를 사용하지 않는 방법을 안하는 이유는, 

개발자가 직접 동기화 문제에 대한 코드를 작성하면서 회피하려고 하면 프로그램 구조가 그만큼 복잡해지고 비용 문제가 발생할 수 있습니다. 

또한 코드 자체가 정확하지 못할 때도 많습니다.

이 때문에, 3번과 같은 방식으로 JVM의 클래스 초기화 과정에서 보장되는 원자적 특성을 이용해 

싱글톤의 초기화 문제에 대한 책임을 JVM에게 떠넘기는 걸 활용합니다.

클래스 안에 선언한 클래스인 holder에서 선언된 인스턴스는 static이기 때문에 클래스 로딩시점에서 한번만 호출됩니다.

실제로 가장 많이 사용되는 일반적인 싱글톤 클래스 사용 방법이 3번입니다.

---
