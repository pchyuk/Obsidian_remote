## 6장 Outline (3p)
- SQL 데이터 정의(Data Definition) 및 데이터 유형(Data Types)
- SQL에서의 제약 조건(Constraints) 지정
- SQL의 기본 검색 쿼리(Basic Retrieval)
- 삽입(INSERT), 삭제(DELETE), 업데이트(UPDATE)의 SQL 문장
- SQL의 추가 기능

---
## Overview (4p)
### SQL 언어
- 관계형 데이터베이스의 상업적 성공을 위한 주요 이유 중 하나로 간주됨

### SQL
- IBM에서 개발
- 원래는 SQUARE라는 언어로 제안
- SEQUEL TO SQUARE의 약자
- **구조화된 쿼리 언어**
- 구문을 사용하여 관계형 데이터 모델을 비공식적으로 또는 실용적으로 렌더링한 것

---
## 관계형 작업 (5p)
*Relational Operation* 
### 관계 대수학 (Relational Algrebra)
### 관계적 계산 (Relational Calculus)
- 튜플 관계 계산
- 도메인 관계 계산

---
## **SQL 데이터 정의 및 데이터 유형**
**SQL Data Definition and Data Types**

## SQL 데이터 정의, 데이터 유형, 표준 (8p)
*SQL Data Definition, Data Types, Standards*

### 용어 (Terminology)
- 관계형 모델(Relational Model, RM) 용어인 **관계, 튜플, 속성**에 사용되는 **테이블, 행, 열**
#### 1. 테이블 (Table)
- 관계형 모델(RM)의 관계(Relation)
#### 2. 행 (Row)
- 관계형 모델(RM)의 튜플(Tuple)
#### 3. 열 (Column)
- 관계형 모델(RM)의 속성(Attribute)

### CREATE 문 (statement)
- 데이터 정의를 위한 주요 SQL 명령

### SQL 언어의 기능 (Features)
#### **1.  데이터 정의 (Data Definition)**
- DDL(Data Definition Language)을 이용해 데이터를 정의하는 행위
- 테이블, 인덱스, 뷰 등과 같은 DB 객체를 생성하고 수정하는데 사용함.
#### **2. 데이터 조작 (Data Manipulation)**
- DML(Data Manipulation Language)를 이용해 데이터를 조작하는 행위
- ex) 삽입, 수정, 삭제, 조회
#### 3. 트랜잭션 제어 (Transaction Control)
#### 4. 인덱싱 (Indexing)
#### 5. 보안 사양(허용 및 취소) (Security specification(Grant and Revoke))
#### 6. 활성 데이터베이스(트리거) (Active DB(Trigger))
#### 7. 멀티미디어 (Multi-media)
#### 8. 분산 데이터베이스 (Distributed Databases)

---
## SQL의 스키마 및 카탈로그 개념 (10p)
*Schema and Catalog Concepts in SQL*

### 기본 표준 SQL 구문을 다룬다.
- 기존 RDBMS 시스템에는 변형이 있다.

### SQL 스키마
- 스키마(schema) 이름을 식별자로서 사용함.
- 각 요소는 소유한 식별자와 설명자를 포함하고 있음.
- 일부 시스템에서는 스키마를 데이터 베이스 라고 부름

### 스키마 요소에는 다음이 포함된다.
#### 1. 테이블 (Tables)
#### 2. 제약 조건 (constraints)
#### 3. 뷰 (view)
#### 4. 도메인 (domains)
#### 5. 기타 구성 요소 (other constructs)

### SQL의 각 문장은 **세미콜론**으로 끝난다.

---
### CREATE SCHEMA 문 (11p)
- `CREATE SCHEMA COMPANY(DB 이름) AUTHORIZATION ‘Jsmith’;`

### 목록 (Catalog)
- SQL 환경에서 **스키마의 컬렉션**으로 이름 지어져 있음

### SQL에는 Catalog를 묶은 카탈로그 클러스터(cluster of catalogs) 라는 개념도 있다.
- 원래 존재하는 개념은 아니고 일부 시스템에서 사용하는 아이디어

---
## SQL에서의 CREATE TABLE 명령 (12p)
*CREATE TABLE Command in SQL*

