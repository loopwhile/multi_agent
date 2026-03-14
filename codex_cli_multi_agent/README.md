# Codex CLI Multi-Agent Setup Guide

이 폴더는 Codex CLI에서 복수의 전문 에이전트와 작업 단위 스킬을 안정적으로 실행하기 위한 독립 패키지입니다.

## 0. 구조 개요

이 패키지의 에이전트 정의는 **프로젝트 루트의 `AGENTS.md`**에 위치합니다.
Codex CLI는 프로젝트 루트의 `AGENTS.md`를 자동 로드하므로, 별도 경로 지정 없이 동작합니다.

```
프로젝트 루트/
├── AGENTS.md              ← Codex CLI 엔트리 포인트 (이 패키지의 에이전트 정의)
├── .codex/config.toml     ← Codex CLI 모델/샌드박스/프로필/에이전트 설정
└── codex_cli_multi_agent/ ← 이 폴더 (코어 문서)
    ├── .agents/skills/    ← 실행 가능한 스킬 단위
    ├── roles/             ← 에이전트 페르소나 정의
    ├── workflows/         ← 협업 절차 및 에스컬레이션 규칙
    ├── validation/        ← 품질 체크리스트 및 출력 계약
    ├── tasks/             ← 작업 단위 템플릿
    ├── docs/              ← 시스템 규칙, 프로젝트 패킷, 핸드오프
    └── adapters/          ← 도구별 실행 매핑 문서
```

## 1. 사용 방법

### 1.1 최초 1회 부트스트랩 프롬프트

프로젝트 루트에서 Codex CLI를 실행한 뒤 아래를 입력합니다:

```text
프로젝트를 분석해서 아래 3개 파일을 템플릿 기반으로 생성/초기화해줘.
1) codex_cli_multi_agent/docs/project_packets/<project>.md
2) codex_cli_multi_agent/tasks/WO-<today>-001-<slug>.md
3) codex_cli_multi_agent/docs/session_handoff.md

그리고 session_handoff의 Current work order가 방금 만든 WO 파일을 가리키게 설정해줘.
```

### 1.2 세션 시작 프롬프트

부트스트랩 직후 또는 세션 재시작 시 아래를 입력합니다:

```text
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 서브에이전트에 위임해.
독립 가능한 작업은 병렬로 처리하고, 모든 중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
Execution Mode와 Parallel Evidence를 보고서에 포함해.
```

> Codex CLI가 루트 `AGENTS.md`를 자동 로드하므로 경로 지정이 불필요합니다.

## 2. 실행 모드

- **Mode A: Logical Multi-Agent** (기본)
  - 단일 Codex 세션에서 `another_me`가 역할을 분해하여 실행
  - 실제 동시 병렬 실행이 아닐 수 있음
- **Mode B: True Parallel**
  - Codex 내장 multi-agent 기능 활성화 필요

### Mode B 활성화

1. `.codex/config.toml`에 `[features] multi_agent = true` (설정 완료)
2. Codex CLI에서 `/experimental` → Multi-agents ON
3. `/agent`로 에이전트 스레드 점검/전환

## 3. 세션 한도 운영

Codex CLI는 세션 한도가 짧을 수 있으므로 상태를 파일로 고정합니다.

### 필수 파일 3개 (최신 상태 유지)

1. `docs/project_packets/<project>.md`
2. `tasks/<current_work_order>.md`
3. `docs/session_handoff.md`

### 남은 한도 판단 기준

- `>= 40%`: 계속 진행
- `25% ~ 39%`: 현재 Phase 마감 준비 병행
- `< 25%`: 신규 구현 중단, handoff 정리 + commit 우선

### 세션 복구 프롬프트

```text
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

## 4. 에이전트 프로필 맵

| 에이전트 | 역할 |
|----------|------|
| `another_me` | 총괄/의사결정/충돌중재 |
| `product_scope_agent` | 범위 고정/WO 생성 |
| `system_architect_agent` | 기술 설계 |
| `builder_agent` | 코드 구현 |
| `quality_verification_agent` | 검증/승인 |
| `operations_workflow_agent` | 배포/운영 |
| `growth_feedback_agent` | 피드백 분석 |

## 5. 개발 단계 권장 라우팅

- 구현: `builder_agent` 중심
- 설계 불확실성: `system_architect_agent` 추가
- 기능 완료 검증: `quality_verification_agent` 필수
- 배포/운영: `operations_workflow_agent`
- 요구사항 변경 시만: `product_scope_agent`
- 회고/성장 분석 시만: `growth_feedback_agent`

## 6. 불변 규칙

- Verify 없는 완료 금지
- Evidence 없는 완료 보고 금지
- 도구 특화 정책은 `adapters/`에만 작성
- 증거 없는 병렬 주장 금지

## 7. 커밋 규칙 (WBS 연동)

- 커밋 메시지에 WBS 식별자 포함: `feat(wbs:M1-P2): add order summary endpoint`
- 하나의 commit에 여러 Phase를 섞지 않음
- 커밋 전 최소 체크:
  - WO 문서 상태 갱신
  - session_handoff 동기화
  - 테스트/검증 Evidence 확보
