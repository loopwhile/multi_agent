# Skills for Gemini CLI

이 파일은 Gemini CLI에서 즉시 실행 가능한 특정 업무 단위(Skill)를 정의합니다.

## Available Skills

### scope-lock (Product Definition)
- **Description**: 현재 프로젝트의 요구사항을 분석하고 고정된 제품 범위를 정의합니다.
- **Instructions**:
  - `docs/03_rules.md`
  - `tasks/scope_lock.md`
- **Output**: 마크다운 형식의 Scope Lock 문서

### architecture-design (System Design)
- **Description**: 기술적 요구사항을 분석하고 시스템 아키텍처 설계를 수행합니다.
- **Instructions**:
  - `docs/03_rules.md`
  - `tasks/architecture_design.md`
- **Output**: 아키텍처 다이어그램 및 설계 문서

### implementation-plan (Development Strategy)
- **Description**: 설계 문서를 바탕으로 실제 코드 구현을 위한 상세 단계를 수립합니다.
- **Instructions**:
  - `docs/03_rules.md`
  - `tasks/implementation_plan.md`
- **Output**: Step-by-step 구현 계획서

### quality-check (Verification & QA)
- **Description**: 결과물이 인수 기준(Acceptance Criteria)을 만족하는지 검증합니다.
- **Instructions**:
  - `docs/03_rules.md`
  - `validation/quality_checklist.md`
- **Output**: Pass/Fail 검증 보고서 및 Evidence 요약

### feedback-analysis (Growth Analysis)
- **Description**: 운영 데이터 및 사용자 피드백을 분석하여 다음 개선 과제를 도출합니다.
- **Instructions**:
  - `docs/03_rules.md`
  - `tasks/feedback_analysis.md`
- **Output**: 인사이트 요약 및 신규 Work Order 제안
