# springboot 복습 : 간단한 web app

** 간단한 회원등록 및 조회 web app
- 목적
  - 스프링부트 초기세팅
  - MyBatis, JPA 기본
  - 간단한 TestCode 작성
  - Error 처리 => Business Exception, Validation Exception
  - Restful api

1) 개발환경
- Intelli J
- Spring Boot 2.6.3
- java 11
- Repository : memory(Map), RDMS(MyBatis), JPA(Spring Data JPA)
- h2(mysql)
- gradle
- view template engine >> thymeleaf(.html)

2) 프로젝트 구조 : 도메인형
```
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── example
    │   │           └── springboot
    │   │               ├── SpringbootApplication(C)
    │   │               ├── ViewController(C)
    │   │               ├── config    
    │   │               │   ├── MybatisCofig1(C)
    │   │               │   └── MybatisCofig2(C)
    │   │               ├── global    
    │   │               │   └── error
    │   │               │         ├── BusinessException(C)
    │   │               │         ├── ErrorCode(E)
    │   │               │         ├── ErrorResponse(C)
    │   │               │         └── GlobalExceptionHandler(C)
    │   │               └── members     
    │   │                      ├── controller
    │   │                      │   └── MemberApiController(C)    
    │   │                      ├── service
    │   │                      │   ├── JpaMemberService(C)    
    │   │                      │   └── MemberService(C)
    │   │                      ├── repository
    │   │                      │   ├── MemberRepository(I)
    │   │                      │   ├── MemoryMemberRepository(C)
    │   │                      │   ├── DBMemberRepository(C)
    │   │                      │   ├── JpaMemberRepository(I)
    │   │                      │   └── SpringDataJpaMemberRepository(I)
    │   │                      ├── domain
    │   │                      │   ├── JPAMember(C)    
    │   │                      │   └── Member(C)
    │   │                      └── dto
    │   │                      │   ├── request
    │   │                      │   │       └── RequestSignUpMemberDto(C)
    │   │                      │   └── response
    │   │                      │           └── ResponseMemberSelectDto(C)
    │   │                      └── exception
    │   │                          ├── ExistAccountException(C)
    │   │                          └── NoFindUserException(C)        
    │   │                      
    │   └── resources
    │       ├── mappers : SQL Query xml
    │       ├── static  : static Resource(js, css, img...)
    │       ├── templates : thymeleaf template : html
    │       ├── mybatis-config.xml           
    │       └── application.yaml
```

3) 비즈니스 요구사항 정리
- 데이터: 회원 id, 이름
- 기능: 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)
- 구조
  - controller : Rest API -> MemberController
  - service : Business Logic -> MemberService
  - repository : Map or JPA or MyBatis ->  MemberRepository(interface)
    - 아직 데이터 저장소가 선정되지 않아서, 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계 
    - 데이터 저장소는 RDB, NoSQL 등등 다양한 저장소를 고민중인 상황으로 가정 
    - 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용
  - domain Model : Member
  - dto : request~Member~Dto, response~Member~Dto

4) TestCode : JUnit5
- 단위 Test : Service, Repository Test

5) Error 처리
- BusinessException extends RuntimeException => 사용자 예외정의 및 처리
- Sign-up Backend Validation Exception 처리
- GlobalExceptionHandler >> 전역 Exception관리
- ErrorCode : Enum으로 관리

6) Rest API
```
* Sign-Up
- POST /api/members
- requestParameter
RequestSignUpMemberDto  |  {} json Object

requestSignUpMemberDto = {
    "name" : ""     |  사용자 이름  |  String
}

- response Data

[ok]
sign-up userId(pk) | Long 
200 ok             | HttpStatus

[error]
ExistMember Exception
{
    "status": 400,
    "code": "A001",
    "message": "This member is exist."
}

------------------------------------------------------

* Get Member List
- GET /api/members
- requestParameter : X
- response Data

List<ResponseMemberSelectDto>  | [{},{},{}....] json Object List
[
    {
      "id" : 1,
      "name" : "mmm"
    },{},{},{}...
]

200 ok                         | HttpStatus
```






