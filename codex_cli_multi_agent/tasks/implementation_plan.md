# Task: Implementation Plan

## 목적

설계를 구체적 구현 계획으로 변환하고 builder_agent의 실행을 안내한다.

## 담당 역할

- **주 담당**: builder_agent (system_architect_agent 협력)
- **리뷰**: system_architect_agent

## 트리거

- Work Order가 DESIGNED 상태일 때

## 입력

- Work Order (DESIGNED 상태)
- Architecture Design Document
- Project Packet

## 절차

### 1. 구현 분해

- [ ] 설계를 구현 가능한 단위 태스크로 분해
- [ ] 각 태스크의 파일 / 모듈 / 함수 수준 변경 범위 명시
- [ ] 태스크 간 의존 순서 정의

### 2. 구현 전략

- [ ] 구현 순서 결정 (의존성 우선)
- [ ] 리스크가 높은 부분 먼저 vs 기반 코드 먼저 판단
- [ ] 테스트 전략 결정 (단위 / 통합 / E2E)

### 3. 검증 포인트 설정

- [ ] 각 태스크 완료 시 확인할 Evidence 정의
- [ ] 중간 검증 포인트 설정 (대규모 작업 시)

### 4. 실행 시작

- [ ] Work Order 상태: DESIGNED → IN-PROGRESS
- [ ] 구현 시작

## 출력

- **산출물**: Implementation Plan + 태스크 분해 목록
- **형식**: `validation/output_contract.md` 준수

## 수락 기준

- [ ] 태스크 분해가 구현 가능한 수준인가
- [ ] 의존 순서가 논리적인가
- [ ] 각 태스크의 Evidence가 정의되었는가
- [ ] system_architect_agent가 설계 의도와 정합하는지 확인

## 참조

- Work Order 생명주기: `workflows/work_order_lifecycle.md`
- Quality Checklist: `validation/quality_checklist.md`
