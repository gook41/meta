# 환경 


# 질문 2025년 7월 20일 13:19 KST

백엔드 프로젝트 생성.
자바 21 / 스프링 3.4.7 /
종속성 :
dependencies {
// 웹, REST API
implementation("org.springframework.boot:spring-boot-starter-web")
// JPA + Hibernate
implementation("org.springframework.boot:spring-boot-starter-data-jpa")
testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
// DB (개발: h2 / 운영: postgresql)
runtimeOnly("com.h2database:h2")
runtimeOnly("org.postgresql:postgresql")
// 검증
implementation("org.springframework.boot:spring-boot-starter-validation")
// OAuth2 + Security
implementation("org.springframework.boot:spring-boot-starter-security")
implementation("org.springframework.boot:spring-boot-starter-oauth2-client")
// OpenAPI/Swagger
implementation("org.springdoc:springdoc-openapi-starter-webmvc-ui")
// 롬복
compileOnly("org.projectlombok:lombok")
annotationProcessor("org.projectlombok:lombok")
// 개발단 자동리로드
developmentOnly("org.springframework.boot:spring-boot-devtools")
// 테스트
testImplementation("org.springframework.boot:spring-boot-starter-test")
annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'

현재 여기까지 진행했어,
수립된 계획과 현재까지의 진행 상황을 md 파일에 작성, 기록해.

현재까지 정해진건  스프링 진행상황과 동일하며, 프론트엔드는 Electron으로 하게 될듯.

현재 시간: 2025년 7월 20일 13:19 KST

# GOOK 프로젝트 진행상황 (WMS, LG상사)

## 1. 프로젝트 개요

- **프로젝트명:** GOOK
- **목표:** LG상사 WMS(창고관리시스템) 개발
- **백엔드:** Java 21, Spring 3.4.7 (Gradle, JAR 배포)
- **프론트엔드:** Electron 예정
- **DB:** 개발용 H2, 운영용 PostgreSQL


## 2. 백엔드 세팅

### 2.1. 환경

- **IDE:** IntelliJ Ultimate
- **JAVA:** 21 (LTS, 2031년까지 지원)
- **Spring Framework:** 3.4.7 (호환 O)


### 2.2. 적용된 종속성 (build.gradle.kts)

```kotlin
dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web") // 웹, REST API
    implementation("org.springframework.boot:spring-boot-starter-data-jpa") // JPA + Hibernate
    testImplementation("org.springframework.restdocs:spring-restdocs-mockmvc")
    runtimeOnly("com.h2database:h2") // 개발용
    runtimeOnly("org.postgresql:postgresql") // 운영용
    implementation("org.springframework.boot:spring-boot-starter-validation")
    implementation("org.springframework.boot:spring-boot-starter-security")
    implementation("org.springframework.boot:spring-boot-starter-oauth2-client")
    implementation("org.springdoc:springdoc-openapi-starter-webmvc-ui")
    compileOnly("org.projectlombok:lombok")
    annotationProcessor("org.projectlombok:lombok")
    developmentOnly("org.springframework.boot:spring-boot-devtools")
    testImplementation("org.springframework.boot:spring-boot-starter-test")
    annotationProcessor("org.springframework.boot:spring-boot-configuration-processor")
}
```


#### - 핵심 포함 내역 요약

- REST 서비스 & Spring MVC
- JPA/Hibernate ORM, 검증 기능(@Valid 등)
- DB 커넥터(H2, PostgreSQL)
- 시큐리티 및 OAuth2 클라이언트(인증/인가)
- API 문서화(Springdoc OpenAPI/Swagger)
- 롬복(코드 단축)
- 개발 편의(devtools)
- 테스트(Restdocs, Junit, etc.)
- 커스텀 프로퍼티 자동완성(annotationProcessor)


## 3. 프론트엔드 계획

- Electron 사용 예정
- 구체 세팅 및 종속성 미확정


## 4. 주요 이슈/향후 일정

- DB 구조 설계, JPA Entity 설계 착수 필요
- OAuth2/시큐리티 환경 테스트 예정
- API 문서 자동화(Springdoc) 기능 검증 필요
- Electron 기반 프론트 구조/데이터 연동 방식 검토 예정
- 배포 환경 세팅(운영용 PostgreSQL, 빌드/배포 자동화)


## 5. 참고/체크 포인트

- 자바, 스프링, 프론트엔드(Electron) 각 버전 호환성 항상 확인
- DB 커넥터 변경 시 build.gradle 손대야 함 (운영/개발 구분)
- @ConfigurationProperties 쓸 경우 spring-boot-configuration-processor 반드시 필요 (자동완성/문서화 편의)
