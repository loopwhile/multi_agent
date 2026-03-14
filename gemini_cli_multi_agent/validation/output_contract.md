# Output Contract

## 목적

이 문서는 **모든 에이전트의 출력이 따라야 하는 공통 계약**을 정의한다.  
어떤 도구에서, 어떤 역할이, 어떤 프로젝트에서 실행되더라도 이 계약을 준수해야 한다.

---

## 적용 범위

- `ai_prompts/roles/`에 정의된 모든 에이전트
- `ai_prompts/tasks/`에 생성되는 모든 Work Order의 결과물
- 모든 AI 실행 도구 (Antigravity, Codex, Gemini CLI 등)

---

## 필수 출력 구조

모든 에이전트 출력은 다음 섹션을 포함해야 한다.  
역할에 따라 일부 섹션이 비어 있을 수 있지만, 섹션 자체는 생략하지 않는다.

### 1. TL;DR

- 이번 작업의 한 줄 핵심 요약
- 형식: `[역할명] — [작업 요약]`

### 2. Scope

- 이 작업의 범위 — 포함/제외 명시
- 스코프 밖의 작업을 수행했다면 근거를 설명

### 3. Assumptions

- 이 작업에서 가정한 전제 조건
- 가정이 틀렸을 때 영향을 받는 부분도 명시
- 없으면 `None` 명시

### 4. What Was Done

- 수행한 구체적 작업 목록
- 변경된 파일, 생성된 문서, 실행된 명령 등
- 모호한 서술 금지. 구체적 사실만 기술

### 5. Evidence

- 작업이 실제로 완료되었음을 증명하는 근거
- 예시: 테스트 통과 로그, 스크린샷, 빌드 성공 로그, diff, 명령 실행 결과
- **Evidence가 없으면 "완료"로 간주하지 않는다**

### 6. Verification Status

- 다음 중 하나를 명시한다:
  - `VERIFIED` — quality_verification_agent가 검증 완료
  - `SELF-CHECKED` — 본인이 자체 확인 (최종 완료로 인정되지 않음)
  - `PENDING-VERIFY` — 검증 대기 중
  - `FAILED` — 검증 실패
- **`SELF-CHECKED`로 Work Order를 CLOSE할 수 없다**

### 7. Risks

- 이 작업으로 인해 발생할 수 있는 위험
- 없으면 `None identified` (빈 값 금지)

### 8. Blockers / Open Issues

- 진행 중 발견된 차단 요소, 미해결 이슈
- 없으면 `None` 명시

### 9. Next Steps / Handoff

- 이 작업 이후에 필요한 다음 단계
- 형식: `{역할명}: {할 일}`
- 인수인계 대상 역할을 명시

---

## 금지 사항

| 금지 항목 | 이유 |
|-----------|------|
| Evidence 없이 "완료" 선언 | 검증 불가능한 완료는 시스템 무결성을 해침 |
| SELF-CHECKED로 Work Order CLOSE | 자기 검증은 최종 완료 기준이 아님 |
| 모호한 서술 ("대체로 완료", "문제없음") | 구체적 사실만 기술해야 후속 판단 가능 |
| 역할 범위 밖의 판단을 출력에 포함 | 에스컬레이션 프로토콜을 통해 처리 |
| 프로젝트별 하드코딩 | 출력 형식은 프로젝트 독립적으로 유지 |
| Risks 섹션 비워두기 | 반드시 `None identified` 이상 명시 |

---

## 예시

```markdown
## TL;DR
[builder_agent] — 로그인 API 엔드포인트 구현 완료

## Scope
- 포함: 로그인 API 라우트, JWT 발급, 에러 핸들링
- 제외: 회원가입, 비밀번호 재설정

## Assumptions
- JWT secret은 환경 변수로 주입된다고 가정
- 사용자 테이블이 이미 존재한다고 가정

## What Was Done
- `app/api/auth/login/route.ts` 생성
- JWT 토큰 발급 로직 구현
- 에러 핸들링 (401, 500) 추가

## Evidence
- 빌드 성공: `npm run build` exit code 0
- 단위 테스트: `npm test -- auth.test.ts` — 4/4 passed

## Verification Status
PENDING-VERIFY

## Risks
- JWT secret 미설정 시 런타임 에러 발생 가능

## Blockers / Open Issues
None

## Next Steps / Handoff
- quality_verification_agent: 통합 테스트 실행 필요
- operations_workflow_agent: 스테이징 배포 대기
```

---

## 버전

- v1.0 — 초기 정의
- v1.1 — TL;DR / Scope / Assumptions / Risks / Handoff 섹션 추가
