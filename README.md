# Multi-Agent Router Project (Codex + Gemini)

이 프로젝트는 `another_me`를 오케스트레이터로 두고,  
Codex CLI와 Gemini CLI에서 동일한 멀티에이전트 운영 프레임을 재사용하기 위한 템플릿입니다.

## 0. Path Convention
- 이 문서(`README.md`)의 경로 표기는 **repo-root-relative** 기준이다.
- 예: `codex_cli_multi_agent/AGENTS.md`, `gemini_cli_multi_agent/README.md`
- 각 패키지 내부 문서(`codex_cli_multi_agent/README.md`, `gemini_cli_multi_agent/README.md`)는 **package-root-relative** 기준을 사용한다.

### 복사 배포 시 경로 기준 분리
- 이 저장소는 템플릿 저장소다.
- 실제 프로젝트에 복사한 뒤에는 `.codex/`, `.gemini/`를 **대상 프로젝트 루트** 기준으로 배치해 운영한다.

## 0.1 Official Runtime Contracts

| Tool | Official Entry Points | Notes |
|------|------------------------|-------|
| Codex CLI | `AGENTS.md`, `.codex/config.toml` | `profiles`, `skills`, multi-agent는 필요 시 선택 |
| Gemini CLI | `GEMINI.md`, `.gemini/settings.json` | `extensions`, subagents, skills는 필요 시 선택 |

## 1. 핵심 개념
- 관리자 에이전트: `another_me`
- 실행 단위: `Milestone > Phase > Work Order`
- 원칙:
  - Verify 없는 완료 금지
  - Evidence 없는 완료 보고 금지
  - 개발 단계에서는 필요한 서브에이전트만 선택 위임

## 2. 폴더 구성
- `AGENTS.md`: 루트 라우터
- `codex_cli_multi_agent/`: Codex 전용 에이전트/워크플로우/설정 패키지
- `gemini_cli_multi_agent/`: Gemini 전용 에이전트/워크플로우/설정 패키지

프로젝트에 적용 시 권장 공존 배치:
- `codex_cli_multi_agent/`
- `gemini_cli_multi_agent/`
- `.codex/`
- `.gemini/`

## 3. 실행 라우팅 규칙
- Codex 세션:
  - `codex_cli_multi_agent/AGENTS.md` 기준으로 실행
  - `codex_cli_multi_agent/*`, `.codex/*` 중심 참조
- Gemini 세션:
  - `gemini_cli_multi_agent/AGENTS.md` 기준으로 실행
  - `gemini_cli_multi_agent/*`, `.gemini/*` 중심 참조
- 한 세션에서 Codex/Gemini 에이전트 정의를 섞지 않음

## 4. 시작 프롬프트

### 4.1 Codex CLI
```text
codex_cli_multi_agent/AGENTS.md를 기준으로 동작해.
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 서브에이전트에 위임해.
독립 가능한 작업은 병렬로 처리하고, 모든 중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
Execution Mode와 Parallel Evidence를 보고서에 포함해.
```

### 4.2 Gemini CLI
```text
gemini_cli_multi_agent/AGENTS.md 기준으로 동작해.
너는 another_me 관리자다.
지금 프로젝트의 사용 가능한 커스텀 서브에이전트 리스트 작성해줘.
그리고 내 요청을 Work Order로 분해해서 필요한 서브에이전트에게만 위임해.
중간/최종 보고는 another_me인 네가 통합해서 나에게만 보고해.
```

## 5. Codex 실행 모드
- `Mode A: Logical Multi-Agent`
  - 단일 런타임 기반 역할 분해/통합
- `Mode B: True Parallel (or near-parallel)`
  - Codex multi-agent 기능 활성화 필요
  - 필수: `Execution Mode`, `Parallel Evidence` 보고

활성화 체크:
1. `.codex/config.toml`에 `features.multi_agent = true`
2. Codex `/experimental`에서 Multi-agents ON
3. `/agent`로 에이전트 스레드 점검

## 6. 세션 복구 운영
Codex 기준 필수 파일 3개:
1. `codex_cli_multi_agent/docs/project_packets/<project>.md`
2. `codex_cli_multi_agent/tasks/<current_work_order>.md`
3. `codex_cli_multi_agent/docs/session_handoff.md`

세션 재시작 시:
- 위 3개를 먼저 로드하고 현재 상태 복구 후 작업 재개

## 7. 추천 운영 순서
1. Scope/Architecture 정리
2. Phase 단위 구현 (`Phase 1개 = Commit 1개`)
3. QA 검증 + Evidence 확보
4. `session_handoff` 갱신 후 다음 세션 연결

## 8. 상세 문서
- Codex 운영 가이드: `codex_cli_multi_agent/README.md`
- Gemini 운영 가이드: `gemini_cli_multi_agent/README.md`
- 라우터 규칙: `AGENTS.md`

## 9. 30초 체크리스트 (복붙용)

### Codex 시작 전
- [ ] 현재 세션이 Codex인지 확인
- [ ] `codex_cli_multi_agent/AGENTS.md` 경로로 시작 프롬프트 입력
- [ ] (Mode B 사용 시) `/experimental` -> Multi-agents ON 확인
- [ ] `% left` 확인 후 전달

### Gemini 시작 전
- [ ] 현재 세션이 Gemini인지 확인
- [ ] `gemini_cli_multi_agent/AGENTS.md` 경로로 시작 프롬프트 입력
- [ ] `.gemini`가 프로젝트 루트 기준으로 유효한지 확인

### 공통 운영
- [ ] 한 세션에서 Codex/Gemini 에이전트 정의 혼용 금지
- [ ] `another_me` 단일 보고 창구 유지
- [ ] 개발 단계는 필요한 서브에이전트만 선택 위임
- [ ] 보고서에 Evidence 포함

### Codex 세션 복구
- [ ] `codex_cli_multi_agent/docs/project_packets/<project>.md` 로드
- [ ] `codex_cli_multi_agent/tasks/<current_work_order>.md` 로드
- [ ] `codex_cli_multi_agent/docs/session_handoff.md` 로드
- [ ] 복구 후 다음 작업 1개 확정

### 종료 전
- [ ] WO 상태 업데이트
- [ ] session_handoff 업데이트
- [ ] (해당 시) Phase 완료 커밋
