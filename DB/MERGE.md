## MySQL MERGE
단일 문에서 여러 DML(INSERT, UPDATE, DELETE) 작업을 수행할 수 있다.

작업이 단일 문 내에서 수행되며, 데이터가 처리되는 횟수가 최소화되어 쿼리 성능이 향상된다.

동작 과정은  
- 기준 컬럼을 두고 동일 값이 존재하지 않는다면 해당 데이터를 **INSERT**한다.
- 기준 컬럼을 두고 동일한 값이 존재하며, (업데이트 해야하는) 특정 (사용자 지정)값이라면 **UPDATE**한다.
- 기준 컬럼을 두고 동일한 값이 존재하며, (삭제 해야하는) 특정 (사용자 지정)값이라면 **DELETE**한다.

이다.

### 사용 예시
```sql
MERGE 변경 테이블 AS A
  USING 기준 테이블 AS B
  ON A.컬럼명 = B.컬럼명
  WHEN MATCHED THEN
    일치시 쿼리문
  WHEN NOT MATCHED THEN
    불일치시 쿼리문
```
