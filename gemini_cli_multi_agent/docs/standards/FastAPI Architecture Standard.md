# FastAPI Architecture Standard

# FastAPI Architecture Standard

> FastAPI 기반 백엔드 프로젝트 표준 구조 문서
> 
> 
> 본 문서는 팀에서 사용하는 **도메인 중심 패키지 구조**와 **역할 분리 규칙**을 정의한다.
> 
> 본 구조는 이론적인 정통 DDD를 그대로 구현하는 것이 아니라, **실무 유지보수성, 협업 효율, 확장성**을 우선한 팀 표준 구조이다.
> 

---

## 1. 목적

이 표준의 목적은 다음과 같다.

- 프로젝트 구조를 일관되게 유지한다.
- 신규 기능 추가 시 파일과 책임의 위치를 명확하게 한다.
- 도메인별 응집도를 높이고 공통 관심사를 분리한다.
- 협업 시 코드 탐색 비용을 줄인다.
- 유지보수, 리팩토링, 온보딩 비용을 낮춘다.
- FastAPI 프로젝트를 단순 파일 나열이 아니라 **운영 가능한 구조**로 관리한다.

---

## 2. 핵심 원칙

### 2.1 도메인 중심 구조

기능은 가능한 한 `domain` 아래의 각 도메인 폴더 단위로 관리한다.

예를 들어 `analytics`, `my_page`, `notice`, `fcm`, `order`, `inventory` 같은 실제 비즈니스 단위를 기준으로 나눈다.

즉, 파일 종류별로 전역 분리하는 구조보다, **도메인별로 응집된 구조**를 우선한다.

### 2.2 공통 관심사 분리

전역 설정이나 여러 도메인에서 공통으로 사용하는 기능은 `config`, `common`으로 분리한다.

### 2.3 과도한 추상화 지양

형식적인 추상화는 만들지 않는다.

- 의미 없는 base class 남발을 지양한다.
- interface 역할을 흉내 내는 불필요한 추상 클래스는 기본적으로 만들지 않는다.
- 억지로 계층을 늘리는 구조보다, 실제 책임 분리가 필요한 지점을 우선한다.

### 2.4 실무형 계층 분리

FastAPI 프로젝트는 보통 단순한 `router + crud` 예제로 시작하지만, 실제 운영에서는 다음이 분리되어야 한다.

- 요청 진입점
- 검증/직렬화
- 비즈니스 로직
- 영속성 로직
- 공통 설정/예외/인증

본 문서는 그 분리를 **도메인 중심 구조** 안에서 유지하는 것을 목표로 한다.

### 2.5 프레임워크보다 구조 우선

FastAPI는 가볍고 유연하지만, 그만큼 구조가 쉽게 무너질 수 있다.

따라서 중요한 것은 FastAPI 문법이 아니라 아래 원칙이다.

- 어디에 무엇을 둘 것인가
- 어떤 레이어가 어떤 책임을 가져야 하는가
- 무엇을 공통화하고 무엇을 도메인 안에 둘 것인가

---

## 3. 최상위 패키지 구조

최상위 패키지는 `config`, `common`, `domain`으로 구성한다.

### 3.1 config

애플리케이션 전역 설정, 프레임워크 설정, 인프라 연동 설정을 둔다.

예시 성격은 다음과 같다.

- settings
- database
- auth
- middleware
- docs

`config`에는 설정 성격의 코드만 둔다.

비즈니스 로직은 넣지 않는다.

### 3.2 common

여러 도메인에서 공통으로 사용하는 기능을 둔다.

예시 성격은 다음과 같다.

- error
- response
- logging
- utils
- pagination

`common`은 공통 모듈 저장소이지, 애매한 코드들을 몰아넣는 공간이 아니다.

특정 도메인에 종속된 기능은 `common`이 아니라 해당 도메인 안에 둔다.

### 3.3 domain

실제 비즈니스 기능을 담당하는 도메인 패키지를 둔다.

예시는 다음과 같다.

- analytics
- my_page
- notice
- fcm
- order
- inventory

---

## 4. 도메인 패키지 기본 구조

각 도메인은 아래 역할을 기본으로 가진다.

