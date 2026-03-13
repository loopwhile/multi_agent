# Task: Release Readiness

## 목적

검증 통과된 빌드가 배포 가능한 상태인지 최종 확인하고 배포를 실행한다.

## 담당 역할

- **주 담당**: operations_workflow_agent
- **승인**: another_me 또는 product_scope_agent
- **협력**: quality_verification_agent (검증 확인)

## 트리거

- Work Order가 VERIFIED 상태일 때

## 입력

- Work Order (VERIFIED 상태)
- Verification Report
- 배포 환경 정보

## 절차

### 1. 배포 전 점검

- [ ] Verification Report의 최종 판정이 PASS인가
- [ ] 모든 Acceptance Criteria가 충족되었는가
- [ ] 기존 기능에 대한 regression 없는가
- [ ] 환경 변수 / 설정이 준비되었는가

### 2. 배포 계획

- [ ] 배포 대상 환경 확인 (staging / production)
- [ ] 배포 순서 결정 (staging → production)
- [ ] 롤백 계획 수립

### 3. Go / No-Go 판정

- [ ] 배포 승인자(another_me 또는 product_scope_agent) 판정
- [ ] Go → 배포 실행 / No-Go → 사유 기록 + IN-PROGRESS 반환

### 4. 배포 실행

- [ ] 배포 스크립트 / CI 실행
- [ ] Smoke test 실행
- [ ] 모니터링 지표 확인

### 5. 배포 후 기록

- [ ] 배포 결과를 Work Order에 기록
- [ ] 상태: VERIFIED → DEPLOYED → CLOSED

## 출력

- **산출물**: Release Report + 배포 결과
- **형식**: `ai_prompts/validation/output_contract.md` 준수

## 수락 기준

- [ ] 배포가 성공적으로 완료되었는가
- [ ] Smoke test가 통과되었는가
- [ ] 롤백 가능 여부가 확인되었는가
- [ ] Work Order가 CLOSED 상태로 전이되었는가

## 참조

- Work Order 생명주기: `ai_prompts/workflows/work_order_lifecycle.md`
- Verification Gate: `ai_prompts/workflows/verification_gate.md`
