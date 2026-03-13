# Validation Policy

## 이 문서의 목적

**Verify 없는 완료 선언 금지** 원칙의 설계 근거, 정책 상세, 운영 가이드를 설명한다.  
검증 게이트 원문은 `workflows/verification_gate.md`에,  
품질 체크리스트는 `validation/quality_checklist.md`에 있다.

---

## 왜 검증 정책이 필요한가

| 문제 | 결과 |
|------|------|
| builder가 자체 "완료" 선언 | 결함이 사용자에게 도달 |
| Evidence 없이 "잘 작동함" | 검증 불가능, 재현 불가능 |
| 검증 건너뛰기 "이번만" | 관행화되어 품질 기준 붕괴 |
| 검증 기준 미합의 | 검증자마다 판단이 달라짐 |

---

## 핵심 정책

### VP1. 독립 검증 원칙

구현자가 아닌 독립 검증자(quality_verification_agent)가 검증한다.  
**SELF-CHECKED는 중간 상태일 뿐 최종 완료로 인정되지 않는다.**

### VP2. Evidence 기반 판정

모든 판정은 Evidence에 근거한다.  
주관적 판단("괜찮아 보인다")이 아니라 객관적 근거(빌드 로그, 테스트 결과)에 기반한다.

### VP3. 게이트 우회 금지

| 시나리오 | 허용 여부 |
|----------|-----------|
| "급해서 검증 생략" | ❌ |
| "사소한 변경이라 불필요" | ❌ |
| "이미 테스트 많이 했다" | ❌ (SELF-CHECKED일 뿐) |
| "문서만 수정했다" | ⚠️ another_me 직접 리뷰로 대체 가능 (Evidence 필수) |

### VP4. 판정 등급

| 등급 | 의미 | 후속 |
|------|------|------|
| **PASS** | 모든 조건 충족 | CLOSED 가능 |
| **CONDITIONAL PASS** | 사소한 미충족 1–2개 | 후속 WO로 처리, CLOSED 가능 |
| **FAIL** | 필수 조건 미충족 | IN-PROGRESS로 반환 |

### VP5. 건설적 피드백

FAIL 판정 시 다음을 포함한다:
- 무엇이 잘못되었는가
- 어떻게 수정해야 하는가
- 어떤 Evidence가 추가로 필요한가

단순 "불합격" 판정은 허용되지 않는다.

---

## 검증 유형별 기준

### 코드 작업

- 빌드 성공 여부
- 기존 테스트 통과 여부
- 새 기능 테스트 존재 여부
- 코딩 컨벤션 준수
- 보안 기본 검토

### 설계 작업

- 결정 근거 명시
- 영향 범위 식별
- 트레이드오프 설명 여부

### 배포 작업

- 배포 성공 여부
- Smoke test 통과
- 롤백 가능 여부 확인

상세 체크리스트: `validation/quality_checklist.md`

---

## 참조 파일

- 검증 게이트: `workflows/verification_gate.md`
- 품질 체크리스트: `validation/quality_checklist.md`
- 출력 계약: `validation/output_contract.md`

---

## 버전

- v1.0 — 초기 정의
