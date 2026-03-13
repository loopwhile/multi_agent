# Review: Workflow Review

## 목적

Agent OS의 워크플로우, 프로세스, 운영 규칙이 **효율적이고 일관적인지** 정기 리뷰한다.

## 리뷰어

- **주 리뷰어**: another_me
- **보조**: operations_workflow_agent

## 리뷰 대상

- `ai_prompts/workflows/` 전체
- `ai_prompts/validation/` 전체
- `docs/03_rules.md`

## 트리거

- 정기 리뷰 (프로젝트 마일스톤 완료 시)
- 프로세스 병목 감지
- 에스컬레이션 빈도 급증

## 체크리스트

### 워크플로우 효율성

- [ ] 불필요한 단계가 있는가
- [ ] 병목이 되는 단계가 있는가
- [ ] 자동화 가능한 단계가 있는가

### 규칙 일관성

- [ ] 실제 운영과 규칙 문서가 일치하는가
- [ ] 사문화된 규칙이 있는가
- [ ] 새로 필요한 규칙이 있는가

### 역할 경계

- [ ] 역할 간 책임 충돌이 발생하지 않았는가
- [ ] 에스컬레이션 빈도가 정상 범위인가
- [ ] 특정 역할에 병목이 집중되지 않았는가

### Verification Gate 효과

- [ ] 검증 게이트가 실제로 결함을 잡고 있는가
- [ ] 과도한 검증으로 속도 저하가 없는가
- [ ] Evidence 품질이 유지되고 있는가

## 판정

| 판정 | 후속 |
|------|------|
| **HEALTHY** | 현행 유지 |
| **ADJUST** | 구체적 개선 항목 + 변경 계획 |
| **RESTRUCTURE** | 구조적 변경 필요 → 별도 Work Order |

## 출력

- Workflow Review Report (Output Contract 형식)
