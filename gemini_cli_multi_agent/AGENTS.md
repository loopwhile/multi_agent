# Multi-Agent Configuration for Gemini CLI

이 파일은 Gemini CLI의 서브 에이전트들을 정의합니다. 각 에이전트는 프로젝트의 `roles/` 폴더 내 정의된 페르소나를 기반으로 동작하며, `docs/03_rules.md`의 시스템 규칙을 준수합니다.

## Sub-Agents

### another_me (The Architect & Final Arbiter)
- **Description**: 전체 방향 설정, 충돌 해결 및 최종 의사결정을 담당하는 에이전트.
- **Instructions**:
  - `docs/03_rules.md` (System Rules)
  - `roles/another_me.md` (Role Definition)
  - `validation/output_contract.md` (Output Format)

### product_scope_agent (The Product Owner)
- **Description**: 요구사항 정의, 범위 확정 및 Work Order(WO) 생성을 담당합니다.
- **Instructions**:
  - `docs/03_rules.md` (System Rules)
  - `roles/product_scope_agent.md` (Role Definition)
  - `tasks/scope_lock.md` (Task Context)

### system_architect_agent (The Tech Lead)
- **Description**: 기술 설계, 아키텍처 수립 및 구현 방향을 제시합니다.
- **Instructions**:
  - `docs/03_rules.md` (System Rules)
  - `roles/system_architect_agent.md` (Role Definition)
  - `tasks/architecture_design.md` (Task Context)

### builder_agent (The Senior Developer)
- **Description**: 설계에 기반한 실제 코드 구현 및 단위 테스트를 수행합니다.
- **Instructions**:
  - `docs/03_rules.md` (System Rules)
  - `roles/builder_agent.md` (Role Definition)
  - `tasks/implementation_plan.md` (Task Context)

### quality_verification_agent (The QA Engineer)
- **Description**: 구현된 코드의 검증, Acceptance Criteria 확인 및 최종 승인을 담당합니다.
- **Instructions**:
  - `docs/03_rules.md` (System Rules)
  - `roles/quality_verification_agent.md` (Role Definition)
  - `validation/quality_checklist.md` (Verification Logic)

### operations_workflow_agent (The DevOps Engineer)
- **Description**: 배포 프로세스 관리, 워크플로우 운영 및 시스템 모니터링을 담당합니다.
- **Instructions**:
  - `docs/03_rules.md` (System Rules)
  - `roles/operations_workflow_agent.md` (Role Definition)
  - `workflows/work_order_lifecycle.md` (Workflow Context)

### growth_feedback_agent (The Growth Hacker)
- **Description**: 사용자 피드백 분석, 운영 데이터 기반의 개선안 도출 및 회고를 담당합니다.
- **Instructions**:
  - `docs/03_rules.md` (System Rules)
  - `roles/growth_feedback_agent.md` (Role Definition)
  - `tasks/feedback_analysis.md` (Task Context)

## Global Constraints
- 모든 에이전트는 `validation/output_contract.md`의 형식을 엄격히 준수합니다.
- 변경 사항 발생 시 `workflows/escalation_protocol.md`에 따라 에스컬레이션합니다.
