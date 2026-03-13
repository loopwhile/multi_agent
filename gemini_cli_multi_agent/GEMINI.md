# Gemini Project Context Template

이 파일은 Gemini CLI의 프로젝트 컨텍스트 진입점이다.

## Runtime Contract
- Workspace settings: `.gemini/settings.json`
- User settings: `~/.gemini/settings.json`
- 같은 키가 있을 때 workspace 설정이 우선한다.

## Session Start
1. `gemini_cli_multi_agent/AGENTS.md`를 읽는다.
2. 현재 프로젝트의 Work Order와 관련 문서를 로드한다.
3. 필요한 서브에이전트만 선택 위임한다.

## Non-Negotiable
- Verify 없는 완료 금지
- Evidence 없는 완료 보고 금지
- 한 세션에서 Codex/Gemini 에이전트 정의 혼용 금지
