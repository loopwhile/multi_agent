# Meeting: Build Sync Meeting

## 목적

구현 진행 중 **진척 상황, 차단 요소, 설계 변경 필요성**을 동기화한다.

## 참석 역할

- builder_agent (진행 보고)
- system_architect_agent (설계 이슈 해결)
- product_scope_agent (스코프 이슈 발생 시)

## 트리거

- Work Order가 IN-PROGRESS 상태일 때
- 주기적 (장기 작업 시) 또는 차단 요소 발생 시

## 아젠다

### 1. 진척 보고 (5분)

- 완료된 태스크
- 진행 중인 태스크
- 잔여 태스크

### 2. 차단 요소 (10분)

- 기술적 차단 요소
- 설계 모호함 / 결정 필요 사항
- 외부 의존성 대기

### 3. 설계 조정 (필요 시)

- 구현 중 발견된 설계 결함
- 대안 제시 → architect 판단

### 4. 스코프 확인 (필요 시)

- 구현 중 발견된 스코프 문제
- 스코프 변경 필요 → product_scope 판단

## 출력

```markdown
## Build Sync Minutes

### 날짜: YYYY-MM-DD
### Work Order: {WO-ID}

### 진척률: {N}%

### 차단 요소
- {차단 1} — 상태: 해결 / 대기

### 결정 사항
- {결정 1}

### Action Items
- [ ] {액션 1} — 담당: {역할}
```
