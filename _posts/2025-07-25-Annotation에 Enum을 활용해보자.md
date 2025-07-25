---
layout: post
title: "Annotation에 Enum을 활용해보자"
date: 2025-07-25
tags: [Spring]
comments: true
---

해당 Post는 실무에서 코드를 개선하기위해 Enum을 사용하려다 어노테이션에서 "Attribute value must be constant" 같은 컴파일 에러를 마주칠 때가 있었습니다. 

이번 글에서는 이 에러의 원인을 알아내고, 실무에서 사용할 수 있는 해결 방법을 정리해 보겠습니다.

---

## 가정

환경은 다음과 같이 가정합니다.

### 예시 코드: Enum과 @Cacheable

다음은 캐시 설정을 위한 Enum입니다.

```java
public enum CacheType {
    A_1M("A_1M", 60, 10000000),
    B_5M("B_5M", 300, 10000000),
    C_10M("C_10M", 600, 10000000);

    private final String cacheName;
    private final int expireAfterWrite;
    private final int maximumSize;

    CacheType(String cacheName, int expireAfterWrite, int maximumSize) {
        this.cacheName = cacheName;
        this.expireAfterWrite = expireAfterWrite;
        this.maximumSize = maximumSize;
    }

    public String getCacheName() {
        return cacheName;
    }
}
```

```java
@Cacheable(cacheNames = "A_1M") // 정상 동작
public String getData() {
    return "some data";
}
```
`@Cacheable`에서 문자열 리터럴은 정상적으로 동작합니다.

Spring의 `@Cacheable` 어노테이션에 `CacheType`이라는 Enum을 호출해 `cacheNames` 속성에 넣어보려 시도한다면?

```java
@Cacheable(cacheNames = CacheType.A_1M.getCacheName()) // 컴파일 에러! (Attribute value must be constant)
public String getData() {
    return "some data";
}
```

위 코드는 "Attribute value must be constant" 에러를 뱉습니다. 

## 원인: 왜 이 에러가 발생할까?

### 원인 1. 컴파일 타임 상수 제약

Java 어노테이션은 클래스 로딩 시 Metaspace 영역에 메타데이터로 저장됩니다. 

이 메타데이터는 불변(immutable)이어야 하며, **컴파일 시점에 값이 확정**되어야 합니다. 

따라서 어노테이션 속성값은 다음과 같은 컴파일 타임 상수만 허용됩니다:

* 리터럴 상수 (예: "A_1M", 42)
* Enum 상수 (예: CacheType.A_1M)
* 클래스 리터럴 (예: String.class)
* 배열 (예: {"A_1M", "B_5M"})

`CacheType.A_1M.getCacheName()` 같은 메서드 호출은 런타임에 평가되므로 컴파일 타임 상수가 아닙니다. 

컴파일러가 값을 미리 알 수 없으니 에러가 발생합니다.

> The value for an annotation must be a compile-time constant.

참고 근거 1: <a href="https://www.baeldung.com/java-compile-time-constants">Baeldung - Java Compile-Time Constants</a>

### 원인 2. Enum 메서드의 동적 특성

Enum은 상수 집합을 정의하는 데 유용하지만, 메서드 호출(예: getCacheName())은 런타임에 실행됩니다. 

`CacheType.A_1M` 자체는 컴파일 타임 상수지만, getCacheName()은 동적이라 어노테이션 속성으로 사용할 수 없습니다.

## 해결 방안
Enum의 동적 값을 어노테이션에 직접 사용할 수 없으니, 컴파일 타임 상수를 활용해야 합니다. 

Enum의 값을 미러링하는 public static final String 상수를 별도 클래스에 정의합니다.

```java
public class CacheNames {
    public static final String A_1M = "A_1M";
    public static final String B_5M = "B_5M";
    public static final String C_10M = "C_10M";
}
```

```java
@Cacheable(cacheNames = CacheNames.A_1M) // 정상 동작
public String getData() {
    // 로직
    return "어떤것";
}
```

`@Cacheable`에 적용하면 다음과 같이 정상동작하게 됩니다.

## 결론

제가 생각하는 위 방법의 장점과 단점은 다음과 같습니다.


### 장점: 타입 안전성과 유지보수성이 좋습니다.

cacheNames을 리터럴로 하드코딩 할 경우 오타로 인해 문제가 생길 수 있고, Enum 값이 변경되면 한번에 변경이 가능합니다.

Enum과 상수 클래스를 동기화하려면 Enum에서 상수 값을 참조하거나 코드 생성 도구를 사용할 수 있습니다.


### 단점: 약간의 코드 중복이 있을 수 있습니다.

제 경우 enum 클래스 파일의 하단에 중복이지만, CacheNames를 같이 두어 유지보수를 같이할 수 있도록 구현 했습니다.

Enum 값이 변경되면 한번에 일일이 수정이 필요합니다.



간단한 경우라면 문자열 리터럴을 사용할 수 있지만, 프로젝트 규모가 커지다보니 하드코딩을 되도록 줄여보려다가 찾아낸 방법입니다.

---


