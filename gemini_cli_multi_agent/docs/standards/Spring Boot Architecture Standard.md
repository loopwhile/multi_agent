# Spring Boot Architecture Standard

# Spring Boot Architecture Standard

> Spring Boot 기반 백엔드 프로젝트 표준 구조 문서
> 
> 
> 본 문서는 팀에서 사용하는 **도메인 중심 패키지 구조**와 **역할 분리 규칙**을 정의한다.
> 
> 본 구조는 이론적인 정통 DDD와 완전히 동일한 개념이 아니라, **실무 유지보수성과 협업 효율을 우선한 팀 표준 구조**이다.
> 

---

## 1. 목적

이 표준의 목적은 다음과 같다.

- 프로젝트 구조를 일관되게 유지한다.
- 신규 기능 추가 시 파일과 책임의 위치를 명확하게 한다.
- 도메인별 응집도를 높이고 공통 관심사를 분리한다.
- 협업 시 코드 탐색 비용을 줄인다.
- 유지보수, 리팩토링, 온보딩 비용을 낮춘다.

---

## 2. 핵심 원칙

### 2.1 도메인 중심 구조

기능은 가능한 한 `domain` 아래의 각 도메인 폴더 단위로 관리한다.

예를 들어 `analytics`, `myPage`, `notice`, `fcm`, `order`, `inventory` 같은 실제 비즈니스 단위를 기준으로 나눈다.

즉, 기술 레이어 전체를 한 곳에 모으는 구조보다, **도메인별로 응집된 구조**를 우선한다.

### 2.2 공통 관심사 분리

전역 설정이나 여러 도메인에서 공통으로 사용하는 기능은 `config`, `common`으로 분리한다.

### 2.3 과도한 추상화 지양

형식적인 추상화는 만들지 않는다.

- `service interface`는 기본적으로 생성하지 않는다.
- `ServiceImpl` 구조도 기본적으로 사용하지 않는다.
- 실제 다형성, 구현 교체, 테스트 분리 등의 필요가 있을 때만 예외적으로 도입한다.

### 2.4 실무형 DDD 스타일

이 구조는 이론적인 DDD를 그대로 재현하는 것이 목적이 아니다.

목표는 다음과 같다.

- 도메인별 책임 분리
- 유지보수 가능한 구조
- 빠른 기능 추가와 수정
- 협업 시 예측 가능한 패키지 구조

---

## 3. 최상위 패키지 구조

최상위 패키지는 `config`, `common`, `domain`으로 구성한다.

### 3.1 config

애플리케이션 전역 설정, 프레임워크 설정, 라이브러리 연동 설정을 둔다.

예시 성격은 다음과 같다.

- interceptor
- security
- querydsl
- swagger

`config`에는 설정 성격의 코드만 둔다.

비즈니스 로직은 넣지 않는다.

### 3.2 common

여러 도메인에서 공통으로 사용하는 기능을 둔다.

예시 성격은 다음과 같다.

- error
- log
- util
- ckeditor

`common`은 공통 모듈 저장소이지, 애매한 코드들을 몰아넣는 공간이 아니다.

특정 도메인에 종속된 기능은 `common`이 아니라 해당 도메인 안에 둔다.

### 3.3 domain

실제 비즈니스 기능을 담당하는 도메인 패키지를 둔다.

예시는 다음과 같다.

- analytics
- myPage
- notice
- fcm
- order
- inventory

---

## 4. 도메인 패키지 기본 구조

각 도메인은 아래 역할을 기본으로 가진다.

- `controller`
- `dto`
- `entity`
- `service`
- `repository`

도메인마다 필요한 하위 패키지를 동일한 패턴으로 유지하는 것을 기본 원칙으로 한다.

---

## 5. controller 역할

`controller`는 외부 요청의 진입점이다.

포함 대상은 다음과 같다.

- `Controller`
- `RestController`

역할은 다음과 같다.

- HTTP 요청 수신
- Request DTO 바인딩
- 인증/인가 정보 전달
- Service 호출
- Response 반환

규칙은 다음과 같다.