- `router`
- `schemas`
- `models`
- `service`
- `repository`

도메인마다 필요한 하위 패키지를 동일한 패턴으로 유지하는 것을 기본 원칙으로 한다.

FastAPI에서는 일반적으로 `controller` 대신 `router`가 더 자연스럽다.

또한 DTO 역할은 보통 `schemas`가 담당한다.

---

## 5. router 역할

`router`는 외부 요청의 진입점이다.

포함 대상은 다음과 같다.

- `APIRouter`
- 엔드포인트 함수
- 의존성 주입 진입점
- 요청/응답 스키마 연결

역할은 다음과 같다.

- HTTP 요청 수신
- Path / Query / Body 파라미터 해석
- 인증 사용자 정보 전달
- Service 호출
- Response 반환

규칙은 다음과 같다.

- 비즈니스 로직을 넣지 않는다.
- DB 처리 로직을 직접 넣지 않는다.
- 가능한 한 Request/Response Schema를 사용한다.
- 검증과 요청 해석까지만 담당하고, 실제 처리 흐름은 Service로 넘긴다.
- Router 함수는 얇고 예측 가능해야 한다.

---

## 6. schemas 역할

`schemas`는 요청/응답 데이터의 검증과 직렬화를 담당한다.

포함 대상은 다음과 같다.

- Request Schema
- Response Schema
- Search Condition Schema
- List Item Schema
- 내부 전달용 Schema가 필요한 경우 그 객체

FastAPI에서는 일반적으로 Pydantic 기반 스키마를 사용한다.

규칙은 다음과 같다.

- API 요청/응답에 ORM Model을 직접 노출하지 않는다.
- Schema 이름만 보고 역할을 유추할 수 있어야 한다.
- 요청 Schema와 응답 Schema는 가능하면 분리한다.
- 목록 응답과 상세 응답도 가능하면 분리한다.
- 검색 조건은 별도 Schema로 관리할 수 있다.

예를 들면 다음과 같은 네이밍을 권장한다.

- `NoticeCreateRequest`
- `NoticeUpdateRequest`
- `NoticeDetailResponse`
- `NoticeListItem`
- `NoticeListResponse`
- `NoticeSearchParams`

`NoticeSchema`, `CommonSchema`, `ResponseData`처럼 의미가 넓고 모호한 이름은 지양한다.

---

## 7. models 역할

`models`는 도메인의 핵심 데이터 모델을 둔다.

포함 대상은 다음과 같다.

- SQLAlchemy ORM Model
- Enum
- 필요 시 값 객체 성격 클래스
- DB 테이블 매핑 객체

규칙은 다음과 같다.

- 해당 도메인의 Model과 Enum은 기본적으로 해당 도메인 `models` 패키지에 둔다.
- 상태 변경은 의미 있는 메서드 또는 명확한 필드 갱신 흐름으로 관리한다.
- 무분별한 업데이트 헬퍼 남발을 지양한다.
- 연관관계는 꼭 필요한 경우에만 사용한다.
- ORM 모델은 DB 표현이고, API 응답 객체와 동일하지 않다.

---

## 8. service 역할

`service`는 도메인의 비즈니스 로직을 담당한다.

역할은 다음과 같다.

- 유스케이스 실행
- 도메인 정책 적용
- 처리 흐름 제어
- Repository 조합
- 트랜잭션 경계 관리 보조
- 외부 연동 호출 orchestration

규칙은 다음과 같다.

- Service는 Router보다 높은 수준의 비즈니스 의미를 가져야 한다.
- Service는 단순 전달 계층으로만 남지 않게 한다.
- 여러 Repository를 조합할 수 있다.
- 하나의 Service가 과도하게 비대해지면 기능 기준으로 분리 검토한다.
- DB 세션을 직접 다루더라도 핵심 책임은 비즈니스 흐름에 있어야 한다.

메서드명은 행위 중심으로 작성한다.

예를 들면 다음과 같다.

- `create_notice`
- `update_notice`
- `delete_notice`
- `get_notice_detail`
- `get_notice_list`
- `publish_notice`

---

## 9. repository 역할

