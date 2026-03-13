# Task: Project Analysis

## 목적

새 프로젝트 또는 기존 프로젝트를 체계적으로 분석하여 Project Packet을 생성한다.

## 담당 역할

- **주 담당**: product_scope_agent + system_architect_agent
- **리뷰**: another_me

## 트리거

- 새 프로젝트 시작
- 기존 프로젝트 인수
- 대규모 리팩토링 전

## 입력

- 프로젝트 레포지토리 또는 기획 문서
- 사용자의 프로젝트 배경 설명

## 절차

### 1. 구조 파악 (product_scope_agent)

- [ ] 프로젝트 목적 / 핵심 가치 / 타겟 사용자 식별
- [ ] 핵심 비즈니스 루프 정의
- [ ] 현재 단계 판단 (MVP / Beta / Production / Maintenance)
- [ ] 기존 마일스톤 / 로드맵 확인

### 2. 기술 분석 (system_architect_agent)

- [ ] 기술 스택 식별
- [ ] 디렉토리 구조 분석
- [ ] 외부 의존성 목록화
- [ ] 기존 아키텍처 결정 사항(ADR) 수집
- [ ] 기술 부채 식별

### 3. 도메인 정리 (product_scope_agent)

- [ ] 도메인 용어 사전 작성
- [ ] 프로젝트 고유 규칙 식별

### 4. Project Packet 생성

- [ ] `docs/project_packets/_template.md` 기반으로 패킷 작성
- [ ] 파일명: `docs/project_packets/{프로젝트명}.md`

## 출력

- **산출물**: Project Packet (`docs/project_packets/{프로젝트명}.md`)
- **형식**: `validation/output_contract.md` 준수

## 수락 기준

- [ ] Project Packet의 모든 필수 섹션이 채워졌는가
- [ ] 핵심 비즈니스 루프가 명확한가
- [ ] 기술 스택과 디렉토리 구조가 정확한가
- [ ] another_me 리뷰 통과

## 참조

- Project Packet 템플릿: `docs/project_packets/_template.md`
- Output Contract: `validation/output_contract.md`
