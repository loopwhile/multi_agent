# System Rules

## 이 문서의 목적

Agent OS 전체에 적용되는 **불변 규칙**을 정의한다.  
이 규칙은 모든 역할, 모든 워크플로우, 모든 도구에서 예외 없이 적용된다.

---

## 절대 규칙 (Non-Negotiable)

### R1. Verify 없는 완료 금지

> 어떤 Work Order도 quality_verification_agent의 검증 없이 CLOSED 상태가 될 수 없다.  
> SELF-CHECKED는 중간 상태일 뿐 최종 완료로 인정되지 않는다.

- 적용: 모든 Work Order
- 유일한 예외: 문서 전용 변경은 another_me 직접 리뷰로 대체 가능 (Evidence 필수)

### R2. Evidence 필수

> 모든 완료 선언에는 구체적 증거가 첨부되어야 한다.  
> "잘 동작함", "문제없음" 같은 모호한 서술은 Evidence로 인정되지 않는다.

- 허용 Evidence: 빌드 로그, 테스트 결과, 스크린샷, diff, API 응답, 배포 로그

### R3. 역할 경계 준수

> 각 에이전트는 자신의 고유 책임 범위 내에서만 판단하고 행동한다.  
> 범위 밖의 판단이 필요하면 에스컬레이션 프로토콜을 따른다.

### R4. 도구 독립성

> 운영 규약의 원천은 ``의 공용 문서다.  
> 특정 도구에서만 동작하는 규칙을 공용 코어에 넣지 않는다.  
> 도구별 차이는 `adapters/`에서만 처리한다.

### R5. 프로젝트 독립성

> 역할 프롬프트에 특정 프로젝트명, API 키, 엔드포인트를 하드코딩하지 않는다.  
> 프로젝트 맥락은 `docs/project_packets/`의 Project Packet으로 주입한다.

---

## 운영 규칙

### R6. Output Contract 준수

모든 에이전트의 출력은 `validation/output_contract.md`의 형식을 따른다.

### R7. Work Order 중심 실행

모든 실질적 작업은 Work Order로 추적한다.  
Work Order 없이 수행된 작업은 기록되지 않으며, 추적이 불가능하다.

### R8. 단일 원천 (Single Source of Truth)

| 항목 | 원천 |
|------|------|
| 역할 정의 | `roles/` |
| 워크플로우 | `workflows/` |
| 검증 기준 | `validation/` |
| 프로젝트 맥락 | `docs/project_packets/` |
| 도구 매핑 | `adapters/` |
| 시스템 규칙 | 이 문서 (`docs/03_rules.md`) |

다른 곳에 복제본을 만들지 않는다.

### R9. 에스컬레이션 우선순위

L1(직접 해결) → L2(another_me 중재) → L3(사용자 판단) 순서를 준수한다.  
모든 충돌에 대해 무조건 L2로 올리지 않는다.

### R10. 변경 시 영향 분석

공용 코어 문서(`` 또는 `docs/`)를 수정할 때는  
해당 문서를 참조하는 다른 문서에 미치는 영향을 먼저 분석한다.

---

## 규칙 변경 절차

이 문서의 규칙을 변경하려면:

1. 변경 제안을 another_me에게 에스컬레이션
2. 영향 범위 분석
3. another_me 승인 후 반영
4. 영향 받는 역할/워크플로우 문서 동기화

---

## 버전

- v1.0 — 초기 정의
