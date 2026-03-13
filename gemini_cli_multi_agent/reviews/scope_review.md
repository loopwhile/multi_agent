# Review: Scope Review

## 목적

Scope Document와 Work Order 초안이 **명확하고 실현 가능하며 검증 가능한지** 리뷰한다.

## 리뷰어

- **주 리뷰어**: another_me
- **보조**: system_architect_agent (기술 실현성)

## 리뷰 대상

- Scope Document
- Work Order 초안 (CREATED 상태)
- 수락 기준 (Acceptance Criteria)

## 체크리스트

### 스코프 명확성

- [ ] 목표가 한 문장으로 설명 가능한가
- [ ] 포함/제외 경계가 명확한가
- [ ] must-have와 nice-to-have가 분리되었는가
- [ ] 스코프 크리프 위험이 통제되었는가

### 수락 기준

- [ ] 각 기준이 검증 가능한가 (Yes/No 판정 가능)
- [ ] 기준이 모호하지 않은가
- [ ] 누락된 기준이 없는가

### 실현성

- [ ] 현재 아키텍처에서 구현 가능한가
- [ ] 예상 공수가 합리적인가
- [ ] 의존성이 식별되었는가

### 비즈니스 정합성

- [ ] 핵심 제품 루프를 보존하는가
- [ ] 사용자 가치에 기여하는가

## 판정

| 판정 | 후속 |
|------|------|
| **APPROVED** | Work Order를 DESIGNED 단계로 진행 |
| **REVISE** | product_scope_agent에게 수정 요청 + 구체적 피드백 |
| **REJECTED** | 사유 기록 + Work Order 폐기 또는 재설계 |

## 출력

- 리뷰 판정 + 피드백 (Output Contract 형식)
