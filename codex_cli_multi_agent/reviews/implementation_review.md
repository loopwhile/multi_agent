# Review: Implementation Review

## 목적

builder_agent의 구현 결과가 **설계 의도와 일치하고, 코드 품질 기준을 충족하는지** 리뷰한다.

## 리뷰어

- **주 리뷰어**: quality_verification_agent
- **보조**: system_architect_agent (설계 정합성)

## 리뷰 대상

- 구현된 코드 (diff / PR)
- Builder의 Evidence
- Work Order (PENDING-VERIFY 상태)

## 체크리스트

### 설계 정합성

- [ ] Architecture Design Document의 의도대로 구현되었는가
- [ ] 임의로 설계를 변경한 부분이 없는가 (있다면 근거가 있는가)

### 코드 품질

- [ ] 프로젝트 코딩 컨벤션을 준수하는가
- [ ] 기능 헤더 주석이 적절한가
- [ ] 불필요한 코드 / dead code가 없는가
- [ ] 네이밍이 명확한가

### 기능성

- [ ] 요구된 기능이 모두 구현되었는가
- [ ] 엣지 케이스가 처리되었는가
- [ ] 에러 핸들링이 적절한가

### 테스트

- [ ] 빌드가 성공하는가 (Evidence 확인)
- [ ] 기존 테스트가 깨지지 않았는가
- [ ] 새 기능에 대한 테스트가 추가되었는가

### 보안

- [ ] 입력 검증이 적절한가
- [ ] 인증/인가 처리가 올바른가
- [ ] 민감 정보 노출이 없는가

## 판정

| 판정 | 후속 |
|------|------|
| **PASS** | VERIFIED → 배포 대기 |
| **CONDITIONAL PASS** | 사소한 수정 후속 WO로 처리, VERIFIED |
| **FAIL** | builder_agent에게 반환 + 구체적 수정 요구 |

## 출력

- Verification Report (ai_prompts/workflows/verification_gate.md 형식)
