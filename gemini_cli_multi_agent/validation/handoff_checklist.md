# Handoff Checklist

## 목적

역할 간 작업 인수인계 시 **누락 없는 전달을 보장**하는 체크리스트.

---

## 일반 핸드오프 체크리스트

모든 역할 전환 시 적용한다.

### 전달자 (보내는 역할)

- [ ] 현재 Work Order 상태를 올바르게 업데이트했는가
- [ ] Output Contract 형식의 결과물을 작성했는가
- [ ] Evidence를 첨부했는가
- [ ] Next Steps / Handoff에 수신 역할과 요청 사항을 명시했는가
- [ ] 오픈 이슈 / 차단 요소를 기록했는가
- [ ] Assumptions을 명시했는가

### 수신자 (받는 역할)

- [ ] 전달받은 산출물이 Output Contract를 준수하는가
- [ ] Evidence가 유효한가
- [ ] 작업 시작에 충분한 정보가 있는가 (불충분하면 발신자에게 반환)

---

## 단계별 핸드오프

### CREATED → DESIGNED (scope → architect)

- [ ] Scope Document가 완성되었는가
- [ ] 수락 기준이 작성되었는가
- [ ] must-have / nice-to-have가 분리되었는가

### DESIGNED → IN-PROGRESS (architect → builder)

- [ ] Architecture Design Document가 첨부되었는가
- [ ] ADR이 작성되었는가
- [ ] 빌더가 바로 구현 시작 가능한가

### IN-PROGRESS → PENDING-VERIFY (builder → quality)

- [ ] 구현 코드가 커밋되었는가
- [ ] 빌드 성공 Evidence가 있는가
- [ ] 테스트 결과가 포함되었는가
- [ ] builder가 SELF-CHECKED를 표기했는가

### VERIFIED → DEPLOYED (quality → operations)

- [ ] Verification Report가 PASS인가
- [ ] 배포 대상 환경이 명시되었는가
- [ ] 롤백 계획이 수립되었는가

---

## 핸드오프 실패 시

수신 역할이 핸드오프를 거부할 수 있는 조건:

| 조건 | 처리 |
|------|------|
| Evidence 부재 | 발신 역할에게 반환 |
| Output Contract 미준수 | 발신 역할에게 보완 요청 |
| 정보 불충분 | 구체적 질문과 함께 반환 |
| 역할 범위 불일치 | 올바른 역할로 재전달 |

---

## 참조

- Output Contract: `ai_prompts/validation/output_contract.md`
- Work Order Lifecycle: `ai_prompts/workflows/work_order_lifecycle.md`
