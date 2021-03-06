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


## BindingResult
: `org.springFramework.validation.BindingResult`   
Errors의 하위 인터페이스로 폼 값을 **커맨드 객체에 바인딩한 결과를 저장**하고 **에러코드로 메시지를 가져옴**   

#### Errors 인터페이스의 에러 발생 여부를 확인하기 위한 메소드
- `boolean hasErrors()`
: 에러가 존재할 경우 true 반환
- `int getErrorCount()`
: 에러 개수 반환
- `boolean hasGlobalErrors()`
: reject() 메소드를 이용해 추가된 글로벌 에러가 존재할 경우 true 반환
- `int getGlovalErrorCount()`
: reject() 메소드를 이용해 추가된 에러 개수 반환
- `boolean hasFieldErrors()`
: rejectValue() 메소드를 이용해 추가된 에러 발생할 경우 true 반환
- `int getFieldErrorCount()`
: rejectValue() 메소드를 이용해 추가 에러 개수 반환
- `boolean hasFieldErrors(String field)`
: rejectValue() 메소드 이용해 추가한 특정 필드의 에러가 존재할 경우 true 반환
- `int getFieldErrorCount(String filed)`
: rejectValue() 메소드를 이용해 추가한 특정 필드의 에러 개수 반환   

- `getFieldErrors`   
: <FieldError> getFieldErrors() 나열   
- `getFileError`   
: 필드와 관련된 오류가 있는 경우 첫번째 오류를 가져옴   

### 🤷🏻‍♀️ @Valid 어노테이션을 사용할 경우 BindingResult를 Mock 객체로 만들어야 할 때는?   
Mock 객체를 사용할 수 있도록 **setup 코드를 삽입**하여 해당 BindingResult를 무시하고 테스트 수행할 수 있도록 하기   
  
```java
    @Before
    public void setup() {
        MockitoAnnotations.initMocks(this);
        Mockito.when(mockBindingResult.hasErrors()).thenReturn(true);
    }
```
