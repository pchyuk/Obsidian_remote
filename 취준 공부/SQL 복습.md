# 1. 소개
## 1.1. SQL과 데이터베이스의 기본 개념
- SQL: Structured Query Language의 약자
	- 데이터베이스를 조작하고 관리하는 데 사용
- SQL의 주요 요소
	- DDL(Data **Definition** Language): DB, 테이블, 컬럼 등의 구조를 정의
		- ***CREATE, ALTER, DROP***
	- DML(Data **Manipulation** Language): 데이터 검색, 조작
		- ***SELECT, INSERT, UPDATE, DELETE***
	- DCL(Data **Control** Language): 데이터 접근 권한 관리
		- ***GRANT, REVOKE*** 등

---
### 1.1.1. 데이터 정의 언어 (DDL)
- 데이터는 관계형 데이터베이스(RDBMS)와 비관계형 데이베이스(NoSQL)로 나뉘며, 각각의 장단점이 있다.

- DDL은 DB의 구조를 **정의(생성 -> CREATE)** 하고 **조작(변경-> ALTER, 삭제-> DROP)** 하는 역할을 한다.

- DDL 예제

예제 1. 데이터베이스 생성
```SQL
CREATE DATABASE my_database;
```
- "my_database" 라는 이름의 DB를 생성하는 구문

예제 2. 테이블 생성
```sql
CREATE TABLE employees (
	id INT,
	name VARCHAR(50),
	age INT,
	department VARCHAR(50)
);
```
- "employees" 라는 이름의 테이블을 생성하는 구문
- 테이블에는 id, name, age, department라는 열(Column)이 정의되어 있다.