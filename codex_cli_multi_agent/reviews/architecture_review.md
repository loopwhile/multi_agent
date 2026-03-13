# Review: Architecture Review

## 목적

Architecture Design Document가 **구조적으로 유효하고 빌드 가능하며 기존 시스템과 정합하는지** 리뷰한다.

## 리뷰어

- **주 리뷰어**: another_me
- **보조**: product_scope_agent (스코프 정합성)

## 리뷰 대상

- Architecture Design Document
- ADR (Architecture Decision Records)
- Work Order (DESIGNED 상태)

## 체크리스트

### 설계 품질

- [ ] clarity > cleverness 원칙을 따르는가
- [ ] 모듈 경계가 명확한가
- [ ] 데이터 흐름이 논리적인가
- [ ] 확장 포인트가 식별되었는가 (과설계 없이)

### ADR 완성도

- [ ] 각 기술 결정에 근거가 있는가
- [ ] 트레이드오프가 명시되었는가
- [ ] 대안이 검토되었는가

### 기존 시스템 정합성

- [ ] 기존 아키텍처와 일관성이 유지되는가
- [ ] 기존 코드를 불필요하게 깨뜨리지 않는가
- [ ] 기술 부채를 증가시키지 않는가

### 빌드 가능성

- [ ] builder_agent가 이 설계로 구현을 바로 시작할 수 있는가
- [ ] 애매한 부분이나 결정되지 않은 사항이 남아있지 않은가

### 보안

- [ ] 보안 취약점이 설계에 내재되지 않았는가
- [ ] 인증/인가 흐름이 적절한가

## 판정

| 판정 | 후속 |
|------|------|
| **APPROVED** | builder_agent에게 구현 시작 허가 |
| **REVISE** | system_architect_agent에게 수정 요청 + 구체적 피드백 |
| **REJECTED** | 재설계 필요 + 근거 |

## 출력

- 리뷰 판정 + 피드백 (Output Contract 형식)
