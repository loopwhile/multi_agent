# Adapter: Antigravity

## 1. 목적
Google Deepmind의 Advanced Agentic Coding 시스템인 **Antigravity** 환경에서 Agent OS를 로드하고 가동하기 위한 실행 지침서.

## 2. 읽기 순서 (초기화 단계)
Antigravity에서 새 세션을 시작할 때, 다음 순서로 공용 문서를 파악하라.
1. `docs/00_overview.md` (체계 구조 이해)
2. `docs/03_rules.md` (절대 규칙 숙지)
3. `docs/project_packets/{프로젝트명}.md` (현재 작업할 프로젝트 맥락)
4. 현재 담당할 역할 프롬프트 (`ai_prompts/roles/*.md`)
5. **이 문서 (antigravity.md)**

## 3. 권장 실행 흐름
- Antigravity는 자율적인 파일 읽기, 쓰기, 터미널 실행, 웹 브라우저 제어가 가능하다.
- `ai_prompts/workflows/*.md`에 정의된 흐름을 따라, 필요한 문서를 자율적으로 식별하고 `view_file` 도구로 스스로 읽어들여라.
- 사용자에게 일일히 문서를 읽어도 되냐고 묻지 말고, 필요하면 스스로 읽은 뒤 행동해라.

## 4. Work Order 생성 & 진행 방식
- 사용자가 새로운 기능을 요구하면, `ai_prompts/tasks/scope_lock.md` 절차를 따른다.
- 새로운 Work Order 생성 시 `ai_prompts/tasks/_template.md` 포맷을 활용하여 물리적인 markdown 파일을 `task_wo_xxx.md` 형태로 프로젝트 트래킹 디렉토리에 직접 생성(`write_to_file`)하라.
- Antigravity의 기본 Agentic task boundaries 기능과 Work Order의 상태 변경을 동기화하라.

## 5. 필수 준수 경로
결과물 도출 및 보고 전 아래 문서를 반드시 적용할 것. (복제 금지, 직접 읽을 것)
- **출력 서식**: `ai_prompts/validation/output_contract.md`
- **검증 기준**: `ai_prompts/validation/quality_checklist.md`

## 6. 제한사항 / 워크어라운드
- **컨텍스트 오염 방지**: 대화가 매우 길어질 경우 이전 단계의 Work Order 상태를 잊어버릴 수 있다. 중요한 상태 변경이 일어날 때마다 `notify_user`나 마크다운 파일 업데이트를 통해 상태를 명시적으로 디스크에 기록하라.

## 7. 금지사항
- 어떠한 경우에도 증거(Evidence) 없이 Work Order를 `VERIFIED` 상태로 전이하지 마라.
- 역할 프롬프트(`ai_prompts/roles`)에 정의된 경계를 넘는 판단을 독단적으로 하지 마라 (항상 에스컬레이션 원칙 준수).
