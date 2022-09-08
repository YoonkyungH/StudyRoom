# Maven
내가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는데 필요한 다른 라이브러리들까지 관리해 네트워크를 통해 자동으로 다운 받아주는.  

프로젝트의 전체적인 라이프사이클을 관리하는 도구.  

# Gradle
기본적으로 빌드 배포 도구(Build Tool)  
안드로이드 앱을 만들 때 필요한 공식 빌드 시스템이며 Java, c/c++, python 등 지원.  

Maven은 XML로 라이브러리를 정의하고 활용해야 하지만,  
Gradle의 경우 별도의 빌드스크립트를 통해 사용할 어플리케이션 버전, 라이브러리등의 항목을 설정 가능함.  

Gradle이 Maven 보다 비교적 늦게 나와 사용성, 성능 등 좋은 스펙을 가짐.  

- Build라는 동적인 요소를 XML로 정의하기에는 어려움  
	- 설정 내용이 길어지고 가독성 저하
	- 의존관계가 복잡한 프로젝트 설정에 부적절
	- 상속 구조를 이용한 멀티 모듈 구현
	- 특정 설정을 소수 모듈에서 공유하기 위해서는 부모 프로젝트를 생성해 상속하도록 해야함(상속의 단점을 받게 됨)
- Gradle은 Groovy를 사용하기 때문에 동적인 빌드는 Groovy 스크립트로 플러그인을 호출하거나 직접 코드를 짤 수 있음
	- Configuration Injection 방식을 사용해 공통 모듈을 상속해 사용하는 단점을 커버
	- 설정 주입시 프로젝트 조건을 체크할 수 있어 프로젝트별로 주입되는 설정을 다르게 가능
- Gradle이 Maven보다 최대 100배 빠름

[참고](https://hyojun123.github.io/2019/04/18/gradleAndMaven/)

---

JAR, WAR 모두 Java jar 툴을 이용해 생성된 압축(아카이브) 파일  
**어플리케이션을 쉽게 배포하고 동작시킬 수 있도록 관련 파일들(리소스, 속성파일 등)을 패키징 해주는 역할**  

서비스 배포시에는 프로젝트를 WAR포맷으로 묶어 /webapps 등의 지정된 경로에 넣고 Tomcat 등의 Web Container를 이용해 deloy하는 식으로 서비스를 많이 올림.  

> "배포하다"
> - release
>   : 같은 제품을 새롭게 만드는 것. (새로운 버전을 release 하다.)
> - deploy
>   : 프로그램 등을 서버와 같은 기기에 설치해 작동 가능하도록 만드는 것.
> - distribute
>   : 제품을 사용자들이 사용할 수 있도록 서비스 등을 제공하는 의미.
>   
> 예를 들어 "A의 버전 x.x.x가 새롭게 release 되었으며 이를 서버에 deploy하여 사용자들이 사용할 수 있도록 distribute 했다."라고 사용할 수 있다.

# JAR
Java Archive  

.jar 확장자 파일에는 Class와 같은 Java 리소스와 속성 파일, 라이브러리 및 액세서리 파일이 포함되어 있다.  

**Java 어플리케이션이 동작할 수 있도록 자바 프로젝트를 압축한 파일**로  실제로 JAR 파일은 플랫폼에 귀속되는 점만 제외하면 WIN ZIP 파일과 동일한 구조이다.  

JAR 파일은 원하는 구조로 구성 가능하며 JDK에 포함하고 있는 JRE만 가지고도 실행이 가능하다.  

# WAR
Web Application Archive  

**.war 확장자 파일은 servlet/jsp 컨테이너에 배치할 수 있는 웹 어플리케이션 압축 파일 포맷**으로 JSP, Serlvet, JAR, class, XML, HTML, Javascript 등 servlet context 관련 파일들로 패키징 되어있다.  

WAR은 응용 프로그램을 위한 포맷이기 때문에 **웹 관련 자원만 포함**하고 있으며 이를 사용하면 웹 어플리케이션을 쉽게 배포 가능하다.  

원하는 구성을 할 수 있는 JAR 포맷과 달리 **WAR은 WEB-INF 및 META-INF 디렉토리로 사전 정의 된 구조를 사용**하며 **WAR 파일을 실행하려면 Tomcat, Weblogic, Websphere 등의 웹 서버 또는 웹 컨테이너(WAS)가 필요**하다.  

WAR 파일도 Java의 JAR 옵션(Java -jar)을 이용해 생성하는 JAR 파일의 일종으로 웹 어플리케이션 전체를 패키징하기 위한 JAR 파일로 생각하면 된다.  

[참고](https://ifuwanna.tistory.com/224)  

---

## WAR file 만들어 배포
build.gradle에 내용 추가  
```java
plugins {  
   ...
   id 'war'  
}  
  
apply plugin: 'war'  
bootWar {  
   archiveName("webservice.war")  
}

bootWar.enabled = false  
war.enabled = true

dependencies {  
   ...
  
   providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'  
}
```
내장 톰캣을 사용하지 않기 위해서는 enabled =.. 와 providedRuntime ... 필요  

```java
public class WebserviceApplication extends SpringBootServletInitializer {  
  
   @Override  
   protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {  
      return super.configure(builder);  
   }
```
위와 같이 SpringBootServletInitializer를 상속받고 오버라이드 해주어야 한다.  
이를 **상속 받는다는 것은 tomcat과 같은 Servlet Container 환경에서 springboot 어플리케이션이 동작 가능하도록 웹 어플리케이션 컨텍스트를 구성한다는 의미**이다.  

(bootWar 설명)  
```java
bootWar {
	archiveBaseName = 'war 패키징 시 이름 설정 Ex. web'
	archiveFileName = 'war 패키징 시 이름 설정 Ex. web.war'
	archiveVersion = "0.0.0"
}
```
적용 후 빌드 WAR (터미널)  
./gradlew bootwar  

WAR 실행 (터미널)  
java -jar ./build/libs/webservice.war  

[참고](https://bigdatamaster.tistory.com/121)

