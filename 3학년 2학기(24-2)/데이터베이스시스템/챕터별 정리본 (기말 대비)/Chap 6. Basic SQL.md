## 6장 Outline
- SQL 데이터 정의(Data Definition) 및 데이터 유형(Data Types)
- SQL에서의 제약 조건(Constraints) 지정
- SQL의 기본 검색 쿼리(Basic Retrieval)
- 삽입(INSERT), 삭제(DELETE), 업데이트(UPDATE)의 SQL 문장
- SQL의 추가 기능

---
## Overview
### SQL 언어
- 관계형 데이터베이스의 상업적 성공을 위한 주요 이유 중 하나로 간주됨

### SQL
- IBM에서 개발
- 원래는 SQUARE라는 언어로 제안
- SEQUEL TO SQUARE의 약자
- **구조화된 쿼리 언어**
- 구문을 사용하여 관계형 데이터 모델을 비공식적으로 또는 실용적으로 렌더링한 것

---
## 관계형 작업
- 관계 대수학 (Relational Algrebra)
- 관계적 계산 (Relational Calculus)
	- 튜플 관계 계산
	- 도메인 관계 계산

---
## **SQL 데이터 정의 및 데이터 유형**
## SQL 데이터 정의, 데이터 유형, 표준
### 용어 (Terminology)
- 관계형 모델 용어인 관계, 튜플, 속성에 사용되는 테이블, 행, 열

### CREATE 문 (statement)
- 데이터 정의를 위한 주요 SQL 명령

### 언어에는 데이터 정의, 데이터 조작, 트랜잭션 제어, 인덱싱, 보안 사양(허용 및 취소), 활성 데이터베이스(트리거), 멀티미디어, 분산 데이터베이스 등의 기능이 있다.

---
## SQL의 스키마 및 카탈로그 개념 (10p)
### 기본 표준 SQL 구문을 다룬다.
- 기존 RDBMS 시스템에는 변형이 있다.
### SQL 스키마
- 스키마 이름으로 식별
- 각 요소에 대한 권한 부여 식별자와 설명자 포함
### 스키마 요소에는 다음이 포함된다.
- 테이블, 제약 조건, 뷰, 도메인 및 기타 구성 요소
### SQL의 각 문장은 **세미콜론**으로 끝난다.

---
### CREATE SCHEMA 문
- `CREATE SCHEMA COMPANY AUTHORIZATION ‘Jsmith’;`
### Catalog
- SQL 환경에서 명명된 스키마 컬렉션
### SQL에는 카탈로그 클러스터 라는 개념도 있다.