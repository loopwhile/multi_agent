# Frontend TypeScript Architecture Standard

# Frontend TypeScript Architecture Standard

> React + Next.js + TypeScript 기반 프론트엔드 프로젝트 표준 구조 문서
> 
> 
> 본 문서는 팀에서 사용하는 **도메인 중심 패키지 구조**, **역할 분리 규칙**, **타입 설계 원칙**을 정의한다.
> 
> 본 구조는 TypeScript 문법 자체보다 **실무 유지보수성, 협업 효율, 확장성, 타입 안정성**을 우선한 팀 표준 구조이다.
> 

---

## 1. 목적

이 표준의 목적은 다음과 같다.

- 프로젝트 구조를 일관되게 유지한다.
- 신규 기능 추가 시 파일과 책임의 위치를 명확하게 한다.
- 도메인별 응집도를 높이고 공통 관심사를 분리한다.
- 타입 정의 위치와 역할을 명확하게 한다.
- 협업 시 코드 탐색 비용을 줄인다.
- 유지보수, 리팩토링, 온보딩 비용을 낮춘다.
- React와 Next.js를 사용하더라도 동일한 구조 철학과 타입 기준으로 운영한다.

---

## 2. 핵심 원칙

### 2.1 도메인 중심 구조

기능은 가능한 한 `domain` 아래의 각 도메인 폴더 단위로 관리한다.

예를 들어 `analytics`, `my-page`, `notice`, `order`, `inventory` 같은 실제 비즈니스 단위를 기준으로 나눈다.

즉, 파일 종류별 전역 분리보다 **도메인별로 응집된 구조**를 우선한다.

### 2.2 라우트 / 페이지는 얇게 유지

라우트 또는 페이지 파일은 화면 진입점 역할만 담당한다.

역할은 다음 정도로 제한한다.

- 레이아웃 조합
- 도메인 컴포넌트 연결
- 페이지 단위 가드 처리
- 페이지 메타 정보 설정
- 화면 단위 조립

실제 비즈니스 로직, API 호출 세부 처리, 도메인 상태 제어는 가능한 한 도메인 레이어로 내린다.

### 2.3 공통 관심사 분리

여러 도메인에서 공통으로 사용하는 기능은 `common`으로 분리한다.

예를 들어 다음 성격의 요소가 해당된다.

- 공통 UI 컴포넌트
- 공통 API 클라이언트
- 공통 hooks
- 공통 유틸
- 전역 상수
- 외부 라이브러리 래퍼
- 전역 공통 타입

### 2.4 UI / 상태 / API / 타입 분리

프론트엔드는 화면 코드가 빠르게 비대해지기 때문에 다음 역할을 분리해야 한다.

- 화면 표현
- 사용자 상호작용 처리
- 서버 통신
- 클라이언트 상태 관리
- 도메인별 데이터 가공
- 타입 정의와 타입 변환

한 파일 안에서 이 모든 것을 처리하지 않는다.

### 2.5 타입은 구조를 보조해야지 구조를 지배하면 안 된다

TypeScript는 코드 품질을 올리기 위한 도구이지, 복잡한 추상화를 정당화하는 수단이 아니다.

- 의미 없는 generic 남발을 지양한다.
- 재사용성 없는 유틸 타입 과잉 설계를 지양한다.
- 타입 체조를 위해 구현이 읽기 어려워지는 구조를 피한다.
- 타입은 실무 가독성과 안정성을 높이는 방향으로 사용한다.

### 2.6 과도한 추상화 지양

형식적인 추상화는 만들지 않는다.

- 의미 없는 base component 남발을 지양한다.
- 재사용되지 않는 generic abstraction을 억지로 만들지 않는다.
- domain별로 다른 요구사항을 억지로 공통화하지 않는다.
- 실제 반복과 유지보수 비용이 있을 때만 공통화한다.

### 2.7 프레임워크보다 구조 우선

