## **SQL Data Definition and Data Types**
## SQL Data Definition,Data Types,Standards
### Terminology
#### Table
- Relation in RM
#### Row
- Tuple in RM
#### Column
- Attribute in RM
#### Features
- **Data Definition**
	- DDL(Data Definition Language)를 이용해 데이터를 정의하는 행위
	- 테이블, 인덱스, 뷰 등과 같은 DB 객체를 생성하고 수정하는데 사용함.
- **Data Manipulation**
	- DML(Data manipulation language)를 이용해 데이터를 조작하는 행위
	- ex) 삽입, 수정, 삭제, 조회
- Transaction Control
- Indexing
- Security psecification
- Active DB
- Multi-media
- Distributed Databses

---
## Schema and Catalog Concepts in SQL
### SQL Schema
- schema 이름을 식별자로서 사용함.
- 각 요소는 소유한 식별자와 설명자를 포함하고 있음.

### Schema elements include
- Tables
- Constraints
- views
- domains
- other constructs

### 또한 각 문장은 세미콜론으로 끝남.

### CREATE SCHEMA STATEMENT
- `CREATE SCHEMA COMPANY(DB 이름임) AUTHORIZATION 'Jsmith';`

### Catalog
- SQL 환경에서 ==스키마의 컬렉션==으로 이름지어져 있음
- 이런 Catalog를 묶은 Cluster of Catalog가 존재하지만 원래 존재하는 개념은 아니고 일부 시스템에서 사용하는 아이디어임.

---
## CREATE TABLE Command in SQL
### Specifying a new relation
- 새로운 relation을 만드는 행위임
- Table의 이름이 필요함
- 초기 제약 사항과 속성 그리고 타입을 명세해야 함.

### Table 예제
`CREATE TABLE COMPANY.EMPLOYEE`
- COMPANY 스키마에 EMPLOYEE라는 테이블 생성

`CREATE TABLE EMPLOYEE`
- 기본 스키마에 EMPLOYEE라는 테이블 생성

### Base Tables (a.k.a base relations)
- Relation 과 Relation의 튜플들은 실제로 DBMS에 파일로서 저장되고 생성된다.

### Virtual Relations(a.k.a Views)
- `CREATE VIEW` STATEMENT를 통해 생성됨.
- 물리적 파일과 동일하지 않음.

### 외부 키 문제 발생 가능성
- 순환 참조 : 둘 이상의 테이블이 서로 참조하는 상황
- 아직 생성되지 않은 테이블을 참조할 가능성
- DBA(DB 관리자)는 이 문제가 일어나지 않도록 무결성을 지킬 수 있는 엄격한 제약을 가해야 함.

---
## Attribute Data Types and Domains in SQL
### Basic Data Types
#### **Numeric** data type
- INT, INTEGER and SMALLINT
- FLOAT or REAL, DOUBLE PRECISION

#### **Character-String** data types
- Fixed length : CHAR(n), CHARACTER(n)
- Varying length : VARCAHR(n), CHAR VARYING(n), CHARACTER VARYING(n)

#### **Bit-string** data types
- bit를 저장하는 데이터 타입
- Fixed length : BIT(n)
- Varying length : BIT VARYING(n)

#### **Boolean** data type
- Values of TRUE or FALSE or NULL

#### **DATE** data type
- YYYY-MM-DD 의 10자리를 가지고 있음.
- Component로 YEAR, MONTH 그리고 DAY를 가짐
- 여러가지 매핑 함수가 존재함.

---
### Additional data types
#### **Timestamp** data type
- DATE와 TIME field를 포함함

#### **INTERVAL** data type
- date나 time, timestamp 값에 증가 또는 감소시킬수 있는 값임
#### **DATE, TIME, Timestamp, INTERVAL** 데이터 타입들은 **문자열 포맷으로 바뀔 수 있음.**

---
### Domain
- ==속성 명세에 사용된 이름==
- 여러 속성에 의해 사용되는 domain의 data type을 바꾸는 것을 쉽게 만들어 줌.
- 속성 값이 가질 수 있는 범위와 데이터 타입을 정의하는 개념.
- 스키마의 가독성을 올려 줌
- `CREATE DOMAIN SSN_TYPE AS CHAR(9)`

### TYPE
- 사용자 정의 유형(UDT, User Defined Type)은 객체 지향 애플리케이션에 지원됨.
- `CREATE TYPE`

---
## **Specifying Constraints in SQL**
## Specifying Constraints in SQL
### Basic Constraints
#### Key Constraints
- Primary Key 의 값은 중복될 수 없음.
#### Entity integrity Constraints
- Primary Key 의 값은 null일 수 없음
#### Referential integrity Constraints
- Foreign key는 이미 존재하는 primary key이거나 null이어야 함.
#### Domain Constraints
- 값의 형식과 범위를 제한하는 제약 조건임.

---
### Attribute Contraints
#### Default Value
- `Default <value>`
#### CHECK clause
- `CHECK <range>`
- `age INT CHECK(age > 18)`

