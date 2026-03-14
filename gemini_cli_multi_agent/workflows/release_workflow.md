# Workflow: Release

## 목적

검증 통과 후 **배포 결정 → 배포 실행 → 배포 후 확인까지의 흐름**.

## 단계

```
1. Release Readiness 확인
   → operations_workflow_agent: 배포 전 점검
   → 배포 계획 수립, 롤백 계획 확인

2. Release Review (DG4)
   → Release Go/No-Go Meeting
   → another_me 또는 product_scope_agent: GO / NO-GO 판정

3. 배포 실행 (GO인 경우)
   → operations_workflow_agent: 배포 스크립트 실행
   → Work Order: VERIFIED → DEPLOYED

4. 배포 후 검증
   → operations_workflow_agent: Smoke test
   → 모니터링 지표 확인
   → 배포 결과 Work Order에 기록

5. Work Order 폐쇄
   → another_me 또는 product_scope_agent: CLOSED 확인
   → Work Order: DEPLOYED → CLOSED

NO-GO인 경우:
  → 사유 기록
  → 필요 조치 명시
  → 재검토 시점 설정
  → Work Order 상태 유지 (VERIFIED)
```

## 참여 역할

| 단계 | 역할 |
|------|------|
| 1 | operations_workflow_agent |
| 2 | another_me, product_scope_agent, quality_verification_agent, operations_workflow_agent |
| 3 | operations_workflow_agent |
| 4 | operations_workflow_agent |
| 5 | another_me / product_scope_agent |

## 참조

- Task: `ai_prompts/tasks/release_readiness.md`
- Review: `ai_prompts/reviews/release_review.md`
- Meeting: `ai_prompts/meetings/release_go_no_go.md`
- Decision Gate DG4: `ai_prompts/validation/decision_gate.md`