React든 Next.js든 중요한 것은 문법이 아니라 아래 원칙이다.

- 어디에 무엇을 둘 것인가
- 어떤 레이어가 어떤 책임을 가져야 하는가
- 무엇을 공통화하고 무엇을 도메인 안에 둘 것인가
- 타입을 어느 레이어에서 정의하고 어느 레이어에서 사용하는가

---

## 3. 최상위 패키지 구조

최상위 구조는 `app`, `common`, `domain`으로 구성한다.

### 3.1 app

애플리케이션 진입점과 전역 조립 영역을 둔다.

예를 들면 다음 성격이 해당된다.

- 라우트 / 페이지 진입점
- 레이아웃
- 전역 Provider 등록
- 라우터 연결
- 인증 가드
- 앱 초기화

핵심은 `app`이 **애플리케이션 조립 레이어**라는 점이다.

### 3.2 common

여러 도메인에서 공통으로 사용하는 기능을 둔다.

예를 들면 다음이 해당된다.

- 공통 UI 컴포넌트
- 공통 API 클라이언트
- 공통 hooks
- 공통 util
- 상수
- 공통 라이브러리 래퍼
- 전역 공통 타입

`common`은 공통 모듈 저장소이지, 애매한 코드들을 몰아넣는 공간이 아니다.

특정 도메인에 종속된 기능은 `common`이 아니라 해당 도메인 안에 둔다.

### 3.3 domain

실제 비즈니스 기능을 담당하는 도메인 패키지를 둔다.

예시는 다음과 같다.

- analytics
- my-page
- notice
- order
- inventory

프론트엔드에서도 화면 기준이 아니라, **비즈니스 기능 기준으로 응집**시키는 것을 원칙으로 한다.

---

## 4. 도메인 패키지 기본 구조

각 도메인은 아래 역할을 기본으로 가진다.

- `api`
- `components`
- `hooks`
- `model`
- `store`
- `types`

모든 도메인이 반드시 모든 하위 폴더를 가질 필요는 없다.

하지만 도메인별 책임을 분리할 때는 위 구조를 기본 패턴으로 삼는다.

TypeScript가 적용된 구조에서는 `types`가 도메인 타입의 기본 위치가 된다.

---

## 5. app 역할

`app`은 프론트엔드 전체를 조립하는 레이어다.

역할은 다음과 같다.

- 페이지 또는 라우트 진입점 정의
- 전역 레이아웃 적용
- Provider 연결
- 페이지 단위 권한 처리
- 도메인 컴포넌트 조합
- 앱 초기 실행 구성

규칙은 다음과 같다.

- `app`에는 도메인 세부 로직을 넣지 않는다.
- API 세부 호출 로직을 직접 작성하지 않는다.
- 상태 관리 구현을 길게 넣지 않는다.
- 타입 선언을 과도하게 두지 않는다.
- 페이지 단위 조립과 연결에 집중한다.

핵심 원칙은 동일하다.

- 페이지는 얇아야 한다.
- 페이지는 조립자여야 한다.
- 실제 로직은 domain으로 내려야 한다.

---

## 6. common 역할

`common`은 여러 도메인에서 재사용되는 공통 모듈을 둔다.

예시는 다음과 같다.

- 공통 버튼, 모달, 테이블
- 공통 API client
- 공통 hooks
- 날짜 포맷 유틸
- 상수
- 외부 라이브러리 설정 래퍼
- 전역 공통 타입

규칙은 다음과 같다.

- 특정 도메인에 종속된 UI는 `common.ui`에 두지 않는다.
- 특정 도메인 API 함수는 `common.api`에 두지 않는다.
- 특정 도메인 전용 타입은 `common.types`에 두지 않는다.
- `common`은 재사용 가능한 범용 기능만 관리한다.
- `utils`는 최소화한다.
- 공통이라는 이유만으로 모든 것을 넣지 않는다.

---

## 7. api 역할

`api`는 도메인 단위의 서버 통신 책임을 가진다.