---
### Specifying Key and Referential Integrity Constraints
- 키와 참조 무결성 제약 조건
#### **PRIMARY KEY** 절
- `Dnumber INT PRIMARY KEY`
- 하나 또는 더 많은 속성을 primary key로 지정함.
- 여러 개를 지정하는 경우 이를 Composite Key라고 부름

#### **UNIQUE** 절
- a.k.a Secondary key or CANDIDATE key
- `Dname VARCHAR(15) UNIQUE`
- Null 값을 허용하며 여러 개를 설정할 수 있다는 점에서 Primary Key와 다름

#### **Foreign KEY** 절
- Default operation : 위반(violation)을 없애는 업데이트
- referential triggered action clause 추가
	- Option include
		- SET NULL
		- CASCADE - 참조하는 것이 바뀌면 똑같이 실행해라
		- SET DEFAULT
	- SET NULL 또는 SET DEFAULT를 사용하게되면 ON DELETE(삭제 시)와 ON UPDATE(업데이트 시) 모두 동일하게 적용
	- CASCADE 옵션은 관계 relation, 여러 값은 가진 속성, 약한 엔티티 타입에 적합함.

---
## Giving Names to Constraints
- 제약사항에 이름 붙이기
### `CONSTRAINT` 키워드 사용하기
- 특정한 제약사항을 식별해 이름 붙이기
- 나중에 대체할 때 좋음

---
### 예시
```SQL
CREATE TABLE EMPLOYEE
	(CONSTRAINT EMPPK
		PRIMARY KEY (Ssn),
	CONSTRAINT EMPSUPERFK
		FOREIGN KEY (Super_ssn) REFERENCES
EMPLOYEE(Ssn)
		ON DELETE SET NULL ON UPDATE CASCADE,
	CONSTRAINT EMPDEPETFK
		FOREIGN KEY(Dno) REFERENCES
DEPARTMENT(Dnumber)
		ON DELETE SET DEFAULT ON UPDATE CASCADE);
```

---
## Specifying Constraints on Tuples Using `CHECK`
- `CHECK`를 사용하여 튜플에 대한 제약 조건 지정

### 각각의 튜플에 `CHECK`를 통해 추가적인 제약 사항이 가능함.

### `CHECK` 절은 CREATE TABLE문장의 끝부분에 와야 함.
- 각 튜플에 개별적으로 적용
- `CHECK (Dept_create_date <= Mgr_start_date)`

---
## **Basic Retrieval Queries in SQL**
## Basic Retrieval Queries in SQL
- Retrieval - 집합
- 집합에 대한 operation들에 대해 알아보는 챕터임.

### SELECT Statement
```SQL
SELECT <attribute list>
FROM <table list>
WHERE <condition>;
```

- attribute list
	- attribute 의 이름들에 대한 리스트임.
- table list
	- relation의 이름들에 대한 리스트임
- condition
	- Boolean Expression
	- =, <, <=, >, >=, <>``
- join condition
	- 여러 relation이 포함되는 경우

---
## Aliasing and Renaming
- 별명을 붙이는 거임
- `AS` 키워드 사용

```SQL
SELECT E.Fname, E.Lname, S.Fname, S.Lname
FROM EMPLOYEE AS E, EMPLOYEE AS S
WHERE E.Super_ssn = S.Ssn;
```

요소들에 대해서도 이름을 다시 지을 수 있음!!

```SQL
EMPLOYEE AS E(Fn, Mi, ...)
```

---
## Unspecified WHERE Clause and Use of the Asterisk
- WHERE 절이 없는 경우 & Asterisk( * )의 사용법

### WHERE 절이 없을 때
- 튜플 선택에 대한 조건이 없음을 나타냄

```SQL
SELECT Ssn
FROM EMPLOYEE;
``` 
- 모든 항목을 선택함 (단일인 경우)
- 이 경우 모든 EMPLOYEE의 Ssn을 가져옴

### 결과는 Cross-product이다
- 가능한 모든 조합에 대해 나옴(여러 개인 경우)

```SQL
SELECT Ssn, Dname
FROM EMPLOYEE, DEPARTMENT
```
- 이 경우 모든 EMPLOYEE의 Ssn에 대해서 DEPARTMENT의 Dname과 조합해 나옴

```
Ssn | Department
123 | RESEARCH
456 | RESEARCH
123 | DEVELOP
456 | DEVELOP
```

### using Asterisk( * )
- 선택된 튜플에서 모든 attributes를 가져옴

---
## Tables as Sets in SQL
Table을 집합으로 사용하는 방법

- SQL은 쿼리의 결과로 중복되는 튜플을 삭제하지 않는다.
- `DISTINCT` 키워트를 통해 중복되는 튜플을 삭제 가능
	- 결과에는 고유한 튜플만 남아야 한다.

### Set Oprations
- UNION, EXCEPT, INTERSECT가 있음
- Multiset으로 하고 싶다면 ALL을 붙여주면 됨
	- UNION ALL, EXCEPT ALL ~~

