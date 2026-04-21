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

1. 데이터베이스 생성
```SQL
CREATE DATABASE my_database;
```
- "my_database" 라는 이름의 DB를 생성하는 구문

2. 테이블 생성
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
- 각 열의 데이터 타입과 크기도 함께 정의되어 있다.
	- INT: 4byte(32bit) 정수형
	- VARCHAR: 가변 길이 문자열
		- CHAR과의 차이: CHAR(50)은 몇 글자를 저장하던지 무조건 50byte를 차지하지만, VARCHAR(50)은 최대 50byte까지 입력 가능하다는 뜻으로 50byte보다 적게 입력한다면 실제 필요한 데이터만큼만 저장된다.
		- VARCHAR은 고정 길이인 CHAR보다 메모리를 효율적으로 사용하지만, 대신 처리 속도가 조금 느릴 수 있다. 

3. 테이블 변경 (열 추가)
```sql
ALTER TABLE employees
ADD COLUMN salary DECIMAL(10, 2);
```
- "employees" 테이블에 "salary"라는 열을 추가
- DECIMAL: 고정 소수점 수치를 정확하게 저장하는 데이터 형식
	- DECIMAL(10, 2): 전체 10자리 중 소수점 이하 2자리를 저장하여 최대 99999999.99까지 표현 가능

4. 테이블 삭제
```sql
DROP TABLE employees;
```
- "employees" 테이블을 삭제하는 구문
- 해당 테이블과 관련된 모든 데이터도 함께 삭제된다.

---
**DDL의 3가지 유형 정리**

| <center>명령어</center> | <center>기능</center>                     |
| -------------------- | --------------------------------------- |
| CREATE               | SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 정의함 |
| ALTER                | TABLE에 대한 정의를 변경하는 데 사용                 |
| DROP                 | SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 삭제함 |

---
### 1.1.2. 데이터 조작 언어 (DML)
- DML: DB에서 데이터를 검색하거나 조작하는 역할

1. 데이터 삽입(*INSERT*)
```sql
INSERT INTO employees (id, name, age, department)
VALUES (1, 'John Doe', 30, 'IT');
```
- "employees" 테이블에 새로운 데이터를 삽입하는 구문
- id에 1, name에 'John Doe', age에 30, department에 'IT'라는 값들을 지정하여 데이터를 삽입

2. 데이터 조회(*SELECT*)
```sql
SELECT * FROM employees;
```
- "employees" 테이블의 모든 데이터를 조회하는 구문
- "* "는 모든 열을 의미하며, DB에서 모든 열과 행을 반환한다.

3. 데이터 업데이트(*UPDATE*)
```sql
UPDATE employees
SET age = 35
WHERE id = 1;
```
- "employees" 테이블에서 id가 1인 데이터의 age 값을 35로 업데이트하는 구문

4. 데이터 삭제(*DELETE*)
```sql
DELETE FROM employees
WHERE id = 1;
```
- "employees" 테이블에서 id가 1인 데이터를 삭제하는 구문
- 혼동 주의!
	- *DELETE*는 테이블의 ***'데이터'*** 를 지우는 **DML**
	- *DROP*은 ***'테이블' 자체*** 를 지우는 **DDL**

---
**DML의 4가지 유형 정리**

|     |     |
| --- | --- |
|     |     |

