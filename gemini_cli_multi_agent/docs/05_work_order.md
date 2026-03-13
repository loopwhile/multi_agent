# Work Order

## 이 문서의 목적

**Work Order 운영 체계**의 설계 근거, 사용 가이드, 작성 원칙을 설명한다.  
Work Order 템플릿 원문은 `ai_prompts/tasks/_template.md`에,  
생명주기 규칙은 `ai_prompts/workflows/work_order_lifecycle.md`에 있다.

---

## Work Order란

Work Order는 Agent OS에서 **작업의 최소 추적 단위**다.  
모든 실질적 작업은 Work Order를 통해 생성 → 설계 → 실행 → 검증 → 완료된다.

Work Order 없이 수행된 작업은:
- 추적되지 않는다
- 검증 대상이 아니다
- 공식 결과물로 인정되지 않는다

---

## 상태 흐름

```
CREATED → DESIGNED → IN-PROGRESS → PENDING-VERIFY → VERIFIED → DEPLOYED → CLOSED
                                                   ↘ FAILED → IN-PROGRESS (재작업)
```

상세 전이 규칙: `ai_prompts/workflows/work_order_lifecycle.md`

---

## Work Order 작성 원칙

### 1. 수락 기준 (Acceptance Criteria) 필수

모든 Work Order에는 "무엇이 충족되면 완료인가"를 명시한다.  
수락 기준이 없는 Work Order는 검증이 불가능하다.

### 2. 스코프 명시

무엇을 **포함**하고 무엇을 **포함하지 않는지** 기술한다.  
스코프가 모호하면 작업 범위가 무한히 확장된다.

### 3. 단일 목적

하나의 Work Order는 하나의 목적을 다룬다.  
여러 목적이 섞이면 분리한다.

### 4. 상태 이력 기록

모든 상태 변경 시 이력을 기록한다.  
누가, 언제, 어떤 상태로 변경했는지 추적 가능해야 한다.

---

## 파일명 규칙

```
WO-YYYYMMDD-{순번}-{짧은설명}.md
```

예시:
- `WO-20260312-001-login-api.md`
- `WO-20260312-002-error-handling.md`

---

## 패스트트랙

다음 조건을 **모두** 만족하면 DESIGNED 단계를 건너뛸 수 있다:

- 변경 범위가 파일 1–2개 이하
- 아키텍처 결정이 불필요
- another_me 또는 product_scope_agent가 패스트트랙 승인

패스트트랙이더라도 **VERIFY는 필수**다.

---

## 참조 파일

- 템플릿: `ai_prompts/tasks/_template.md`
- 생명주기: `ai_prompts/workflows/work_order_lifecycle.md`
- 검증 게이트: `ai_prompts/workflows/verification_gate.md`

---

## 버전

- v1.0 — 초기 정의
