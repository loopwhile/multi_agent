# Adapter: Codex App

## 1. 목적
Cursor, VSCode 기반의 **Codex App (GUI 기반 AI 어시스턴트)** 환경에서 Agent OS를 로드하고 가동하기 위한 실행 지침서.

## 2. 읽기 순서 (초기화 단계)
Codex App의 Chat이나 Composer에서 새 대화를 시작할 때, 명시적으로 아래 문서를 Context(`@`)로 불러와라.
1. `@docs/00_overview.md`
2. `@docs/03_rules.md`
3. 현재 담당할 역할 `@roles/{역할}.md`
4. 프로젝트 패킷 `@docs/project_packets/{프로젝트명}.md`

## 3. 권장 실행 흐름
- GUI 기반이므로 사용자(another_me)와의 핑퐁 대화가 잦다. 
- 복잡한 작업은 Composer 모드에서 실행하여 여러 파일을 동시에 제안(Diff)하라.
- 1회의 프롬프팅으로 너무 많은 단계(기획→설계→구현)를 건너뛰려 하지 말고, `workflows`에 정의된 Gate 단위로 멈추고 승인을 요청하라.

## 4. Work Order 생성 & 진행 방식
- 사용자가 "WO 하나 만들어줘"라고 지시하면 `@tasks/_template.md`를 참고하여 채팅창에 작성해 주거나 지정된 파일에 저장한다.
- 상태 변경 시나리오: 코드 수정 제안 전반에 걸쳐 해당 WO 파일의 상태값을 직접 수정하는 사항을 패치에 포함시켜라.

## 5. 필수 준수 경로
출력물을 작성하기 전 다음 파일들을 반드시 참조하라.
- **출력 계약**: `validation/output_contract.md`
- **에스컬레이션**: `workflows/escalation_protocol.md`

## 6. 제한사항 / 워크어라운드
- **자동 검증의 한계**: Codex App 자체적으로 테스트 코드를 백그라운드에서 직접 돌려주는 기능이 제한적일 수 있다. 따라서 Evidence를 확보하기 위해 "사용자에게 `npm run test`를 터미널에서 실행해 결과를 붙여넣어 달라고 요청"하는 단계를 Work Order 검증 단계로 삼아라.

## 7. 금지사항
- `SELF-CHECKED` 상태이면서 검증자의 승인 없이 WO를 CLOSED로 마킹하도록 코드를 제안하지 마라.
