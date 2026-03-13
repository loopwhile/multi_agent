# Task: Architecture Design

## 목적

확정된 스코프에 대한 기술 설계를 수행하고 Work Order를 DESIGNED 상태로 전이한다.

## 담당 역할

- **주 담당**: system_architect_agent
- **입력 제공**: product_scope_agent
- **리뷰**: another_me

## 트리거

- Work Order가 CREATED 상태일 때
- 스코프 변경으로 재설계 필요 시

## 입력

- Work Order (CREATED 상태)
- Scope Document
- Project Packet
- 기존 아키텍처 문서

## 절차

### 1. 설계 분석

- [ ] 요구사항에서 기술 과제 추출
- [ ] 기존 아키텍처와의 정합성 확인
- [ ] 기술 제약 / 의존성 식별

### 2. 설계 결정

- [ ] 시스템 구조 / 모듈 경계 설계
- [ ] 데이터 흐름 정의
- [ ] API / 인터페이스 설계
- [ ] 기술 결정에 대한 ADR 작성 (결정 + 근거 + 트레이드오프)

### 3. 영향 분석

- [ ] 변경되는 파일 / 모듈 목록
- [ ] 기존 기능에 미치는 영향
- [ ] 성능 / 보안 영향 평가

### 4. Work Order 업데이트

- [ ] 설계 결과를 Work Order의 설계 섹션에 첨부
- [ ] 상태: CREATED → DESIGNED

## 출력

- **산출물**: Architecture Design Document + ADR + Work Order 업데이트
- **형식**: `ai_prompts/validation/output_contract.md` 준수

## 수락 기준

- [ ] 기술 결정에 근거와 트레이드오프가 명시되었는가
- [ ] 영향 범위가 식별되었는가
- [ ] 기존 아키텍처와 일관성이 유지되는가
- [ ] builder_agent가 이 설계로 구현을 시작할 수 있는가

## 참조

- Work Order 생명주기: `ai_prompts/workflows/work_order_lifecycle.md`
- Output Contract: `ai_prompts/validation/output_contract.md`
