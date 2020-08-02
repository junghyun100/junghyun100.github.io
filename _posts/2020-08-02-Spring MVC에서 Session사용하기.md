---
layout: post
title: "Spring MVC에서 Session사용하기"
tags: [Spring 에서의 Session 사용법]
comments: true
---
 
해당 Post는 Spring MVC에서 제공하는 세션 관련 애노테이션에 대해 알아보고, 해당 해노테이션에 대해서 정리한 파일입니다.

---

### @SessionAttributes & @ModelAttribute

@SessionAttributes 파라미터로 지정된 이름과 같은 이름이 @ModelAttribute에 지정되어 있을 경우 메소드가 반환되는 값은 세션에 저장됩니다.

아래의 예제는 세션에 값을 초기화하는 목적으로 사용되었습니다.

```js
@SessionAttributes("user")
public class LoginController {
  @ModelAttribute("user")
  public User setUpUserForm() {
  return new User();
  }
}
```
 
@SessionAttributes의 파라미터와 같은 이름이 @ModelAttribute에 있을 경우 세션에 있는 객체를 가져온 후, 클라이언트로 전송받은 값을 설정합니다.
```js
@Controller
@SessionAttributes("user")
public class LoginController {
......
  @PostMapping("/dologin")
  public String doLogin(@ModelAttribute("user") User user, Model model) {
......
  }
}
```

### @SessionAttribute

메소드에 @SessionAttribute가 있을 경우 파라미터로 지정된 이름으로 등록된 세션 정보를 읽어와서 변수에 할당합니다.

```js
@GetMapping("/info")
public String userInfo(@SessionAttribute("user") User user) {
//...
//...
return "user";
}
```

### SessionStatus

SessionStatus 는 컨트롤러 메소드의 파라미터로 사용할 수 있는 스프링 내장 타입입니다.

이 오브젝트를 이용하면 현재 컨트롤러의 @SessionAttributes에 의해 저장된 오브젝트를 제거할 수 있습니다.
```js
@Controller
@SessionAttributes("user")
public class UserController {
...... 
    @RequestMapping(value = "/user/add", method = RequestMethod.POST)
    public String submit(@ModelAttribute("user") User user, SessionStatus sessionStatus) {
  ......
  sessionStatus.setComplete();
                                   ......

   }
 }
 ```
 
## Spring MVC - form tag 라이브러리

modelAttribute속성으로 지정된 이름의 객체를 세션에서 읽어와서 form태그로 설정된 태그에 값을 설정합니다.

```js
<form:form action="login" method="post" modelAttribute="user">
Email : <form:input path="email" /><br>
Password : <form:password path="password" /><br>
<button type="submit">Login</button>
</form:form>
```

---
