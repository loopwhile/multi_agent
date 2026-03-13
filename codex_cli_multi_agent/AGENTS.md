# Multi-Agent Configuration for Codex CLI

이 파일은 Codex CLI 환경에서 사용하는 멀티/서브 에이전트의 공통 엔트리 포인트다.
이 패키지는 특정 프로젝트 루트에 복사해도 동작하도록 로컬 경로 기반으로 설계되어 있다.

## Global Rules
- 모든 에이전트는 `docs/03_rules.md`를 우선 준수한다.
- 완료 보고 시 Evidence(테스트 로그, 빌드 로그, diff, API 응답) 없이 완료 선언 금지.
- Verify 없는 CLOSED 금지.
- 사용자 직접 응답은 `another_me`만 수행한다.
- 다른 서브 에이전트는 `another_me`의 위임을 받아 작업하며, 결과는 `another_me`에게만 보고한다.
- 작업 추적은 WBS(마일스톤/페이즈)와 Work Order를 함께 사용한다.
- 기본 커밋 단위는 `Phase 1개 = Commit 1개`다.
- 개발 단계에서 모든 서브 에이전트를 일괄 호출하지 않는다.
- `another_me`가 작업 성격에 따라 필요한 서브 에이전트만 선택 위임한다.

## Sub-Agents

### another_me (The Architect & Final Arbiter)
- Role File: `roles/another_me.md`
- Task Context: `tasks/project_analysis.md`
- Recommended Profile: `another_me`

### product_scope_agent (The Product Owner)
- Role File: `roles/product_scope_agent.md`
- Task Context: `tasks/scope_lock.md`
- Recommended Profile: `product_scope_agent`

### system_architect_agent (The Tech Lead)
- Role File: `roles/system_architect_agent.md`
- Task Context: `tasks/architecture_design.md`
- Recommended Profile: `system_architect_agent`

### builder_agent (The Senior Developer)
- Role File: `roles/builder_agent.md`
- Task Context: `tasks/implementation_plan.md`
- Recommended Profile: `builder_agent`

### quality_verification_agent (The QA Engineer)
- Role File: `roles/quality_verification_agent.md`
- Validation Context: `validation/quality_checklist.md`
- Recommended Profile: `quality_verification_agent`

### operations_workflow_agent (The DevOps Engineer)
- Role File: `roles/operations_workflow_agent.md`
- Workflow Context: `workflows/work_order_lifecycle.md`
- Recommended Profile: `operations_workflow_agent`

### growth_feedback_agent (The Growth Analyst)
- Role File: `roles/growth_feedback_agent.md`
- Task Context: `tasks/feedback_analysis.md`
- Recommended Profile: `growth_feedback_agent`

## Skill Routing
- Skills are in `.agents/skills/`.
- Use `scope-lock`, `architecture-design`, `implementation-plan`, `quality-check`, `feedback-analysis` as task units.

## Orchestration Protocol (Manager = another_me)
1. 세션 시작 시 `another_me`가 현재 프로젝트의 사용 가능한 서브 에이전트 목록을 작성한다.
2. `another_me`는 요청을 Work Order 단위로 분해하고, 작업을 서브 에이전트에 위임한다.
3. 독립 가능한 작업은 병렬로 위임한다.
4. 병렬 실행이 불가능한 환경에서는 동일 계획을 순차로 실행하되, 병렬 계획안을 함께 보고한다.
5. 모든 서브 에이전트 결과물은 Evidence와 함께 수집해 `another_me`가 통합 판단 후 사용자에게 단일 보고한다.
6. 역할 충돌 시 `workflows/escalation_protocol.md`를 따른다.

## Execution Mode Definition
- `Mode A: Logical Multi-Agent (single runtime)`
  - 하나의 Codex 세션에서 `another_me`가 역할을 분해/시뮬레이션하여 실행한다.
  - 병렬 계획은 가능하지만 실제 런타임 동시 실행이 아닐 수 있다.
