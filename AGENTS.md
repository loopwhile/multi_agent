# Router AGENTS

이 파일은 Codex CLI와 Gemini CLI를 같은 프로젝트에서 함께 사용할 때의 중립 라우터다.
자동 감지에만 의존하지 말고, 각 세션 시작 프롬프트에서 반드시 경로를 명시한다.

## Tool Routing

### If running in Codex CLI
- Follow `codex_cli_multi_agent/AGENTS.md`
- Read only `codex_cli_multi_agent/*` and `.codex/*` for orchestration
- Session start prompt:
  - `codex_cli_multi_agent/AGENTS.md 기준으로 동작해.`

### If running in Gemini CLI
- Follow `gemini_cli_multi_agent/AGENTS.md`
- Read only `gemini_cli_multi_agent/*` and `.gemini/*` for orchestration
- Session start prompt:
  - `gemini_cli_multi_agent/AGENTS.md 기준으로 동작해.`

## Non-Negotiable
- Do not mix Codex and Gemini agent definitions in a single run.
- 개발 단계에서는 `another_me`가 필요한 서브에이전트만 선택 위임한다.
- 보고서에는 Evidence를 포함한다.