- 비즈니스 로직을 넣지 않는다.
- Entity를 직접 반환하지 않는다.
- 가능한 한 Request/Response DTO를 사용한다.
- 검증과 요청 해석까지만 담당하고, 실제 처리 흐름은 Service로 넘긴다.

---

## 6. dto 역할

`dto`는 계층 간 데이터 전달 객체를 둔다.

포함 대상은 다음과 같다.

- Request DTO
- Response DTO
- Search Condition DTO
- 화면 응답용 DTO
- 필요 시 내부 전달용 DTO

규칙은 다음과 같다.

- API 응답에 Entity를 직접 노출하지 않는다.
- DTO 이름만 보고 역할을 유추할 수 있어야 한다.
- 요청 DTO와 응답 DTO는 가능하면 분리한다.
- 범용적인 이름보다 목적이 드러나는 이름을 사용한다.

예를 들면 다음과 같은 네이밍을 권장한다.

- `NoticeCreateRequestDto`
- `NoticeUpdateRequestDto`
- `NoticeDetailResponseDto`
- `NoticeListResponseDto`
- `NoticeSearchCondition`

`NoticeDto`, `CommonDto`, `ResultDto`처럼 의미가 넓고 모호한 이름은 지양한다.

---

## 7. entity 역할

`entity`는 도메인의 핵심 데이터 모델을 둔다.

포함 대상은 다음과 같다.

- JPA Entity
- Enum
- 필요 시 값 객체 성격 클래스

규칙은 다음과 같다.

- 해당 도메인의 Entity와 Enum은 기본적으로 해당 도메인 `entity` 패키지에 둔다.
- 상태 변경은 의미 있는 메서드로 표현한다.
- 무분별한 setter 사용은 지양한다.
- 연관관계는 꼭 필요한 경우에만 사용한다.
- 양방향 관계는 유지 비용이 크므로 신중히 사용한다.

---

## 8. service 역할

`service`는 도메인의 비즈니스 로직을 담당한다.

역할은 다음과 같다.

- 유스케이스 실행
- 트랜잭션 처리
- 도메인 정책 적용
- Repository 조합
- 처리 흐름 제어

규칙은 다음과 같다.

- 인터페이스는 기본적으로 만들지 않는다.
- 형식적인 `ServiceImpl` 구조는 사용하지 않는다.
- Service는 Controller보다 높은 수준의 비즈니스 의미를 가져야 한다.
- Service가 단순 전달 계층으로만 남지 않게 한다.
- 하나의 Service가 과도하게 비대해지면 기능 기준으로 분리 검토한다.

메서드명은 행위 중심으로 작성한다.

예를 들면 다음과 같다.

- `createNotice`
- `updateNotice`
- `deleteNotice`
- `getNoticeDetail`
- `getNoticeList`

---

## 9. repository 역할

`repository`는 영속성 계층을 담당한다.

구성은 다음을 기본으로 한다.

- `Repository`
- `RepositoryCustom`
- `RepositoryImpl`

역할은 다음과 같다.

- DB 저장/조회
- QueryDSL 기반 동적 쿼리 처리
- 복잡한 목록 조회, 검색, 조건 조회 분리

규칙은 다음과 같다.

- 단순 CRUD는 기본 `Repository`에서 처리한다.
- 복잡한 조회는 `Custom + Impl`로 분리한다.
- 비즈니스 정책은 Repository보다 Service에 둔다.
- Repository는 저장/조회 책임에 집중한다.

---

## 10. config 구성 원칙

`config`는 전역 설정만 관리한다.

포함 가능한 대표 예시는 다음과 같다.

- Spring Security 설정
- Interceptor 설정
- QueryDSL 설정
- Swagger/OpenAPI 설정
- WebMvc 설정
- CORS 설정
- Jackson/ObjectMapper 설정

규칙은 다음과 같다.

- 도메인 정책을 넣지 않는다.
- 설정 클래스는 목적이 명확해야 한다.
- 특정 도메인에 강하게 종속된 코드는 가급적 해당 도메인으로 이동한다.

---

## 11. common 구성 원칙

`common`은 여러 도메인에서 공통으로 재사용되는 기능을 관리한다.

포함 가능한 대표 예시는 다음과 같다.

