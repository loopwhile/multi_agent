# Work Order Lifecycle

## 목적

이 문서는 모든 작업의 **생성부터 완료까지의 생명주기**를 정의한다.  
어떤 역할이든, 어떤 도구에서든, 이 흐름 위에서 동작한다.

---

## 상태 정의

| 상태 | 설명 | 진입 조건 |
|------|------|-----------|
| `CREATED` | Work Order가 생성됨 | product_scope_agent가 요구사항을 Work Order로 변환 |
| `DESIGNED` | 기술 설계가 첨부됨 | system_architect_agent가 설계 스펙을 첨부 |
| `IN-PROGRESS` | 구현 진행 중 | builder_agent가 작업 시작 |
| `PENDING-VERIFY` | 구현 완료, 검증 대기 | builder_agent가 구현 완료 + Evidence 제출 |
| `VERIFIED` | 검증 통과 | quality_verification_agent가 검증 완료 (PASS) |
| `FAILED` | 검증 실패 | quality_verification_agent가 검증 실패 판정 |
| `DEPLOYED` | 배포 완료 | operations_workflow_agent가 배포 완료 (선택적) |
| `CLOSED` | 최종 완료 | VERIFIED 또는 DEPLOYED 상태에서만 전이 가능 |

---

## 상태 전이 규칙

```
CREATED → DESIGNED → IN-PROGRESS → PENDING-VERIFY → VERIFIED → DEPLOYED → CLOSED
                                                   ↘ FAILED → IN-PROGRESS (재작업)
```

### 금지된 전이

| 금지 | 이유 |
|------|------|
| `IN-PROGRESS → CLOSED` | Verify 건너뛰기 금지 |
| `PENDING-VERIFY → CLOSED` | 검증 완료 없이 폐쇄 금지 |
| `FAILED → CLOSED` | 실패한 채로 폐쇄 금지 |
| `CREATED → IN-PROGRESS` | 설계 없이 바로 구현 시작 금지 (단순 작업 예외 아래 참조) |

### 예외: 단순 작업 패스트트랙

다음 조건을 모두 만족하면 `CREATED → IN-PROGRESS` 직행을 허용한다:
- 변경 범위가 파일 1–2개 이하
- 아키텍처 결정이 불필요
- another_me 또는 product_scope_agent가 패스트트랙 승인

---

## 역할별 책임

| 단계 | 책임 역할 | 수행 내용 |
|------|-----------|-----------|
| CREATE | product_scope_agent | 요구사항을 Work Order 템플릿에 작성 |
| DESIGN | system_architect_agent | 기술 설계, 아키텍처 결정 첨부 |
| EXECUTE | builder_agent | 코드 구현, 빌드, Evidence 생성 |
| VERIFY | quality_verification_agent | Output Contract 기준으로 검증 |
| DEPLOY | operations_workflow_agent | 배포 실행, 운영 확인 |
| CLOSE | another_me 또는 product_scope_agent | 최종 완료 확인 |

---

## Work Order 참조

- Work Order 인스턴스는 `tasks/`에 생성
- 템플릿: `tasks/_template.md`
- 파일명 형식: `WO-YYYYMMDD-{순번}-{짧은설명}.md`
  - 예: `WO-20260312-001-login-api.md`

---

## 검증 게이트 연동

- 상세 검증 기준은 `workflows/verification_gate.md` 참조
- 품질 체크리스트는 `validation/quality_checklist.md` 참조
- 출력 형식은 `validation/output_contract.md` 준수

---

## 에스컬레이션

- 역할 간 판단 충돌 시 `workflows/escalation_protocol.md` 참조
- 최종 중재는 another_me가 수행

---

## 버전

- v1.0 — 초기 정의
