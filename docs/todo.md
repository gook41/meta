# - 구현할 기능 목록

- yml 설정 (prod / dev)
- 작성해야할 도커파일 내용.
- 필요한 클래스 또는 bean 등.


## 1. 구현할 기능 목록 (기본적으로 WMS가 하려는 기능 기반 리스트)

대충 이거 베이스로 시작해야 함. 더 세부 기능은 요구사항 나오면 쪼개면 됨.

### A. 인증 / 보안

- 구글 OAuth2 로그인
- FormLogin (직원/관리자용) 
- 사용자 역할(Role) 및 권한 설정 (ex. 관리자, 일반직원 등)
- 세션 및 JWT 인증 토큰 설정 (선택)


### B. 사용자/계정 관리

- 사용자 등록, 조회, 수정, 삭제
- 권한(role) 기반 접근 제어


### C. 입고관리

- 입고 요청 생성
- 입고 품목 등록/조회
- 입고 완료 처리


### D. 출고관리

- 출고 요청 생성
- 출고 품목 등록/조회
- 출고 완료 처리


### E. 재고관리

- 재고 현황 조회
- 재고 이동 처리
- 실사 처리


### F. 보고서 / 로그

- 입/출고 이력 로그
- 사용자 작업 로그
- 시스템 로그 (에러 등)


## 2. yml 설정 (prod / dev 기준)

- dev는 in-memory용 h2, ddl-auto는 개발 중이라 update.
- prod는 PostgreSQL 붙이고, ddl-auto는 validate 고정.
- prod의 DB 비번, 유저는 env 파일로 집어넣는다.

## 3. 도커파일 내용 (기본적인 백엔드용)
path : container/dockerfile.md

```dockerfile
# Dockerfile
FROM eclipse-temurin:21-jdk-alpine

WORKDIR /app

COPY build/libs/gook-*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
```

그리고 `docker-compose.yml`을 써서 PostgreSQL + 백엔드 하나 구성 가능하게 하자.

```yml
version: '3.8'
services:
  db:
    image: postgres:15
    container_name: gook_db
    environment:
      POSTGRES_USER: prod_user
      POSTGRES_PASSWORD: prod_pass
      POSTGRES_DB: gook_prod
    volumes:
      - gook_db_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  backend:
    build: ..
    container_name: gook_backend
    depends_on:
      - db
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=prod
      - DB_HOST=db
      - DB_USER=prod_user
      - DB_PASS=prod_pass

volumes:
  gook_db_data:
```

## 4. 필요한 클래스 / Bean들 (기초 골격 기준)

### A. Security 관련 (`@Configuration`)

- SecurityConfig (Form 로그인 + OAuth2)
- CustomOAuth2UserService
- OAuth2SuccessHandler
- JWT 관련 클래스들 (TokenProvider, Filter 등 필요하면)


### B. Auth 관련 도메인/서비스

- User 엔티티
- Role 엔티티
- UserRepository
- UserService
- AuthController


### C. 입고/출고/재고 관련

- InboundOrder, OutboundOrder 엔티티
- Inventory 엔티티
- 각 서비스/리포지토리/컨트롤러


### D. 공통 유틸/설정

- GlobalExceptionHandler (공통 에러 처리)
- SwaggerConfig (springdoc-openapi)
- application.yml, profile별 분리
- Log 설정 (logback-spring.xml 작업해라)




---

### 차후 추가 기능 

- JWT 쓸거냐?
- 회사/사내용 SSO OAuth2 : 미정 
- 입출고 처리 방식은 오프라인에서도 지원 :  QR-code 
- 입출고 처리는 실시간인가 배치인가? 미정
- 스웨거 문서 어디까지 공개? 개발 단계에선 전체 공개. 배포시 비공개.