역할은 다음과 같다.

- API 요청 함수 정의
- URL, query parameter, request body 조합
- 서버 응답 수신
- 도메인 단위 API 호출 모듈 관리
- API 응답 타입 연결

규칙은 다음과 같다.

- UI 로직을 넣지 않는다.
- DOM 상태를 직접 제어하지 않는다.
- 토스트, 모달, 페이지 이동 같은 화면 제어를 넣지 않는다.
- HTTP 통신 책임에 집중한다.
- 공통 API 클라이언트는 `common.api`를 사용하고, 도메인별 요청 함수만 도메인 `api`에 둔다.
- API 함수 시그니처에는 가능한 한 명확한 request / response 타입을 연결한다.

---

## 8. components 역할

`components`는 도메인 전용 UI 컴포넌트를 둔다.

역할은 다음과 같다.

- 도메인 전용 화면 조각 표현
- 리스트, 상세, 폼, 카드, 섹션 구성
- 사용자 인터랙션 이벤트 연결
- 도메인 hooks와 조합
- 명확한 props 타입 사용

규칙은 다음과 같다.

- 여러 도메인에서 재사용되는 범용 UI는 `common.ui`로 이동한다.
- 특정 도메인 맥락이 있으면 해당 도메인 `components`에 둔다.
- 너무 많은 책임을 가진 거대한 컴포넌트는 분리한다.
- 페이지 전체를 하나의 거대한 컴포넌트로 만들지 않는다.
- props는 가능한 한 명시적으로 타입을 정의한다.

---

## 9. hooks 역할

`hooks`는 도메인 단위의 화면 로직과 상호작용 흐름을 둔다.

역할은 다음과 같다.

- 데이터 조회 훅
- 액션 훅
- 필터 / 정렬 / 검색 상태 제어
- 폼 처리 흐름 보조
- 도메인 UI와 API 사이 연결
- 반환 타입과 입력 타입의 명확화

규칙은 다음과 같다.

- 도메인 hooks는 해당 도메인 안에 둔다.
- 전역적으로 재사용되는 범용 hook만 `common.hooks`로 이동한다.
- hook은 화면 로직을 추상화하되, 의미 없는 래퍼 hook을 남발하지 않는다.
- 하나의 hook이 너무 많은 책임을 가지면 조회용 / 액션용 / 상태용으로 분리한다.
- hook 반환값은 예측 가능하게 유지한다.

예를 들면 다음과 같은 네이밍을 권장한다.

- `useNoticeList`
- `useNoticeDetail`
- `useNoticeActions`

---

## 10. model 역할

`model`은 프론트엔드에서 도메인 데이터를 다루기 위한 보조 로직을 둔다.

예를 들면 다음 성격이 해당된다.

- 도메인 상수
- 응답 데이터 가공 로직
- 표시용 값 변환
- 도메인별 formatter
- mapper
- selector 성격 로직
- UI 모델 변환 로직

규칙은 다음과 같다.

- API 호출은 넣지 않는다.
- React component를 넣지 않는다.
- UI와 강하게 결합된 로직은 컴포넌트 또는 hook에 둔다.
- 도메인 데이터 표현과 가공 책임에 집중한다.
- API 응답 타입과 UI 표시 타입이 다르면 `model`에서 변환한다.

즉 `model`은 프론트엔드에서 도메인 데이터를 더 읽기 쉽게 다루기 위한 계층이다.

---

## 11. store 역할

`store`는 도메인 단위의 공유 클라이언트 상태를 관리한다.

예를 들면 다음 성격이 해당된다.

- 필터 상태
- 정렬 상태
- 선택 항목 상태
- 화면 간 공유되는 도메인 상태
- 클라이언트에서 유지되어야 하는 도메인별 UI 상태

규칙은 다음과 같다.