### 새로운 관계 지정 (Specifying a new relation)
*Specifying a new relation*
- 새로운 relation을 만드는 행위임
- Table의 이름을 제공해 줘야 함.
- 초기 제약 사항과 속성 그리고 타입을 명세해야 함.

### 선택적으로 스키마를 지정 가능
#### `CREATE TABLE COMPANY.EMPLOYEE`
- COMPANY 스키마에 EMPLOYEE라는 테이블 생성

#### `CREATE TABLE EMPLOYEE`
- 기본 스키마에 EMPLOYEE라는 테이블 생성

---
### 기본 테이블 (기본 관계) (13p)
*Base Tables (base relation)*
- Relation 과 Relation의 튜플들은 실제로 DBMS에 파일로서 저장되고 생성된다.

### 가상 관계 (뷰)
*Virtual Relations (Views)*
- `CREATE VIEW` STATEMENT를 통해 생성됨.
- **어떤 물리적 파일과도 동일하지 않다.**

---
### + 외부 키 문제 발생 가능성
- 순환 참조 : 둘 이상의 테이블이 서로 참조하는 상황
- 아직 생성되지 않은 테이블을 참조할 가능성
- DBA(DB 관리자)는 이 문제가 일어나지 않도록 무결성을 지킬 수 있는 엄격한 제약을 가해야 함.

---
## 회사 관계형 데이테베이스 스키마 (14p)

![[Pasted image 20241206223425.png]]

---
## COMPANY 관계형 데이터베이스 스키마에 대한 가능한 데이터베이스 상태 중 하나 (15p)

![[Pasted image 20241206223737.png]]

---
![[Pasted image 20241206233316.png]]

---
## COMPANY 스키마를 정의하기 위한 SQL CREATE TABLE 데이터 정의 문 (17p)

![[Pasted image 20241206233408.png]]

---
## COMPANY 스키마를 정의하기 위한 SQL CREATE TABLE 데이터 정의 문 (18p)

![[Pasted image 20241206233609.png]]

---
![[Pasted image 20241206233718.png]]

---
## SQL의 속성 데이터 유형 및 도메인 (21p)
*Attribute Data Types and Domains in SQL*

### 기본 데이터 타입
#### **숫자** 데이터 타입
- 정수: INTEGER, INT, SMALLINT
- 부동 소수점(실수): FLOAT 또는 REAL, DOUBLE PRECISION

#### **문자 문자열** 데이터 타입
- 고정 길이: CHAR(n), CHARACTER(n)
- 가변 길이: VARCHAR(n), CHAR VARYING(n), CHARACTER VARYING(n)

---
#### **비트 문자열** 데이터 타입
- bit를 저장하는 데이터 타입
- 고정 길이: BIT(n)
- 가변 길이: BIT VARYING(n)

#### **Boolean** 데이터 타입
- TRUE 또는 FALSE 또는 NULL 값

#### **DATE** 데이터 타입
- YYYY-MM-DD 의 10자리를 가지고 있음.
- Component로 YEAR, MONTH 그리고 DAY를 가짐
- RDBMS에서 날짜 형식을 변경하기 위해 사용 가능한 여러 가지 매핑 함수가 존재함.

---
### 추가 데이터 타입 (23p)
#### **Timestamp** 데이터 타입
- DATE와 TIME field를 포함함
	- 초의 소수 자릿수에 대한 최소 6개 위치 추가
	- 선택 사항 WITH TIME ZONE 한정자

#### **INTERVAL** 데이터 타입
- date나 time, timestamp 값에 증가 또는 감소시킬 수 있는 값임

#### **DATE, TIME, Timestamp, INTERVAL** 데이터 타입들은 **문자열 포맷으로 바뀔 수 있음.**

---
#### **도메인** (Domain)
- **속성 명세에 사용된 이름**
- 여러 속성에 의해 사용되는 domain의 data type을 바꾸는 것을 쉽게 만들어 줌.
- 속성 값이 가질 수 있는 범위와 데이터 타입을 정의하는 개념.
- 스키마의 가독성을 올려 줌
- `CREATE DOMAIN SSN_TYPE AS CHAR(9)`

#### **타입** (TYPE)
- 사용자 정의 유형(UDT, User Defined Type)은 객체 지향 애플리케이션에 지원됨.
- `CREATE TYPE`

