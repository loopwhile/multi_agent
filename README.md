# Multi-Agent Project (Codex CLI + Gemini CLI)

이 프로젝트는 `another_me`를 오케스트레이터로 두고,
Codex CLI와 Gemini CLI에서 동일한 멀티에이전트 운영 프레임을 재사용하기 위한 템플릿입니다.

## 프로젝트에 적용하기

### 복사 대상 (필수)

아래 파일/폴더를 **대상 프로젝트 루트**에 복사합니다:

```
AGENTS.md                  ← Codex CLI 엔트리 포인트
GEMINI.md                  ← Gemini CLI 엔트리 포인트
.codex/                    ← Codex CLI 설정
.gemini/                   ← Gemini CLI 설정
.geminiignore              ← Gemini CLI 격리 설정
codex_cli_multi_agent/     ← Codex 에이전트 코어 문서
gemini_cli_multi_agent/    ← Gemini 에이전트 코어 문서
```

> **Codex CLI만 사용**: `AGENTS.md`, `.codex/`, `codex_cli_multi_agent/` 만 복사
> **Gemini CLI만 사용**: `GEMINI.md`, `.gemini/`, `.geminiignore`, `gemini_cli_multi_agent/` 만 복사

### 복사 제외

| 파일 | 이유 |
|------|------|
| `README.md` | 이 템플릿 저장소의 가이드 문서 (대상 프로젝트의 README와 충돌) |
| `LICENSE` | 대상 프로젝트의 라이선스를 따름 |
| `.git/` | 이 저장소의 Git 히스토리 |

### 복사 후 해야 할 것

1. `.codex/config.toml` — 사용할 모델명, API 설정 확인
2. `.gemini/settings.json` — MCP 서버 등 환경 설정 확인
3. Codex CLI 사용 시 — **부트스트랩 프롬프트**로 프로젝트 패킷 초기화 (`codex_cli_multi_agent/README.md` 참고)
4. Gemini CLI 사용 시 — `gemini_cli_multi_agent/HANDOVER.md`에 프로젝트 정보 기입

## 공식 런타임 규약 (Official Runtime Contracts)

| Tool | Entry Point | Config | 비고 |
|------|------------|--------|------|
| **Codex CLI** | 루트 `AGENTS.md` | `.codex/config.toml` | 프로젝트 루트의 `AGENTS.md`를 자동 로드 |
| **Gemini CLI** | 루트 `GEMINI.md` | `.gemini/settings.json` | 프로젝트 루트의 `GEMINI.md`를 자동 로드 |

> 각 CLI는 자기 엔트리 파일만 자동으로 인식하므로, 별도 라우터가 필요 없습니다.

## 폴더 구조

```
├── AGENTS.md                    ← Codex CLI 자동 로드 (엔트리 포인트)
├── GEMINI.md                    ← Gemini CLI 자동 로드 (엔트리 포인트)
├── .codex/                      ← Codex CLI 설정 (config.toml, agents/*.toml)
├── .gemini/                     ← Gemini CLI 설정 (settings.json)
├── .geminiignore                ← Gemini CLI에서 Codex 관련 파일 제외
├── codex_cli_multi_agent/       ← Codex 전용 에이전트 패키지 (roles, tasks, docs, workflows)
├── gemini_cli_multi_agent/      ← Gemini 전용 에이전트 패키지 (roles, tasks, docs, workflows)
└── README.md
```

## 격리 전략 (Ignore Files)

| 파일 | 역할 |
|------|------|
| `.geminiignore` | Gemini CLI 실행 시 `.codex/`, `codex_cli_multi_agent/` 제외 |

- **Gemini CLI**: `.geminiignore`로 Codex 관련 파일을 완전 격리
- **Codex CLI**: 공식 ignore 메커니즘이 없음. 단, Codex CLI는 `AGENTS.md`만 자동 로드하므로 `GEMINI.md`를 인식하지 않음

## 핵심 개념

- **관리자 에이전트**: `another_me` (오케스트레이터)
- **실행 단위**: `Milestone > Phase > Work Order`
- **불변 원칙**:
  - Verify 없는 완료 금지
  - Evidence 없는 완료 보고 금지
  - 개발 단계에서는 필요한 서브에이전트만 선택 위임

## 빠른 시작

### Codex CLI

```text
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 서브에이전트에 위임해.
독립 가능한 작업은 병렬로 처리하고, 모든 중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
Execution Mode와 Parallel Evidence를 보고서에 포함해.
```

> Codex CLI는 루트 `AGENTS.md`를 자동 로드하므로 별도 경로 지정이 불필요합니다.

### Gemini CLI

```text
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 필요한 서브에이전트에게만 위임해.
중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
```

> Gemini CLI는 루트 `GEMINI.md`를 자동 로드하므로 별도 경로 지정이 불필요합니다.

## Codex 실행 모드

- **Mode A: Logical Multi-Agent** — 단일 런타임에서 역할 분해/통합
- **Mode B: True Parallel** — Codex multi-agent 기능 활성화 필요

Mode B 활성화:
1. `.codex/config.toml`에 `features.multi_agent = true` (설정 완료)
2. Codex CLI에서 `/experimental` → Multi-agents ON
3. `/agent`로 에이전트 스레드 점검

## 상세 문서

| 문서 | 위치 |
|------|------|
| Codex 운영 가이드 | `codex_cli_multi_agent/README.md` |
| Gemini 운영 가이드 | `gemini_cli_multi_agent/README.md` |
| 시스템 규칙 (Codex) | `codex_cli_multi_agent/docs/03_rules.md` |
| 시스템 규칙 (Gemini) | `gemini_cli_multi_agent/docs/03_rules.md` |

## 30초 체크리스트

### Codex 시작 전
- [ ] `.codex/config.toml` 확인
- [ ] (Mode B) `/experimental` → Multi-agents ON
- [ ] 시작 프롬프트 입력

### Gemini 시작 전
- [ ] `.gemini/settings.json` 확인
- [ ] 시작 프롬프트 입력

### 공통
- [ ] 한 세션에서 Codex/Gemini 에이전트 혼용 금지
- [ ] `another_me` 단일 보고 창구 유지
- [ ] 보고서에 Evidence 포함
