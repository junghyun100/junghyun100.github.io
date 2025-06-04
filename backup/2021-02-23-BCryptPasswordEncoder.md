---
layout: post
title: "BCryptPasswordEncoder"
tags: [Spring Security]
comments: true

---

BCryptPasswordEncoder에 대해 정리를 한 내용 입니다.

대부분의 내용은 스프링 시큐리티 래퍼런스에 있는 내용에서 가져왔습니다.

---

# BCryptPasswordEncoder

BCryptPasswordEncoder 구현체는 널리 사용되고 있는 

bcrypt 알고리즘으로 비밀번호를 해싱합니다. 

bcrypt는 의도적으로 느리게 동작하기 때문에 비밀번호를 해독하기 어렵습니다. 

다른 적응형 단방향 함수와 마찬가지로 시스템에서 비밀번호 하나를 검증하는 데 

1초가량 걸리도록 조정해야 해야합니다.

그리고 예시를 봅시다.

```java
	// Create an encoder with strength 16
	BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(16);
	String result = encoder.encode("abc123");
	System.out.println("Encoded Password : " + result);
	System.out.println("matches() result : "+encoder.matches("abc123", result));
```

reference에 있는 예시를 조금 더 보기 쉽게 바꾸었습니다.

![img.png](/images/2021년/0223/BCrypt.PNG)

성공적으로 암호화가 된다는 사실을 알 수 있었습니다.

그런데 궁금증이 생겼습니다.

## 첫번째

```java
	Map encoders = new HashMap();
	encoders.put("bcrypt", new BCryptPasswordEncoder());

	PasswordEncoder passwordEncoder = new DelegatingPasswordEncoder("bcrypt", encoders);
	String test1 = passwordEncoder.encode("abc123!");
	String test2 = passwordEncoder.encode("abc123!");
	String test3 = passwordEncoder.encode("abc123!");

	System.out.println("result Value : " + test1);
	System.out.println("result Value : " + test2);
	System.out.println("result Value : " + test3);
	System.out.println("matches() result : " + passwordEncoder.matches("abc123",test1));
	System.out.println("matches() result : " + passwordEncoder.matches("abc123",test2));
	System.out.println("matches() result : " + passwordEncoder.matches("abc123",test3));
```

위의 실행을 했을 때 encoding된 결과값은 아래와 같습니다.

![img.png](/images/2021년/0223/Question1.PNG)

여기서 encoding된 암호값은 앞에 `$2a$10$`가 공통으로 붙어있습니다.

이것의 뜻은 

1. $2b 는 bcrypt 의 버전정보 입니다.

2. $10 은 10 round 를 의미하며, 값이 클 수록 연산의 cost 가 증가합니다.

## 두번째

이어서 이것은 어디서 설정이 되었는가?

그에 대한 것도 찾아보기위해 내부코드를 살펴봤습니다.

```java
	public BCryptPasswordEncoder() {
		this(-1);
	}
	
	...

    public BCryptPasswordEncoder(BCryptVersion version, int strength, SecureRandom random) {
        if (strength != -1 && (strength < BCrypt.MIN_LOG_ROUNDS || strength > BCrypt.MAX_LOG_ROUNDS)) {
        throw new IllegalArgumentException("Bad strength");
        }
        this.version = version;
        this.strength = (strength == -1) ? 10 : strength;
        this.random = random;
    }
```

인코더를 처음 불러렀을 때 아무것도 넣어주지 않아서 -1이라는 값이 strength에 설정이 되고,

이것이 -1일 경우 10으로 변경되어서 설정 됩니다.

따라서 기본 생성자를 불렀기 때문에 

`$2a$10`라는 값이 나타났고, 

아까의 예제에서 strength를 16으로 설정해서

`$2a$16`이라는 값이 나왔습니다.

# 함수의 종류

각 함수들은 어떤 기능을 하는가?

### encode()

패스워드를 인코딩 합니다.

### matches()

인코딩된 암호가 인코딩된 후 제출된 원시 암호와 일치하는지 확인합니다. 

암호가 일치하지 않으면 true로 반환됩니다.

### upgradeEncoding()

인코딩된 암호를 더 나은 보안을 위해 다시 인코딩해야 하는 경우 true를, 

그렇지 않으면 false를 반환합니다.

## 참조

<a href="https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/bcrypt/BCryptPasswordEncoder.html">스프링 공식 문서</a>

<a href="https://docs.spring.io/spring-security/site/docs/current/reference/html5/#authentication-password-storage-dpe">스프링 시큐리티 레퍼런스</a>

---