---
## **SQL에서 제약 조건 지정**
**Specifying Constraints in SQL**

## SQL에서 제약 조건 지정 (26p)
*Specifying Constraints in SQL*
### 관계 모델(RM)에는 SQL에서 지원되는 3가지 기본 제약 사항이 있다.
#### 1. **키** 제약 조건 (Key Constraints)
- Primary Key 의 값은 중복될 수 없음.
#### 2. **엔티티 무결성** 제약 조건 (Entity integrity Constraints)
- Primary Key 의 값은 null일 수 없음
#### 3. **참조 무결성** 제약 조건 (Referential integrity Constraints)
- Foreign key는 이미 존재하는 **primary key**이거나 **null**이어야 함.
#### + 4. **도메인** 제약 조건 (Domain Constraints)
- 값의 형식과 범위를 제한하는 제약 조건임.

---
## 속성 제약 조건 지정 (27p)
*Attribute Contraints*

### 속성 도메인에 대한 기타 제약 사항
*Other Restrictions on attribute domains*

#### 속성의 기본 값 (Default Value of an attribute)
- `Default <value>`
- 속성의 기본 값은 NULL이 될 수 없다.

#### CHECK 절
- `Dnumber INT NOT NULL CHECK(Dnumber > 0 AND Dnumber < 21);`
- `CHECK <range>`
- `age INT CHECK(age > 18)`

---
## 키 및 참조 무결성 제약 조건 지정 (28p)
*Specifying Key and Referential Integrity Constraints*

### **PRIMARY KEY** 절
- 하나 또는 더 많은 속성을 primary key로 지정함.
- `Dnumber INT PRIMARY KEY`
- 여러 개를 지정하는 경우 이를 Composite Key라고 부름

### **UNIQUE** 절
- 대체(보조)(alternate(secondary)) 키를 지정한다.
	- 후보자(CANDIDATE) 키(관계형 모델(RM)의 키)
- a.k.a Secondary key or CANDIDATE key
- `Dname VARCHAR(15) UNIQUE`
- Null 값을 허용하며 여러 개를 설정할 수 있다는 점에서 Primary Key와 다름

---
### **Foreign KEY** 절 (29p)
#### 기본 작업 : 위반(violation) 시 업데이트 거부
#### 참조 트리거 작업 절 추가
##### 옵션에는 다음 명령들이 포함된다.
- SET NULL
- CASCADE - 참조하는 것이 바뀌면 똑같이 실행해라
- SET DEFAULT
##### DBMS에서 SET NULL 또는 SET DEFAULT를 사용하게 되면 ON DELETE(삭제 시)와 ON UPDATE(업데이트 시) 모두 동일하게 적용
##### CASCADE 옵션은 **"relation" 관계, 여러 값을 가진(multivaled) 속성, 약한 엔티티 타입(weak entity)에 적합**함.

---
## 제약 조건에 이름 지정 (30p)
*Giving Names to Constraints*

### `CONSTRAINT` 키워드 사용
- 특정한 제약사항을 식별해 제약 조건(constraint)에 이름 붙이기
- 나중에 대체, 변경할 때 유용함

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

![[Pasted image 20241207012647.png]]

---
## `CHECK`를 사용하여 튜플에 대한 제약 조건 지정 (32p)
*Specifying Constraints on Tuples Using `CHECK`*

### 각각의 튜플에 `CHECK`를 통해 추가적인 제약 사항이 가능함.
### `CHECK` 절은 CREATE TABLE 문장의 끝부분에 와야 함.
- 각 튜플에 개별적으로 적용
- `CHECK (Dept_create_date <= Mgr_start_date)`

---
## **SQL의 기본 검색 쿼리**
**Basic Retrieval Queries in SQL**

## SQL의 기본 검색 쿼리 (34p)
*Basic Retrieval Queries in SQL*
- Retrieval - 집합, 검색
- 집합에 대한 operation들에 대해 알아보는 챕터임.

### SELECT Statement
- 데이터베이스에서 정보를 검색하기 위한 기본 문장 하나 

### SQL은 테이블에 모든 속성 값에서 동일한 두 개 이상의 튜플을 가질 수 있도록 허용
- 관계형 모델(RM)과는 다름
	- 관계형 모델은 엄격하게 집합 이론 기반(set-theory based)
