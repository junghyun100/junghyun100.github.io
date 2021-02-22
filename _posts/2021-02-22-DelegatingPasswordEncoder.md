---
layout: post
title: "DelegatingPasswordEncoder란?"
tags: [Spring Security]
comments: true

---

DelegatingPasswordEncoder에 대해 정리를 한 내용 입니다.

대부분의 내용은 스프링 시큐리티 래퍼런스에 있는 내용에서 가져왔습니다.

---

비밀번호 암호화를 위해서 찾아보다가

스프링 시큐리티에서 `PasswordEncoder`에 대해서 알게 되었습니다.

그래서 관련 자료들을 찾아보던 도중 `Delegating PasswordEncoder`를

먼저 알아보았습니다.

# DelegatingPasswordEncoder

```

Prior to Spring Security 5.0 the default PasswordEncoder was 

NoOpPasswordEncoder which required plain text passwords. 

Based upon the Password History section you might expect 

that the default PasswordEncoder is now something like BCryptPasswordEncoder. 

However, this ignores three real world problems:

* There are many applications using old password encodings that cannot easily migrate

* The best practice for password storage will change again.

* As a framework Spring Security cannot make breaking changes frequently

- 스프링 시큐리티 래퍼런스 발췌

```

이것을 구글 번역기에 돌려봤습니다.

```
Spring Security 5.0 이전에 

기본 PasswordEncoder는 일반 텍스트 암호가 필요한 NoOpPasswordEncoder였습니다. 

암호 기록 섹션에 따라 기본 PasswordEncoder가 이제 BCryptPasswordEncoder와 비슷하다고 

예상 할 수 있습니다. 

그러나 이것은 세 가지 실제 문제를 무시합니다.

* 쉽게 마이그레이션 할 수없는 이전 암호 인코딩을 사용하는 많은 응용 프로그램이 있습니다.

* 암호 저장에 대한 모범 사례가 다시 변경됩니다.

* 프레임 워크로서 Spring Security는 자주 브레이킹 변경을 할 수 없습니다.

```

번역이 되었지만 이해하기 어려워서 조금 찾아보았더니 

```

스프링 시큐리티에서 제공하는 PasswordEncoder는 사용자가 등록한 비밀번호를 

단방향으로 변환하여 저장하는 용도로 사용된다. 

그리고 시대적인 흐름에 따라서 점점 고도화된 암호화 알고리즘 구현체가 적용되어간다. 

이런 과정에서 서비스에 저장된 비밀번호에 대한 암호화 알고리즘을 변경하는 일은 

상당히 많은 노력을 요구하게 된다.

```

즉, <strong>단방향의 변환된 암호를 풀어서 다시 암호화해야 하는 것이 쉬운 일은 아니다.</strong>

그래서 나온 해결책이 `DelegatingPasswordEncoder`입니다.

## 기본 DelegatingPasswordEncoder 생성

> dependency에 대한 내용들은 래퍼런스의 조금 위를 보면 나오니 바로 실습.

```java
PasswordEncoder passwordEncoder =
        PasswordEncoderFactories.createDelegatingPasswordEncoder();
```
기본 값으로 설정된 `passwordEncoder`는 이렇게 만들어 줍니다.

## 커스텀 DelegatingPasswordEncoder 생성

```java
		String idForEncode = "bcrypt";
        Map encoders = new HashMap();
        encoders.put(idForEncode, new BCryptPasswordEncoder());
        encoders.put("noop", NoOpPasswordEncoder.getInstance());
        encoders.put("pbkdf2", new Pbkdf2PasswordEncoder());
        encoders.put("sha256", new StandardPasswordEncoder());
//		encoders.put("scrypt", new SCryptPasswordEncoder()); <- 에러가 발생함. 
//		에러 내용은 참고에 사진을 달아둡니다. 원인을 아시는 분은 댓글 남겨 주세요.

```

이렇게 생성된 `DelegatingPasswordEncoder`의 저장 형태는 아래와 같습니다.

> {id}encodedPassword

첫 번째 `id`는 `encoders.put`의 첫번째 파라미터의 값입니다.

두 번째 뒤에 들어가는 암호화된 값이 들어갑니다.

## Test
```java
@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
		String idForEncode = "bcrypt";
		Map encoders = new HashMap();
		encoders.put(idForEncode, new BCryptPasswordEncoder());
		encoders.put("noop", NoOpPasswordEncoder.getInstance());
		encoders.put("pbkdf2", new Pbkdf2PasswordEncoder());
		encoders.put("sha256", new StandardPasswordEncoder());

		PasswordEncoder passwordEncoder = new DelegatingPasswordEncoder(idForEncode, encoders);
		String test1 = passwordEncoder.encode("abc123");

		passwordEncoder = new DelegatingPasswordEncoder("noop", encoders);
		String test2 = passwordEncoder.encode("abc123");

		passwordEncoder = new DelegatingPasswordEncoder("pbkdf2", encoders);
		String test3 = passwordEncoder.encode("abc123");

		passwordEncoder = new DelegatingPasswordEncoder("sha256", encoders);
		String test4 = passwordEncoder.encode("abc123");


		System.out.println("result Value : " + test1);
		System.out.println("result Value : " + test2);
		System.out.println("result Value : " + test3);
		System.out.println("result Value : " + test4);
		System.out.println("matches() result : " + passwordEncoder.matches("abc123",test1));
		System.out.println("matches() result : " + passwordEncoder.matches("abc123",test2));
		System.out.println("matches() result : " + passwordEncoder.matches("abc123",test3));
		System.out.println("matches() result : " + passwordEncoder.matches("abc123",test4));
	}

}
```

`DelegatingPasswordEncoder`에 `id`값과 암호화된값이 제대로 출력이 나오는지 확인해 봤습니다.

![img.png](/images/2021년/0222/result.PNG)

아래에 있는 `matches()`는 두개가 동일한 값인가 확인해주는 것입니다.

자세한 동작 과정에 대해서는 

4가지 암호화 방법에 대해서 조금 더 찾아본 후 정리하도록 하겠습니다.

## 참고

![img.png](/images/2021년/0222/deprecated.PNG)

사진을 보면 삭선되어 있는 암호화 방식들이 있습니다.

### 첫번째 

`NoOpPasswordEncoder` 방식의 내용입니다.

```
This PasswordEncoder is not secure. 

Instead use an adaptive one way function like 

BCryptPasswordEncoder, Pbkdf2PasswordEncoder, or SCryptPasswordEncoder.
```

마우스를 올리면 해당 방식은 안전하지 않으므로 다른 방식을 사용함을 권고합니다.

### 두번째

![img.png](/images/2021년/0222/deprecated-2.PNG)

다이제스트 기반의 암호화 방식이니 단방향 암호화 방식을 사용하는 것을 권고합니다.

### 참고.

에러내용.

![img.png](/images/2021년/0222/error.PNG)


<a href="https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/StandardPasswordEncoder.html">참고 : 스프링 공식문서</a>

<a href="https://docs.spring.io/spring-security/site/docs/current/reference/html5/#authentication-password-storage-dpe">참고 : 스프링 시큐리티 래퍼런스</a>

<a href="https://java.ihoney.pe.kr/498">참고 : 허니몬(Honeymon)의 자바guru 블로그</a>

---
