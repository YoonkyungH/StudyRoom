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

### 테이블 생성
```
CREATE DATABASE [생성하고 싶은 DB 이름];
```

### 테이블 삭제
```
DROP DATABASE [삭제하려는 데이터베이스 이름];
```

### 테이블 확인
```
SHOW DATABASES;
```

### 데이터베이스 사용하기
➡️ 지금부터 입력하는 명령어는 이 데이터베이스(스키마)를 대상으로 함
```
USE [사용하려는 db명];
```

## SQL과 테이블 구조
**S**tructured - 표로 잘 정리된, 구조화 된   
**Q**uery - DB에게 생성해줘, 삭제해줘 .. "질의하다"   
**L**anguage - DB도, 개발자도 이해할 수 있는 언어   

![여우&곰](https://user-images.githubusercontent.com/57247474/153405715-046bc56f-bd55-4088-8b0d-ea59e767d285.png)   

> ![행&열](https://user-images.githubusercontent.com/57247474/153406365-c7c6248a-c4e2-463c-9d5b-5e4283166992.png)   
   