- 멀티셋(Multiset) 또는 백 동작(bag behavior)
- 튜플 ID를 키로 사용할 수 있음

---
## 기본 SQL 쿼리의 SELECT-FROM-WHERE 구조 (35p)
### SELECT 문의 기본 형식
```SQL
SELECT <attribute list>
FROM <table list>
WHERE <condition>;
```

#### attribute list
- attribute 의 이름들에 대한 리스트임.
#### table list
- relation의 이름들에 대한 리스트임
#### condition
- Boolean Expression
- =, <, <=, >, >=, <>``
#### join condition
- 여러 relation이 포함되는 경우

---
### 논리적 비교 연산자 (Logical comparison operators) (36p)
- =, <, <=, >, >=, <>

### 투영 속성 (Projection attributes)
- 값을 검색할 속성

### 선택 조건 (Selection condition)
- 검색된 튜플에 대해 참이어야 하는 bool 조건. 
- 선택 조건에는 여러 관계가 관련된 경우 조인 조건(8장 참고)이 포함.

---
## 모호한 속성 이름 (40p)
*Ambiguous Attribute Names*

### 서로 다른 관계의 두 개(또는 그 이상) 속성에 동일한 이름을 사용할 수 있음
- 속성이 서로 다른 관계(relations)에 있는 한
- 모호함(ambiguity)을 방지하기 위해 관계 이름(relation name)으로 속성 이름을 한정해야 함

![[Pasted image 20241207021254.png]]

---
## 별칭 지정 및 이름 변경 (41p)
*Aliasing and Renaming*
### 별칭 또는 튜플 변수
- 쿼리에서 EMPLOYEE 관계를 두 번 참조하기 위해 대체 관계 이름 E 및 S를 선언한다.
- 별명을 붙이는 거임
- `AS` 키워드 사용

### 각 직원에 대해 직원의 성과 이름을 검색하고 직속 상사의 성과 이름을 검색
```SQL
SELECT E.Fname, E.Lname, S.Fname, S.Lname
FROM EMPLOYEE AS E, EMPLOYEE AS S
WHERE E.Super_ssn = S.Ssn;
```
- 여러 테이블에서 동일하거나 유사한 속성에 이름을 약어로 표시하고 접두사를 붙이는 것이 권장되는 관행이다.

![[Pasted image 20241207022527.png]]

---
- 속성 이름도 바꿀 수 있다.
	- 요소들에 대해서도 이름을 다시 지을 수 있음!!
	- `EMPLOYEE AS E(Fn, Mi, Ln, Ssn, Bd, Addr, Sex, Sal, Sssn, Dno)`
- 관계 EMPLOYEE에 이제 튜플 변수에 해당하는 변수 이름 E가 있다는 점에 유의
- 대부분의 SQL 구현에서 `AS` 를 삭제할 수 있다.
---
## 지정되지 않은 WHERE 절과 별표 사용 (44p)
*Unspecified WHERE Clause and Use of the Asterisk*
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
- 가능한 모든 가능한 튜플 조합에 대해 나옴(여러 개인 경우)
	- 또는 데카르트 곱의 대수 연산 - 8장 참고

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

---
### Asterisk ( * ) 지정
- 선택한 튜플의 모든 속성(attribute) 값을 검색함
- * 는 관계 이름 앞에 붙을 수 있다.
	- 예: EMPLOYEE *

![[Pasted image 20241207023717.png]]

---
## SQL에서 집합으로서의 테이블 (48p)
*Tables as Sets in SQL*
- Table을 집합으로 사용하는 방법

### SQL은 쿼리의 결과로 중복되는 튜플을 자동으로 삭제하지 않는다.
### `DISTINCT` 키워트를 통해 중복되는 튜플을 삭제 가능
- 결과에는 고유한 튜플만 남아야 한다.

![[Pasted image 20241002104457.png]]

### 작업 설정 (Set Oprations)
- UNION, EXCEPT, INTERSECT가 있음
- Multiset으로 하고 싶다면 ALL을 붙여주면 됨
	- UNION ALL, EXCEPT ALL ~~

---
#### Equi-join
```SQL
SELECT *
FROM Employee e, Department d
WHERE e.dno = d.dno;
```

