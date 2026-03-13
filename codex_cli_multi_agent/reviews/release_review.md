# Review: Release Review

## 목적

배포 전 최종 점검. 검증 통과 여부, 배포 준비 상태, 롤백 계획을 확인한다.

## 리뷰어

- **주 리뷰어**: another_me 또는 product_scope_agent
- **보조**: operations_workflow_agent

## 리뷰 대상

- Work Order (VERIFIED 상태)
- Verification Report
- 배포 계획

## 체크리스트

### 검증 상태

- [ ] Verification Report의 판정이 PASS 또는 CONDITIONAL PASS인가
- [ ] CONDITIONAL PASS인 경우 미충족 항목이 배포에 영향 없는가
- [ ] 모든 Acceptance Criteria가 충족되었는가

### 배포 준비

- [ ] 배포 대상 환경이 확인되었는가
- [ ] 환경 변수 / 설정이 준비되었는가
- [ ] 데이터 마이그레이션 필요 여부 확인

### 리스크

- [ ] 롤백 계획이 수립되었는가
- [ ] 배포 실패 시 영향 범위가 파악되었는가
- [ ] 사용자 영향이 평가되었는가

## 판정

| 판정 | 후속 |
|------|------|
| **GO** | 배포 실행 허가 |
| **NO-GO** | 사유 기록 + 필요 조치 명시 |

## 출력

- Release decision (GO/NO-GO) + 근거 (Output Contract 형식)
