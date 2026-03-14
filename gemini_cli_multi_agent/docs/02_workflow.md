# Workflow

## 이 문서의 목적

Agent OS의 **표준 운영 워크플로우**를 설명한다.  
실행 세부사항은 `ai_prompts/workflows/`에 있으며, 이 문서는 전체 흐름의 개요와 설계 근거를 다룬다.

---

## 핵심 워크플로우

### 1. Work Order Lifecycle

모든 작업은 Work Order 단위로 관리된다.

```
사용자 요청
  → product_scope: CREATED
    → system_architect: DESIGNED
      → builder: IN-PROGRESS → PENDING-VERIFY
        → quality_verification: VERIFIED / FAILED
          → operations: DEPLOYED (선택)
            → CLOSED
```

**규칙:**
- 모든 상태 전이는 `ai_prompts/workflows/work_order_lifecycle.md`의 규칙을 따른다
- VERIFY를 거치지 않는 CLOSE는 존재하지 않는다

**상세**: `ai_prompts/workflows/work_order_lifecycle.md`

---

### 2. Verification Gate

모든 Work Order 결과물은 **독립 검증**을 거친다.

| 조건 | 설명 |
|------|------|
| Evidence 존재 | 빌드 로그, 테스트 결과 등 구체적 근거 |
| 독립 검증자 | 구현자가 아닌 quality_verification_agent가 판정 |
| Quality Checklist 통과 | 유형별 체크리스트 항목 충족 |
| 수락 기준 충족 | Work Order에 명시된 Acceptance Criteria |

**상세**: `ai_prompts/workflows/verification_gate.md`

---

### 3. Escalation Protocol

역할 간 판단 충돌 시 에스컬레이션 경로:

```
L1: 당사자 간 직접 해결 (근거 교환)
  → L2: another_me 중재 (최종 판정)
    → L3: 사용자 판단 (전략적 결정)
```

**규칙:**
- L1에서 합의 불가 시만 L2로 전환
- another_me의 L2 판정은 최종. 같은 건으로 재에스컬레이션 금지
- L3은 전략적 방향 판단에만 사용

**상세**: `ai_prompts/workflows/escalation_protocol.md`

---

## 워크플로우 확장 원칙

새 워크플로우를 추가할 때 다음 원칙을 따른다:

1. **공용 코어에서만 정의** — `ai_prompts/workflows/`에 작성
2. **adapter에 복제하지 않음** — 도구별 실행 차이만 adapter에 기술
3. **기존 워크플로우와 충돌 검사** — 상태 전이 규칙, 역할 책임과 충돌이 없는지 확인
4. **검증 게이트 연동** — 새 워크플로우도 Verify 원칙을 준수해야 함

---

## 참조 파일

- `ai_prompts/workflows/work_order_lifecycle.md`
- `ai_prompts/workflows/verification_gate.md`
- `ai_prompts/workflows/escalation_protocol.md`

---

## 버전

- v1.0 — 초기 정의