- 서버 응답 자체를 무조건 store에 넣지 않는다.
- 서버 상태와 클라이언트 상태를 구분한다.
- 단순한 로컬 상태까지 store로 올리지 않는다.
- 도메인 간 공유가 아닌 단순 컴포넌트 상태는 로컬 state로 유지한다.
- store는 필요한 경우에만 도입한다.
- store state와 action 타입도 명확하게 유지한다.

---

## 12. types 역할

`types`는 도메인 단위 타입 정의의 기본 위치다.

포함 대상은 다음과 같다.

- API Request 타입
- API Response 타입
- 도메인 엔티티 표현 타입
- 화면 표시용 View Model 타입
- 폼 값 타입
- 필터 / 검색 조건 타입
- 컴포넌트 Props 타입
- store state 타입

규칙은 다음과 같다.

- 도메인 전용 타입은 해당 도메인 `types`에 둔다.
- 진짜 전역 공통 타입만 `common.types`에 둔다.
- `api response type`, `ui model type`, `form type`을 필요 시 분리한다.
- 너무 거대한 단일 `types.ts` 파일을 만들지 않는다.
- 타입 이름은 역할이 드러나야 한다.
- 타입과 인터페이스 사용은 팀 기준으로 일관되게 유지한다.

예를 들면 다음과 같은 구조가 가능하다.

- `notice-api.types.ts`
- `notice-model.types.ts`
- `notice-form.types.ts`
- `notice-props.types.ts`

---

## 13. 타입 설계 원칙

### 13.1 타입은 역할 기준으로 나눈다

프론트엔드에서 다루는 데이터는 하나의 타입으로 끝나지 않을 수 있다.

예를 들어 `notice` 도메인에는 다음이 분리될 수 있다.

- 서버 응답 타입
- UI 표시 타입
- 폼 입력 타입
- 검색 조건 타입

이들을 모두 하나의 `Notice` 타입으로 뭉개지 않는다.

### 13.2 API 타입과 UI 타입을 구분한다

서버 응답 구조와 화면에서 필요한 구조는 다를 수 있다.

예를 들면 다음과 같다.

- 서버는 날짜를 문자열로 보낸다.
- 프론트는 날짜 포맷팅된 표시값이 필요하다.
- 서버는 코드값을 보낸다.
- 프론트는 label 값이 필요하다.

이 경우 API 응답 타입과 UI 표시 타입을 분리하고, 변환은 `model`에서 수행한다.

### 13.3 Form 타입을 분리한다

폼은 API 요청 타입과 완전히 같지 않을 수 있다.

예를 들면 다음과 같다.

- 입력 중간 상태가 필요하다.
- nullable 값이 많다.
- 문자열 기반 입력을 숫자로 변환해야 한다.
- validation 전과 후 타입이 다르다.

이 경우 Form 타입을 별도로 둔다.

### 13.4 Props 타입은 컴포넌트 가까이에 둔다

컴포넌트 전용 Props 타입은 가능한 한 해당 컴포넌트 파일 근처 또는 도메인 `types`에 둔다.

전역 공통 props가 아니면 `common.types`로 올리지 않는다.

### 13.5 any 사용 금지 원칙

`any`는 기본적으로 사용하지 않는다.

허용 가능한 경우는 다음처럼 매우 제한적이어야 한다.

- 외부 라이브러리 타입이 부실한 경우
- 임시 마이그레이션 단계
- 정말 타입을 정확히 알 수 없는 외부 입력 처리

이 경우에도 장기 방치하지 않는다.

### 13.6 unknown 우선

정체를 모르는 외부 입력은 `any`보다 `unknown`을 우선 사용한다.

이후 타입 가드나 파싱 로직으로 좁힌다.

### 13.7 공통 타입 비대화 방지

다음 조건을 만족하지 않으면 `common.types`로 올리지 않는다.

- 여러 도메인에서 실제로 재사용된다.
- 특정 도메인 의미가 없다.
- 범용 계약으로 유지될 가능성이 높다

---

## 14. 네이밍 규칙

### 14.1 폴더 네이밍

