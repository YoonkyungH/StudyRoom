## MVC 패턴
변경이 라이프 사이클이 다르면 엄청 불편하다.  
예를 들어, UI의 일부 수정과 비즈니스 로직을 수정하는 일은 각각 다르게 발생할 가능성이 높고 대부분 서로에게 영향을 미치지 않는다.  
이를 하나의 코드로 관리한다는 것은 유지보수성이 떨어지게 만든다.  

그래서 MVC 패턴이 등장하였다.  
Model-View-Controller  

**Controller**: HTTP 요청을 받아 비즈니스 로직을 수행  
**Model**: 뷰에 출력할 데이터를 담음  
**View**: HTML을 생성  

> ### 컨트롤러 Controller & 서비스 Service  
> 컨트롤러에 비즈니스 로직을 둘 수는 있지만, 이는 컨트롤러가 **너무 많은 역할을 담당**하게 된다.  
> 그래서 비즈니스 로직을 **Service 서비스 계층을 별도로 생성하여 처리**한다.  
> 그리고 **Controller 컨트롤러**는 비즈니스 로직이 있는 **서비스를 호출**하는 역할을 담당한다.  
>   
> 비즈니스 로직을 변경하면 비즈니스 로직을 호출하는 컨트롤러의 코드도 함께 변경될 수 있다.  

(MVC 패턴에서는 항상 컨트롤러를 거친 후 뷰를 탄다.)

> (참고) **/WEB-INF**  
> : 해당 경로에 JSP 파일을 넣으면 외부에서 직접 JSP를 호출할 수 없다. (컨트롤러를 반드시 거쳐 JSP를 호출시키기 위하여)

> ### redirect vs forward  
> **리다이렉트**는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가 클라이언트가 redirect 경로로 다시 요청한다.  
> 따라서 **클라이언트가 인지할 수 있고 URL 경로 또한 실제로 변경**된다.  
>  
> 반면, 포워드는 **서버 내부**에서 일어나는 호출로 **클라이언트가 인지할 수 없다.**

### SpringMVC
: Spring에서 제공하는 웹 모듈로 Model, View, Controller 세 가지 구성요소를 사용해 사용자의 다양한 HTTP Request를 처리하고 단순한 텍스트 형식의 응답부터 REST 형식의 응답은 물론 View를 표시하는 html을 반환하는 응답까지 다양한 응답을 제공하는 프레임워크

> **RequestDispatcher**  
> : 클라이언트로부터 최초에 들어온 요청을 JSP/Servlet 내에서 원하는 자원으로 요청을 넘기는 역할을 수행하거나 특정 자원에 처리를 요청하고 처리 결과를 얻어오는 기능을 수행하는 클래스이다.

### 스프링 MVC 구조
주 구성요소로는 Model, View, Controller가 있지만 이들이 유기적으로 동작하기 위해 DispatcherServlet(Front Controller), Handler(Controller), ModelAndView, ViewResolver가 존재한다.

<img width="1390" alt="image" src="https://user-images.githubusercontent.com/57247474/180902552-0bc6c2b9-7678-4401-8877-48f010d6e800.png">


- **DispatcherServlet(Front Controller)**  
: 제일 앞단에서 HTTP Request를 처리하는 컨트롤러  
- **Controller(Handler)**  
: HTTP Request를 처리해 Model을 만들고 View를 지정  
> **HandlerMapping** 우선순위  
> 1. RequestMappingHandlerMapping: 어노테이션 기반 컨트롤러인 `@RequestMapping`에서 사용  
> 2. BeanNameUrlHandlerMapping: 스프링 빈의 이름으로 핸들러를 찾음  
>   
> **HandlerAdapter** 우선순위  
> 1. RequestMappingHandlerAdapter: 어노테이션 기반의 컨트롤러인 `@RequestMapping`에서 사용  
> 2. HttpRequestHandlerAdapter: HttpRequestHandler 처리  
> 3. SimpleControllerHandlerAdapter: Controller 인터페이스(어노테이션 아님, 과거에 사용) 처리  
- **ModelAndView**  
: Controller에 의해 반환된 Model과 View가 Wrapping 된 객체  
> **Model vs ModelAndView**  
> Model은 데이터만 저장,  
> ModelAndView는 데이터와 이동하고자 하는 view page를 함께 저장  
- **ViewResolver**  
: ModelAndView를 처리하여 View 그리기  
> 뷰 리졸버를 등록하기 위해서는 application.properties에 다음을 등록  
> `spring.mvc.view.prefix=/경로`  
> `spring.mvc.view.suffix=.확장명`  

### 흐름
서블릿 호출  
-> HttpServlet이 제공하는 service() 호출  
-> 스프링 MVC는 DispatcherServlet의 부모인 FrameworkServlet에서 service()를 오버라이드 하고있음  
-> FrameworkServlet.service()를 시작으로 여러 메소드가 호출되며 **DispatcherServlet.doDispatcher()** 를 호출  

> ### doDispatcher() (핵심)
> 코드를 분석해보면
>
> 1. 핸들러 조회
> 2. 핸들러 어댑터 조회 - 핸들러 처리 가능한 어댑터
> 3. 핸들러 어댑터 실행
> 4. 핸들러 어댑터를 통해 핸들러 실행
> 5. ModelAndView 반환
> 6. 뷰 리졸버를 통해 뷰 찾기
> 7. View 반환
> 8. 뷰 랜더링

### 실무에서 사용하는 스프링 MVC 코드
```java
...
@RequestMapping("/save")
public ModelAndView save(HttpServletReqeust request, HttpServletResponse response) {
  String username = request.getParameter("username");
  int age = Integer.parseInt(request.getParameter("age"));
  
  Member member = new Member(username, age);
  memberRepository.save(member);
  
  ModelAndView mv = new ModelAndView("save-result");
  mv.addObect("member", member);
  
  return mv;
}
```

위 코드를 실제 실무에서 사용하는 코드로 변경하면 아래의 코드가 된다.  

```java
...
@PostMapping("/save")
public String save(
  @RequestParam("username") String username,
  @RequestParam("age") int age,
  Model model)
{
  Member member = new Member(username, age);
  memberRepository.save(member);
  
  model.addAttribute("member", member);
  
  return "save-result";
}
  
```

> `@RestController`  
> : **@Controller**는 반환값이 String이면 뷰 이름으로 인식하여 **뷰를 찾고 랜더링**한다.  
> 반면, @RestController는 반환시 **HTTP 메시지 마디에 바로 입력**하여 실행 결과로 반환 하려는 문자열을 그대로 반환받을 수 있다.
