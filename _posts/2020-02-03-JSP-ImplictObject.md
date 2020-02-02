---
layout: post
title: "JSP 내장 객체"
tags: [JSP]
comments: true
---
 
해당 Post는 JSP에서의 내장 객체에 대해 정리한 파일입니다.

---

### Implict Object

개발자가 선언하지 않았음에도 어떻게 JSP에서 사용할 수 있는 객체들이 있습니다.

이게 가능한 이유를 설명드리겠습니다.

JSP는 항상 서블릿으로 바뀌어서 실행된다고 했었습니다.

이때 이 JSP가 변형된 파일에서 코드의 위쪽에 보면은 미리 선언된 객체들이 존재합니다.

이런 객체들을 JSP에서 바로 사용할 수가 있는데요. 이런 객체들을 내장 객체라고 합니다.

service라는 메서드 안에 보면 아래의 사진처럼 나타납니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/implictObject.PNG" alt="My Image">

> response, request, application, seesion, out와 같이 미리 선언된 객체들이 존재한다는 것을 알 수 있었습니다.

#### 내장객체의 종류

<img src="https://t1.daumcdn.net/cfile/tistory/191371335024CFD92C" alt="My Image">

사진출처 : <a href = "https://devbada.tistory.com/199">Web Develop Engineer - minam.cho</a>

간단한 예시를 통해 알아보겠습니다.

```java

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Implict Object</title>
</head>
<body>
<%
	StringBuffer url = request.getRequestURL();
	out.print("url : " + url.toString());
	out.print("<br>");
%>
</body>
</html>

```

해당소스코드에서 requset를 통해 URL을 받아오고,

url에서 그 값을 받은 후 print하는 코드입니다.

해당 결과는 아래의 사진과 같습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/implictObject%EA%B2%B0%EA%B3%BC.PNG" alt="My Image">

### 조금 더 알아보기..

내장객체를 JSP 선언문에서 사용할 수 있을까요? 사용할 수 없다면 왜 그럴까요?

선언문(<%! %>) 이외의 표현식(<% %>, <%= %>, <%@ %>)은 JSP가 서블릿으로 바뀌어질 때

public void jspService 메서드 안에 코드가 작성됩니다.

기본적으로 출력(out), 세션(out), 요청(request), 응답(response)등을 담당하는 객체들이 내장되어 있는데 내장된 객체는 해당 jspService  메소드 '안'에서 사용 할 수 있습니다. 

JSP선언문에서 선언하면 써블릿에서는 service메소드 밖에 선언하는 것입니다. 하지만 내장객체는 써블릿의 service메소드 안에 선언되어 있기 때문에 사용할 수 없습니다.

---
