---
layout: post
title: "Spring의 @Transactional 동작 원리와 동작 하지 않는 경우 문제 해결"
date: 2025-07-11
tags: [Spring]
comments: true
---

실무에서 Transactional이 제대로 동작하지 않는 경우가 종종 있습니다.

해당 Post는 이 글에서는 `@Transactional`의 내부 동작 원리와 동작하지 않는 경우 

그리고 이를 해결하는 방법을 자세히 정리했습니다.

---


## 1. @Transactional의 동작 원리

Spring은 프록시 기반 AOP(Aspect-Oriented Programming)를 활용해 `@Transactional`을 구현합니다. 

동작 과정을 단계별로 살펴보면 다음과 같습니다.

1.**프록시 객체 생성** 

Spring 컨테이너는 `@Transactional`이 붙은 클래스나 메서드를 감지하면 해당 빈(Bean)에 **프록시 객체**를 생성합니다. 

이 프록시는 실제 객체를 감싸며 트랜잭션 시작, 커밋, 롤백과 같은 로직을 추가로 처리합니다. 

프록시는 두 가지 방식으로 생성됩니다.

  - **CGLIB**: 클래스 기반 프록시.
  - **JDK 동적 프록시**: 인터페이스 기반 프록시.

2.**프록시 호출**  

`@Transactional` 메서드가 호출되면 실제 객체 대신 프록시 객체가 호출됩니다. 

프록시는 트랜잭션 매니저를 통해 데이터베이스 커넥션을 얻고 트랜잭션을 시작합니다.

3.**비즈니스 로직 실행**

프록시는 실제 객체의 메서드(비즈니스 로직)를 호출하며, 트랜잭션은 활성 상태로 유지됩니다.

4.**트랜잭션 종료**  

메서드가 정상적으로 끝나면 트랜잭션을 **커밋**합니다.

  - **런타임 예외** 발생 시 **롤백**.
  - **체크 예외** 발생 시 기본적으로 **커밋** (설정 변경 가능).

5.**결과 반환**  

프록시는 결과를 반환하고 트랜잭션 관련 자원을 정리합니다.

## 2. @Transactional이 동작하지 않는 주요 원인과 해결법

`@Transactional`이 기대대로 작동하지 않는 데는 여러 이유가 있습니다. 

아래에서 주요 원인과 해결 방법을 정리했습니다.

### 2.1. 프록시 기반으로 인해 발생

#### 문제 1. 같은 클래스 내 메서드 호출

같은 클래스 내에서 `@Transactional` 메서드를 직접 호출하면 프록시를 거치지 않아 트랜잭션이 적용되지 않습니다.

**예시**
```java
@Service
public class MyService {
    public void outer() {
        inner(); // 프록시 미호출 -> 트랜잭션 미적용
    }

    @Transactional
    public void inner() {
        // 트랜잭션 로직
    }
}
```

**원인**: 프록시는 외부 호출에만 동작하며, 내부 호출은 실제 객체의 메서드를 직접 호출합니다.

**해결법**: 별도의 서비스 클래스로 분리하거나 `ApplicationContext`를 사용해 프록시를 호출.

**예시**
```java
@Service
public class MyService {
    private final ApplicationContext context;

    @Autowired
    public MyService(ApplicationContext context) {
        this.context = context;
    }

    public void outer() {
        MyService proxy = context.getBean(MyService.class);
        proxy.inner(); // 프록시 호출 -> 트랜잭션 적용
    }

    @Transactional
    public void inner() {
        // 트랜잭션 로직
    }
}
```

#### 문제 2. public이 아닌 메서드

프록시는 `public` 메서드만 오버라이드합니다. 

`private`, `protected`, `default` 메서드에 `@Transactional`을 붙이면 트랜잭션이 적용되지 않습니다.

**해결법**
- `@Transactional`은 `public` 메서드에만 사용.
- 인터페이스를 정의해 프록시 생성을 보장.

### 2.2. 예외 처리 방식 문제

Spring은 기본적으로 **런타임 예외** 발생 시 롤백하고, **체크 예외** 발생 시 커밋합니다.

**예시**
```java
@Transactional
public void myMethod() throws IOException {
    throw new IOException("체크 예외"); // 롤백되지 않음
}
```

**원인**: Spring은 런타임 예외만 롤백 대상으로 간주합니다.

**해결법**: `@Transactional(rollbackFor = Exception.class)`를 사용해 체크 예외도 롤백.

