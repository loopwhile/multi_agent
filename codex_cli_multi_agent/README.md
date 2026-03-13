# Codex CLI Multi-Agent Setup Guide

이 폴더는 Codex CLI에서 복수의 전문 에이전트와 작업 단위 스킬을 안정적으로 실행하기 위한 독립 패키지다.

## 0. Path Convention
- 이 문서의 경로 표기는 **package-root-relative** 기준이다.
- 예: `AGENTS.md`, `docs/session_handoff.md`, `.codex/config.toml`

### 복사 배포 시 경로 기준 분리
- 이 폴더를 실제 프로젝트로 복사하면, 아래 항목은 **대상 프로젝트 루트** 기준으로 둔다.
- `AGENTS.md`(라우터), `.codex/`, `.gemini/`(공존 운영 시)
- 반면 `codex_cli_multi_agent/*` 내부 참조는 계속 package-root-relative로 유지한다.

## 1. 이 패키지의 목표
- 어떤 프로젝트 루트에 복사해도 바로 동작하도록 구성
- 공용 코어 문서(roles/workflows/validation/tasks/docs)를 로컬 포함
- `.codex/config.toml`의 역할별 프로필로 모델 라우팅
- `gemini_cli_multi_agent`와 동시 공존 시에도 충돌 없이 운영

## 2. 폴더 구조
- `AGENTS.md`: 서브 에이전트 진입 규칙
- `.codex/config.toml`: Codex CLI 모델/샌드박스/프로필 설정
- `.agents/skills/`: 실행 가능한 스킬 단위
- `roles/`, `workflows/`, `validation/`, `tasks/`, `docs/`: 공용 코어 문서
- `adapters/`: 도구별 실행 매핑 문서

## 2.1 Codex + Gemini 공존 배치 (권장)
프로젝트 루트에 아래 4개를 동시에 둔다.
- `codex_cli_multi_agent/`
- `gemini_cli_multi_agent/`
- `.codex/` (Codex 전용)
- `.gemini/` (Gemini 전용)

주의:
- 루트 `AGENTS.md`는 자동 인식에만 의존하지 말고, 세션 프롬프트에서 항상 경로를 명시한다.
- Codex 세션에서는 `codex_cli_multi_agent/*`만 참조하도록 지시한다.

## 2.2 루트 AGENTS 라우터 템플릿 (선택)
프로젝트 루트 `AGENTS.md`를 아래처럼 중립 라우터로 둘 수 있다.

