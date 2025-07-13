---
layout: post
title: "not eligible for getting processed by all BeanPostProcessors 경우 문제 해결"
date: 2025-07-13
tags: [Spring]
comments: true
---

해당 Post는 사이드 프로젝트를 하다가 "not eligible for getting processed by all BeanPostProcessors" 경우가 있었습니다.

`warning`으로 실제적인 에러는 아니기에 실행 시 중단되거나 하지는 않지만 이를 해결하는 방법을 정리했습니다.

---

```kotlin
"Bean '어떤Bean' of type [...] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)"
```
위와 같은 경고성 문구가 발생하는 경우가 있습니다.

## 가정

환경은 다음과 같이 가정합니다.

버전: Spring Boot 3.2, Spring Framework 6.1

예를 들어, 두 개의 Bean CircularDependencyA와 CircularDependencyB가 서로를 참조하는 순환 의존성이 발생했다고 가정합니다.

```java
// Java 예시 (baeldung에서 제공한 순환 의존성 발생 코드) 
@Component
public class CircularDependencyA {

    private CircularDependencyB circB;

    @Autowired
    public CircularDependencyA(CircularDependencyB circB) {
        this.circB = circB;
    }
}

@Component
public class CircularDependencyB {

    private CircularDependencyA circA;

    @Autowired
    public CircularDependencyB(CircularDependencyA circA) {
        this.circA = circA;
    }
}
```
```kotlin
// Kotlin 예시 (baeldung에서 제공한 순환 의존성 발생 코드를 변형)
@Component
class CircularDependencyA(private val circB: CircularDependencyB)

@Component
class CircularDependencyB(private val circA: CircularDependencyA)
```

이런 설정에서 애플리케이션이 시작될 때, Spring은 Bean을 생성하려다 순환 때문에 실패하거나 경고를 띄웁니다. 

특히, AOP나 트랜잭션 같은 BeanPostProcessor가 적용되지 않아 "not eligible" 메시지가 로그에 나타납니다.

## 원인: 왜 이 경고가 발생할까?

### 원인1. 순환 의존성(Circular Dependencies)

Bean CircularDependencyA가 Bean CircularDependencyB를 의존하고, 

Bean CircularDependencyB가 다시 Bean CircularDependencyA를 의존하는 경우 Spring은 생성 순서를 결정하지 못합니다.

Spring이 Bean을 생성하려 할 때, 의존성을 주입하기 전에 이미 다른 Bean이 필요해지기 때문입니다.

결과적으로, Bean이 완전히 초기화되지 않아 AOP auto-proxying 같은 BeanPostProcessor가 적용되지 않습니다.

> A circular dependency occurs when a bean A depends on another bean B, and the bean B depends on bean A as well: Bean A → Bean B → Bean A.

참고 근거1: <a href="https://www.baeldung.com/circular-dependencies-in-spring">Baeldung - Circular Dependencies in Spring</a>

### 원인2. BeanPostProcessor의 초기화 순서 문제

BeanPostProcessor 자체가 다른 Bean을 참조하거나, 의존성을 가질 때 발생합니다.

Spring은 BeanPostProcessor를 애플리케이션 컨텍스트 시작 초기에 인스턴스화합니다. 

이 과정에서 참조되는 Bean들은 나중에 적용되는 BeanPostProcessor(예: auto-proxying)를 받을 수 없습니다.

> For any such bean, you should see an informational log message: Bean someBean is not eligible for getting processed by all BeanPostProcessor interfaces (for example: not eligible for auto-proxying).

참고 근거2: <a href="https://docs.spring.io/spring-framework/reference/core/beans/factory-extension.html">Spring Framework 공식 문서 - Container Extension Points</a>

## 해결 방안

순환 의존성을 제거하는 식이면 좋겠지만, @Lazy 어노테이션을 사용해 지연 초기화를 적용하는 것을 공식문서에서도 권장한다.

### Java에서의 해결

- **@Lazy 사용**: 의존성을 지연 초기화하여 프록시를 생성합니다. Bean이 실제로 필요할 때 초기화됩니다.

```java
@Component
public class CircularDependencyA {

    private CircularDependencyB circB;

    @Autowired
    public CircularDependencyA(@Lazy CircularDependencyB circB) {
        this.circB = circB;
    }
}

@Component
public class CircularDependencyB {

    private CircularDependencyA circA;

    @Autowired 
    public CircularDependencyB(CircularDependencyA circA) {
        this.circA = circA;
    }
}
```

- **Setter Injection으로 전환**: Constructor 대신 setter를 사용해 Bean을 먼저 생성한 후 의존성을 주입합니다. (**Spring 문서에서 제안하는 방법**)

```java
@Component
public class CircularDependencyA {

    private CircularDependencyB circB;

    @Autowired
    public void setCircB(@Lazy CircularDependencyB circB) {
        this.circB = circB;
    }

    public CircularDependencyB getCircB() {
        return circB;
    }
}

```

### Kotlin에서의 해결

- Kotlin은 immutable한 constructor injection을 선호하지만, 순환 시 lateinit var이나 @Lazy를 사용합니다.
- **@Lazy 사용**: Java와 유사하게 constructor에 적용합니다.

```kotlin
@Component
class CircularDependencyA(@Lazy private val circB: CircularDependencyB)

@Component
class CircularDependencyB(private val circA: CircularDependencyA)
```

- **lateinit var와 Setter Injection**: 필드를 lateinit으로 선언하고 setter로 주입합니다.

```kotlin
@Component
class CircularDependencyA {
    lateinit var circB: CircularDependencyB

    @Autowired
    fun setCircB(@Lazy circB: CircularDependencyB) {
        this.circB = circB
    }
}
```

참고 근거3: <a href="https://medium.com/spring-boot-tips-and-tricks/resolving-circular-dependencies-in-spring-with-dynamic-proxies-and-lazy-annotation-a38c51f57fe9">Medium - Spring Circular Dependencies with @Lazy</a>

참고 근거4: <a href="https://www.baeldung.com/circular-dependencies-in-spring">Baeldung 문서</a>

---


