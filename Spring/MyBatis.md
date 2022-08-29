# MyBatis
(쿼리를 직접 작성해 명시해야 하기 때문에 ORM으로 보기는 어려움)

: 객체지향 언어인 자바의 관계형 데이터베이스 프로그래밍을 조금 더 쉽게 할 수 있도록 도와주는 개발 프레임워크

쿼리 기반 웹 어플리케이션을 개발할 때 가장 많이 사용되는 SQL Mapper 프레임워크이다.
(Spring과는 전혀 관계 없는 독립적인 프레임워크)
\*SQL Mapper: DAO 객체와 SQL문을 매핑해주는 Persistence Framework
즉, Data Access Layer에 속하는 프레임워크

복잡한 JDBC 코드를 걷어내 깔끔한 소스코드를 유지할 수 있다.

자바 객체(object)와 SQL 사이에서 자동 매핑을 도와주는 프레임워크이다.

XML 형태로 쓰인 JDBC 코드라고 생각해도 될만큼 [JDBC](obsidian://open?vault=Note&file=Spring%2FJDBC)의 모든 기능을 제공한다.
> \*JDBC
> : Java Database Connectivity,
> 자바에서 DB 프로그래밍을 하기 위해 사용되는 API

## 특징
- 복잡한 쿼리나 다이나믹한 쿼리에 강함(반대로 비슷한 쿼리는 남발해야 함)
- **프로그램 코드와 SQL 쿼리의 분리**로 코드 간결성 및 유지보수성 향상
- resultType, resultClass등 Vo를 사용하지 않고 조회 결과를 사용자 정의 DTO, MAP등으로 매핑하여 사용 가능
- 빠른 개발 가능, 생산성 향상

#### JDBC 이용해 프로그래밍 할 경우
- 클래스나  JSP와 같은 코드 안에 SQL문 작성
- 따라서 SQL의 변경 등이 발생할 경우 프로그램 수정 필요
  -> 유연성이 떨어지고 코드가 복잡해 가독성이 떨어짐

#### MyBatis를 사용하지 않고 JDBC 직접 이용할 경우
- 개발자가 반복적으로 작성해야 할 코드가 많아지고, 서비스 로직 코드와 쿼리를 분리하기 어려움
- 커넥션 풀의 설정 등 개발자가 신경 쓸 부분이 많아 여러가지 어려움

#### 따라서 JDBC를 이용해 직접 개발하기 보단 MyBatis와 같은 프레임워크를 사용한다.

## MyBatis3 구조
<img width="685" alt="image" src="https://user-images.githubusercontent.com/57247474/187131129-785cc4ef-f45a-41db-bfb7-fddc4d3e60e7.png">

MyBatis는 DAO와 JDBC 사이 위치하여 서로를 매핑

## MyBatis3 주요 구성 요소
> \*MyBatis configuration file
> : MyBatis3의 작업 설정을 설명하는 XML 파일
> 
> 데이터베이스의 연결 대상, 매핑 파일의 경로, MyBatis3의 작업 설정 등과 같은 세부 사항을 설명하는 파일이다.
> Spring과 통합하여 사용할 때 데이터베이스의 연결 대상과 매핑 파일 경로 설정을 구성 파일에 지정할 필요가 없다. 하지만 MyBatis3의 기본 작업을 변경하거나 확장할 때 설정이 수행된다.

> \*org.apache.ibatis.session.SqlSessionFactoryBuilder
> : MyBatis3 구성 파일을 읽고 생성하는 SqlSessionFactory 구성 요소
> 
> 이 구성 요소는 Spring과 통합되어 사용할 경우 애플리케이션 클래스에서 직접 처리하지 않는다.

> \*org.apache.ibatis.session.SqlSessionFactory
> : SqlSession을 생성하는 구성 요소
> 
> 이 구성 요소는 Spring과 통합되어 사용할 경우 애플리케이션 클래스에서 직접 처리하지 않는다.

> \*org.apache.ibatis.session.SqlSession
> : SQL 실행 및 트랜잭션 제어를 위한 API를 제공하는 구성 요소
> 
> MyBatis3를 사용하여 데이터베이스에 액세스할 때 가장 중요한 역할을 하는 구성 요소이다.
> 이 구성 요소를 Spring과 통합하여 사용할 경우 애플리케이션 클래스에서 직접 처리하지 않는다.

> \*Mapper interface
> : typesafe에서 매핑 파일에 정의된 SQL을 호출하는 인터페이스
> 
> MyBatis3는 매퍼 인터페이스에 대한 구현 클래스를 자동으로 생성하므로 개발자는 인터페이스만 생성하면 된다.

> \*Mapping file
> : SQL 및 OR매핑 설정을 하는 XML 파일

## MyBatis3 주요 구성 요소 Database Access 순서
<img width="691" alt="image" src="https://user-images.githubusercontent.com/57247474/187131223-fce5dab9-e58c-4867-bb85-bfde4ca9e1a4.png">

- (1)~(3): 응용 프로그램 시작시 수행되는 프로세스
- (4)~(10): 클라이언트의 각 요청에 대해 수행되는 프로세스

(1): 응용 프로그램이 SqlSessionFactoryBuilder를 위해 SqlSessionFactory를 빌드하도록 요청  
(2): SqlSessionFactoryBuilder는 SqlSessionFactory를 생성하기 위한 MyBatis 구성 파일을 읽음  
(3): SqlSessionFactoryBuilder는  MyBatis 구성 파일의 정의에 따라 SqlSessionFactory 를 생성  

(4): 클라이언트가 응용 프로그램에 대한 프로세스 요청  
(5): 응용 프로그램은 SqlSessionFactoryBuilder를 사용하여 빌드된 SqlSessionFactory에서 SqlSession을 가져옴  
(6): SqlSessionFactory는 SqlSession을 생성하고 이를 어플리케이션에 반환  
(7): 응용 프로그램이 SqlSession에서 매퍼 인터페이스의 구현 개체를 가져옴  
(8): 응용 프로그램이 매퍼 인터페이스 메소드를 호출  
(9): 매퍼 인터페이스의 구현 개체가 SqlSession 메소드를 호출하고 SQL 실행을 요청  
(10): SqlSession은 매핑 파일에서 실행할 SQL을 가져와 SQL   

>1. MyBatis Config 파일(DB 정보, mapper 파일 등록, typeAlias 설정 등을 포함)을 읽어 SqlSessionFactoryBuilder 객체 생성
>2. SqlSessionFactoryBuilder 객체(단순히 SqlSessionFactory 객체를 생성해주기 위한 용도)를 이용해 SqlSessionFactory 객체를 생성
>3. 앱 실행 중(런타임)에 CRUD 처리가 발생하면 SqlSessionFactory로 SqlSession 객체를 생성
>4. SqlSession 객체를 이용해 DB 요청 후 결과 값을 반환받음
>
>config 파일은 XML 형식이며 `<properties>, <typeAliases>, <environments>, <mappers>` 정보를 포함한다. (property 파일 정의, mapper.xml에서 사용할 alias, DB 정보 설정, 어떤 XML 파일이 mapper 파일인지)


## MyBatis-Spring 컴포넌트 구조
**MyBatis-Spring**은 MyBatis를 스프링 프레임워크에 녹여 좀 더 쉽게 사용하고자 하는 연동 모듈이다.


> \*org.mybatis.spring.SqlSessionFactoryBean
> : SqlSessionFactory를 작성하고 Spring DI 컨테이너에 개체를 저장하는 구성 요소
> 
> 표준 MyBatis3에서 SqlSessionFactory는 MyBatis 구성 파일에 정의된 정보를 기반으로 한다.
> 그러나 SqlSessionFactoryBean을 사용하면 MyBatis 구성 파일이 없어도 SqlSessionFactory를 빌드할 수 있다.

>\*org.mybatis.spring.mapper.MapperFactoryBean
>: Singleton Mapper 개체를 만들고 Spring DI 컨테이너에 개체를 저장하는 구성 요소
>
>MyBatis3 표준 매커니즘에 의해 생성된 매퍼 객체는 스레드가 안전하지 않다.
>따라서 각 스레드에 대한 인스턴스를 할당해야 했다. MyBatis-Spring 구성 요소에 의해 생성된 매퍼 객체는 안전한 매퍼 객체를 생성할 수 있다. 그래서 서비스 등 싱글톤 구성 요소에 DI를 적용할 수 있다.

>\*org.mybatis.spring.SqlSessionTemplate
>: SqlSession 인터페이스를 구현하는 Singleton 버전의 SqlSession 구성 요소
>
>MyBatis3 표준 매커니즘에 의해 생성된 SqlSession 개체가 스레드에 안전하지 않다.
>따라서 각 스레드에 대한 인스턴스를 할당해야 했다. MyBatis-Spring 구성 요소에서 생성된 SqlSession 개체는 안전한 스레드 SqlSession 개체를 생성할 수 있다. 그래서 서비스 등 싱글톤 구성요소에 DI를 적용할 수 있다.

## MyBatis-Spring 주요 구성 요소 Database Access 순서
<img width="688" alt="image" src="https://user-images.githubusercontent.com/57247474/187131300-9ca00360-c7e2-4e7f-8a84-f3fcc1fe7d02.png">

- (1)~(4): 응용 프로그램 시작시 수행되는 프로세스
- (5)~(11): 클라이언트의 각 요청에 대해 수행되는 프로세스

(1): SqlSessionFactoryBean은 SqlSessionFactoryBuilder를 위해 SqlSessionFactory를 빌드하도록 요청  
(2): SqlSessionFactoryBuilder는 SqlSessionFactory를 생성하기 위한 MyBatis 구성 파일을 읽음  
(3): SqlSessionFactoryBuilder는  MyBatis 구성 파일의 정의에 따라 생성된 SqlSessionFactory는 Spring DI 컨테이너에 의해 저장  
(4): MapperFactoryBean은 안전한 SqlSession(SqlSessionTemplate) 및 스레드 안전 매퍼 개체(Mapper 인터페이스의 프록시 객체)를 생성. 따라서 생성되는 매퍼 객체는 스프링 DI 컨테이너에 의해 저장되며 서비스 클래스 등에 DI가 적용. 매퍼 개체는 안전한 SqlSession(SqlSessionTemplate)을 사용하여 스레드 안전 구현 제공.  

(5): 클라이언트가 응용 프로그램에 대한 프로세스 요청  
(6): 애플리케이션(서비스)은 DI 컨테이너에서 주입한 매퍼 개체(매퍼 인터페이스를 구현하는 프록시 개체)의 방법을 호출  
(7): 매퍼 객체는 호출된 메소드에 해당하는 SqlSession(SqlSessionTemplate) 메소드를 호출  
(8): SqlSession(SqlSessionTemplate)은 프록시 사용 및 안전한 SqlSession 메소드를 호출  
(9): 프록시 사용 및 스레드 안전 SqlSession은 트랜잭션에 할당된 MyBatis3 표준 SqlSession을 사용. 트랜잭션에 할당된 SqlSession이 존재하지 않는 경우 SqlSessionFactory 메소드를 호출하여 표준 MyBatis3의 SqlSession을 가져옴  
(10): SqlSessionFActory는 MyBatis3 표준 SqlSession을 반환. 반환된 MyBatis3 표준 SqlSessioin이 트랜잭션에 할당되기 때문에 동일한 트랜잭션 내에 있는 경우 새 SqlSession을 생성하지 않고 동일한 SqlSession을 사용. on 메소드를 호출하고 SQL실행을 요청  
(11): MyBatis3 표준 SqlSession은 매핑 파일에서 실행할 SQL을 가져와 실행  
  
[보고 공부한 게시물](https://khj93.tistory.com/entry/MyBatis-MyBatis%EB%9E%80-%EA%B0%9C%EB%85%90-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%A0%95%EB%A6%AC)  
[보고 공부한 게시물](https://pangtrue.tistory.com/141)

---

````java
implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.2.0'
````
스프링부트에서 마이바티스를 사용할 때 위 코드를 넣어주면 의존성 있는 다른 마이바티스 관련 설정들도 함께 가져오기 때문에 하나만 추가해주어도 된다.

DataBase 설정
```java
# MySQL Driver  
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  
  
# DB URL  
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/webservice?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul  
  
# DB username  
spring.datasource.username=root  
  
# DB password  
spring.datasource.password=[비밀번호]  
  
# Console SQL print  
spring.jpa.show-sql=true  
  
# DDL use (create/update/create-drop/validate/none)  
spring.jpa.hibernate.ddl-auto=update  
  
# SQL formatting  
spring.jpa.properties.hibernate.format_sql=true  
  
# MyBatis  
spring.datasource.mapper-locations=classpath:/mapper/**/*.xml
```

어플리케이션 클래스와 과 같은 위치에 config 파일 생성
```java
package com.dbsrud.webservice;  
  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.mybatis.spring.SqlSessionFactoryBean;  
import org.mybatis.spring.SqlSessionTemplate;  
import org.mybatis.spring.annotation.MapperScan;  
import org.springframework.beans.factory.annotation.Qualifier;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.boot.context.properties.ConfigurationProperties;  
import org.springframework.boot.jdbc.DataSourceBuilder;  
import org.springframework.context.ApplicationContext;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
  
import javax.sql.DataSource;  
  
@Configuration  
// 패키지명  
@MapperScan(value = "com.dbsrud.webservice", sqlSessionFactoryRef = "SqlSessionFactory")  
public class MyBatisConfig {  
  
    @Value("${spring.datasource.mapper-locations}")  
    String mPath;  
  
    @Bean(name = "dataSource")  
    @ConfigurationProperties(prefix = "spring.datasource")  
    public DataSource DataSource() {  
        return DataSourceBuilder.create().build();  
    }  
  
  
    @Bean(name = "SqlSessionFactory")  
    public SqlSessionFactory SqlSessionFactory(  
            @Qualifier("dataSource") DataSource DataSource,  
            ApplicationContext applicationContext) throws Exception  
    {  
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();  
        sqlSessionFactoryBean.setDataSource(DataSource);  
        sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources(mPath));  
  
        return sqlSessionFactoryBean.getObject();  
    }  
  
    @Bean(name = "SessionTemplate")  
    public SqlSessionTemplate SqlSessionTemplate(@Qualifier("SqlSessionFactory") SqlSessionFactory firstSqlSessionFactory) {  
        return new SqlSessionTemplate(firstSqlSessionFactory);  
    }  
}
```

resources/mapper/mapper.xml
```xml
파일만 생성해두고 추후 코드 작성
```

## findAll()
API 구성하기  
Controller
```java
package com.dbsrud.webservice.controller;  
  
import com.dbsrud.webservice.service.MemberService;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.http.HttpStatus;  
import org.springframework.http.ResponseEntity;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestMethod;  
import org.springframework.web.bind.annotation.RestController;  
  
@RestController  
@RequestMapping(value = "/api/v1/app/")  
public class MemberController {  
  
    @Autowired  
    MemberService memberService;  
  
    @RequestMapping(value = "findAll", method = RequestMethod.POST)  
    public ResponseEntity<?> findAll() {  
        ResponseDTO responseDTO = new ResponseDTO();  
        responseDTO.setResultCode("S0001");  
        responseDTO.setRes(memberService.findAll());  
  
        return new ResponseEntity<>(responseDTO, HttpStatus.OK);  
    }  
}
```

값을 받기 위한 DTO 생성
```java
package com.dbsrud.webservice.controller;  
  
public class ResponseDTO {  
  
    private String resultCode;  
    private Object res;  
  
    public String getResultCode() {  
        return resultCode;  
    }  
  
    public void setResultCode(String resultCode) {  
        this.resultCode = resultCode;  
    }  
  
    public Object getRes() {  
        return res;  
    }  
  
    public void setRes(Object res) {  
        this.res = res;  
    }  
}
```

Service에서 Mapper 인터페이스를 호출하도록
```java
package com.dbsrud.webservice.service;  
  
import com.dbsrud.webservice.mapper.MemberMapper;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Service;  
  
import java.util.ArrayList;  
import java.util.HashMap;  
  
@Service  
public class MemberService {  
  
    @Autowired  
    MemberMapper memberMapper;  
  
    public ArrayList<HashMap<String, Object>> findAll() {  
        return memberMapper.findAll();  
    }  
}
```

즉, Controller에서 UserService 클래스의 findAll을 호출하고 Service에서 Mapper 인터페이스를 호출한다.  
Mapper 인터페이스
```java
package com.dbsrud.webservice.mapper;  
  
import org.apache.ibatis.annotations.Mapper;  
import org.springframework.stereotype.Repository;  
  
import java.util.ArrayList;  
import java.util.HashMap;  
  
@Mapper  
@Repository  
public interface MemberMapper {  
  
    ArrayList<HashMap<String, Object>> findAll();  
}
```

그리고 생성해두었던 Mapper.xml에서 코드를 작성해주면 된다.  
resources/mapper/mapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.dbsrud.webservice.mapper.MemberMapper">  
  
    <select id="findAll" resultType="HashMap">  
        SELECT * FROM member  
    </select>  
  
</mapper>
```

여기까지 코드를 작성한 뒤 포스트맨을 사용해 http://localhost:8080/api/v1/app/findAll을 POST 방식으로 호출하면 DB에 들어있는 값을 확인할 수 있다.  
<img width="684" alt="image" src="https://user-images.githubusercontent.com/57247474/187131381-41bdda81-df8d-4ad1-892d-2acb7eaa3eab.png">

## createMember()
insert를 구현하기 위해 Mapper에 creatMember() 생성
```java
void createMember(Model model);
```

MemberService에 메소드 추가
```java
public void createMember(Model model) {  
    memberMapper.createMember(model);  
}
```

요청 받을 RequestDTO 생성
```java
@Getter @Setter  
public class RequestDTO {  
  
    private Long id;  
    private int age;  
    private String password;  
    private String user_id;  
    private String username;  
}
```

Controller에서 requestDTO를 받아 멤버를 생성하기 위해 createMember 추가
```java
    @RequestMapping(value = "createMember", method = RequestMethod.POST)  
    public String createMember(Model model, RequestDTO request) {  
  
        model.addAttribute("id", request.getId());  
        model.addAttribute("age", request.getAge());  
        model.addAttribute("password", request.getPassword());  
        model.addAttribute("user_id", request.getUser_id());  
        model.addAttribute("username", request.getUsername());  
  
        memberService.createMember(model);  
//        memberService.createMember(request);  
  
        return "OK";  
    }
```

그리고 INSERT 쿼리를 실행하기 위해 mapper.xml에 insert를 추가
```xml
<insert id="createMember" parameterType="com.dbsrud.webservice.controller.RequestDTO">  
    insert into member  
    (id, age, password, user_id, username)    values    (#{id}, #{age}, #{password}, #{user_id}, #{username})
</insert>
```

member 테이블에 Insert  
<img width="673" alt="image" src="https://user-images.githubusercontent.com/57247474/187131459-973034f5-a5b0-4bac-94d8-756e930f6cf8.png">

[MyBatis 사용하기 참고](https://bongra.tistory.com/185)