```md
# Router AGENTS

- If running in Codex CLI:
  - Follow `codex_cli_multi_agent/AGENTS.md`
  - Read only `codex_cli_multi_agent/*` and `.codex/*` for orchestration

- If running in Gemini CLI:
  - Follow `gemini_cli_multi_agent/AGENTS.md`
  - Read only `gemini_cli_multi_agent/*` and `.gemini/*` for orchestration

- Do not mix Codex and Gemini agent definitions in a single run.
```

## 3. 사용 방법
1. 대상 프로젝트 루트로 이 폴더의 내용을 복사한다.
2. 프로젝트 루트에서 Codex CLI를 실행한다.
3. 최초 1회 부트스트랩 프롬프트로 필수 파일 3개를 생성/초기화한다.
4. 같은 세션에서 바로 `4.1 세션 시작 프롬프트`를 실행한다.
5. 세션이 끊기거나 재시작한 경우에만 `4.1 세션 시작 프롬프트`로 복구한다.

### 3.1 최초 1회 부트스트랩 프롬프트
아래를 그대로 입력:

```text
codex_cli_multi_agent/AGENTS.md 기준으로 동작해.
프로젝트를 분석해서 아래 3개 파일을 템플릿 기반으로 생성/초기화해줘.
1) codex_cli_multi_agent/docs/project_packets/<project>.md
2) codex_cli_multi_agent/tasks/WO-<today>-001-<slug>.md
3) codex_cli_multi_agent/docs/session_handoff.md

그리고 session_handoff의 Current work order가 방금 만든 WO 파일을 가리키게 설정해줘.
```

생성 완료 후에는 모델이 임의로 만든 프롬프트를 따르지 말고, 아래 `4.1 세션 시작 프롬프트`를 그대로 실행한다.

## 4. another_me 관리자 모드 (권장)
아래 방식으로 시작하면 `another_me`가 관리자 역할로 나머지 커스텀 서브 에이전트를 관리한다.

### 4.0 실행 모드
- `Mode A: Logical Multi-Agent (single runtime)` (기본)
  - 하나의 Codex 세션에서 another_me가 역할을 분해해 실행
  - 실제 동시 병렬 실행이 아닐 수 있음
- `Mode B: True Parallel (external orchestrator required)`
  - 외부 오케스트레이터가 있을 때만 실제 병렬 실행으로 인정
  - Codex 내장 multi-agent(실험) 기능을 활성화하면 유사 병렬 실행 가능

### 4.0.1 Mode B 활성화 (Codex)
1. `codex_cli_multi_agent/.codex/config.toml`에 이미 설정됨:
   - `[features] multi_agent = true`
   - `[agents.<name>] config_file = ".codex/agents/<name>.toml"`
2. Codex CLI에서 `/experimental` 열고 `Multi-agents`를 `ON`한 뒤 재시작한다.
3. `/agent`로 현재 활성 agent thread를 점검/전환한다.
4. 최종 보고에 `Execution Mode`, `Parallel Evidence`를 반드시 포함한다.

### 4.1 세션 시작 프롬프트
최초 부트스트랩 직후 또는 세션 재시작 시 아래를 그대로 입력:

```text
codex_cli_multi_agent/AGENTS.md를 기준으로 동작해.
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 서브에이전트에 위임해.
독립 가능한 작업은 병렬로 처리하고, 모든 중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
Execution Mode와 Parallel Evidence를 보고서에 포함해.
```

### 4.2 운영 규칙
- 사용자 보고 채널: `another_me` 단일 창구
- 실행 방식: 가능한 작업은 병렬 위임, 불가능하면 순차 실행 + 병렬 계획안 보고
- 완료 조건: `quality_verification_agent` 검증 + Evidence 첨부
- 실행 단위: `Milestone > Phase > Work Order`
- 기본 정책: `Phase 1개 완료 = Commit 1개 (Codex CLI가 직접 commit)`
- 개발 단계 기본 정책: 모든 서브 에이전트 일괄 호출 금지
- `another_me`가 업무 성격을 판단해 필요한 서브 에이전트만 선택 위임
- 병렬 관련 보고 시 `Execution Mode`와 `Parallel Evidence`를 함께 제시
- 증거 없으면 “병렬 처리 완료” 표현 금지

### 4.3 추천 요청 패턴
- "another_me로 시작해서 현재 작업을 병렬 가능한 단위로 쪼개고 실행해."
- "서브 에이전트별 진행 현황과 리스크만 요약 보고해."
- "최종 보고는 What/Why/Risks/Verify/TODO 형식으로 제출해."
- "개발 단계에서는 필요한 에이전트만 선택해서 위임해."
- "이번 보고에 Execution Mode와 Parallel Evidence를 반드시 포함해."

### 4.4 개발 단계 권장 라우팅
- 구현: `builder_agent` 중심
- 설계 불확실성 발생 시: `system_architect_agent` 추가
- 기능 완료 검증: `quality_verification_agent` 필수
- 배포/운영 작업: `operations_workflow_agent`
- 요구사항 변경 시에만: `product_scope_agent`
- 회고/성장 분석이 필요할 때만: `growth_feedback_agent`

## 5. 세션 한도 운영 (권장)
Codex CLI는 세션 한도가 짧을 수 있으므로 상태를 대화가 아닌 파일로 고정한다.

### 5.1 남은 한도 확인
- Codex CLI 상태줄의 표시를 기준으로 확인한다.
- 예시: `gpt-5.3-codex medium · 82% left · <path>`
- `left` 비율이 낮아지면 구현보다 handoff 정리를 우선한다.
- 세션 시작/중간 게이트에서 사용자가 `% left`를 한 줄로 전달한다.
- `another_me`가 해당 값을 기준으로 `계속 진행 vs handoff 마감`을 판단한다.

판단 기준:
- `>= 40%`: 계속 진행
- `25% ~ 39%`: 현재 Phase 마감 준비 병행
- `< 25%`: 신규 구현 중단, handoff 정리 + commit 우선

### 5.2 세션 분할 운영 (필수)
아래 3개 파일을 최신 상태로 유지한다.
1. `docs/project_packets/<project>.md`
2. `tasks/<current_work_order>.md`
3. `docs/session_handoff.md`

세션 종료 직전 루틴:
1. `docs/session_handoff.md` 업데이트
2. `tasks/<current_work_order>.md` 상태 업데이트
3. Evidence 경로/명령 기록
4. 현재 활성 Phase 완료 시 commit 수행

세션 재시작 프롬프트:
```text
codex_cli_multi_agent/AGENTS.md 기준으로 동작해.
너는 another_me 관리자다.
먼저 아래 파일을 읽고 현재 상태를 복구해:
1) codex_cli_multi_agent/docs/project_packets/<project>.md
2) codex_cli_multi_agent/tasks/<current_work_order>.md
3) codex_cli_multi_agent/docs/session_handoff.md

복구 후:
- 현재 상태 요약
- 즉시 실행할 다음 작업 1개
- 병렬 가능한 서브에이전트 작업 분해안
을 보고하고 실행해.
```

## 6. 권장 프로필 맵
- `another_me` → 총괄/의사결정/충돌중재
- `product_scope_agent` → 범위 고정/WO 생성
- `system_architect_agent` → 기술 설계
- `builder_agent` → 코드 구현
- `quality_verification_agent` → 검증/승인
- `operations_workflow_agent` → 배포/운영
- `growth_feedback_agent` → 피드백 분석

## 7. 불변 규칙
- Verify 없는 완료 금지
- Evidence 없는 완료 보고 금지
- 도구 특화 정책은 `adapters/`에만 작성
- 증거 없는 병렬 주장 금지

## 8. 커밋 규칙 (WBS 연동)
- Commit message에 WBS 식별자를 포함한다.
  - 예: `feat(wbs:M1-P2): add order summary endpoint`
- 하나의 commit에 여러 Phase를 섞지 않는다.
- 커밋 직전 최소 체크:
  - WO 문서 상태 갱신
  - session_handoff 동기화
  - 테스트/검증 Evidence 확보