- `Mode B: True Parallel (external orchestrator required)`
  - 외부 오케스트레이터(멀티 세션/멀티 워커)가 있는 경우에만 실제 병렬 실행으로 인정한다.
  - 독립 실행 로그/세션 증거가 있어야 한다.
  - Codex 내장 multi-agent를 사용할 경우 `.codex/config.toml`의 `[features] multi_agent = true`와 `[agents.*]` 설정이 필요하다.

## Mode B Activation Checklist
1. `.codex/config.toml`에 `features.multi_agent = true`가 설정되어 있다.
2. `/experimental`에서 Multi-agents가 ON 상태다.
3. 역할별 `config_file`이 `.codex/agents/*.toml`에 존재한다.
4. 보고서에 `Execution Mode`와 `Parallel Evidence`를 포함한다.

## Selective Delegation Rule (Development Phase)
1. 기본값은 `minimal delegation`이다. (필요한 역할만 호출)
2. 개발 단계 표준 라우팅:
   - 구현 중심: `builder_agent` (+ 필요 시 `system_architect_agent`)
   - 검증 필요: `quality_verification_agent`
   - 배포/운영 작업: `operations_workflow_agent`
   - 요구사항 변경 발생 시에만: `product_scope_agent`
3. `growth_feedback_agent`는 운영 데이터/회고 요청이 있을 때만 호출한다.
4. “모든 서브 에이전트에게 동시에 지시”는 금지하며, 근거가 있을 때만 다중 위임한다.
5. `another_me`는 위임 이유를 중간 보고에 1줄로 명시한다.

## WBS + Commit Gate
1. 모든 실행은 `Milestone > Phase > Work Order` 계층으로 관리한다.
2. 한 번에 Phase 1개만 활성화한다.
3. 활성 Phase의 Acceptance Criteria와 Evidence가 충족되면 Codex CLI가 직접 commit을 수행한다.
4. 커밋 메시지는 WBS 식별자를 포함한다. 예: `feat(wbs:M2-P1): implement ...`
5. 커밋 전 확인:
   - 관련 WO 상태가 `PENDING-VERIFY` 이상
   - `tasks/<WO>.md`와 `docs/session_handoff.md` 동기화 완료

## % Left Decision Gate
1. 세션 시작 시 사용자 입력으로 `% left`를 1줄 전달받는다.
2. 중간 게이트(하위 작업/서브태스크 완료 시점)마다 `% left` 재확인한다.
3. `another_me` 판단 규칙:
   - `>= 40%`: 계속 진행
   - `25% ~ 39%`: 현재 Phase 마감 준비 병행
   - `< 25%`: 신규 구현 중단, handoff 정리 및 commit 우선

## Reporting Contract
- 사용자와의 커뮤니케이션 채널은 `another_me` 단일 창구로 유지한다.
- 중간 보고 형식:
  - 진행 상태 (진행중/대기/완료)
  - 병렬 작업 트랙별 담당 에이전트
  - Execution Mode (`Mode A` 또는 `Mode B`)
  - Parallel Evidence (실제 병렬 증거가 있을 때만)
  - 리스크 및 의사결정 필요 항목
- 최종 보고 형식:
  - What / Why / Risks / Verify / TODO
  - Execution Mode
  - Parallel Evidence

## Parallel Claim Policy
1. 증거가 없으면 “병렬 처리 완료”라고 표현하지 않는다.
2. `Mode A`에서는 “병렬 계획/병렬 트랙 설계”로만 표현한다.
3. “실제 병렬 실행” 표현은 `Mode B` + 증거(독립 세션 로그/타임스탬프/워크러너 출력)일 때만 사용한다.

## Portable Initialization
1. Load `docs/03_rules.md`.
2. Load one role file from `roles/`.
3. Load one project packet from `docs/project_packets/` (create from template if absent).
