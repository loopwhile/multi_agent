# Project Packet System

## 이 문서의 목적

**Project Packet**의 설계 근거, 사용 원칙, 작성 가이드를 설명한다.  
템플릿 원문은 `docs/project_packets/_template.md`에 있다.

---

## 왜 Project Packet이 필요한가

Agent OS의 역할 프롬프트는 **프로젝트 독립적**이다.  
그렇다면 프로젝트별 맥락(기술 스택, 도메인 용어, 아키텍처 결정 등)은 어디서 오는가?

**답: Project Packet이 주입한다.**

```
역할 프롬프트 (범용)  +  Project Packet (프로젝트 특수)  =  실행 컨텍스트
```

---

## Project Packet에 넣을 것

| 항목 | 설명 |
|------|------|
| 프로젝트 메타데이터 | 이름, 레포, 기술 스택, 현재 단계 |
| 프로젝트 개요 | 목적, 핵심 가치, 타겟 사용자 |
| 핵심 비즈니스 루프 | 사용자 행동 → 가치 전달 구조 |
| 기술 아키텍처 요약 | 구조, 주요 디렉토리, 외부 의존성 |
| 도메인 용어 | 이 프로젝트의 고유 용어 사전 |
| ADR 요약 | 아키텍처 결정 사항 |
| 현재 마일스톤 | 진행 현황 |
| 프로젝트 고유 규칙 | 범용 규칙 외 추가 규칙 |
| 기술 부채 | 알려진 기술 부채 |

---

## Project Packet에 넣으면 안 되는 것

| 금지 항목 | 올바른 위치 |
|-----------|-------------|
| 역할 정의 | `ai_prompts/roles/` |
| 워크플로우 규칙 | `ai_prompts/workflows/` |
| 검증 기준 | `ai_prompts/validation/` |
| 출력 계약 | `ai_prompts/validation/output_contract.md` |
| 도구별 설정 | `ai_prompts/adapters/` |

---

## 사용 방법

### 신규 프로젝트 시작 시

1. `docs/project_packets/_template.md`를 복사
2. `docs/project_packets/{프로젝트명}.md`로 저장
3. 각 섹션을 프로젝트에 맞게 채움
4. 에이전트 실행 시 역할 프롬프트와 함께 이 파일을 로드

### 기존 프로젝트 갱신 시

1. 기존 Project Packet을 열어 갱신
2. 특히 마일스톤, ADR, 기술 부채를 최신 상태로 유지
3. 최종 갱신일을 업데이트

---

## 참조 파일

- 템플릿: `docs/project_packets/_template.md`
- 시스템 규칙 R5 (프로젝트 독립성): `docs/03_rules.md`

---

## 버전

- v1.0 — 초기 정의
