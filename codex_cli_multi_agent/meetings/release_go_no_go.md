# Meeting: Release Go/No-Go

## 목적

배포 전 최종 **Go / No-Go 결정**을 내린다.

## 참석 역할

- another_me (최종 판정)
- product_scope_agent (비즈니스 판단)
- quality_verification_agent (검증 결과 보고)
- operations_workflow_agent (배포 준비 상태 보고)

## 트리거

- Work Order가 VERIFIED 상태이고 배포 예정일 때

## 아젠다

### 1. 검증 결과 보고 (5분)

- Verification Report 요약
- PASS / CONDITIONAL PASS 여부
- 미충족 항목 (있는 경우)

### 2. 배포 준비 상태 (5분)

- 환경 준비 상태
- 롤백 계획
- 모니터링 준비

### 3. 비즈니스 판단 (5분)

- 배포 시점의 적절성
- 사용자 영향 평가
- 커뮤니케이션 준비

### 4. 리스크 최종 확인 (5분)

- 기술 리스크
- 비즈니스 리스크
- 롤백 시나리오

### 5. 최종 판정 (5분)

- **GO**: 배포 실행 허가 → operations_workflow_agent 실행
- **NO-GO**: 사유 기록 + 필요 조치 + 재검토 시점

## 출력

```markdown
## Release Decision

### 날짜: YYYY-MM-DD
### Work Order: {WO-ID}
### 판정: GO / NO-GO

### 근거
- {근거 1}

### 조건 (GO with conditions인 경우)
- {조건 1}

### NO-GO 사유 (해당 시)
- {사유 1}
- 재검토 시점: {날짜/조건}
```
