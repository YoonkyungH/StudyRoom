## @Modifying Annotation
@Query 어노테이션을 사용해 update 행위를 하려고 하자 에러가 발생했고 그 해결책은 `@Modifying`이었다.

@Modifying은 @Query 어노테이션을 통해 작성된 INSERT, UPDATE, DELETE 쿼리에서 사용되는 어노테이션이다.

JpaRepository에서 기본적으로 제공하는 메소드나 메소드 네이밍으로 만들어지는 쿼리에서는 적용되지 않는다.

clearAutomatically, flushAutomatically 속성을 변경할 수 있다.  
(두 default 값은 false)

해당 어노테이션을 사용하지 않아 update시 에러가 발생했던 것은  
@Query로 정의된 JPQL이 직접 DB에 쿼리를 날린다.  
이때 영속성 컨텍스트에 있는 데이터는 영향을 받지 못한다.  
(영속성 컨텍스트에 정의된 데이터가 우선 순위를 가지며, 덮어쓰여지지 않는다.)

이때문에 변경한 데이터가 영속성 컨텍스트에 저장되지 못하여 기존 데이터를 출력했던 것이다.

### clearAutomatically
이 옵션은 @Query로 정의된 JPQL을 실행 후 자동으로 영속성 컨텍스트를 비워주는 것이다.

이렇게 영속성 컨텍스트를 비워주게 되면 데이터베이스에서 가져온 모든 데이터들이 영속성 컨텍스트에 저장되어 변경한 최신 상태를 가질 수 있게된다.


[도움이 된 포스트](https://joojimin.tistory.com/71)
