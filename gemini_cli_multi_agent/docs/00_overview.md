# Agent OS — Overview

## 이 문서의 목적

이 문서는 **Agent OS**의 전체 구조를 설명한다.  
Agent OS는 AI 실행 도구(Antigravity, Codex, Gemini CLI 등)에 의존하지 않는  
범용 멀티에이전트 팀 운영 체계다.

---

## 핵심 원칙

| 원칙 | 설명 |
|------|------|
| **도구 독립성** | 운영 규약의 원천은 repo 안의 공용 문서다. 도구는 실행 표면(adapter)일 뿐이다 |
| **역할 분리** | 각 에이전트는 고유한 책임을 가지며, 책임 중복이 없다 |
| **Work Order 중심** | 모든 작업은 Work Order 단위로 생성 → 설계 → 실행 → 검증 → 완료된다 |
| **Verify 필수** | 검증 없는 완료 선언은 시스템적으로 금지된다 |
| **프로젝트 독립성** | 역할 프롬프트에 프로젝트 특수사항을 넣지 않는다. Project Packet으로 주입한다 |

---

## 시스템 구성

```
Agent OS
├── roles/        ← 에이전트 역할 정의 (도구·프로젝트 독립)
├── workflows/    ← 실행 흐름 (Work Order lifecycle, 검증 게이트, 에스컬레이션)
├── validation/   ← 출력 계약, 품질 체크리스트
├── tasks/        ← Work Order 인스턴스
├── adapters/     ← 도구별 실행 매핑 (얇은 레이어)
└── docs/         ← 공용 규약 문서 (이 문서 포함)
```

---

## 계층 구조

```
another_me.md                          ← 최고위 사고 프레임 / 총괄
├── product_scope_agent.md             ← 제품 범위 / 요구사항
├── system_architect_agent.md          ← 기술 설계
├── builder_agent.md                   ← 코드 구현
├── quality_verification_agent.md      ← 품질 검증
├── operations_workflow_agent.md       ← 운영 / 배포
└── growth_feedback_agent.md           ← 피드백 / 성장
```

---

## 관련 문서

| 문서 | 설명 |
|------|------|
| [01_agent_system.md](01_agent_system.md) | 에이전트 시스템 아키텍처 |
| [02_workflow.md](02_workflow.md) | 운영 워크플로우 |
| [03_rules.md](03_rules.md) | 시스템 규칙 |
| [04_output_contract.md](04_output_contract.md) | 출력 계약 해설 |
| [05_work_order.md](05_work_order.md) | Work Order 해설 |
| [06_validation_policy.md](06_validation_policy.md) | 검증 정책 |
| [07_project_packet_template.md](07_project_packet_template.md) | 프로젝트 패킷 |
| [08_tool_adapter_policy.md](08_tool_adapter_policy.md) | 도구 adapter 정책 |

---

## 버전

- v1.0 — Phase 0 + Phase 1 초기 구축
