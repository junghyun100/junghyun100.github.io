---
layout: post
title: "Gradle에서 Jar 파일 생성하기"
tags: [Gradle, Jar]
comments: true

---

Gradle에서 Jar 파일 생성하는 방법에 대해 간략하게 정리했습니다.

---

# Jar파일이 왜 필요한가요?

로컬의 개발 환경(ex: Spring boot)에서 어플리케이션을 실행하는 것은 

IDE에서 `Run`을 누르거나, `Intellij`를 사용한다면 `[프로젝트명]Application`에서 

좌측의 `▶`를 클릭하면 실행이 됩니다. 

그러나 spring boot 어플리케이션을 배포할 때는 

패키징된 jar 파일을 명령어로 실행시키는 방식을 사용하기 때문입니다.

# Jar 파일 생성하기

테스트를 위해 `JarTest`라는 이름의 프로젝트를 하나 만들어줬습니다.

<img src="/images/2021년/0323/프로젝트생성직후.PNG">{: .center-image}

프로젝트의 생성 직후의 모습은 다음과 같습니다.

## Gradle 누르기

이제 오른쪽의 Gradle을 눌러줍니다.

<img src="/images/2021년/0323/오른쪽.PNG">{: .center-image}

이렇게 눌러줬을 때 `Tasks`, `Dependencies`, `Run Configuration`이 있습니다.

저희는 Jar로 들어가는 것을 원하니 `Tasks` > `build`로 들어가 줍시다.

<img src="/images/2021년/0323/오른쪽2.PNG">{: .center-image}

여기에는 다양한 기능들이 있습니다.

그 중 `bootJar`와 `clean`, `jar`를 실행해 보겠습니다.

## jar

<img src="/images/2021년/0323/Jar.PNG">{: .center-image}

`jar`를 눌렀습니다.

그러먼 아래에는 실행이 돌아가면서 `Task`가 동작합니다.

이때 내용으로는 jar가 `SKIPPED`되었다고 나오면서, jar 동작이 끝이 납니다.

<img src="/images/2021년/0323/Jar2.PNG">{: .center-image}

왼편을 보면 build 폴더가 생겨있고, 몇가지 파일들이 생성된 것을 확인할 수 있습니다.

<img src="/images/2021년/0323/Jar3.PNG">{: .center-image}

그러나 디렉토리를 살펴봐도 만들고자 했던 Jar파일은 찾을 수 없습니다.

## clean

<img src="/images/2021년/0323/Clean.PNG">{: .center-image}

이번엔 `clean`을 동작 시켜봤습니다.

clean를 동작시키면 이름처럼 무언가를 없애줄 것을 예상할 수 있습니다.

<img src="/images/2021년/0323/Clean2.PNG">{: .center-image}

실행을 시켜보면 아까 jar를 동작시켜서 만들었던 `build 파일들이 사라지게` 됩니다.

## bootJar

<img src="/images/2021년/0323/BootJar.PNG">{: .center-image}

이번에 `bootJar`를 실행시켜봅니다.

<img src="/images/2021년/0323/BootJar2.PNG">{: .center-image}

실행을 시켰을 때 `build` > `libs`를 들어가면

우리가 만들고자했던 jar파일이 

`[프로젝트명]+[build.gradle의 version].jar`으로 생성되었습니다.

### jar파일을 실행하려면 아래의 명령을 실행하면 됩니다.

```java
java –jar [jar파일의 이름].jar
```

---