- 공통 예외 처리
- 에러 코드
- 공통 응답 객체
- 로깅 관련 기능
- 공통 유틸
- 외부 에디터나 공통 모듈 연동 코드

규칙은 다음과 같다.

- 특정 도메인 전용 기능은 `common`에 두지 않는다.
- `common`이 비대해지지 않도록 관리한다.
- `util`은 최소화한다.
- 진짜로 공통인지 먼저 검토한 후 이동한다.

---

## 12. 네이밍 규칙

### 12.1 클래스 네이밍

클래스명은 역할이 명확해야 한다.

- Controller: `{Domain}Controller`, `{Domain}RestController`
- Service: `{Domain}Service`
- Repository: `{Domain}Repository`
- Repository Custom: `{Domain}RepositoryCustom`
- Repository Impl: `{Domain}RepositoryImpl`
- Entity: 도메인 개념 명사
- Enum: 상태/타입 의미가 드러나는 명사

예시는 다음과 같다.

- `NoticeController`
- `NoticeRestController`
- `NoticeService`
- `NoticeRepository`
- `NoticeRepositoryCustom`
- `NoticeRepositoryImpl`
- `Notice`
- `NoticeType`
- `NoticeStatus`

### 12.2 DTO 네이밍

DTO는 역할이 이름에 드러나야 한다.

권장 예시는 다음과 같다.

- `NoticeCreateRequestDto`
- `NoticeUpdateRequestDto`
- `NoticeDetailResponseDto`
- `NoticeListResponseDto`
- `NoticeSearchCondition`

지양 예시는 다음과 같다.

- `NoticeDto`
- `ResponseDto`
- `DataDto`

---

## 13. 의존 방향 원칙

기본 의존 흐름은 다음과 같다.

- Controller는 Service를 호출한다.
- Service는 Repository를 호출한다.
- Repository는 DB와 상호작용한다.

추가 원칙은 다음과 같다.

- Controller는 Repository를 직접 호출하지 않는다.
- Controller는 Entity를 직접 반환하지 않는다.
- Repository는 HTTP 개념을 알면 안 된다.
- Config와 Common은 도메인을 지원할 수 있지만, 도메인 정책을 대체하면 안 된다.

---

## 14. 구현 규칙

### 14.1 Controller 규칙

- Request/Response DTO를 사용한다.
- 검증은 `@Valid` 등을 적극 활용한다.
- 비즈니스 로직은 Service로 위임한다.
- 예외 처리는 공통 예외 처리기로 위임한다.

### 14.2 Service 규칙

- 유스케이스 단위의 비즈니스 흐름을 관리한다.
- 트랜잭션 범위를 명확히 한다.
- 메서드명은 행위 중심으로 작성한다.
- 불필요한 public 메서드는 만들지 않는다.

### 14.3 Repository 규칙

- 단순 조회는 기본 Repository에서 처리한다.
- 조건이 복잡한 조회는 QueryDSL Custom 구현으로 분리한다.
- 페이징, 정렬, 검색 조건이 있는 조회는 성능을 고려한다.

### 14.4 Entity 규칙

- 연관관계 남용을 피한다.
- 의미 없는 setter를 지양한다.
- 상태 변경은 메서드로 표현한다.
- 생성 시 필수값은 생성자나 정적 팩토리로 보장한다.

---

## 15. 예외 / 로그 / 문서화 / QueryDSL 기준

### 15.1 예외 처리

공통 예외 처리는 `common.error`에서 관리한다.

원칙은 다음과 같다.

- 정책 위반은 명확한 예외로 표현한다.
- 단순 `RuntimeException` 남발을 지양한다.
- 에러 메시지보다 에러 코드 체계를 우선 관리한다.

### 15.2 로그 처리

로그 관련 공통 처리는 `common.log`에서 관리한다.

원칙은 다음과 같다.

- 핵심 흐름만 로그를 남긴다.
- 민감정보는 로그에 남기지 않는다.
- 디버깅용 로그를 장기 방치하지 않는다.

### 15.3 Swagger / API 문서화

Swagger/OpenAPI 관련 설정은 `config.swagger`에서 관리한다.

원칙은 다음과 같다.

- 외부 노출 API는 문서화한다.
- Request/Response DTO 기준으로 문서화한다.
- Entity가 API 스키마로 직접 노출되지 않도록 한다.

