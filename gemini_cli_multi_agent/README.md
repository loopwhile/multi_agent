# Gemini CLI Multi-Agent Setup Guide

이 폴더는 Gemini CLI에서 멀티/서브에이전트를 운영하기 위한 독립 패키지다.

## 0. Path Convention
- 이 문서의 경로 표기는 **package-root-relative** 기준이다.
- 예: `AGENTS.md`, `roles/`, `.gemini/agents/`

### 복사 배포 시 경로 기준 분리
- 이 폴더를 실제 프로젝트로 복사하면, Gemini 런타임 핵심 파일은 **대상 프로젝트 루트** 기준으로 관리한다.
- `GEMINI.md` (프로젝트 컨텍스트)
- `.gemini/settings.json` (워크스페이스 설정; 필요 시 사용자 전역 `~/.gemini/settings.json`을 override)

## 0.1 Gemini Runtime Contract (Official Alignment)
- Gemini CLI의 프로젝트 진입점은 `GEMINI.md`와 `.gemini/settings.json`이다.
- 이 패키지(`gemini_cli_multi_agent/*`)는 역할/워크플로우/검증 문서를 제공하는 운영 레이어다.
- 고급 확장(extensions, subagents, skills, hooks)은 필요 시 별도 활성화한다.

## 1. 폴더 구조
- `AGENTS.md`: 서브 에이전트 정의
- `SKILL.md`: 스킬 정의
- `.gemini/agents/`: Gemini 전용 에이전트 정의
- `roles/`, `workflows/`, `tasks/`, `validation/`, `docs/`: 공용 코어 문서
- `adapters/`: 도구별 매핑 문서

## 2. Gemini + Codex 공존 배치 (권장)
프로젝트 루트에 아래 4개를 동시에 둔다.
- `codex_cli_multi_agent/`
- `gemini_cli_multi_agent/`
- `.codex/` (Codex 전용)
- `.gemini/` (Gemini 전용)

주의:
- Gemini 세션에서는 `gemini_cli_multi_agent/*`만 참조하도록 명시한다.
- Codex 세션에서는 `codex_cli_multi_agent/*`만 참조하도록 명시한다.
- 자동 탐지에만 의존하지 말고, 시작 프롬프트에서 항상 경로를 명시한다.

## 2.1 루트 AGENTS 라우터 템플릿 (선택)
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

### 3.1 세션 시작 프롬프트 (Gemini)
아래를 그대로 입력:

```text
gemini_cli_multi_agent/AGENTS.md 기준으로 동작해.
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 필요한 서브에이전트에게만 위임해.
중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
```

### 3.2 서브에이전트 호출 예시
- `@builder_agent 코드 구현을 시작해줘.`
- `@system_architect_agent 현재 구조를 리뷰해줘.`
- `@quality_verification_agent 결과를 검증해줘.`

### 3.3 스킬 실행 예시
- `/skill-activate quality-check`
- `scope-lock 스킬로 요구사항 정의서를 작성해줘.`

## 4. 공존 모드 운영 규칙
- 개발 단계에서 모든 서브에이전트 일괄 호출 금지
- `another_me`가 업무 성격에 따라 필요한 에이전트만 선택 위임
- 보고 시 Evidence 포함
- 작업 추적은 Work Order 문서(`tasks/`) 기준으로 유지

## 5. 핵심 규칙
모든 에이전트는 `docs/03_rules.md`를 우선 준수한다.
- **R1. Verify 없는 완료 금지**
- **R2. Evidence 필수**