`repository`는 영속성 계층을 담당한다.

역할은 다음과 같다.

- DB 저장/조회
- 검색 조건 조합
- 목록 조회, 페이징, 정렬
- ORM 쿼리 분리
- DB 접근 세부사항 캡슐화

규칙은 다음과 같다.

- 단순 저장/조회도 Service에서 직접 하지 말고 Repository를 통해 일관되게 접근한다.
- 복잡한 조회는 Repository에 모은다.
- 비즈니스 정책은 Repository보다 Service에 둔다.
- Repository는 저장/조회 책임에 집중한다.
- SQLAlchemy 쿼리, 필터 조합, eager loading, pagination 세부사항은 Repository에 둔다.

FastAPI는 구조 제약이 약해서 Service에서 ORM 코드를 바로 쓰기 쉬운데,

프로젝트가 커질수록 Repository 분리가 유지보수에 유리하다.

---

## 10. config 구성 원칙

`config`는 전역 설정만 관리한다.

포함 가능한 대표 예시는 다음과 같다.

- 환경변수 설정
- 애플리케이션 설정 객체
- 데이터베이스 연결 설정
- 세션 팩토리
- 인증/인가 설정
- JWT 설정
- 미들웨어 등록
- CORS 설정
- OpenAPI / Docs 설정
- 로깅 설정

규칙은 다음과 같다.

- 도메인 정책을 넣지 않는다.
- 설정 클래스와 설정 함수는 목적이 명확해야 한다.
- 특정 도메인에 강하게 종속된 코드는 가급적 해당 도메인으로 이동한다.

---

## 11. common 구성 원칙

`common`은 여러 도메인에서 공통으로 재사용되는 기능을 관리한다.

포함 가능한 대표 예시는 다음과 같다.

- 공통 예외 처리
- 에러 코드
- 공통 응답 포맷
- 페이지네이션 응답
- 공통 의존성 함수
- 로깅 관련 기능
- 공통 유틸

규칙은 다음과 같다.

- 특정 도메인 전용 기능은 `common`에 두지 않는다.
- `common`이 비대해지지 않도록 관리한다.
- `utils`는 최소화한다.
- 진짜로 공통인지 먼저 검토한 후 이동한다.
- 공통 응답 포맷도 남용하지 않는다. FastAPI 기본 응답 모델 구조와 충돌하지 않게 운영한다.

---

## 12. 네이밍 규칙

### 12.1 모듈 / 파일 네이밍

파일명은 역할이 명확해야 한다.

- Router: `notice_router.py`
- Service: `notice_service.py`
- Repository: `notice_repository.py`
- Model: `notice.py`, `notice_status.py`
- Schema: `notice_request.py`, `notice_response.py`, `notice_schema.py`

프로젝트 규모가 크지 않다면 `schemas.py`, `models.py`처럼 한 파일에 묶을 수 있다.

규모가 커지면 역할 기준으로 나누는 것을 권장한다.

### 12.2 클래스 네이밍

클래스명은 역할이 명확해야 한다.

- Router 관련 태그/명칭: `notice`
- Service: `NoticeService`
- Repository: `NoticeRepository`
- ORM Model: `Notice`
- Enum: `NoticeStatus`, `NoticeType`
- Request Schema: `NoticeCreateRequest`, `NoticeUpdateRequest`
- Response Schema: `NoticeDetailResponse`, `NoticeListItem`, `NoticeListResponse`

### 12.3 함수 네이밍

함수명은 행위가 드러나야 한다.

권장 예시는 다음과 같다.

- `create_notice`
- `update_notice`
- `delete_notice`
- `get_notice_detail`
- `get_notice_list`
- `mark_notice_as_published`

지양 예시는 다음과 같다.

- `process`
- `execute`
- `handle_data`
- `run_logic`

---

## 13. 의존 방향 원칙

기본 의존 흐름은 다음과 같다.

- Router는 Service를 호출한다.
- Service는 Repository를 호출한다.
- Repository는 DB와 상호작용한다.

추가 원칙은 다음과 같다.

