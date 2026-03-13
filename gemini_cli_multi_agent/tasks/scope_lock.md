# Task: Scope Lock

## 목적

요구사항을 확정하고 Work Order로 변환 가능한 수준까지 스코프를 고정한다.

## 담당 역할

- **주 담당**: product_scope_agent
- **협력**: system_architect_agent (기술 실현성 확인)
- **승인**: another_me

## 트리거

- 새 기능 요청
- 스코프 변경 요청
- growth_feedback_agent의 개선 제안

## 입력

- 사용자 요청 또는 개선 제안서
- 기존 Product Scope Document (있는 경우)
- Project Packet

## 절차

### 1. 요구사항 구조화

- [ ] 요청을 구조화된 요구사항으로 변환
- [ ] 기능 목표 / 비기능 요구사항 분리
- [ ] must-have vs nice-to-have 분류

### 2. 기술 실현성 확인

- [ ] system_architect_agent에게 실현성 검토 요청
- [ ] 기술적 제약 / 의존성 식별
- [ ] 예상 공수 판단

### 3. 스코프 경계 확정

- [ ] 포함 범위 명시
- [ ] 제외 범위 명시 (with 근거)
- [ ] 영향 받는 기존 기능 식별

### 4. Work Order 초안 생성

- [ ] `tasks/_template.md` 기반으로 Work Order 작성
- [ ] 수락 기준 (Acceptance Criteria) 작성
- [ ] 우선순위 배정

## 출력

- **산출물**: Scope Document + Work Order 초안
- **형식**: `validation/output_contract.md` 준수

## 수락 기준

- [ ] must-have / nice-to-have가 분리되었는가
- [ ] 포함/제외 경계가 명확한가
- [ ] 수락 기준이 검증 가능한가
- [ ] another_me 승인

## 참조

- Work Order 템플릿: `tasks/_template.md`
- Work Order 생명주기: `workflows/work_order_lifecycle.md`
