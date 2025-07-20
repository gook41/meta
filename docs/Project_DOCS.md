나 혼자 하는 WMS 프로젝트 문서 양식/가이드라인 
Jira 쓰니까 Epic 단위로 나눈 다음 이 문서들 기준으로 필요한 태스크 나눠서 관리함.

## 🔖 1. 기획 문서 (기본 @docs/planning.md)

### 📌 프로젝트 개요

- **프로젝트명**: GOOK WMS 시스템
- **목표**:
    - 사내용 창고관리 시스템을 웹 + 데스크탑 환경(Electron)에서 제공
    - 입/출고, 재고 관리 기능을 실시간으로 처리
    - 보안은 JWT 기반 인증 + (Optional) OAuth2 (구글 또는 사내 SSO)


### 📋 주요 요구 사항

| 구분 | 내용 |
| :-- | :-- |
| 인증 방식 | JWT 기반 로그인, OAuth2 (구글, SSO 논의 중) |
| 사용자 구분 | Role: 관리자 / 직원 등 |
| 데이터베이스 | dev: H2 / prod: PostgreSQL |
| UI/UX 환경 | Electron 기반 프론트, 내부직원 전용 UI/UX |
| 처리방식 | 실시간 입/출고 (배치는 논의 중) |
| 코드 인식 | QR 코드로 입출고 처리 |
| 로그 | 사용자, 시스템 로그 저장 및 조회 기능 |
| 문서화 | 개발 중 Swagger 태깅 전체 공개, 배포 시 비공개 전환 |

### ✅ 기능 명세 (카테고리별 기능 정의)

#### A. 인증 / 계정

- 회원가입, 로그인 (JWT)
- OAuth2 로그인 (구글)
- 사용자 정보 조회/수정


#### B. 입고 기능

- 입고 요청 생성
- 입고 품목 등록 (QR 스캔 기반)
- 입고 완료 처리


#### C. 출고 기능

- 출고 요청 생성
- 출고 품목 등록 (QR 스캔 기반)
- 출고 완료 처리


#### D. 재고 기능

- 현재 재고 조회 (필터 가능)
- 재고 위치 변경
- 재고 실사 등록


#### E. 관리자 기능

- 사용자 관리 (목록/등록/삭제)
- 시스템 로그, 입출고 이력


## 🛠️ 2. 설계 문서

### 📦 백엔드 설계 (@docs/backend-design.md)

#### 1. 전체 아키텍처

```text
[Electron 앱] → [Spring API 서버] ↔ [PostgreSQL]
                        ↓
                  [JWT Auth + OAuth2]
```


#### 2. 기술 스택

- Java 21, Spring Boot 3.4.7
- JPA, QueryDSL, Spring Security
- H2 (dev), PostgreSQL (prod)
- Gradle, Docker, docker-compose
- Swagger(Springdoc) for API 문서 자동화
- QR 인식 처리: 프론트에서 찍은 값 받아서 처리만 백엔드에서 함


#### 3. 도메인 모델 (Entity 관계 – ERD 생략.)

| Entity | 필드 | 설명 |
| :-- | :-- | :-- |
| User | id, email, role, createdAt | 사용자 기본 정보 |
| Inventory | id, location, qty, productId | 재고 위치 및 수량 |
| InboundOrder | id, items, createdAt | 입고 주문 |
| OutboundOrder | id, items, createdAt | 출고 주문 |
| Log | id, userId, action, timestamp | 작업 로그 |

#### 4. 디렉토리 구조 예시

도메인 디렉토리 컨벤션.

```
com.gook.app
├── config
├── domain
│   ├── user
│   ├── inventory
│   └── order (입출고 통합)
├── security
│   ├── jwt
│   └── oauth
├── controller
├── service
├── repository
└── dto
```


#### 5. 주요 설계 포인트

- OAuth 로그인 성공 시 토큰(JWT) 응답
- QR 코드는 UUID 기반으로 매핑되도록 설계
- Swagger는 springdoc-openapi 기준 프로파일 dev 에서만 활성화
- 배포는 도커 통해서 완전 이식 가능하게 만듦
- logback 설정으로 access log 따로 분리


### 💻 프론트 설계 (@docs/frontend-design.md)

#### 1. 전체 구조

- Electron (React or Vue)
- 백엔드와 REST API로 통신
- QR 인식은 Electron에서 WebCam 접근해서 처리
- API는 HTTPS → localhost 통신 예외 허용해야 함


#### 2. 사용자 화면 설계

| 페이지 | 기능 |
| :-- | :-- |
| 로그인 | JWT 발급 및 저장 (로컬 스토리지) |
| 대시보드 | 입/출고 현황 요약, 실시간 재고 |
| 입고 등록 | QR 스캔 → 백엔드 입고 생성 API 호출 |
| 출고 등록 | QR 스캔 → 출고 처리 |
| 사용자 관리 | 관리자 전용 메뉴, 사용자 추가/차단 기능 등 |

#### 3. 주요 기술 결정 사항

- Electron 환경 설정에 따라 백엔드 주소 설정 동적으로 처리 (.env 사용)
- QR 리더기 or WebCam 활용
- API 실패/성공 응답에 따른 UI 표시 처리 명확하게


## 📝 문서 작성 가이드라인 (@docs/README.md)

### ✅ 공통 문서 원칙

| 항목 | 기준 |
| :-- | :-- |
| 용어 | 내부 기술 용어 사전 유지 (`QR`, `입고`, `출고`, `JWT`, `Session`) |
| 포맷 | 마크다운 `.md`, VSCode 기준 렌더링 최적화 |
| 문서 저장 위치 | `/docs/` 폴더 내 모듈 단위 기능 정리 |
| 버전 관리 | GitHub에 push할 때마다 문서 최신화 여부 확인 |
| 차트/그림 | 필요 시 draw.io → SVG로 export해서 포함 |

### 문서 체크리스트

- [ ] 기획 문서 → 모든 요구사항 반영됨?
- [ ] 백엔드 설계 → 엔티티/서비스 관계 정리됨?
- [ ] 프론트 설계 → 화면-API 연결 플로우 있음?
- [ ] Swagger → dev profile에서 openapi 접속 확인됨?
- [ ] 문서 최신화 날짜 표기됨?

이렇게 문서 틀/템플릿 만들고 나면,

각 기능 구현할 때는 이 문서 기준으로 Jira에서 할 일 쪼개기.