#### Theta-join
```SQL
SELECT *
FROM Employee e, Department d
WHERE e.ssn > 200;
```

#### Natural-join 
```SQL
SELECT e.dno, d.dname, e.ssn, e.name, e.address
FROM Employee e, Department d
WHERE e.dno = d.dno;
```
- equi-join과의 차이점
	- natural join의 경우 **데이터 값이 동일한 속성은 한 개만 남긴다**.

#### Self-join`
```SQL
SELECT e2.*
FROM Employee e1, Employee e2
WHERE e1.name = 'kim' and e1.dno = e2.dno;
```

---
## 부분 문자열 패턴 매칭 및 산술 연산자 (51p)
*Substring Pattern Matching and Arithmetic Operators*

### `LIKE` 비교 연산자
- 문자열 패턴 매칭엔 사용
- `%` - 0 개 이상의 임의의 문자를 대체함
- `_` - 정확히 한 개의 문자를 대체함

#### 예시
```SQL
SELECT Fname
FROM EMPLOYEE
WHERE Address LIKE '%Cheongju%'

WHERE Ssn LIKE '__1__8901';
```

---
### `BETWEEN` 비교 연산자 (53p)
```SQL
SELECT *
FROM EMPLOYEE
WHERE (Salary BETWEEN 30000 AND 40000);
```

---
## 산술 연산자 (54p)
*Arithmetic Operations*

### 표준 산술 연산자
*Standard arithmitic operations*
- +, -, * , / 다 있음.
- 보통 SELECT에 포함되어 사용함

```SQL
SELECT E.Fname, E.Lname, 1.1* E.Salary AS Increased_sal
FROM EMPLOYEE AS E, WORKS_ON AS W, PROJECT AS P
WHERE E.Ssn = W.Essn AND W.Pno = P.Pnumber AND P.Pname =
'ProductX';
```

---
## 쿼리 결과 정렬 (56p)
*Ordering of Query Results*

### `ORDER BY` 절 사용
- Keyword
	- `DESC` (내림차순)
	- `ASC` (오름차순)
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
## 기본 SQL 검색 쿼리 블록 (57p)
*BASIC SQL Retrieval Query Block*

```SQL
SELECT <attribute list>
FROM <table list>
[WHERE <condition>]
[ORDER BY <attribute list> <keyword>]
```

---
## **SQL의 INSERT, DELETE 및 UPDATE 문**
**INSERT, DELETE, and UPDATE Statements in SQL**

## SQL의 INSERT, DELETE 및 UPDATE 문 (59p)
*INSERT, DELETE, and UPDATE Statements in SQL*

### 데이터베이스를 수정하는 데 사용되는 세 가지 명령: **INSERT**, **DELETE** 및 **UPDATE**
### INSERT
- 일반적으로 관계(테이블)에 튜플(행)을 삽입한다.
### UPDATE
- 조건을 만족하는 관계(테이블)의 여러 튜플(행)을 업데이트할 수 있다.
### DELETE
- 조건을 만족하는 관계(테이블)의 여러 튜플(행)을 업데이트할 수도 있다.

---
## INSERT (60p)
### 가장 단순한 형태로는 **관계에 하나 이상의 튜플을 추가**하는 데 사용된다.
### 속성 값은 CREATE TABLE 명령에서 **지정된 속성과 동일한 순서로 나열**되어야 한다.
- 속성 값들은 CREATE TABLE 커맨드의 순서로 적혀 있어야 함.
### **데이터 타입에 대한 제약 조건은 자동으로 준수**된다.
### DDL 사양의 일부로서 **모든 무결성 제약 조건이 적용**된다.
- 부족한 데이터에 대해서는 NULL을 자동적으로 할당함.

--- 
### 튜플에 대한 관계 이름과 값 목록을 지정 (61p)
### NULL을 포함한 모든 값이 제공

```SQL
INSERT INTO EMPLOYEE(Fname, Lname, Dno, Ssn)
VALUES ('Richard', 'Marini', 4, '653298653')
or
INSERT INTO EMPLOYEE
VALUES ('아무튼 많은 정보들...')
```

---
### query의 결과를 통해 Insert로 여러 tuple을 넣는 방법도 있음. (62p)

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
## 테이블의 대량 로딩 (64p)
*Bulk Loading of Tables*

