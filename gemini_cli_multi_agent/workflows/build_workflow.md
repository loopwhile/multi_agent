# Workflow: Build

## 목적

설계 완료 후 **구현 → 검증까지의 실행 흐름**.

## 단계

```
1. Architecture Design
   → system_architect_agent: 기술 설계
   → Work Order: CREATED → DESIGNED

2. Design Review (DG2)
   → another_me: 설계 승인
   → Design Review Meeting (필요 시)
   → 판정: APPROVED / REVISE / REJECTED

3. Implementation Plan
   → builder_agent: 구현 계획 수립
   → 태스크 분해, 의존 순서, Evidence 정의

4. 구현 실행
   → builder_agent: 코드 작성
   → Work Order: DESIGNED → IN-PROGRESS
   → Build Sync Meeting (장기 작업 / 차단 시)

5. 구현 완료
   → builder_agent: Evidence 첨부
   → Verification Status: SELF-CHECKED
   → Work Order: IN-PROGRESS → PENDING-VERIFY
   → Handoff Checklist 확인

6. Verification (DG3)
   → quality_verification_agent: 검증 실행
   → Verification Plan에 따라 테스트
   → 판정: PASS → VERIFIED / FAIL → IN-PROGRESS (재작업)
```

## 참여 역할

| 단계 | 역할 |
|------|------|
| 1 | system_architect_agent |
| 2 | another_me |
| 3 | builder_agent, system_architect_agent |
| 4 | builder_agent |
| 5 | builder_agent |
| 6 | quality_verification_agent |

## 참조

- Task: `ai_prompts/tasks/architecture_design.md`
- Task: `ai_prompts/tasks/implementation_plan.md`
- Task: `ai_prompts/tasks/verification_plan.md`
- Review: `ai_prompts/reviews/architecture_review.md`
- Review: `ai_prompts/reviews/implementation_review.md`
- Meeting: `ai_prompts/meetings/design_review_meeting.md`
- Meeting: `ai_prompts/meetings/build_sync_meeting.md`
- Handoff: `ai_prompts/validation/handoff_checklist.md`
- Decision Gates DG2, DG3: `ai_prompts/validation/decision_gate.md`
