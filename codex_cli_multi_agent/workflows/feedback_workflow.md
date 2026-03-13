# Workflow: Feedback

## 목적

배포 후 **피드백 수집 → 분석 → 개선 제안 → 차기 Work Order 생성까지의 순환 흐름**.

## 단계

```
1. 데이터 수집
   → operations_workflow_agent: 운영 데이터 제공
   → growth_feedback_agent: 사용자 피드백 수집

2. 분석
   → growth_feedback_agent: 피드백 구조화 + 지표 분석
   → Feedback Report 작성

3. 개선 제안
   → growth_feedback_agent: Improvement Proposals 작성
   → 영향도 × 실현 용이성 기준 우선순위

4. Feedback Retrospective Meeting
   → 전체 역할: 피드백 리뷰 + 개선 논의
   → 채택 / 보류 / 기각 결정

5. 스코프 반영
   → product_scope_agent: 채택된 제안을 Work Order로 변환
   → 새 Work Order 생성 → Kickoff Workflow로 순환
```

## 순환 구조

```
Release → Feedback → Kickoff → Build → Release → ...
```

이 워크플로우가 완료되면 채택된 개선 제안이 새 Work Order로 변환되어  
Kickoff Workflow로 진입한다. 이것이 Agent OS의 **운영 순환 루프**다.

## 참여 역할

| 단계 | 역할 |
|------|------|
| 1 | operations_workflow_agent, growth_feedback_agent |
| 2 | growth_feedback_agent |
| 3 | growth_feedback_agent |
| 4 | 전원 |
| 5 | product_scope_agent |

## 참조

- Task: `ai_prompts/tasks/feedback_analysis.md`
- Meeting: `ai_prompts/meetings/feedback_retrospective.md`
- Growth Feedback Agent: `ai_prompts/roles/growth_feedback_agent.md`
- Kickoff Workflow: `ai_prompts/workflows/kickoff_workflow.md`
