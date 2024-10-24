# UseCase 작성 연습
## 요구사항 명세서 (SRS)
- 요구사항 명세서(Software Requirement Specification)은 개발 대상 시스템의 기능적, 기능외적, 구조적 조건 등을 조직적으로 기술한 명세
	- "무엇을 만들지"를 구체화한 것으로 개발목표가 요구사항 명세서로부터 도출됨
	- 본 교과목의 주요 결과물

- IEEE 830, ISO/IEC/IEEE 29148 같은 표준에서 제공하는 템플릿을 바탕으로 조직적으로 작성하는 것이 통상적임
	- 표준 템플릿을 우리 수업에서는 간소화 한 버전을 사용하고 있음
	- 찾아볼 것
	- 모든 내용이 유용하진 않음 
	- IEEE 830: 우리가 접근 가능, 되게 오래된 문서
	- ISO/IEC/IEEE 29148: 돈 내야함, 어둠의 경로로 접근 가능
		- 현 수업에서는 이걸 간소화 한 버전을 사용

---
## 어떻게 요구사항을 정할 것인가?
- *"누구"의 요구*를 들어줘야 하는 것인지 정하기
- 각 이해당사자(stakeholder)의 문제점이 무엇인지 정리
	- PainPoint(힘든 점?)를 찾아라
		- ~~인생이 힘들다 이딴거 말고~~
	- 기능적 요구사항
- 각 문제가 해결된 조건이 무엇인지 찾기
- 문제 해결 조건을 만족할 수 있는 체계를 만들기

---
## 어떻게 요구사항을 정의할 것인가?
### 헛된 기대
- 고객이나 사용자로부터 요구사항을 받을 수 있다.
- 만병통치약 기술을 쓰면 요구사항을 압도할 수 있다.
- 한번에 최종적인 요구사항을 파악할 수 있다.
- 요구사항들 중에는 서로 모순된 사항이 발생할 수 없다 
- 요구사항을 잘 정리할 수 있는 한 가지 언어/체계가 존재한다.
- 좋은 요구사항이란 개발 목표의 모든 세부사항을 빠뜨림 없이 기술한 것이다.

- 되겠냐고

---
![[Pasted image 20241018095727.png]]

---
## 어떻게 요구사항을 정의할 것인가?
### 올바른 방향
- 고객이 자신이 원하는 바를 알아차리도록 돕기 위해 요구사항 명세를 작성한다.

- 요구사항 명세는 ==여러 버전을 거쳐 발전하는 *'과정'*==
	- 어차피 버전업은 계속되어야 한다.
	- 아마 최종 발표자료 만들 때까지 계속 바꾸고 있을 것이다.

- 요구사항 명세는 ==*'무엇'* 을 만들지 집중==하지,*'어떻게'* 만들지를 구체화하지 않는다. 

- 상황에 따라 다양한 표현 방법을 활용해 요구사항 명세를 작성한다.

- 모르는 것은 모른다고, 못 정하는 것은 못 정했다고 쓴다.

- + 아직 본 적이 없는 것을 함께 볼 수 있게 만들기

---
## 전략
1. 간략한 Use-case
2. *중심 스토리 쓰기*
3. 상세한 Use-case
	- 우리가 쓴 Use-case Discription
1. Use-case 모델링
2. 요구사항 정리
3. 프로토타이핑
4. Use-case 갱신
5. 요구사항 갱신

---
## 간략한 유즈케이스 (Brief Use-case)
### 형태
- 제목: 주어 + 동사 + 목적어 
- 내용: 액터(사용자)가 일련의 순서로 목적을 달성하는 대표적인 상황을 한 문단(2~4문장)으로 적음

### 목적
- 다양한 Use-case를 총체적으로 발굴, 생동감 있게 발전
- 액터, 시스템의 범위, 주변 시스템, 액터와 시스템, 시스템과 주변 시스템 간의 상호작용 방식을 파악
- 시스템이 활동하는 상황(도메인)의 용어를 정의하고 배경지식을 정리

---

이번 학기의 목표: ***요구사항 명세서(SRS)*** 작성하기