폴더명은 소문자 기반으로 작성하고, 단어 구분은 `kebab-case`를 기본으로 한다.

예시는 다음과 같다.

- `my-page`
- `common`
- `notice`
- `order-history`

### 14.2 파일 네이밍

파일명도 역할이 드러나게 작성한다.

권장 예시는 다음과 같다.

- `notice-api.ts`
- `use-notice-list.ts`
- `notice-filter-store.ts`
- `notice-mapper.ts`
- `notice-constants.ts`
- `notice-api.types.ts`
- `notice-model.types.ts`
- `notice-form.types.ts`

### 14.3 컴포넌트 네이밍

React 컴포넌트는 `PascalCase`를 사용한다.

예시는 다음과 같다.

- `NoticeListSection.tsx`
- `NoticeDetailSection.tsx`
- `NoticeSearchForm.tsx`
- `OrderSummaryCard.tsx`

### 14.4 Hook 네이밍

Hook은 반드시 `use`로 시작한다.

예시는 다음과 같다.

- `useNoticeList`
- `useNoticeDetail`
- `useNoticeActions`

파일명은 `use-notice-list.ts`처럼 작성할 수 있다.

### 14.5 타입 네이밍

타입 이름은 역할이 드러나야 한다.

권장 예시는 다음과 같다.

- `NoticeListItemResponse`
- `NoticeDetailResponse`
- `NoticeSearchParams`
- `NoticeFormValues`
- `NoticeViewModel`
- `NoticeFilterState`
- `NoticeTableProps`

지양 예시는 다음과 같다.

- `NoticeType`
- `DataType`
- `CommonResponseType`
- `ItemType`

### 14.6 interface / type 사용 기준

팀 기준을 하나로 맞추는 것이 중요하다.

권장 기준 예시는 다음과 같다.

- 객체 구조 표현은 `interface` 또는 `type` 중 하나로 통일
- union, mapped type, utility type 조합은 `type` 사용
- 라이브러리 확장이나 declaration merging이 필요한 경우 `interface` 사용

핵심은 혼용 자체보다 **일관성**이다.

---

## 15. 의존 방향 원칙

기본 의존 흐름은 다음과 같다.

- `app`은 `domain`과 `common`을 조합한다.
- `domain`은 `common`을 사용할 수 있다.
- `common`은 `domain`을 알면 안 된다.

도메인 내부에서는 대체로 다음 흐름을 따른다.

- `components`는 `hooks`, `model`, `types`, `common.ui`를 사용할 수 있다.
- `hooks`는 `api`, `store`, `model`, `types`를 사용할 수 있다.
- `api`는 `types`와 `common.api`를 사용할 수 있다.
- `model`은 `types`를 사용할 수 있다.
- `store`는 `types`를 사용할 수 있다.

추가 원칙은 다음과 같다.

- `app`에서 도메인 세부 구현을 길게 작성하지 않는다.
- `common`이 특정 도메인에 의존하면 안 된다.
- `api`가 화면 표현을 알면 안 된다.
- `components`가 서버 통신 세부사항을 길게 가지지 않게 한다.
- `types`는 가능한 한 하위 의존성을 가지지 않는다.
- `types`가 `components`를 참조하는 구조는 만들지 않는다.

---

## 16. 구현 규칙

### 16.1 라우트 / 페이지 규칙

- 페이지는 진입점 역할만 담당한다.
- 페이지에서 API 호출 세부 구현을 직접 길게 작성하지 않는다.
- 페이지는 레이아웃과 도메인 컴포넌트를 조합한다.
- 페이지 단위 권한 처리나 가드는 여기서 조립한다.
- Next.js에서도 route file은 얇게 유지한다.

### 16.2 컴포넌트 규칙

- 컴포넌트는 표현 책임을 우선한다.
- 도메인 컴포넌트는 해당 도메인 안에 둔다.
- 범용 UI는 `common.ui`에 둔다.
- 거대한 페이지 컴포넌트는 섹션 단위로 분리한다.
- props 타입은 명시적으로 선언한다.

