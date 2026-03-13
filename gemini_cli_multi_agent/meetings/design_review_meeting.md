# Meeting: Design Review Meeting

## 목적

Architecture Design이 완료된 후 **설계 품질, 빌드 가능성, 리스크**를 팀 차원에서 검토한다.

## 참석 역할

- another_me (진행 / 최종 판정)
- system_architect_agent (설계 발표)
- builder_agent (빌드 가능성 피드백)
- quality_verification_agent (검증 가능성 확인)

## 트리거

- Work Order가 DESIGNED 상태 도달
- 아키텍처 대규모 변경 시

## 아젠다

### 1. 설계 발표 (10분)

- 시스템 구조 / 데이터 흐름
- 주요 기술 결정 (ADR)
- 트레이드오프

### 2. 빌드 가능성 검토 (10분)

- builder_agent: 구현 관점에서의 우려 사항
- 모호한 부분 / 결정이 부족한 부분

### 3. 검증 가능성 검토 (5분)

- quality_verification_agent: 테스트 전략 실현 가능성
- 검증 시 예상 어려운 부분

### 4. 리스크 논의 (5분)

- 기술 리스크
- 일정 리스크
- 의존성 리스크

### 5. 판정 (5분)

- APPROVED / REVISE / REJECTED (architecture_review.md 기준)

## 출력

```markdown
## Design Review Minutes

### 날짜: YYYY-MM-DD
### Work Order: {WO-ID}
### 설계 판정: APPROVED / REVISE / REJECTED

### 주요 피드백
- {피드백 1}
- {피드백 2}

### 수정 요청 (REVISE 시)
- {수정 1}
- {수정 2}

### Action Items
- [ ] {액션 1} — 담당: {역할}
```
