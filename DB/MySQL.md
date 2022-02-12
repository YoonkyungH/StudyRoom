## [MySQL🐬]
MySQL: Relational Database(RDB, 관계형 데이터베이스).  
(NoSQL 비관계형 데이터베이스의 예. MongoDB)

[🔗codeanywhere](https://codeanywhere.com) 에서 MySQL을 본인 컴퓨터에 설치하지 않고도 실습할 수 있음.

### 데이터베이스의 목적
"코드를 통해 데이터를 조작 가능하다."

SQL 문법을 가지고 데이터를 제어할 수 있다는 것이 큰 장점이다.
그리고 저장된 수많은 데이터들을 앱, 웹을 통해 공유할 수 있고 빅데이터나 인공지능을 이용해 분석할 수 있다.

[🔗codeanywhere](https://codeanywhere.com)에서 사용할 때는 New Container를 눌러 터미널을 켠다.   
그리고 `mysql -uroot`로 MySQL을 시작할 수 있다.   

단, 비밀번호를 설정하기 위해서는 초기 접속시 `mysql -uroot -p` 이렇게 -p 옵션을 붙여줘야 한다.

### MySQL의 구조
![구조](https://user-images.githubusercontent.com/57247474/153403831-7c2662a9-f090-4d7a-84ad-22b7d4ef1dd5.png)   

**DataBase, Schema(스키마)**: table들을 그룹화할 때 쓰는 일종의 folder.  


## MySQL Schema의 사용

### 데이터베이스 생성
```sql
CREATE DATABASE [생성하고 싶은 DB 이름];
```

### 데이터베이스 삭제
```sql
DROP DATABASE [삭제하려는 데이터베이스 이름];
```

### 데이터베이스 확인
```sql
SHOW DATABASES;
```

### 데이터베이스 사용하기
➡️ 지금부터 입력하는 명령어는 이 데이터베이스(스키마)를 대상으로 함
```sql
USE [사용하려는 db명];
```

## SQL과 테이블 구조
**S**tructured - 표로 잘 정리된, 구조화 된   
**Q**uery - DB에게 생성해줘, 삭제해줘 .. "질의하다"   
**L**anguage - DB도, 개발자도 이해할 수 있는 언어   

![여우&곰](https://user-images.githubusercontent.com/57247474/153405715-046bc56f-bd55-4088-8b0d-ea59e767d285.png)   

> ![행&열](https://user-images.githubusercontent.com/57247474/153406365-c7c6248a-c4e2-463c-9d5b-5e4283166992.png)   


### GitHub cheatsheet
[🔗 MySQL cheatsheet](https://gist.github.com/bradtraversy/c831baaad44343cc945e76c2e30927b3)   

## MySQL Table
### 테이블 생성
```sql
CREATE TABLE [테이블명] (
   -> [컬럼명] [데이터 타입] (옵션 Ex. NOT NULL AUTO_INCREMENNT),
   ...
   -> PRIMARY KEY([pk로 설정할 컬럼])
   );
```   

- `NOT NULL`: NULL 허용 X
- `AUTO_INCREMENT`: 새로운 행이 추가될 때마다 값 +1
- `PRIMARY KEY()`: ()안에 든 값을 **primary key**로 지정   

### 테이블 조회
```sql
SHOW TABLES;
```

### 테이블 삭제
```sql
DROP TABLE [테이블명];
```   

> #### 📌 MySQL 계정 비밀번호 변경
> `SET PASSWORD = PASSWORD('[바꿀 비밀번호]');`   

### 테이블 구조 복사
```sql
CREATE TABLE [복사해 생성될 테이블] LIKE [복사될 테이블];
```   

### 전체 데이터 복사
```sql
INSERT INTO [붙여넣기 될 테이블] SELECT * FROM [복사할 테이블];
```

## MySQL CRUD
**C**reate   
**R**ead   
**U**pdate   
**D**elete   

여기서 **Create, Read**가 가장 중요하며, **Update와 Delete**는 없을 수도 있는 기능이다.   

### ✏️ INSERT
```sql
INSERT INTO [table_name] (column1, 2, ...)
VALUES (value1, 2, ...);
```   

column1과 value1 이렇게 매칭되기 때문에 **순서**를 지켜주어야 한다.   

이때, `DESC [테이블명];` 으로 컬럼을 봐가면서 하면 편하다.   

- `NOW()`: **현재시간**을 불러오는 함수   
- `SELECT * FROM [테이블명];`: `*`은 모든 데이터를 보겠다는 뜻이라 해당 구문은 **이 테이블의 모든 데이터를 보여달라**는 것   

#### 예제
![image](https://user-images.githubusercontent.com/57247474/153560057-ac1adfa1-2abe-4447-a394-498159573f5e.png)   

### ✏️ SELECT
하지만 전체 데이터가 아닌 **특정 컬럼만** 포함하여 보고싶을 때가 있다.   

- `SELECT [필요한 컬럼], [], .. FROM [테이블];`   
![](https://images.velog.io/images/dbsrud11/post/65fe81ed-a341-4ba9-b268-cfdacfa17e73/image.png)   
이 경우 id, title, author만 볼 수 있도록 했다.   

> - `SELECT` 뒤에는 컬럼이 나온다.   
> - `FROM`은 생략 가능하다.


- `SELECT [필요한 컬럼], [], .. FROM [테이블] WHERE [원하는 조건];`   
![](https://images.velog.io/images/dbsrud11/post/0e5692b0-4e51-4e24-bf56-be8564122e7f/image.png)   
이 경우 id, title, author을 가져오되, **author이 yoon에 해당하는 데이터만 가져와라**   

> - `WHERE`는 `FROM` 다음에 등장한다.   


### 정렬
- `SELECT [필요한 컬럼], [], .. FROM [테이블] WHERE [원하는 조건] ORDER BY [기준] DESC;`   
![](https://images.velog.io/images/dbsrud11/post/93407365-94e0-4586-ba03-96e7149219a5/image.png)   
id를 기준으로 **역순으로 정렬**시켰다.   


### 제약조건
- `SELECT [필요한 컬럼], [], .. FROM [테이블] ORDER BY [기준] DESC LIMIT [제한을 둘 숫자];`   
![](https://images.velog.io/images/dbsrud11/post/871eed51-8714-44b5-8c8e-89feb31f2e16/image.png)   
위와 다르게 author의 조건으로 하면 두개의 데이터밖에 조회되지 않기 때문에 해당 조건을 없애고 5개의 데이터를 역순으로 조회하였다.   
그리고 `LIMIT 3` 조건을 걸어 **데이터 3개만 조회**하였다.   


### ✏️ UPDATE
> ### ⚠️ WHERE문을 잊지말자   

![](https://images.velog.io/images/dbsrud11/post/ff3fbe27-0bbe-4e2a-80bd-bc019aaa1432/image.png)   

이게 현재 topic 테이블 데이터 상태이다.   

여기서 id=2의 **title**과 **description**의 내용을 수정할 것이다.   


- `UPDATE [테이블명] SET [수정할 컬럼명]='[수정할 내용]', ... WHERE id=[];`   
![](https://images.velog.io/images/dbsrud11/post/34d62c54-edee-47da-b34a-1cbaee39e871/image.png)   
이렇게 id=2의 내용을 업데이트 시켰다. 이때, `WHERE id=2`를 **절대 잊으면 안된다‼️**   
이 내용 없이 명령을 수행해버리면 **모든 행의 데이터가 바뀌어 버린다.**   


### ✏️ DELETE
> ### ⚠️ WHERE문을 잊지말자   

- `DELETE FROM [테이블명] WHERE id=[];`   
![](https://images.velog.io/images/dbsrud11/post/8e358d28-43ea-479f-bf11-37e748a50e98/image.png)   
이번에도 UPDATE와 마찬가지로 **WHERE가 굉장히 중요**하다.   
그리고 DELETE 자체는 **굉장히 위험한 명령어**이므로 주의하여 사용해야 한다.   


### + LEFT JOIN
```sql
SELECT * FROM [테이블명] LEFT JOIN [조인할 테이블명] ON [테이블명].[컬럼명] = [테이블명].[컬럼명];
```   
: 보고싶은 컬럼만 `*` 자리에 `,`으로 구분해 나열하여도 된다.

- `ON` 뒤에는 **기준**이 온다.
- `AS`를 활용해 **명시하고 싶은 컬럼명**을 적어주어도 된다. 이의 위치는 SELECT 다음