- Router는 Repository를 직접 호출하지 않는다.
- Router는 ORM Model을 직접 응답으로 반환하지 않는다.
- Repository는 HTTP 개념을 알면 안 된다.
- Schema는 API 계약을 표현하지만, 비즈니스 정책을 대신하지 않는다.
- Config와 Common은 도메인을 지원할 수 있지만, 도메인 정책을 대체하면 안 된다.

---

## 14. 구현 규칙

### 14.1 Router 규칙

- Request/Response Schema를 사용한다.
- 라우터 함수는 얇게 유지한다.
- 비즈니스 로직은 Service로 위임한다.
- 세션/유저/권한 등 의존성 주입은 진입점에서 정리한다.
- 엔드포인트 함수 안에서 ORM 쿼리를 직접 작성하지 않는다.

### 14.2 Service 규칙

- 유스케이스 단위의 비즈니스 흐름을 관리한다.
- 권한, 상태 전이, 정책 검증 등 핵심 규칙은 Service에 둔다.
- 메서드명은 행위 중심으로 작성한다.
- 하나의 메서드가 지나치게 많은 책임을 가지면 분리한다.
- 외부 시스템 호출이 있더라도 조합 책임은 Service가 가진다.

### 14.3 Repository 규칙

- 저장/조회 책임에 집중한다.
- 검색 조건, 정렬, 페이징 등 DB 접근 세부사항을 다룬다.
- N+1, eager loading, count 쿼리, offset/limit 등 성능을 고려한다.
- 비즈니스 정책 판단은 넣지 않는다.

### 14.4 Model 규칙

- ORM Model은 DB 구조를 표현한다.
- API 응답 객체로 직접 사용하지 않는다.
- 상태값은 Enum으로 명확히 관리한다.
- 의미 없는 공용 헬퍼 메서드 남발을 지양한다.
- created_at, updated_at 등 공통 필드는 공통 베이스 모델 도입을 검토할 수 있지만, 과도한 추상화는 피한다.

### 14.5 Schema 규칙

- Request와 Response를 분리한다.
- 목록용과 상세용을 분리한다.
- Schema는 검증과 직렬화 책임에 집중한다.
- ORM 객체를 그대로 노출하지 않고 필요한 필드만 명시한다.

---

## 15. 예외 / 인증 / 문서화 / DB 기준

### 15.1 예외 처리

공통 예외 처리는 `common.error`에서 관리한다.

원칙은 다음과 같다.

- 정책 위반은 명확한 커스텀 예외로 표현한다.
- 단순 `Exception` 남발을 지양한다.
- HTTP 상태코드와 도메인 에러코드를 분리해서 관리할 수 있다.
- FastAPI의 예외 핸들러를 전역으로 등록해 일관된 에러 응답을 유지한다.

### 15.2 인증 / 인가

인증 관련 설정은 `config.auth` 또는 유사 패키지에서 관리한다.

원칙은 다음과 같다.

- 인증 방식은 전역 정책으로 관리한다.
- 현재 사용자 조회, 토큰 검증, 권한 확인 의존성은 공통화할 수 있다.
- 실제 도메인 권한 판단은 필요 시 Service에서 한 번 더 검증한다.

### 15.3 Swagger / API 문서화

FastAPI는 기본적으로 OpenAPI 문서를 제공하므로 이를 적극 활용한다.

원칙은 다음과 같다.

- Request/Response Schema 기준으로 문서화한다.
- 엔드포인트 설명, 태그, 응답 모델을 일관되게 작성한다.
- ORM Model이 API 스키마로 직접 노출되지 않도록 한다.

### 15.4 DB / ORM 기준

DB 설정은 `config.database` 또는 유사 패키지에서 관리한다.

원칙은 다음과 같다.

- 세션 생성과 생명주기 관리는 전역 설정에서 담당한다.
- 도메인 로직은 세션 생성 방식 자체를 몰라도 되게 유지한다.
- ORM은 SQLAlchemy 계열 사용을 기본 가정하되, 구조 원칙은 다른 ORM에도 동일하게 적용 가능하다.
- 마이그레이션 도구는 Alembic 같은 별도 체계로 운영한다.

---

## 16. 표준 예시 트리

