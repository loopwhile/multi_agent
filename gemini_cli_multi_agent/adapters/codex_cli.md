# Adapter: Codex CLI

## 1. 목적
터미널/CLI 전용인 **Codex CLI** 환경에서 Agent OS를 가동하기 위한 실행 지침서.

## 2. 읽기 순서 (초기화 단계)
터미널에서 명령을 내리기 전 프롬프트 컨텍스트에 포함시켜야 할 핵심 파일. 단, CLI는 컨텍스트 토큰 관리가 중요하므로 최소한만 읽는다.
1. `docs/03_rules.md`
2. `ai_prompts/roles/{현재역할}.md`
3. `docs/project_packets/{프로젝트명}.md`

## 3. 권장 실행 흐름
- CLI 환경에서는 파이프라인(`|`)과 스크립트 기반 연쇄 작업이 주가 된다.
- CLI 명령의 결과 도출 시, 불필요한 서술어를 빼고 명확한 Markdown 형태나 Diff 형태로 반환하라.
- 각 역할의 책임을 CLI 명령어의 개별 실행 단위(step)로 매핑하라.

## 4. Work Order 생성 & 진행 방식
- 터미널 텍스트 처리 도구(cat, echo 등)와 연계하여 새 WO 파일을 생성한다.
- 예: "새 WO 템플릿 기반으로 뼈대 만들어" 지시를 받으면 `ai_prompts/tasks/_template.md`의 형식을 출력하여 쉘 스트림 기반으로 파일 저장이 가능하게 리턴하라.

## 5. 필수 준수 경로
- **출력 계약**: `ai_prompts/validation/output_contract.md` (CLI 특성상 서식이 깨지지 않도록 Code block 내부에 작성 권장)
- **품질 체크리스트**: `ai_prompts/validation/quality_checklist.md`

## 6. 제한사항 / 워크어라운드
- **대화 내역 단절**: 세션 기반 상태유지가 어려울 수 있으므로, 이전 과정의 Context가 필요하다면 "이전 Work Order 파일(예: `.md` 파일)을 cat하여 컨텍스트로 제공하라"고 사용자나 랩퍼 스크립트에 지시해야 한다.

## 7. 금지사항
- 터미널에 곧바로 치명적 명령어(DB Drop, Force Push 등)를 추천하거나 사전 승인 없이 실행되게 하는 스크립트 생성을 엄금한다. `operations_workflow_agent` 라도 반드시 Handoff 리뷰 흔적을 남겨야 실행 권한을 가진다.