---
## Substring Pattern Matching and Arithmetic Operators
### `LIKE` 비교 연산자
- `%` - 0 개 이상의 임의의 문자를 대체함
- `_` - 정확히 한 개의 문자를 대체함
#### using
```SQL
SELECT Fname
FROM EMPLOYEE
WHERE Address LIKE '%Cheongju%'
```

### `BETWEEN` 비교 연산자
```SQL
SELECT *
FROM EMPLOYEE
WHERE (Salary BETWEEN 30000 AND 40000);
```

---
## Arithmetic Operations
### Standard arithmitic operations
- +, -, * , / 다 있음.
- 보통 SELECT에 포함되어 사용함

```SQL
SELECT E.Fname, E.Lname, 1.1* E.Salary AS Increased_sal
FROM EMPLOYEE AS E, WORKS_ON AS W, PROJECT AS P
WHERE E.Ssn = W.Essn AND W.Pno = P.Pnumber AND P.Pname =
'ProductX';
```

---
### `ORDER BY` 절 사용
- Keyword
	- `DESC`(내림차순)
	- `ASC`(오름차순)
- 일반적으로 쿼리 끝에 옴

```SQL
SELECT D.Dname, E.Lname, E.Fname, P.Pname
FROM DEPARTMENT AS D, EMPLOYEE AS E, WORKS_ON AS W, PROJECT
AS P
WHERE D.Dnumber = E.Dno AND E.Ssn = W.Essn AND W.Pno =
P.Pnumber
ORDER BY D.Dname, E.Lname, E.Fname;
```

---
## BASIC SQL Retrieval Query Block
```SQL
SELECT <attribute list>
FROM <table list>
[WHERE <condition>]
[ORDER BY <attribute list> <keyword>]
```

---
## **INSERT, DELETE, and UPDATE Statements in SQL**
## INSERT,DELETE ,and UPDATE Stat.in SQL
- insert, delete, update command is modify the DB

### INSERT
- insert a tuple in a relation
- add more than one tuples
- Attribute value들은 CREATE TABLE 커맨드의 순서로 적혀 있어야 함.
- data type에 대한 제약사항은 자동적으로 관측됨
- 부족한 데이터에 대해서는 NULL을 자동적으로 할당함.
```SQL
INSERT INTO EMPLOYEE(Fname, Lname, Dno, Ssn)
VALUES ('Richard', 'Marini',4,'653298653')
or
INSERT INTO EMPLOYEE
VALUES ('아무튼 많은 정보들...')
```

- query의 결과를 통해 Insert로 여러 tuple을 넣는 방법도 있음.
```SQL
CREATE TABLE WORKS_ON_INFO
(
	Emp_name VARCHAR(15),
	Proj_name VARCHAR(15),
	Hours_per_week DECIMAL(3,1)
);
INSERT INTO WORKS_ON_INFO(Emp_name, Proj_name,
Hours_per_week)
SELECT E.Lname,P.Pname,W.Hours
FROM PROJECT P, WORKS_ON W, EMPLOYEE E
WHERE P.Pnumber = W.Pno AND W.Essn = E.Ssn;
```

---
## Bulk Loading of Tables
- 대용량의 tuple을 relation에 넣는 것임.
- `LIKE` 을 이용한 Attribute 복사
```SQL
CREATE TABLE D001EMP LIKE dept_emp;
/* dept_emp와 동일한 attribute를 갖는 D001EMP TABLE 생성 */
INSERT INTO D001EMP (SELECT * from dept_emp)
/* dept_emp의 모든 tuple을 D001EMP로 삽입 */
```

- 생성하면서 데이터 복사하기
```SQL
CREATE TABLE D5EMPS LIKE EMPLOYEE (
	SELECT E.* FROM EMPLOYEE AS E WHERE E.Dno=5
) WITH DATA;
```

---
## DELETE
- WHERE 절을 포함하여 삭제할 tuple을 정함
```SQL
DELETE FROM EMPLOYEE
WHERE Lname = 'Brown';
```

- 무결성 참조가 강조됨.
- 제약사항에 CASADE 제약사항이 있어도 tuple들은 한번에 한 테이블에서만 사라짐
- WHERE절이 없다면 모든 tuple이 사라짐
```SQL
DELETE FROM EMPLOYEE;
```

---
## UPDATE
- Attribute의 값을 수정하기 위한 연산임
- WHERE 절을 통해 수정할 tuple을 선택함
- 추가적으로 SET 절을 이용해 수정될 새 속성들을 명세할 수 있음
```SQL
UPDATE PROJECT
	SET PLOCATION = 'bellaire',DNUM = 5
	WHERE PNUMBER = 10
UPDATE EMPOLYEE
SET SALARY = SALARY * 1.1
WHERE DNO IN (SELECT DNUMBER FROM DEPARTMENT WHERE DNAME = 'RESEARCH');
```

- 각 명령어는 한 relation에 대해서의 tuple을 수정한다.
- 참조 무결성은 DDL 명세의 부분적으로 명시된 것으로 강제된다.