### 15.4 QueryDSL

QueryDSL 설정은 `config.querydsl`에서 관리한다.

활용 기준은 다음과 같다.

- 동적 검색 조건
- 복잡한 목록 조회
- 다중 조건 페이징
- 커스텀 projection

복잡한 조회는 `RepositoryCustom`, `RepositoryImpl`로 분리한다.

---

## 16. 표준 예시 트리

```
src/main/java/com/example/project
├── config
│   ├── interceptor
│   ├── querydsl
│   ├── security
│   └── swagger
├── common
│   ├── ckeditor
│   ├── error
│   ├── log
│   └── util
└── domain
    ├── analytics
    │   ├── controller
    │   ├── dto
    │   ├── entity
    │   ├── repository
    │   └── service
    ├── fcm
    │   ├── controller
    │   ├── dto
    │   ├── entity
    │   ├── repository
    │   └── service
    ├── inventory
    │   ├── controller
    │   ├── dto
    │   ├── entity
    │   ├── repository
    │   └── service
    ├── myPage
    │   ├── controller
    │   ├── dto
    │   ├── entity
    │   ├── repository
    │   └── service
    ├── notice
    │   ├── controller
    │   │   ├── NoticeController.java
    │   │   └── NoticeRestController.java
    │   ├── dto
    │   │   ├── NoticeCreateRequestDto.java
    │   │   ├── NoticeUpdateRequestDto.java
    │   │   ├── NoticeDetailResponseDto.java
    │   │   ├── NoticeListResponseDto.java
    │   │   └── NoticeSearchCondition.java
    │   ├── entity
    │   │   ├── Notice.java
    │   │   ├── NoticeStatus.java
    │   │   └── NoticeType.java
    │   ├── repository
    │   │   ├── NoticeRepository.java
    │   │   ├── NoticeRepositoryCustom.java
    │   │   └── NoticeRepositoryImpl.java
    │   └── service
    │       └── NoticeService.java
    └── order
        ├── controller
        ├── dto
        ├── entity
        ├── repository
        └── service
```

---

## 17. 이 구조의 장점

이 구조의 장점은 다음과 같다.

- 도메인별 응집도가 높다.
- 코드 위치를 예측하기 쉽다.
- 협업 시 탐색 비용이 낮다.
- 실무에서 유지보수하기 좋다.
- 기술 설정과 비즈니스 영역이 분리된다.
- 과도한 추상화 없이 구조 일관성을 유지할 수 있다.

---

## 18. 주의사항

이 구조를 사용할 때는 다음을 경계한다.

### 18.1 common 비대화

공통이라는 이유로 모든 코드를 `common`에 넣으면 구조가 무너진다.

### 18.2 service 비대화

도메인 서비스 하나가 지나치게 커지면 기능 단위 분리를 검토해야 한다.

### 18.3 형식적인 DDD 흉내

이 구조의 목적은 DDD처럼 보이는 것이 아니라,

실제로 유지보수 가능한 시스템을 만드는 것이다.

### 18.4 Entity / DTO 경계 붕괴

편의상 Entity를 직접 API 응답으로 사용하면 유지보수 비용이 커진다.

---

## 19. 최종 운영 원칙

이 프로젝트의 백엔드 구조는 아래 원칙으로 운영한다.

1. 기능은 `domain` 중심으로 구성한다.
2. 전역 설정은 `config`에 둔다.
3. 공통 기능은 `common`에 둔다.
4. 각 도메인은 기본적으로 `controller`, `dto`, `entity`, `service`, `repository` 구조를 가진다.
5. 서비스 인터페이스는 기본적으로 만들지 않는다.
6. 복잡한 조회는 QueryDSL 기반 Custom Repository로 분리한다.
7. Entity와 DTO의 역할을 분리한다.
8. 구조의 목적은 이론적 순수성이 아니라 실무 유지보수성과 협업 효율이다.

---

## 20. 한 줄 정의

> 이 표준은 **도메인 중심 패키지 구조 + 실무형 계층 분리 + 과도한 추상화 배제**를 기반으로 하는 Spring Boot 백엔드 아키텍처 표준이다.
>