# Task: Feedback Analysis

## 목적

사용자 피드백과 운영 데이터를 분석하여 구조화된 개선 제안을 생성한다.

## 담당 역할

- **주 담당**: growth_feedback_agent
- **전달 대상**: product_scope_agent (스코프 반영 검토)

## 트리거

- 정기 피드백 리뷰 주기
- 사용자 이슈 급증
- 핵심 지표 이상 감지

## 입력

- 사용자 피드백 (리뷰, 설문, 지원 요청)
- 운영 데이터 (analytics, 로그)
- operations_workflow_agent의 운영 보고서

## 절차

### 1. 데이터 수집

- [ ] 피드백 소스 식별 및 수집
- [ ] 운영 지표 수집 (DAU, 전환율, 이탈률 등)

### 2. 분석

- [ ] 피드백을 카테고리별 분류
- [ ] 빈도 / 심각도별 정렬
- [ ] 패턴 식별 (반복되는 불만, 요청)
- [ ] 지표 추세 분석

### 3. 개선 제안 작성

- [ ] 각 제안에 근거 데이터 첨부
- [ ] 영향도 × 실현 용이성 기준으로 우선순위 매김
- [ ] Improvement Proposal 형식으로 작성

### 4. 전달

- [ ] product_scope_agent에게 Improvement Proposal 전달
- [ ] 스코프 반영 여부는 product_scope_agent가 판단

## 출력

- **산출물**: Feedback Report + Improvement Proposals
- **형식**: `ai_prompts/validation/output_contract.md` 준수
- **제안 형식**: `ai_prompts/roles/growth_feedback_agent.md`의 Improvement Proposal 형식

## 수락 기준

- [ ] 분석이 데이터에 근거하는가 (감이 아닌 수치)
- [ ] 제안에 예상 영향도가 포함되는가
- [ ] 핵심 제품 루프를 해치는 제안이 없는가
- [ ] 우선순위 근거가 명확한가

## 참조

- Growth Feedback Agent: `ai_prompts/roles/growth_feedback_agent.md`
- Output Contract: `ai_prompts/validation/output_contract.md`