### 대용량의 tuple을 relation에 넣는 것
### `LIKE` 을 이용한 Attribute 복사
```SQL
CREATE TABLE D001EMP LIKE dept_emp;
/* dept_emp와 동일한 attribute를 갖는 D001EMP TABLE 생성 */
INSERT INTO D001EMP (SELECT * from dept_emp)
/* dept_emp의 모든 tuple을 D001EMP로 삽입 */
```

### 생성하면서 데이터 복사하기
```SQL
CREATE TABLE D5EMPS LIKE EMPLOYEE (
	SELECT E.* FROM EMPLOYEE AS E WHERE E.Dno=5
) WITH DATA;
```

---
## DELETE (66p)
### 관계(relation)에서 튜플을 제거
#### 삭제할 튜플을 선택하기 위한 WHERE 절을 포함 
- WHERE 절을 포함하여 삭제할 tuple을 정함
```SQL
DELETE FROM EMPLOYEE
WHERE Lname = 'Brown';
```

#### 참조 무결성을 적용해야 함.
- 무결성 참조가 강조됨.

#### 제약사항에 CASADE 제약사항이 있어도 tuple들은 한번에 한 테이블에서만 사라짐 
- 참조 무결성 제약 조건에 CASCADE가 지정되지 않은 경우

#### WHERE 절이 없다면 모든 tuple이 사라짐
- 그러면 테이블이 빈 테이블이 됨
```SQL
DELETE FROM EMPLOYEE;
```

#### 삭제되는 튜플의 수는 WHERE 절을 만족하는 관계의 튜플 수에 따라 달라짐.

---
## UPDATE (69p)
### 하나 이상의 선택된 튜플의 속성 값을 수정하는 데 사용
- Attribute의 값을 수정하기 위한 연산
### WHERE 절을 통해 수정할 tuple을 선택
### 추가적으로 SET 절을 이용해 수정될 새 속성들을 명세할 수 있음
```SQL
UPDATE PROJECT
	SET PLOCATION = 'bellaire',DNUM = 5
	WHERE PNUMBER = 10
UPDATE EMPOLYEE
SET SALARY = SALARY * 1.1
WHERE DNO IN (SELECT DNUMBER FROM DEPARTMENT WHERE DNAME = 'RESEARCH');
```

### 각 명령어는 한 relation에 대해서의 tuple을 수정
### DDL 사양의 일부로 지정된 참조 무결성이 적용
- 참조 무결성은 DDL 명세의 부분적으로 명시된 것으로 강제됨

---
## SQL의 추가 기능 (73p)
*Additional Features of SQL*

### **복잡한 검색 쿼리를 지정**하기 위한 기술 
- 7장 참조
### SQL 문을 포함한 다양한 프로그래밍 언어로 프로그램 작성
- **임베디드 및 동적 SQL, SQL/CLI(Call Level Interface)** 및 그 전신인 **ODBC, SQL/PSM(Persistent Stored Module)**
- 10장 참조
### 물리적 데이터베이스 설계 매개변수, 관계에 대한 파일 구조 및 액세스 경로를 지정하기 위한 명령 세트
- 예: **CREATE INDEX**

---
### 트랜잭션 제어 명령
### 사용자에게 권한 부여(GRANT) 및 취소(REVOKE) 지정
### 트리거 생성을 위한 구성 요소
### 객체 관계형으로 알려진 향상된 관계형 시스템은 관계를 클래스로 정의한다. 
### 추상 데이터 유형(사용자 정의 유형- UDT라고 함)은 CREATE TYPE에서 지원된다.

---
## 요약 (75p)
*Summary*

### SQL
#### 관계형 데이터베이스 관리를 위한 포괄적인 언어
#### 데이터 정의, 쿼리, 업데이트, 제약 조건 지정 및 뷰 정의

### 다룬 내용 (Covered)
#### 1. 테이블 생성을 위한 데이터 정의 명령
*Data definition commands for creating tables*
#### 2. 제약 조건 지정 명령
*Commands for constraint specification*
#### 3. 간단한 검색 쿼리
*Simple retrieval queries*
#### 4. 데이터베이스 업데이트 명령
*Database update commands*
