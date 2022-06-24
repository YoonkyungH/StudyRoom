## Spring Data JPA에서의 Delete vs DeleteById
우선, Delete와 DeleteById는 Spring Data API 중 CrudRepository 인터페이스에 정의되어 있다.

JpaRepository를 상속받아 메소드들을 사용해 데이터베이스에 delete 쿼리를 날릴 수 있다.

> ### Delete
> 넘어온 엔티티에 대해 null 체크를 한 수 EntityManager를 통해 삭제를 수행한다.

> ### DeleteById
> deleteById는 내부적으로는 결국 delete를 호출한다.
>
> 넘어온 id 값을 통해 findById를 사용하여 delete에 인자로 넘겨줄 데이터를 조회한다.
>
> 넘어온 id 값이 null 값이라면 EmptyResultDataAccessException 예외를 발생시킨다.


**결국에는 둘 다 delete를 호출하여 삭제를 수행**하고있다.

보통 delete는 findById와 조합하여 사용한다.

그리고 deleteById는 이미 delete와 findById가 합쳐져 있는 것이라고 보면 된다.

### 언제 어떤 메소드를 사용해야 할까  
deleteById는 서비스 로직에서 메소드를 하나만 사용해도 **조회&삭제**를 함께 수행할 수 있다는 장점이 있다. 

또한, 내부적으로 id에 대한 null 체크 과정이 있기 때문에 서비스 로직에서 id가 null 인지 체크해주지 않더라도 의도치 않은 NullPointerException 발생을 예방할 수 있다.

대신 위에서 언급했듯 null 값인 id를 넘겨받았다면 EmptyResultDataAccessException 예외가 발생하기 때문에 에러 메시지를 커스텀할 수 없다.

그래서 delete를 findById와 조합해 사용하기도 하는 것이다.  
(이 경우 개발자가 원하는 메시지를 클라이언트에게 제공할 수 있다.)

성능에 대해서는 큰 차이가 없기 때문에 원하는 메소드를 사용하면 될 것 같다.

[참고한 포스트](https://hwanchang.tistory.com/7)
