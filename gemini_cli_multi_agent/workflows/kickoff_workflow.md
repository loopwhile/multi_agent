# Workflow: Kickoff

## 목적

새 프로젝트 또는 주요 작업 시작 시의 **초기화 흐름**.

## 단계

```
1. 요청 접수
   → another_me: 요청의 구조/규모 판단

2. Project Analysis (신규 프로젝트 시)
   → product_scope_agent + system_architect_agent
   → 산출물: Project Packet

3. Scope Lock
   → product_scope_agent: 스코프 정의 + Work Order 초안
   → system_architect_agent: 기술 실현성 확인

4. Scope Review (DG1)
   → another_me: 스코프 승인
   → 판정: APPROVED / REVISE / REJECTED

5. Kickoff Meeting
   → 전체 역할 정렬
   → 역할 배분, 일정 합의

6. Work Order 확정 → CREATED 상태
```

## 참여 역할

| 단계 | 역할 |
|------|------|
| 1 | another_me |
| 2 | product_scope_agent, system_architect_agent |
| 3 | product_scope_agent, system_architect_agent |
| 4 | another_me |
| 5 | 전원 |
| 6 | product_scope_agent |

## 참조

- Task: `tasks/project_analysis.md`
- Task: `tasks/scope_lock.md`
- Review: `reviews/scope_review.md`
- Meeting: `meetings/kickoff_meeting.md`
- Decision Gate DG1: `validation/decision_gate.md`
