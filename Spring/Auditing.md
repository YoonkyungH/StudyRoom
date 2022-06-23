## JPA Auditing
Spring Data JPA에서 Auditing 기능을 제공한다.  
(JPA에서도 Auditing 기능 사용 가능. Spring Data JPA에서 더 쉽게 제공할 뿐.)

auditing 기능은 시간에 대한 값을 자동으로 넣어주는 기능을 이른다.

이 기능을 사용하지 않는다면 도메인을 영속성 컨텍스트에 저장하거나 조회 수행 후 update 하는 경우 매번 시간 데이터를 직접 입력시켜야 하므로 번거롭다.

auditing 기능은 자동으로 시간을 매핑하여 데이터베이스 테이블에 넣어준다.  
(audit이 db값이 변경되었을 때 누가, 언제 변경하였는지 감시한다.)

> **제공하는 어노테이션**
> - @CreatedDate
> - @LastModifiedDate
> - @CreateBy
> - @LastModifiedBy
> 등이 있다.


### 사용법
1. @EnableJpaAuditing 어노테이션을 프로젝트 어플리케이션 파일에 추가해준다.
2. 필요한 Entity에 AuditingEntityListener를 등록해준다.  
등록하려는 엔티티 클래스에 @EntityListeners를 선언해주고 AuditingEntityListener를 등록한다.  
(Ex. @EntityListeners(AuditingEntityListener.class)  
3. Audit할 필드를 정의한다.  
@CreatedDate는 등록 날짜, @LastModifiedDate는 수정 날짜 필드, @CreatedBy는 작성자 필드, @LastModifiedBy는 수정자 필드로 해당하는 엔티티에 맞는 어노테이션을 붙여주면 된다.  

### 예시 코드
- 어플리케이션에 @EnableJpaAuditing 어노테이션을 붙여준다.
```java
@EnableJpaAuditing
@SpringBootApplication
public class xxxApplication {
}
```

- BaseTimeEntity.java
```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class BaseTimeEntity {

    @CreatedDate
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

- 필요한 클래스에 아래와 같이 상속 받는다.
```java
public class 클래스명 extends BaseTimeEntity {
}
```