```
app
├── main.py
├── config
│   ├── auth
│   │   ├── jwt.py
│   │   └── security.py
│   ├── database
│   │   ├── base.py
│   │   ├── session.py
│   │   └── settings.py
│   ├── docs
│   │   └── openapi.py
│   └── middleware
│       └── logging_middleware.py
├── common
│   ├── error
│   │   ├── error_code.py
│   │   ├── exceptions.py
│   │   └── handlers.py
│   ├── logging
│   │   └── logger.py
│   ├── response
│   │   └── pagination.py
│   └── utils
│       └── datetime_utils.py
└── domain
    ├── analytics
    │   ├── router
    │   ├── schemas
    │   ├── models
    │   ├── repository
    │   └── service
    ├── fcm
    │   ├── router
    │   ├── schemas
    │   ├── models
    │   ├── repository
    │   └── service
    ├── inventory
    │   ├── router
    │   ├── schemas
    │   ├── models
    │   ├── repository
    │   └── service
    ├── my_page
    │   ├── router
    │   ├── schemas
    │   ├── models
    │   ├── repository
    │   └── service
    ├── notice
    │   ├── router
    │   │   └── notice_router.py
    │   ├── schemas
    │   │   ├── notice_request.py
    │   │   ├── notice_response.py
    │   │   └── notice_search.py
    │   ├── models
    │   │   ├── notice.py
    │   │   ├── notice_status.py
    │   │   └── notice_type.py
    │   ├── repository
    │   │   └── notice_repository.py
    │   └── service
    │       └── notice_service.py
    └── order
        ├── router
        ├── schemas
        ├── models
        ├── repository
        └── service
```

---

## 17. 이 구조의 장점

이 구조의 장점은 다음과 같다.

- 도메인별 응집도가 높다.
- 코드 위치를 예측하기 쉽다.
- 협업 시 탐색 비용이 낮다.
- FastAPI 특유의 자유도를 유지하면서도 구조가 무너지지 않는다.
- 설정과 비즈니스 영역이 분리된다.
- 테스트 대상이 명확해진다.
- Router, Service, Repository 책임이 분리되어 유지보수가 쉬워진다.

---

## 18. 주의사항

이 구조를 사용할 때는 다음을 경계한다.

### 18.1 common 비대화

공통이라는 이유로 모든 코드를 `common`에 넣으면 구조가 무너진다.

### 18.2 service 비대화

도메인 서비스 하나가 지나치게 커지면 기능 단위 분리를 검토해야 한다.

### 18.3 router 비대화

FastAPI는 엔드포인트 함수에 로직을 넣기 쉬워서 Router가 쉽게 비대해진다.

Router는 진입점 역할에 집중해야 한다.

### 18.4 ORM Model / Schema 경계 붕괴

편의상 ORM 객체를 그대로 응답으로 내보내면 유지보수 비용이 커진다.

### 18.5 세션 관리 혼선

세션 생성, 커밋, 롤백 책임이 중구난방이 되면 장애 대응이 어려워진다.

트랜잭션 관리 원칙을 프로젝트 초기에 명확히 정해야 한다.

---

## 19. 최종 운영 원칙

이 프로젝트의 FastAPI 백엔드 구조는 아래 원칙으로 운영한다.

1. 기능은 `domain` 중심으로 구성한다.
2. 전역 설정은 `config`에 둔다.
3. 공통 기능은 `common`에 둔다.
4. 각 도메인은 기본적으로 `router`, `schemas`, `models`, `service`, `repository` 구조를 가진다.
5. Router에는 비즈니스 로직을 넣지 않는다.
6. Schema와 ORM Model의 역할을 분리한다.
7. 복잡한 DB 접근은 Repository로 분리한다.
8. 핵심 정책과 처리 흐름은 Service에서 관리한다.
9. 구조의 목적은 이론적 순수성이 아니라 실무 유지보수성과 협업 효율이다.

---

## 20. 한 줄 정의

> 이 표준은 **도메인 중심 패키지 구조 + 실무형 계층 분리 + 과도한 추상화 배제**를 기반으로 하는 FastAPI 백엔드 아키텍처 표준이다.
>