### 16.3 Hook 규칙

- Hook은 사용자 상호작용 흐름과 화면 로직을 담당한다.
- 데이터 조회, 액션, 필터 로직은 hook으로 분리할 수 있다.
- 단순 wrapper 수준의 무의미한 hook은 만들지 않는다.
- hook은 도메인 의미가 이름에 드러나야 한다.
- 반환 타입이 너무 복잡해지면 구조를 단순화한다.

### 16.4 API 규칙

- HTTP 요청 책임에 집중한다.
- 공통 헤더, 인증, 인터셉터는 `common.api`에서 처리한다.
- 도메인 API 파일은 요청 조립과 호출만 담당한다.
- UI 상태 제어 로직은 넣지 않는다.
- request / response 타입을 명시적으로 연결한다.

### 16.5 Store 규칙

- 전역 상태를 남발하지 않는다.
- 도메인 단위 공유 상태만 store로 관리한다.
- 서버 상태 캐시는 store와 별도로 생각한다.
- 필터, 탭, 선택값처럼 클라이언트 상태 성격이 강한 것에 우선 적용한다.
- state와 action 구조가 과도하게 복잡해지지 않도록 관리한다.

### 16.6 Type 규칙

- API 타입과 UI 타입을 구분한다.
- Form 타입을 필요 시 분리한다.
- 도메인 전용 타입은 도메인 안에 둔다.
- 공통 타입은 최소화한다.
- 추론이 충분히 가능한 곳은 과도한 타입 선언을 피한다.
- 외부 입력은 검증 없이 신뢰하지 않는다.

### 16.7 Runtime Validation 규칙

TypeScript는 런타임 검증을 하지 못하므로, 외부 입력이 중요한 지점에서는 별도 검증이 필요하다.

예를 들면 다음과 같다.

- 폼 입력 검증
- query string 파싱
- API 응답 파싱이 중요한 경우
- localStorage / sessionStorage 데이터 복원
- 외부 SDK 응답 처리

필요 시 Zod 같은 검증 도구를 사용할 수 있다.

이 경우 schema는 도메인 가까이에 두고, 타입과 역할이 섞이지 않게 관리한다.

---

## 17. 표준 예시 트리

```
src
├── app
│   ├── guards
│   │   └── auth-guard.tsx
│   ├── layout
│   │   ├── AppLayout.tsx
│   │   └── AdminLayout.tsx
│   ├── providers
│   │   ├── query-provider.tsx
│   │   └── router-provider.tsx
│   └── routes
│       ├── analytics-page.tsx
│       ├── inventory-page.tsx
│       ├── notice-page.tsx
│       ├── order-page.tsx
│       └── my-page.tsx
├── common
│   ├── api
│   │   ├── api-client.ts
│   │   └── interceptors.ts
│   ├── assets
│   │   └── images
│   ├── constants
│   │   └── app-constants.ts
│   ├── hooks
│   │   └── use-modal.ts
│   ├── lib
│   │   ├── query-client.ts
│   │   └── router.ts
│   ├── types
│   │   ├── api.types.ts
│   │   └── common-props.types.ts
│   ├── ui
│   │   ├── Button.tsx
│   │   ├── Modal.tsx
│   │   ├── Pagination.tsx
│   │   └── Table.tsx
│   └── utils
│       └── date-format.ts
└── domain
    ├── analytics
    │   ├── api
    │   ├── components
    │   ├── hooks
    │   ├── model
    │   ├── store
    │   └── types
    ├── inventory
    │   ├── api
    │   ├── components
    │   ├── hooks
    │   ├── model
    │   ├── store
    │   └── types
    ├── my-page
    │   ├── api
    │   ├── components
    │   ├── hooks
    │   ├── model
    │   ├── store
    │   └── types
    ├── notice
    │   ├── api
    │   │   └── notice-api.ts
    │   ├── components
    │   │   ├── NoticeDetailSection.tsx
    │   │   ├── NoticeListSection.tsx
    │   │   ├── NoticeSearchForm.tsx
    │   │   └── NoticeTable.tsx
    │   ├── hooks
    │   │   ├── use-notice-actions.ts
    │   │   ├── use-notice-detail.ts
    │   │   └── use-notice-list.ts
    │   ├── model
    │   │   ├── notice-constants.ts
    │   │   ├── notice-mapper.ts
    │   │   └── notice-validator.ts
    │   ├── store
    │   │   └── notice-filter-store.ts
    │   └── types
    │       ├── notice-api.types.ts
    │       ├── notice-form.types.ts
    │       ├── notice-model.types.ts
    │       └── notice-props.types.ts
    └── order
        ├── api
        ├── components
        ├── hooks
        ├── model
        ├── store
        └── types
```

