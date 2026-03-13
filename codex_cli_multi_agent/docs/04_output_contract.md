# Output Contract

## 이 문서의 목적

모든 에이전트가 결과물을 제출할 때 따르는 **출력 형식 계약**을 해설한다.  
실제 계약 원문은 `validation/output_contract.md`에 있으며,  
이 문서는 계약의 설계 근거와 사용 가이드를 제공한다.

---

## 왜 출력 계약이 필요한가

| 문제 | 해결 |
|------|------|
| 에이전트마다 출력 형식이 제각각 | 통일된 필수 섹션 정의 |
| "완료"의 기준이 모호 | Evidence + Verification Status 필수 |
| 검증자가 무엇을 확인해야 할지 불명확 | 구조화된 체크포인트 |
| 후속 담당자가 맥락을 파악하기 어려움 | Next Steps + Handoff 명시 |

---

## 표준 출력 형식

모든 에이전트 출력은 다음 섹션을 포함한다:

```markdown
## TL;DR
{한 줄 핵심 요약}

## Scope
{이 작업의 범위 — 포함/제외 명시}

## Assumptions
{이 작업에서 가정한 것들}

## What Was Done
{수행한 구체적 작업 목록}

## Evidence
{작업 완료를 증명하는 근거}

## Verification Status
{VERIFIED / SELF-CHECKED / PENDING-VERIFY / FAILED}

## Risks
{발견된 위험 요소}

## Blockers / Open Issues
{미해결 사항}

## Next Steps / Handoff
{다음 단계와 담당 역할}
```

---

## 섹션별 가이드

### TL;DR

- 형식: `[역할명] — [작업 요약]`
- 한 줄로 작성. 세부 사항은 뒤에서 다룬다.

### Scope

- 이 작업이 다루는 범위와 다루지 않는 범위를 명시
- 스코프 밖의 작업을 수행했다면 근거를 설명

### Assumptions

- 이 작업에서 가정한 전제 조건
- 가정이 틀렸을 때 영향을 받는 부분도 명시

### Evidence

- 구체적이고 검증 가능한 근거만 허용
- 금지: "잘 동작함", "문제없음", "확인했음"
- 허용: 빌드 로그, 테스트 결과, 스크린샷, diff, CLI 출력

### Verification Status

| 상태 | 의미 | Work Order CLOSE 가능 |
|------|------|----------------------|
| `VERIFIED` | 독립 검증자가 확인 | ✅ |
| `SELF-CHECKED` | 본인 자체 확인 | ❌ |
| `PENDING-VERIFY` | 검증 대기 | ❌ |
| `FAILED` | 검증 실패 | ❌ |

### Risks

- 이 작업으로 인해 발생할 수 있는 위험
- 없으면 "None identified" (빈 값 금지)

### Next Steps / Handoff

- 다음에 해야 할 일과 담당 역할
- 형식: `{역할명}: {할 일}`

---

## 원문 참조

실제 운영에 적용되는 계약 원문: `validation/output_contract.md`

---

## 버전

- v1.0 — 초기 정의
