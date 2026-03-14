# Gemini CLI Multi-Agent Setup Guide

이 폴더는 Gemini CLI에서 복수의 전문 에이전트와 특정 작업 단위(Skill)를 효율적으로 가동하기 위해 구성되었습니다.

## 0. 구조 개요

이 패키지의 에이전트 정의는 **프로젝트 루트의 `GEMINI.md`**에 위치합니다.
Gemini CLI는 프로젝트 루트의 `GEMINI.md`를 자동 로드하므로, 별도 경로 지정 없이 동작합니다.

```
프로젝트 루트/
├── GEMINI.md                    ← Gemini CLI 엔트리 포인트 (이 패키지의 에이전트 정의)
├── .gemini/settings.json        ← Gemini CLI 설정
├── .geminiignore                ← Codex 관련 파일 제외
└── gemini_cli_multi_agent/      ← 이 폴더 (코어 문서)
    ├── HANDOVER.md              ← 세션 복구용 핸드오버 파일
    ├── SKILL.md                 ← 스킬 정의
    ├── roles/                   ← 에이전트 페르소나 정의
    ├── workflows/               ← 협업 절차 및 에스컬레이션 규칙
    ├── validation/              ← 품질 체크리스트 및 출력 계약
    ├── tasks/                   ← 작업 단위 템플릿
    ├── docs/                    ← 시스템 규칙
    └── adapters/                ← 도구별 실행 매핑 문서
```

## 1. 사용 방법

### 1.1 세션 시작 프롬프트

프로젝트 루트에서 Gemini CLI를 실행한 뒤 아래를 입력합니다:

```text
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 필요한 서브에이전트에게만 위임해.
중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
```

> Gemini CLI가 루트 `GEMINI.md`를 자동 로드하므로 경로 지정이 불필요합니다.

### 1.2 서브 에이전트 호출

대화 중에 특정 에이전트의 전문성이 필요할 때:

- `@builder_agent 코드 구현을 시작해줘.`
- `@system_architect_agent 현재 구조에 대해 리뷰해줘.`
- `@quality_verification_agent 구현된 내용에 대한 테스트 결과(Evidence)를 검증해줘.`

### 1.3 스킬 실행

특정 작업을 즉시 수행하고 싶을 때:

- `/skill-activate quality-check` (품질 검사 스킬 활성화)
- `scope-lock 스킬을 사용해서 요구사항 정의서를 작성해줘.`

## 2. 에이전트 목록

| 에이전트 | 역할 |
|----------|------|
| `another_me` | 총괄/의사결정/충돌중재 |
| `product_scope_agent` | 요구사항 정의/범위 확정 |
| `system_architect_agent` | 기술 설계/아키텍처 |
| `builder_agent` | 코드 구현/단위 테스트 |
| `quality_verification_agent` | 검증/승인 |
| `operations_workflow_agent` | 배포/운영/모니터링 |
| `growth_feedback_agent` | 피드백 분석/회고 |

## 3. 핵심 규칙

모든 에이전트는 `docs/03_rules.md`를 읽고 행동하도록 설정되어 있습니다.

- **R1. Verify 없는 완료 금지**: 모든 작업은 QA 에이전트의 검증을 거쳐야 합니다.
- **R2. Evidence 필수**: "완료"라고 말할 때는 반드시 증거(로그, 테스트 결과 등)를 제시해야 합니다.
- **R3. Fact Check**: 파일, 경로, 라이브러리를 사용하기 전에 반드시 존재 여부를 확인합니다.
- **R4. Output Contract**: 모든 출력은 `validation/output_contract.md`의 형식을 따릅니다.

## 4. 세션 복구

세션 시작 시 `HANDOVER.md`를 먼저 읽어 현재 상태를 복원합니다.
루트 `GEMINI.md`의 Session Startup Protocol에 정의된 순서를 따릅니다.