---

## 18. 이 구조의 장점

이 구조의 장점은 다음과 같다.

- 도메인별 응집도가 높다.
- 코드 위치를 예측하기 쉽다.
- 페이지와 도메인 로직이 분리된다.
- 공통 UI와 도메인 UI 경계가 명확하다.
- 타입 정의 위치와 역할이 명확해진다.
- API 타입과 UI 타입이 섞이는 문제를 줄일 수 있다.
- React와 Next.js를 같은 사고방식으로 운영할 수 있다.
- 협업 시 탐색 비용이 낮다.
- 프론트엔드가 커져도 구조가 무너지기 어렵다.

---

## 19. 주의사항

이 구조를 사용할 때는 다음을 경계한다.

### 19.1 common 비대화

공통이라는 이유로 모든 것을 `common`에 넣으면 구조가 무너진다.

### 19.2 페이지 비대화

페이지 파일에 조회, 상태, 가공, 이벤트 처리까지 모두 넣으면 구조가 빠르게 붕괴한다.

### 19.3 hook 비대화

하나의 hook이 조회, 생성, 수정, 삭제, 필터, 폼, 권한까지 다 가지면 관리가 어려워진다.

### 19.4 타입 비대화

하나의 `types.ts` 파일에 모든 타입을 몰아넣으면 오히려 탐색성이 떨어진다.

### 19.5 공통 타입 남용

조금 비슷하다고 전부 `common.types`로 올리면 도메인 경계가 무너진다.

### 19.6 any 의존

마이그레이션 편의 때문에 `any`를 방치하면 TypeScript 도입 효과가 급격히 떨어진다.

### 19.7 타입 체조

타입 안정성을 높이려다 오히려 구현이 읽기 어렵고 수정이 어려워지면 실패다.

---

## 20. 최종 운영 원칙

이 프로젝트의 TypeScript 프론트엔드 구조는 아래 원칙으로 운영한다.

1. 기능은 `domain` 중심으로 구성한다.
2. 전역 조립은 `app`에서 담당한다.
3. 공통 기능은 `common`에 둔다.
4. 각 도메인은 기본적으로 `api`, `components`, `hooks`, `model`, `store`, `types` 구조를 가진다.
5. 페이지와 라우트는 얇게 유지한다.
6. 공통 UI와 도메인 UI를 분리한다.
7. 서버 통신은 도메인 `api`로 분리한다.
8. 화면 로직은 `hooks`로 정리한다.
9. 타입은 역할 기준으로 분리한다.
10. API 타입과 UI 타입을 필요 시 구분한다.
11. 공통 타입은 최소화한다.
12. 구조의 목적은 이론적 순수성이 아니라 실무 유지보수성과 협업 효율이다.

---

## 21. 한 줄 정의

> 이 표준은 **도메인 중심 패키지 구조 + 프론트엔드 조립 레이어 분리 + 타입 역할 분리 + 과도한 추상화 배제**를 기반으로 하는 React + Next.js + TypeScript 공통 프론트엔드 아키텍처 표준이다.
>