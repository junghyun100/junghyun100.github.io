---
layout: post
title: "Multipart/form-data란?"
tags: [Multipart/form-data]
comments: true

---

프로젝트를 하면서 알게 되었던 Multipart/form-data에 대해 정리를 한 내용 입니다.

---

이번 프로젝트를 진행하면서 `Vue` 페이지 쪽에서 백엔드 쪽으로 이미지 파일을 전송하는 경우가 있었습니다.

이때 사용한 `Multipart`에 대해서 간단히 알아봤습니다.

# form이란?

입력 양식 전체를 감싸는 태그입니다.

`form`은 컨트롤 요소(`control element`)로 구성됩니다. 

> 텍스트, 버튼, 라디오 등이 컨트롤

* name : form의 이름, 서버로 보내질 때 이름의 값으로 데이터 전송합니다.

* action : form이 전송되는 서버 url 또는 html 링크입니다.

* method : 전송 방법 설정. get은 default, <br>post는 데이터를 url에 공개하지 않고 숨겨서 전송하는 방법입니다.

* autocomplete : 자동 완성. on으로 하면 form 전체에 자동 완성 허용합니다.

* enctype : 폼 데이터(form data)가 서버로 제출될 때 <br>해당 데이터가 인코딩되는 방법을 명시합니다.

그 중 오늘의 포스트는 `entype`의 속성값 중 하나입니다.

이 속성은 `<form>` 요소의 method 속성값이 `“post”`인 경우에만 사용할 수 있습니다.

## entype 속성값

*  application/x-www-form-urlencoded
> default 값으로, 모든 문자들을 서버로 보내기 전에 인코딩됨을 명시합니다.

* text/plain
> 공백 문자(space)는 "+" 기호로 변환하지만, <br>나머지 문자는 모두 인코딩되지 않음을 명시합니다.

* multipart/form-data ( ★ 오늘의 포스팅 주제)
> 모든 문자를 인코딩하지 않음을 명시합니다.<br>이 방식은 `<form>` 요소가 파일이나 이미지를 서버로 전송할 때 주로 사용합니다.

```java
<form action="/home/uploadfiles" method="post" enctype="multipart/form-data">
    파일명 : <input type="file" name="myfile">
    <button type="submit">제출하기</button>
</form>
```

## getParameter()의 사용

`POST` 방식에서 `request.getParameter()`메서드를 

`WAS`에서 알아서 처리할 수 있도록 되어있는 이유는 

`form`에서 method가 `POST`방식일 때는 디폴트값으로 

`enctype="application/x-www-form-urlencoded"` 옵션이 설정 되어있기 때문에 

이를 `WAS`에서 인식하고 알아서 `in`/`output`방식으로 데이터를 처리하기 때문입니다.

따라서. 이미지를 위해서 전송하는 경우 `enctype`가 `Multipart`로 설정해야하기 때문에

`request.getParameter()`로 데이터를 불러올 수 없게 됩니다.

참고 : [출처]https://gunbin91.github.io/jsp/2019/05/28/jsp_11_file.html  [gunbin91 Blog]

## Multipart가 생긴 이유

파일을 업로드 할 때, 사진 설명을 위한 `input`과 사진을 위한 `input` 2개가 들어간다고 가정합니다. 

이 두 `input` 간에 `Content-type`은 사진 설명은 `application/x-www-form-urlencoded` 이 될 것이고, 

사진 파일은 `image/jpeg`입니다. 

두 종류의 데이터가 하나의 `HTTP Request Body`에 들어가야 하는데, 

한 `Body`에서 이 2 종류의 데이터를 구분에서 넣어주는 방법도 필요해졌습니다.

그래서 등장하는 것이 `multipart` 타입입니다.

참고 : [출처]http://kaludin.egloos.com/v/2270972

## MultipartRequest

`MultipartRequest` 객체를 알아보기 위해서 아래의 사이트를 참고하였습니다.

[출처]http://www.servlets.com/cos/javadoc/com/oreilly/servlet/MultipartRequest.html

```java
MultipartRequest multi = new MultipartRequest(request, savePath, sizeLimit, "utf-8", new DefaultFileRenamePolicy());
```

해당 객체에 들어가는 파라미터들은 아래와 같습니다.

* HttpServletRequest request = request 객체

* String saveDirectory =저장될 서버 경로 

* int maxPostSize = 파일 최대 크기

* String encoding = 인코딩 방식

* FileRenamePolicy policy = 같은 이름의 파일명 방지 처리

## 관련 메서드

### mr.getContentType(“name”);

보낸 파일의 형태가 무엇인지를 반환합니다. 

=> 이미지파일만 허용하는 등의 기능에서 사용 ex ) image/jpeg (jpg일 경우)

### mr.getOriginalFileName(“form에서 보낸 파일의 name”);

form에서 선택한 원래 파일의 이름

### mr.getFilesystemName(“form에서 보낸 파일의 name”);

업로드된 파일의 이름( 리네임 되었을 시 리네임된 이름 )

### mr.getFile(“form에서 보낸 파일의 name”);

업로드한 파일의 객체를 반환 – File

### mr.getFileNames()

파일형태의 인풋데이터의 `name` 속성을 `Enumeration`객체로 반환해줍니다.

따라서 for-each문으로 `name`을 뽑아낼 수 있습니다.

---
