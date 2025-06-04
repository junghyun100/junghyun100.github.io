---
layout: post
title: "Swagger-Gradle"
tags: [Swagger]
comments: true

---

Swagger를 공부하면서 정리를 한 내용들 입니다.

---

# Swagger

<img src="https://res.cloudinary.com/practicaldev/image/fetch/s--_oGMLSEb--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/wnteed7b20ci6cz59twl.png">

스웨거(Swagger)는 개발자가 REST 웹 서비스를 

설계, 빌드, 문서화, 소비하는 일을 도와주는 오픈 소스 소프트웨어 프레임워크입니다. 

API 서버가 어떤 데이터를 주고 받는지에 대한 문서 작업을 간단한 설정을 통해서

지정 URL들을 HTML 화면으로 보여줍니다.

## Swagger 설정

설정하는 방법은 Gradle 방법을 기준으로 작성했습니다.

### 1. 의존성 추가

일단 의존성을 추가해야합니다.

그러기 위해서 MVNRepository 사이트에서 2가지를 찾습니다.

<img src="/images/2021년/0206/SwaggerMVNRepository1.PNG">

<img src="/images/2021년/0206/SwaggerMVNRepository2.PNG">

swagger의 사용을 위한 용도와 swagger를 보기 좋게 하려는 UI에 대한 의존성입니다.

이것을 build.gradle에 추가해줍니다.

```
dependencies {
	...
	compile group: 'io.springfox', name: 'springfox-swagger2', version: '2.9.2'
	compile group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.9.2'
	...
}
```

### 2. 프로젝트에 등록

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
                .select()
                .paths(PathSelectors.any())
                .apis(RequestHandlerSelectors.any())
                .build();
    }
    private ApiInfo apiInfo(){
            return new ApiInfoBuilder()
                    .title("User Infomation with Swagger")
                    .description("User정보에 대한 Swagger")
                    .build();
    }
}
```

Docket Bean을 정의한 후 select 메서드는 ApiSelectorBuilder 인스턴스를 반환합니다.

RequestHandlerSelectors와 PathSelectors를 이용해서 어떤 경로, 어떤 정보를 

Swagger측에 보여줄 것인지 설정할 수 있습니다.

any()가 예로 되어있는데 이것은 전체 API에 대한 문서를 사용하도록 했습니다.

* @EnableSwagger2
  
		Swagger2 버전을 활성화 하겠다는 어노테이션입니다.

* Docket

		Swagger 설정의 핵심이 되는 Bean입니다.
		API 자체에 대한 스펙은 컨트롤러에서 작성합니다.

	* groupName()
		
			Docket Bean이 한 개일 경우 기본 값은 default이므로, 생략가능
			여러 Docket Bean을 생성했을 경우
			groupName이 충돌하지 않아야 하므로, 각 Docket Bean의 버전을 명시합니다.
	
	* select()
	  
			ApiSelectorBuilder를 생성합니다.
	
	* apis()
	  
			api 스펙이 작성되어 있는 패키지를 지정합니다.
			즉, 컨트롤러가 존재하는 패키지를 basepackage로 지정하여,
	  		RequestMapping( GetMapping, PostMapping ... )이 선언된 API를 문서화합니다.

	* paths()

			apis()로 선택되어진 API중 특정 path 조건에 맞는 API들을 다시 필터링하여 문서화합니다.
	
	* apiInfo()
	  
			제목, 설명 등 문서에 대한 정보들을 보여주기 위해 호출합니다.
			파라미터 정보는 다음과 같습니다.
			title, description, version, termsOfServiceUrl, contact, 
	  		license, licenseUrl, vendorExtensions

### 3. 어노테이션 추가

이제 우리가 원하는 API를 문서에 넣기 위해 컨트롤러 측에 어노테이션으로 설정합니다.

* @Api

해당 클래스가 Swagger 리소스라는 것을 명시합니다.

      value : 
      태그를 작성합니다.

      tags : 
      사용하여 여러 개의 태그를 정의할 수도 있습니다.

* @ApiOperation

  한 개의 operation(즉 API URL과 Method)을 선언합니다.

      value : 
      API에 대한 간략한 설명(제목같은 느낌으로)을 작성합니다.
  
      notes : 
      더 자세한 설명을 작성해줍니다.
* @ApiResponse

  operation의 가능한 response를 명시합니다.

      code : 
      응답코드를 작성합니다.
  
      message : 
      응답에 대한 설명을 작성합니다.
  
      responseHeaders : 
      헤더를 추가할 수도 있습니다.
* @ApiParam

  파라미터에 대한 정보를 명시합니다.

      value : 
      파라미터 정보를 작성합니다.
  
      required :
      필수 파라미터이면 true, 아니면 false를 작성합니다.
  
      example : 
      테스트를 할 때 보여줄 예시를 작성합니다.


아래는 약간의 예시입니다.

```java

@Api
@RestController
@RequestMapping("/api/v1/users")
@CrossOrigin(origins = {"*"})
@RequiredArgsConstructor
public class UserController {

    // 일부 내용 삭제
    ...
    @ApiOperation(value="사용자 한명 조회", notes="사용자 한명 조회")
    @ApiImplicitParam(name = "userEmail", value = "사용자 이메일", required = true)
    @GetMapping("/{userEmail}")
    public BaseResponse user(@PathVariable String userEmail) {

```

* @ApiModel 

		어떤 객체에 대해서 작성 할 것인가에 대해서 정해주는 어노테이션입니다.

* @ApiModelProperty

		객체의 필드 내용을 작성하는 어노테이션입니다.
		작성한 내용은 API에서 확인이 가능합니다.
        * value - 파라메터 이름
        * name - Swagger에서 입력 받을 리얼 파라메터 명
        * notes - 부가설명
        * example - 지정된 임의 테스트 값을 입력 함
        * required - 필수 입력 여부

* @JsonForamt

		해당 어노테이션은 Date 타입인 reg_date를 JsonFormat으로 변경합니다.
		계층간의 데이터를 주고 받을 때 Json형식으로 주고받기 때문에 Format을 변경해주지 않으면 
  	Date 값이 전달이 되지가 않습니다.

```java

@Data
@NoArgsConstructor
@Entity
@ApiModel
public class Post {
	public Post(String title) {
		this.title = title;
	}
	
	@Id
	@GeneratedValue
	@ApiModelProperty(value="아이디")
	private Long id;
	
	@ApiModelProperty(required = true, value="제목")
	private String title;
	
	@ApiModelProperty(required = true, value="내용")
	private String content;
}

```

### 확인

<a href="http://localhost:8080/swagger-ui.html"> http://localhost:8080/swagger-ui.html </a>

이제 해당 사이트의 URL에 들어가서 확인해봅니다.

URL의 설정은 다른 서버 포트 주소를 사용할 수 있으니 찾아보면 될 듯 합니다.

---
