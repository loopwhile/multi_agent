# Adapter: Gemini CLI

## 1. 목적
**Gemini CLI** 환경에서 Agent OS를 가동하기 위한 실행 지침서.

## 2. 읽기 순서 (초기화 단계)
Gemini CLI 명령어에 파일 플래그(`-f` 등)로 주입해야 할 기준 문서 목록.
1. `docs/03_rules.md` (불변 규칙)
2. `ai_prompts/roles/{현재역할}.md` (단일 역할로 한정)
3. 해당 작업 도메인의 프로젝트 패킷 파일

## 3. 권장 실행 흐름
- Gemini는 매우 긴 컨텍스트 윈도우를 지원하므로, 필요한 경우 프로젝트 내 여러 문서와 소스 코드를 한 번에 CLI 컨텍스트로 주입하여 판단의 정확성을 높일 수 있다.
- 역할 간 전환이 필요할 경우, CLI 세션을 새롭게 띄워 "다른 역할 프롬프트"를 로드하는 명시적인 롤 체인징 방식을 권장한다.

## 4. Work Order 생성 & 진행 방식
- `ai_prompts/tasks/_template.md` 파일을 참조하여, 터미널 표준 출력(STDOUT)으로 즉시 완성된 마크다운 WO 문서를 뱉어내게 한다.

## 5. 필수 준수 경로
- **출력 계약**: `ai_prompts/validation/output_contract.md`
- **의사결정 게이트**: `ai_prompts/validation/decision_gate.md`

## 6. 제한사항 / 워크어라운드
- **상호작용의 비동기성**: 생성 답변의 응답이 바로 적용되지 않고 텍스트로만 반환되는 CLI 특성상, `Next Steps` 섹션에 사용자가 직접 복사해서 쉘에 실행해야 할 명령어나 행동을 아주 구체적으로 기재해야 한다.

## 7. 금지사항
- 무의미한 서론/인사말 추가 불가. 오직 Output Contract에서 규정한 TL;DR 및 지정된 마크다운 섹션만 출력한다.
