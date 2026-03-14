# Tool Adapter Policy

## 이 문서의 목적

**도구 adapter**의 설계 원칙, 경계 규칙, 위반 감지 기준을 설명한다.  
adapter spec 원문은 `ai_prompts/adapters/_adapter_spec.md`에 있다.

---

## 핵심 원칙: 도구는 실행 표면이다

```
운영 규약의 원천 = repo 안의 공용 문서 (ai_prompts/)
도구 = 그것을 읽는 실행 표면 (adapter)
```

Antigravity, Codex App, Codex CLI, Gemini CLI는 모두 같은 역할, 같은 워크플로우, 같은 검증 기준을 따른다.  
차이점은 "어떻게 로드하고 실행하는가"뿐이며, 이것만 adapter에 기술한다.

---

## Adapter에 넣을 것

| 항목 | 예시 |
|------|------|
| 실행 환경 특성 | 컨텍스트 윈도우 크기, 파일 접근 방식 |
| 역할 로드 방법 | 시스템 프롬프트 / 파일 읽기 / 파이프 주입 |
| 도구별 제한사항 | 브라우저 접근 불가, 세션 간 단절 등 |
| 도구별 워크어라운드 | 공용 워크플로우의 도구별 실행 조정 |

---

## Adapter에 절대 넣지 않을 것

| 금지 항목 | 이유 | 올바른 위치 |
|-----------|------|-------------|
| 역할의 책임 정의 | 역할 분산 → 불일치 발생 | `ai_prompts/roles/` |
| 워크플로우 본문 | 도구별 워크플로우 분기 금지 | `ai_prompts/workflows/` |
| 검증 기준 | 도구별 품질 기준 차등 금지 | `ai_prompts/validation/` |
| Output Contract | 출력 형식의 도구별 변형 금지 | `ai_prompts/validation/output_contract.md` |
| 프로젝트 특수사항 | 프로젝트는 Packet으로 | `docs/project_packets/` |

---

## 위반 감지 기준

adapter가 다음 상태가 되면 **구조적 위반**이다:

1. **행동 지침 비대화**: 10줄 이상의 "~해야 한다 / ~하지 마라" 형태의 지침을 포함
2. **역할 규칙 복제**: 공용 역할의 책임이나 금지 사항을 adapter에 반복 기술
3. **하드코딩**: 특정 프로젝트명, API 키, 엔드포인트를 포함
4. **워크플로우 재정의**: 공용 워크플로우의 단계를 생략, 변경, 재정의
5. **출력 형식 변형**: Output Contract의 필수 섹션을 변형

위반 발견 시: 해당 내용을 adapter에서 제거하고, 공용 코어 문서에 통합하거나 폐기한다.

---

## 도구별 전용 조직 금지

> **Antigravity 전용 팀, Codex 전용 팀처럼 별도 조직을 만들면 안 된다.**

하나의 역할 세트, 하나의 워크플로우, 하나의 검증 기준.  
도구가 달라도 "같은 팀"이 "같은 규칙"으로 동작한다.

---

## 새 도구 추가 시

1. `ai_prompts/adapters/_adapter_spec.md`의 공통 구조를 따른다
2. 4개 섹션(환경 특성, 로드 방법, 제한사항, 워크어라운드)만 기술
3. 공용 코어 문서의 내용을 복제하지 않는다
4. 기존 adapter와의 일관성을 확인한다

---

## 참조 파일

- adapter spec: `ai_prompts/adapters/_adapter_spec.md`
- 기존 adapter 예시: `ai_prompts/adapters/antigravity.md` 외 3개
- 시스템 규칙 R4 (도구 독립성): `docs/03_rules.md`

---

## 버전

- v1.0 — 초기 정의
