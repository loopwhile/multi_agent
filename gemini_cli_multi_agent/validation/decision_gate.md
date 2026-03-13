# Decision Gate

## 목적

Work Order 생명주기의 **주요 전이 지점에서 명시적 판정을 강제**하는 게이트 정의.  
Verification Gate가 "코드 검증"에 집중한다면, Decision Gate는 "진행 여부 판단" 전체를 다룬다.

---

## Decision Gate 목록

### DG1: Scope Approval Gate

- **전이**: CREATED → DESIGNED
- **판정자**: another_me
- **기준**: Scope Review 통과

| 확인 항목 | 필수 |
|-----------|------|
| 스코프 경계 명확 | ✅ |
| 수락 기준 검증 가능 | ✅ |
| 기술 실현성 확인 | ✅ |
| 비즈니스 정합성 | ✅ |

### DG2: Design Approval Gate

- **전이**: DESIGNED → IN-PROGRESS
- **판정자**: another_me
- **기준**: Architecture Review 통과

| 확인 항목 | 필수 |
|-----------|------|
| 설계 품질 확인 | ✅ |
| 빌드 가능성 확인 | ✅ |
| 기존 시스템 정합성 | ✅ |
| 보안 검토 | ✅ |

### DG3: Verification Gate

- **전이**: PENDING-VERIFY → VERIFIED / FAILED
- **판정자**: quality_verification_agent
- **기준**: `workflows/verification_gate.md`

| 확인 항목 | 필수 |
|-----------|------|
| Evidence 존재 | ✅ |
| 독립 검증 | ✅ |
| Quality Checklist 통과 | ✅ |
| 수락 기준 충족 | ✅ |

### DG4: Release Gate

- **전이**: VERIFIED → DEPLOYED
- **판정자**: another_me 또는 product_scope_agent
- **기준**: Release Review 통과

| 확인 항목 | 필수 |
|-----------|------|
| Verification Report PASS | ✅ |
| 배포 환경 준비 | ✅ |
| 롤백 계획 수립 | ✅ |
| Go/No-Go 판정 | ✅ |

---

## Gate 우회 규칙

- **DG3 (Verification Gate)**: 절대 우회 불가
- **DG1, DG2**: 패스트트랙 조건 충족 시 DG1+DG2를 another_me 약식 승인으로 대체 가능
  - 패스트트랙 조건: `workflows/work_order_lifecycle.md` 참조
- **DG4**: 배포가 불필요한 WO는 DG4 생략 (VERIFIED → CLOSED)

---

## 판정 기록

모든 Gate 판정은 Work Order의 상태 이력에 기록한다:

```markdown
| 시점 | 상태 변경 | 역할 | Gate | 판정 |
|------|-----------|------|------|------|
| 2026-03-12 | → DESIGNED | another_me | DG1 | APPROVED |
```

---

## 참조

- Verification Gate: `workflows/verification_gate.md`
- Work Order Lifecycle: `workflows/work_order_lifecycle.md`
- Scope Review: `reviews/scope_review.md`
- Architecture Review: `reviews/architecture_review.md`
- Release Review: `reviews/release_review.md`
