# Task: Verification Plan

## 목적

구현 결과물에 대한 검증 계획을 수립하고 Verification Gate 통과를 위한 기준을 사전 정의한다.

## 담당 역할

- **주 담당**: quality_verification_agent
- **입력 제공**: builder_agent

## 트리거

- Work Order가 IN-PROGRESS 상태일 때 (구현과 병행)
- Work Order가 PENDING-VERIFY 상태일 때 (검증 실행 시)

## 입력

- Work Order (수락 기준 포함)
- Implementation Plan
- Architecture Design Document

## 절차

### 1. 검증 범위 정의

- [ ] 검증 대상 코드 / 기능 목록
- [ ] 검증 제외 범위와 근거

### 2. 검증 기준 수립

- [ ] Work Order의 Acceptance Criteria에서 검증 항목 추출
- [ ] Quality Checklist 중 적용할 항목 선택
- [ ] 각 항목의 PASS/FAIL 판정 기준 명시

### 3. 테스트 설계

- [ ] 단위 테스트: 필요 케이스 목록
- [ ] 통합 테스트: 인터페이스 경계 테스트
- [ ] 브라우저/UI 테스트: 사용자 시나리오 (해당 시)
- [ ] 엣지 케이스: 경계값, 에러 케이스

### 4. 검증 실행 & 판정

- [ ] Evidence 수집 (빌드 로그, 테스트 결과, 스크린샷)
- [ ] Verification Report 작성
- [ ] 최종 판정: PASS / CONDITIONAL PASS / FAIL

## 출력

- **산출물**: Verification Plan + Verification Report
- **형식**: `validation/output_contract.md` 준수
- **보고서 형식**: `workflows/verification_gate.md`의 검증 보고서 형식

## 수락 기준

- [ ] 모든 Acceptance Criteria에 대응하는 검증 항목이 있는가
- [ ] 판정 기준이 재현 가능한가 (다른 검증자도 같은 판정 가능)
- [ ] Evidence가 구체적이고 검증 가능한가

## 참조

- Verification Gate: `workflows/verification_gate.md`
- Quality Checklist: `validation/quality_checklist.md`
- Output Contract: `validation/output_contract.md`
