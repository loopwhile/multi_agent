# Verification Gate

## 목적

**Verify 없는 완료 선언을 시스템적으로 금지**하는 게이트 정의.  
모든 Work Order는 이 게이트를 통과해야만 CLOSED 상태로 전이된다.

---

## 게이트 통과 조건

Work Order가 VERIFIED 또는 DEPLOYED 상태로 전이되려면 다음을 **모두** 충족해야 한다:

### 1. Evidence 존재

- 작업 결과를 증명하는 구체적 근거가 제출되어야 한다
- 허용되는 Evidence 유형:
  - 빌드 로그 / 테스트 결과
  - 스크린샷 / 화면 녹화
  - API 응답 결과
  - diff / 코드 변경 요약
  - 배포 로그
  - 수동 검증 절차 + 결과

### 2. 독립 검증자에 의한 검증

- 구현자 자신이 아닌 **quality_verification_agent** 또는 **another_me**가 검증
- `SELF-CHECKED`는 중간 상태로만 허용, 최종 게이트 통과 불가

### 3. Quality Checklist 통과

- `ai_prompts/validation/quality_checklist.md`의 해당 유형 체크리스트를 기준으로 판정
- 판정 결과: PASS, CONDITIONAL PASS, FAIL

### 4. 수락 기준 충족

- Work Order에 명시된 Acceptance Criteria가 모두 충족
- 부분 충족은 CONDITIONAL PASS로 허용하되, 미충족 항목을 명시해야 함

---

## 게이트 판정 결과

| 판정 | 후속 상태 | 설명 |
|------|-----------|------|
| **PASS** | → VERIFIED | 모든 조건 충족. CLOSE 가능 |
| **CONDITIONAL PASS** | → VERIFIED (조건부) | 사소한 미충족 1–2개. 후속 Work Order로 처리 |
| **FAIL** | → FAILED | 필수 조건 미충족. IN-PROGRESS로 돌려보냄 |

---

## 게이트 우회 금지

| 금지 시나리오 | 이유 |
|---------------|------|
| builder가 자체 VERIFIED 선언 | 독립 검증 원칙 위반 |
| Evidence 없이 "잘 동작함"으로 PASS | 검증 불가능 |
| 시간 부족으로 검증 생략 | 품질 부채를 만듦 |
| "사소한 변경이니 검증 불필요" 주장 | 범위와 무관하게 게이트 필수 |

### 유일한 예외

- **문서 전용 변경** (코드 변경 없음): another_me가 직접 리뷰하여 VERIFIED 가능
- 이 경우에도 Evidence (diff 또는 변경 요약)는 필수

---

## 검증 보고서 형식

검증 완료 시 quality_verification_agent는 다음 형식으로 보고한다:

```markdown
## Verification Report

### Work Order
{WO-ID}

### 검증 항목

| 항목 | 결과 | 근거 |
|------|------|------|
| Output Contract 준수 | PASS / FAIL | {근거} |
| Evidence 유효성 | PASS / FAIL | {근거} |
| 수락 기준 충족 | PASS / FAIL | {근거} |
| 품질 체크리스트 | PASS / FAIL | {근거} |

### 최종 판정
{PASS / CONDITIONAL PASS / FAIL}

### 사유
{판정 근거}

### 미충족 항목 (CONDITIONAL PASS 시)
{후속 처리 필요 항목}
```

---

## 참조

- 품질 기준: `ai_prompts/validation/quality_checklist.md`
- 출력 형식: `ai_prompts/validation/output_contract.md`
- 상태 전이: `ai_prompts/workflows/work_order_lifecycle.md`

---

## 버전

- v1.0 — 초기 정의