**예시**
```java
@Transactional(rollbackFor = Exception.class)
public void myMethod() throws IOException {
    throw new IOException("체크 예외"); // 롤백됨
}
```

### 2.3. 트랜잭션 전파(Propagation) 설정 문제

트랜잭션 전파 속성(`Propagation`)이 잘못 설정되면 트랜잭션이 의도대로 동작하지 않을 수 있습니다.

**주요 전파 속성**

- `REQUIRED` (기본값): 기존 트랜잭션에 참여, 없으면 새로 시작.
- `REQUIRES_NEW`: 항상 새 트랜잭션 시작.
- `NESTED`: 중첩 트랜잭션 시작.
- `SUPPORTS`: 트랜잭션이 있으면 참여, 없으면 비트랜잭션 실행.
- `NOT_SUPPORTED`: 트랜잭션 일시 중지 후 비트랜잭션 실행.
- `NEVER`: 트랜잭션이 있으면 예외 발생.
- `MANDATORY`: 트랜잭션이 반드시 있어야 함.

**예시**
```java
@Transactional(propagation = Propagation.NEVER)
public void myMethod() {
    // 트랜잭션 내 호출 시 예외 발생
}
```

**해결법**: 요구사항에 맞는 전파 속성을 설정 (예: `REQUIRES_NEW` 또는 `NESTED`).

### 2.4. 빈 등록 및 AOP 설정 문제

`@Transactional`이 붙은 클래스가 Spring 빈으로 등록되지 않으면 프록시가 생성되지 않습니다.

**예시**
```java
public class MyService { // @Service 누락
    @Transactional
    public void myMethod() {
        // 트랜잭션 미적용
    }
}
```

**해결법**

- `@Component`, `@Service`, `@Repository` 등으로 빈 등록.
- `@EnableTransactionManagement`를 `@Configuration` 클래스에 추가.

### 2.5. @Transactional 우선순위 및 readOnly 설정

- **문제**: 클래스와 메서드에 `@Transactional`이 모두 있으면 **메서드 설정**이 우선 적용됩니다.
- **문제**: `readOnly=true`로 설정된 트랜잭션에서 데이터 변경(INSERT, UPDATE, DELETE)을 시도하면 예외가 발생하거나 변경이 반영되지 않습니다.

**예시**
```java
@Transactional(readOnly = true)
public class MyService {
    public void readOnlyMethod() {
        // 데이터 변경 시도 -> 예외 발생
    }
}
```

**해결법**

- 데이터 변경이 필요한 메서드는 `readOnly=false` (기본값)으로 설정.
- 클래스와 메서드 간 `@Transactional` 설정 충돌을 점검.

---

### 2.6. 다중 스레드 환경

트랜잭션은 **스레드 로컬**에 저장되므로 다른 스레드에서 실행된 로직은 같은 트랜잭션에 참여하지 않습니다.

**해결법**

- `@Async` 호출에서 트랜잭션이 필요하면 대상 메서드에 `@Transactional` 추가.
- 스레드 풀 설정을 확인해 트랜잭션 전파가 올바르게 작동하도록 조정.

**해결법**

- 인터페이스를 정의하거나 CGLIB 프록시를 명시적으로 활성화.
- Spring Boot에서는 기본적으로 CGLIB 프록시가 사용됩니다.

---

## 3. 디버깅 팁

### 로그 확인

- **SQL 로그**
```properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```
  
- **트랜잭션 로그**
```properties
logging.level.org.springframework.transaction=DEBUG
```

### 프록시 확인

`AopUtils.isAopProxy(bean)` 또는 `bean.getClass()`로 프록시 객체 여부 확인.

### 트랜잭션 상태 점검
`TransactionSynchronizationManager`로 트랜잭션 상태 확인

```java
import org.springframework.transaction.support.TransactionSynchronizationManager;

public void checkTransaction() {
    System.out.println("트랜잭션 활성 여부: " + TransactionSynchronizationManager.isActualTransactionActive());
}
```

### 4. 결론

`@Transactional`은 트랜잭션 관리를 쉽게 만들어주지만, 

프록시 기반 AOP의 특성과 설정 문제를 이해하지 않으면 예상치 못한 문제가 발생할 수 있습니다. 

위 부분들을 참고하면서 진행하면 원인을 파악하고 해결하는 것이 용이할 수 있습니다.

---


