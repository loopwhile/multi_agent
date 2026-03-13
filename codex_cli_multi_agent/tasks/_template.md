# Work Order: {제목}

## 메타데이터

| 항목 | 값 |
|------|-----|
| ID | WO-YYYYMMDD-NNN |
| 상태 | CREATED / DESIGNED / IN-PROGRESS / PENDING-VERIFY / VERIFIED / FAILED / DEPLOYED / CLOSED |
| 생성일 | YYYY-MM-DD |
| 생성자 | {역할명 또는 사용자} |
| 담당 역할 | {실행 담당 역할} |
| 프로젝트 | {프로젝트명 — project packet 참조} |
| Session Handoff | `docs/session_handoff.md` |
| 패스트트랙 | Yes / No |
| 우선순위 | Critical / High / Medium / Low |

---

## 요구사항

### 목표
{이 Work Order가 달성해야 하는 것}

### 스코프
{포함 범위와 제외 범위를 명시}

### 수락 기준
{이것이 충족되면 VERIFIED로 전이 가능한 조건 목록}

- [ ] {기준 1}
- [ ] {기준 2}
- [ ] {기준 3}

---

## 설계 (DESIGNED 단계에서 작성)

### 기술 접근
{system_architect_agent가 작성하는 기술 설계}

### 영향 범위
{변경되는 파일/모듈/시스템 목록}

### 의존성
{선행 Work Order, 외부 의존성}

---

## 실행 기록 (IN-PROGRESS 단계에서 업데이트)

### 수행 내용
{builder_agent가 작성하는 구체적 작업 목록}

### Evidence
{빌드 로그, 테스트 결과, diff 등}

---

## 검증 결과 (VERIFY 단계에서 작성)

### 검증자
{quality_verification_agent}

### 검증 항목

| 항목 | 결과 | 근거 |
|------|------|------|
| {체크 항목 1} | PASS / FAIL | {근거} |
| {체크 항목 2} | PASS / FAIL | {근거} |

### 최종 판정
- `VERIFIED` / `FAILED`
- 사유: {판정 근거}

---

## 배포 (선택)

### 배포 환경
{staging / production}

### 배포 결과
{operations_workflow_agent 기록}

---

## 상태 이력

| 시점 | 상태 변경 | 역할 | 비고 |
|------|-----------|------|------|
| {날짜} | → CREATED | {역할} | {초기 생성} |

---

## Session Sync

### Handoff 연결 상태
- [ ] `docs/session_handoff.md`의 `Current work order`가 이 WO 파일을 가리킨다.
- [ ] 종료 직전 `Done / In progress / Next 3 actions / Commands to resume`를 동기화했다.

### 마지막 동기화
- 시각: YYYY-MM-DD HH:mm (KST)
- 담당: {another_me 또는 실행 담당 역할}
- 요약: {동기화 요약}

---

## 비고

{추가 메모, 참조 링크, 관련 Work Order 등}
