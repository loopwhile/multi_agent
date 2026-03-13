# Workflow: Review

## 목적

Work Order 생명주기의 **각 Gate에서 수행하는 리뷰의 통합 흐름**.

## 리뷰 흐름 맵

```
Scope Review (DG1)
  → Architecture Review (DG2)
    → Implementation Review (DG3)
      → Release Review (DG4)
        → Workflow Review (정기)
```

## 리뷰 실행 규칙

### 1. 리뷰 요청

- 전 단계의 역할이 리뷰 대상 산출물과 함께 리뷰를 요청
- Handoff Checklist를 충족해야 리뷰 요청 가능

### 2. 리뷰 실행

- 리뷰어가 해당 Review Prompt의 체크리스트를 순회
- 각 항목에 대해 PASS/FAIL + 근거 기록

### 3. 판정

- APPROVED / REVISE / REJECTED (또는 PASS / CONDITIONAL PASS / FAIL)
- REVISE/FAIL 시 구체적 피드백 필수

### 4. 반복

- REVISE: 작성자가 수정 → 재리뷰
- REJECTED/FAIL: Work Order 상태를 이전 단계로 되돌림

## 리뷰 ↔ Gate 매핑

| Gate | 리뷰 | 리뷰어 |
|------|------|--------|
| DG1 | Scope Review | another_me |
| DG2 | Architecture Review | another_me |
| DG3 | Implementation Review | quality_verification_agent |
| DG4 | Release Review | another_me / product_scope_agent |
| 정기 | Workflow Review | another_me |

## 참조

- Reviews: `reviews/*.md`
- Decision Gates: `validation/decision_gate.md`
- Handoff Checklist: `validation/handoff_checklist.md`
