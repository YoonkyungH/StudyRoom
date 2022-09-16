# HTML
: 확장자가 html인 파일로 **클라이언트의 브라우저에 의해 내용을 해석해 실행**하며 자바나 톰캣 서버가 설치되어 있지 않아도 정상적으로 실행된다.

- 변화가 없는 단순 상수값을 출력할 때 사용

# JSP
: Java Server Page, html 코드에 java 코드를 넣어 동적 웹 페이지를 생성하는 웹 어플리케이션 도구  
(jsp가 실행되면 자바 서블릿으로 변환되며 웹 어플리케이션 서버에서 동작되면서 필요한 기능을 수행하고 그렇게 생성된 데이터를 웹 페이지와 함께 클라이언트로 응답)

**톰캣 서버가 번역해 그 결과를 html 태그로 변환한 후 웹 브라우저에 내려 보내는데**, 톰캣이 이를 동작시키기 위해서는 JSP 파일은 확장자가 jsp여야 하며 페이지에 jsp임을 알리는 페이지 지시자인 `<%@ page%>`가 반드시 필요하다.

jsp문서는 html 태그 사이 <% %> (스크립트릿 태그)를 추가해 그 안에 자바 코드를 집어넣으면 서블릿 컨테이너는 이 부분을 jsp로 인식하여 이를 해석한 후 html 형태로 변환한다.

- 서버 측에서 동작해야 할 코드들이 있을 경우 사용. 서버에서 보낸 데이터에 따라 값이 바뀔 수 있는 변수에 저장된 내용들을 출력할 때 사용.

> 즉, 클라이언트가 브라우저의 주소 입력란에 요청할 jsp 페이지 이름을 입력하면 웹 서버에게 jsp 페이지를 요청하는 것이다. **웹 서버는 jsp 페이지를 찾아 클라이언트에게 html로 응답하는데 서블릿 컨테이너는 <% %> 부분을 jsp로 인식하여 이를 해석한 후 html 형태로 변환**한다. 그래서 jsp 페이지 소스는 스크립트릿 태그가 없어지고 html로만 구성된 문서 형태가 된다.

#### jsp 동작
1. 클라이언트의 test.jsp 요청  
2. jsp 컨테이너가 jsp 파일을 읽음  
3. jsp 컨테이너가 generate 작업을 통해 servlet(.java) 파일을 생성  
4. .java 파일은 다시 .class 파일로 컴파일  
5. Execute(실행)을 통해 html 파일을 생성하여 jsp 컨테이너에게 전달  
6. jsp는 http 프로토콜을 통해 html 페이지를 클라이언트에게 전달  

## jsp spring project
[참고](https://7942yongdae.tistory.com/115)  
- jsp 컴파일을 위한 라이브러리를 추가 (build.gradle)  
`implementation "org.apache.tomcat.embed:tomcat-embed-jasper"`  
jsp를 지원하는 대표 엔진은 apache의 tomcat  

- jsp를 사용하기 위해 프로젝트 구조 변경  
스프링부트에서 타임리프를 사용할 때는 다운로드 한 그대로 프로젝트 구조를 사용해도 되지만, 별도의 템플릿 엔진이나 jsp를 사용할 경우 webapp과 WEB-INF 폴더를 포함한 구조로 프로젝트 구성을 변경해주어야 한다. (main/webapp/WEB-INF/(view 해도 되고))  

webapp 폴더는 스프링에서 웹을 정의하는 root 폴더이며, WEB-INF는 J2EE 사양에 따라 정의된 표준 폴더 구조이다.

- 프로젝트 구조 변경 후 뷰 리졸버의 설정 값을 변경  
(viewResolver의 Path를 수정)  
application.properties 파일에서 다음을 추가  
```java
spring.mvc.view.prefix=/WEB-INF/(view/를 붙였었다면 여기까지)
spring.mvc.view.suffix=.jsp
```

## JSTL
: JSP Standard Tag Library  
JSP의 기본 태그가 아닌 JSP 확장 태그  

사용하려면 JSTL API 및 자바 구현체 2개의 라이브러리 혹은 API와 구현체가 함께 번들 형태로 구성되어 있는 라이브러리가 필요하다.  

[좋은 참고](https://atoz-develop.tistory.com/entry/JSP-JSTL-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EB%B0%8F-%EC%84%B8%ED%8C%85)  
위 링크를 따라 해당 [링크]([https://mvnrepository.com/artifact/javax.servlet/jstl/1.2](https://mvnrepository.com/artifact/javax.servlet/jstl/1.2))에서 JSTL API+구현체 번들 라이브러리를 다운받는다. 그리고 인텔리제에서 command+; 단축키를 이용해 Project Structure에 들어가 Module에 다운 받은 jar를 추가해준다.  
그리고 build.gradle에 `implementation 'javax.servlet:jstl'`를 추가해주고 코드에 `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`를 선언하여 사용할 수 있다.  

예시  
```jsp
..
<c:forEach items="${memberList}" var="member">  
	<tr>  
		<td>${member.id}</td>  
		<td>${member.username}</td>  
		<td>${member.age}</td>  
		<td>${member.user_id}</td>  
		<td>${member.password}</td>  
	</tr>                            
</c:forEach>
..
```


---

## Thymeleaf
: html, xml, js, css 등을 처리할 수 있는 웹 및 독립형 환경에서 사용이 가능한 java 템플릿 엔진  

html 파일을 가져와 파싱하고 그걸 분석하여 정해진 위치에 데이터를 뿌려주는 역할을 수행한다.

html같은 웹 형태여서 was 없이도 브라우저에서 직접 띄워 확인할 수 있다. jsp처럼 전용 문법을 사용하지 않고 그냥 html 엘리먼트에 속성 값을 넣어준다.

타임리프는 jsp와 달리 서버에서 컴파일 되지 않고 바로 웹 브라우저에서 해석이 되는데 이는 브라우저가 해석할 수 있는 마크업 언어로 이루어져있기 때문에 가능한 것이다.

> - **웹서버**
>   : **정적**인 컨텐츠(html, css, js)를 제공하는 서버  
>   Ex. Apache, Nginx  
>   
> - **WAS**
>   : DB 조회나, 어떤 로직을 처리해야 하는 **동적**인 컨텐츠를 제공하는 서버  
>   Ex. Tomcat, jeus  
 
