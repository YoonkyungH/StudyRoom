# RestTemplate
: Spring 3.0부터 지원하는 템플릿으로 HTTP 통신을 RESTful 형식에 맞게 손쉬운 사용을 제공해주는 템플릿   

- REST API 서비스를 요청 후 응답 받을 수 있도록 설계
- HTTP 프로토콜의 메소드(GET, POST, PUT, DELETE)들에 적합한 여러 메소드 제공
	- HTTP 요청 후 JSON, XML, String과 같은 응답을 받을 수 있음
- Header, Content-Type등을 설정해 외부 API 호출
- Java에서 사용되는 다른 템플릿(Ex. JdbcTemplate)처럼 단순 메소드 호출만으로 복잡한 작업을 쉽게 처리 가능
>
## RestTemplate method
![](https://images.velog.io/images/dbsrud11/post/52a12c3f-8759-414b-9491-f8a3ca85cad1/image.png)
