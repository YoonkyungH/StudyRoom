# ExceptionHandler
### `@ControllerAdvice`   
: **컨트롤러를 보조하는 클래스**   

`@Controller` 어노테이션이 있는 모든 곳에서 예외를 잡을 수 있게 해준다.   
`@ControllerAdvice` 안에 있는 `@ExceptionHandler`는 모든 컨트롤러에서 발생하는 예외 상황을 잡을 수 있다.   

즉, `@Controller` 어노테이션을 갖거나, xml 설정 파일에서 컨트롤러로 명시된 클래스에서 Exception이 발생하면 감지한다.   
(`@RestControllerAdvice` 어노테이션과 유사)  

Controller와 RestController만 ExceptionHandler의 감시 대상이 된다. (Service만 감시 대상으로 등록할 수는 없음)   

하지만 Controller와 Service를 호출한 경우, **Service에서 Exception이 발생해도 결국 Controller로부터 문제가 발생했음을 감지하게 때문에 Handler가 작동**한다.   
(그리고 이 어노테이션으로 특정한 클래스만 명시할 수도 있다.)   


### `@ResponseBody`   
: `@RestController` = `@Controller` + `@ResponseBody`   

ExceptionHandler는 controller 혹은 restController가 아니다.   
응답 값은 컨트롤러처럼 **String 또는 ModelAndView**만 가능하다.   
따라서 응답 객체를 반환하고자 한다면 `@ResponseBody` 어노테이션을 메소드 위 명시해주어야 한다.   


### `@ExceptionHandler`   
: `@Controller`, `@RestController`**가 적용된 Bean에서 발생하는 예외를 잡아 하나의 메소드에서 처리해주는 기능**   

`@ExceptionHandler`에 설정한 **예외가 발생하면 handler가 실행**된다.   
`@Service`, `@Repository`가 적용된 Bean에서는 **사용할 수 없다.**   

`@ControllerAdvice` **가 명시된 클래스 내부 메소드에 사용**한다.   
Attribute로 Exception 클래스를 받는다.   

즉, RuntimeException.class 또는 그보다 더 상위 클래스인 Exception.class 등을 넘기면 된다.   

**`@ExceptionHandler(XXException.class)`라고 작성한 경우, `@ControllerAdvice`에서 명시한 클래스에서 `throw new XXException(...)`이 발생하면 핸들러는 이를 감지하고 해당 메소드를 수행**한다.   

메소드는 여러개 작성할 수 있으며 이에 따라 `@ExceptionHandler`에 다른 Attribute 값을 넘김으로써 각 예외를 다르게 처리할 수 있다.   

value를 통해 어떤 예외를 잡을지 지정할 수 있다. 하지만 지정하지 않으면 모든 예외를 잡아버린다.   
물론 여러 예외를 한꺼번에 정할 수도 있다. (Ex. `@ExceptionHandler({Exception.class, ...})` ➡️ List 형태로)    

[참고](https://tecoble.techcourse.co.kr/post/2021-05-10-controller_advice_exception_handler/)
