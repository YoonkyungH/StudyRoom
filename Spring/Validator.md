## 유효성 검증
### Validator
: 스프링에서 도메인 객체를 검증할 수 있도록 제공하는 인터페이스   

Controller로 HTTP 요청을 `@ModelAttribute` 모델에 바인딩 할 때 주로 사용된다.   
이 인터페이스는 `supports()`와 `validate()` 메소드로 구성되어 있다.   

`supports()`는 이 검증기가 **검증할 수 있는 오브젝트 타입인지 확인해주는 메소드**로 이 메소드를 **통과한 경우에만** `validate()`가 호출된다.   

### 유효성 검사 Annotation
- `@Valid`: 대상 객체의 확인 조건을 만족하는 경우 통과   
(Spring3 부터 Spring MVC에서는 컨트롤러 메소드의 파라미터를 자동으로 검증하는 이 자동검증 어노테이션을 제공한다.)   

- `@Size(min=, max=)`: 문자열 또는 배열이 **지정된 값 사이**일 경우 통과   

- `@Positive`: **양수**만   

- `@NotEmpty`: Null, 빈 문자열 불가

[➡️ 어노테이션 참고](https://bamdule.tistory.com/35)   

#### 동작 원리
모든 요청은 디스패처 서블릿을 통해 컨트롤러로 전달된다. 그리고 컨트롤러의 메소드를 호출하는 과정에서는 메소드 값을 처리해주는 ArgumentResolver가 동작한다.   
`@Valid` 또한 **ArgumentResolver에 의해 처리**된다.   

`@Valid`는 기본적으로 **컨트롤러에서만 동작**한다.   
**다른 계층에서 파라미터를 검증**하기 위해서는 `@Valicated`**와 결합**되어야 한다.   

***파라미터의 유효성 검증은 컨트롤러에서 처리하고,***   
***서비스나 리포지토리 계층에서는 유효성 검증을 하지 않는 것이 바람직하다.***   

하지만, 그러지 못할 경우가 발생한다면 아주 경우가 없는 것은 아니다.   
Spring은 **AOP 기반으로 메소드 요청을 가로채 유효성 검증**을 할 수 있도록 `@Validated`를 제공한다.   

(이는 JSR 표준 기술이 아니며 Spring 프레임워크에서 제공하는 어노테이션임)   
(컨트롤러, 서비스, 리포지토리 .. 계층에 무관하게 **스프링 빈이라면 유효성 검증 가능**)   

`@Valid`와 `@Validated`에 의한 예외 클래스는 다르다.   
- `@Valid`: MethodArgumentNotValidException   
- `@Validated`: ConstraintViolationException   
