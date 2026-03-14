# Agent System Architecture

## 이 문서의 목적

에이전트 팀의 **구성, 역할 경계, 상호작용 규칙**을 설명한다.  
개별 역할의 상세는 `ai_prompts/roles/`에 있으며, 이 문서는 전체 시스템의 설계 근거를 다룬다.

---

## 설계 원칙

### 1. 단일 책임 (Single Responsibility)

각 에이전트는 하나의 명확한 책임 영역을 가진다.  
두 역할이 같은 판단을 내리는 상황이 발생하면 구조적 결함이다.

### 2. 분리된 관심사 (Separation of Concerns)

| 관심사 | 담당 |
|--------|------|
| 무엇을 만들 것인가 | product_scope_agent |
| 어떻게 설계할 것인가 | system_architect_agent |
| 어떻게 만들 것인가 | builder_agent |
| 제대로 만들어졌는가 | quality_verification_agent |
| 어떻게 운영할 것인가 | operations_workflow_agent |
| 어떻게 개선할 것인가 | growth_feedback_agent |
| 전체 방향과 최종 판단 | another_me |

### 3. 에스컬레이션 계층

- **L1**: 당사자 간 직접 해결
- **L2**: another_me 중재
- **L3**: 사용자 판단 (전략적 결정에만)

---

## 역할 간 상호작용 맵

```
product_scope ──요구사항──→ system_architect ──설계──→ builder
      ↑                                                  │
      │                                                  ↓
growth_feedback ←──운영 데이터── operations ←──배포── quality_verification
```

### 순방향 흐름 (요청 → 결과물)

1. **product_scope** → Work Order 생성 (CREATED)
2. **system_architect** → 기술 설계 첨부 (DESIGNED)
3. **builder** → 코드 구현 (IN-PROGRESS → PENDING-VERIFY)
4. **quality_verification** → 검증 (VERIFIED / FAILED)
5. **operations** → 배포 (DEPLOYED)

### 역방향 흐름 (피드백 → 개선)

1. **operations** → 운영 데이터를 **growth_feedback**에 전달
2. **growth_feedback** → 개선 제안을 **product_scope**에 전달
3. **product_scope** → 새 Work Order 생성으로 순환

---

## 역할이 아닌 것

| 오해 | 실제 |
|------|------|
| 역할 = 사람 | 역할 = 실행 모드. 한 사람이나 한 AI가 여러 역할을 순차 수행 가능 |
| 역할 = 도구 | 역할은 도구 독립적. 어떤 도구에서든 같은 역할로 동작 |
| 역할 = 프로젝트 전용 | 역할은 프로젝트 독립적. 프로젝트 맥락은 Project Packet으로 주입 |

---

## 참조 파일

- 역할 프롬프트: `ai_prompts/roles/*.md`
- 에스컬레이션 규칙: `ai_prompts/workflows/escalation_protocol.md`
- Work Order 생명주기: `ai_prompts/workflows/work_order_lifecycle.md`

---

## 버전

- v1.0 — 초기 정의
