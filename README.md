# simple-back

<!-- workspace-readme-learning:start -->
## 파일과 연결한 학습 안내

아래 설명은 이 폴더의 실제 소스와 빌드 설정을 기준으로 정리했습니다. 기존 소개의 기능 설명은 연결된 파일과 함께 확인할 수 있습니다.

### 주요 파일과 역할

| 파일 | 역할과 읽을 내용 |
| --- | --- |
| [build.gradle](<build.gradle>) | Gradle 플러그인·JDK·의존성과 빌드 작업 설정 |
| [Dockerfile](<Dockerfile>) | 컨테이너 이미지의 빌드·실행 단계 |
| [src/main/java/org/example/simpleback/SimpleBackApplication.java](<src/main/java/org/example/simpleback/SimpleBackApplication.java>) | Spring Boot 애플리케이션 진입점 — `main` |
| [src/main/java/org/example/simpleback/ui/UserController.java](<src/main/java/org/example/simpleback/ui/UserController.java>) | 요청 매핑·입력 바인딩과 응답 처리 |
| [src/main/java/org/example/simpleback/app/UserService.java](<src/main/java/org/example/simpleback/app/UserService.java>) | 업무 처리와 외부 의존성 호출 |
| [src/main/java/org/example/simpleback/domain/port/UserRepository.java](<src/main/java/org/example/simpleback/domain/port/UserRepository.java>) | 데이터 저장·조회 인터페이스 또는 구현 |
| [src/main/java/org/example/simpleback/infra/persistence/SpringDataUserRepository.java](<src/main/java/org/example/simpleback/infra/persistence/SpringDataUserRepository.java>) | Spring Data의 엔티티 저장·조회 계약 |
| [settings.gradle](<settings.gradle>) | 프로젝트 구성 자료 |
| [src/main/java/org/example/simpleback/app/UserQueryUseCase.java](<src/main/java/org/example/simpleback/app/UserQueryUseCase.java>) | 업무 처리와 외부 의존성 호출 |
| [src/main/java/org/example/simpleback/domain/model/User.java](<src/main/java/org/example/simpleback/domain/model/User.java>) | Java 타입과 동작 정의 — `User`, `create` |
| [src/main/java/org/example/simpleback/infra/persistence/UserJpaEntity.java](<src/main/java/org/example/simpleback/infra/persistence/UserJpaEntity.java>) | DB 테이블과 대응하는 영속 엔티티 — `from` |
| [src/main/java/org/example/simpleback/infra/persistence/UserRepositoryAdapter.java](<src/main/java/org/example/simpleback/infra/persistence/UserRepositoryAdapter.java>) | 데이터 저장·조회 인터페이스 또는 구현 — `count`, `save`, `findAll` |
| [src/test/java/org/example/simpleback/app/UserServiceTest.java](<src/test/java/org/example/simpleback/app/UserServiceTest.java>) | 테스트 코드 |
| [src/test/java/org/example/simpleback/SimpleBackApplicationTests.java](<src/test/java/org/example/simpleback/SimpleBackApplicationTests.java>) | 테스트 코드 |
| [src/test/java/org/example/simpleback/ui/UserControllerTest.java](<src/test/java/org/example/simpleback/ui/UserControllerTest.java>) | 테스트 코드 |

### 하위 프로젝트·문서

- [PJMOO_TIL/README.md](<PJMOO_TIL/README.md>)
- [simple-back-ghcr/README.md](<simple-back-ghcr/README.md>)

### 실행과 설정 확인

- [build.gradle](<build.gradle>)의 플러그인과 의존성을 기준으로 구성합니다. 선언된 Java toolchain은 17입니다.
- Windows에서는 저장소 루트에서 `.\gradlew.bat bootRun`을 사용합니다.
- 환경 설정: [src/main/resources/application-local.yml](<src/main/resources/application-local.yml>), [src/main/resources/application-prod.yml](<src/main/resources/application-prod.yml>), [src/main/resources/application.yml](<src/main/resources/application.yml>), [src/test/resources/application-test.yml](<src/test/resources/application-test.yml>).
- 코드·설정에서 참조하는 환경 변수 이름: `AIVEN_MYSQL_DB`, `AIVEN_MYSQL_HOST`, `AIVEN_MYSQL_PASSWORD`, `AIVEN_MYSQL_PORT`, `AIVEN_MYSQL_USER`, `APP_MESSAGE`, `PORT`, `SPRING_JPA_HIBERNATE_DDL_AUTO`. 기본값과 필수 여부는 각 참조 위치에서 확인합니다.

### 관련 PDF와 보충 설명

- [7/2 강의](<../260629_ex/새 폴더/7-2/README.md>): 계층형·클린 아키텍처와 상태 관리의 책임 분리를 연결합니다.
- [7/23 강의](<../260629_ex/새 폴더/7-23/README.md>): 엔티티·영속성 컨텍스트·연관관계와 N+1을 연결합니다.

이 링크는 구현을 이해하기 위한 관련 기초 자료입니다. 해당 강의가 이 저장소의 모든 기능이나 이후 버전의 API를 설명한다는 뜻은 아닙니다.

### 읽는 순서와 복습

- 요청 처리 → 유스케이스·서비스 → 포트·저장소·외부 API의 의존 방향을 읽습니다. 외부 구현을 교체할 때 바뀌는 코드와 업무 규칙을 가진 코드가 구분되는지 확인합니다.
- Entity와 Repository에서 시작해 Service의 트랜잭션 및 연관 객체 접근을 읽습니다. 변경 감지 시점, 지연 로딩과 SQL 횟수, DTO 변환의 경계를 확인합니다.

테스트 소스가 포함되어 있습니다. 이 문서 수정 작업에서는 애플리케이션·DB·외부 API 테스트를 실행하지 않았으므로 실행 결과를 보장하는 기록은 아닙니다.

<!-- workspace-readme-learning:end -->

## 오늘 실습 정리: Docker · 네트워크 · GHCR

### 1. Docker 이미지 빌드와 실행

- `Dockerfile`은 멀티 스테이지 빌드를 사용한다. Gradle/JDK 이미지에서 `bootJar`로 JAR를 만들고, 더 가벼운 JRE 이미지에 결과물만 복사해 실행한다.
- 애플리케이션 컨테이너는 `8080` 포트를 사용하며 `java -jar app.jar`로 시작한다.

```sh
docker build -t simple-back:local .
docker run --rm -p 8080:8080 simple-back:local
```

### 2. Docker 네트워크 확인

`docker network inspect app-net`은 이름이 `app-net`인 네트워크의 연결된 컨테이너와 설정을 확인하는 명령이다. 다음 오류는 Docker는 정상 동작하지만 해당 네트워크가 아직 만들어지지 않았다는 의미다.

```text
Error response from daemon: network app-net not found
```

먼저 현재 네트워크를 확인하고, 없으면 생성한다.

```sh
docker network ls
docker network create app-net
docker network inspect app-net
```

### 3. GitHub Container Registry(GHCR) 자동 배포

`.github/workflows/docker-publish.yml`은 `main` 브랜치에 push할 때 실행된다.

1. 소스를 checkout하고 Docker Buildx를 준비한다.
2. `GITHUB_TOKEN`으로 `ghcr.io`에 로그인한다. 워크플로에는 `packages: write` 권한이 필요하다.
3. 이미지 이름을 소문자로 바꾼 뒤 `ghcr.io/<owner>/<repository>:latest` 태그로 빌드·푸시한다.

이미지를 받거나 실행할 때는 다음처럼 사용할 수 있다.

```sh
docker pull ghcr.io/pjmoo/simple-back-ghcr:latest
docker run --rm -p 8080:8080 ghcr.io/pjmoo/simple-back-ghcr:latest
```

Spring Boot 4 기반의 간단한 사용자 조회 REST API 실습 프로젝트입니다.
## 기술 스택
![Java 17](https://img.shields.io/badge/Java-17-007396?logo=openjdk&logoColor=white) ![Spring Boot 4](https://img.shields.io/badge/Spring_Boot-4.1.1-6DB33F?logo=springboot&logoColor=white) ![Gradle](https://img.shields.io/badge/Gradle-Wrapper-02303A?logo=gradle&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?logo=mysql&logoColor=white) ![Lombok](https://img.shields.io/badge/Lombok-BC4521?logoColor=white) ![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?logo=prometheus&logoColor=white)

`ui`, `app`, `domain`, `infra` 계층으로 구성된 클린 아키텍처를 적용합니다.
설정은 12-Factor 원칙에 따라 환경변수를 사용하며 `local`, `prod`, `test` 프로필로 분리합니다.
`GET /`은 애플리케이션 상태를, `GET /users`는 JPA 기반 사용자 목록을 반환합니다.
MockMvc 슬라이스 테스트와 Gradle Wrapper 기반 실행 가능한 fat JAR 빌드를 지원합니다.

로컬 실행 전 `.env.example`을 `.env`로 복사하고 Aiven MySQL 연결 정보를 입력합니다.

```sh
cp .env.example .env
# .env에 연결 정보를 입력한 뒤 실행
./gradlew bootRun
```

기본 `local` 프로필은 프로젝트 루트의 `.env`를 자동으로 읽습니다. 파일은 Java properties 형식이므로 값에 따옴표나 `export`를 붙이지 않습니다.
`AIVEN_MYSQL_HOST`, `AIVEN_MYSQL_PORT`, `AIVEN_MYSQL_DB`, `AIVEN_MYSQL_USER`, `AIVEN_MYSQL_PASSWORD`를 모두 입력해야 하며, MySQL 연결에는 TLS를 사용합니다.
`.env`는 Git에 포함하지 않습니다.

운영 환경에서는 `SPRING_PROFILES_ACTIVE=prod`와 동일한 다섯 환경변수를 배포 환경에 설정합니다.
운영 프로필의 스키마 검증 기본값은 `validate`이므로 DB 스키마를 미리 준비해야 합니다.

<!-- pdf-til-supplement:start -->
## TIL 부연 설명 — PDF와 연결하기

기존 실습 내용을 이해하기 위한 PDF 기반 부연 설명이다. 아래 예시는 개념을 설명하기 위한 것이며, 이 프로젝트에서 실행해 관찰한 결과와는 구분한다. 페이지 번호는 표지를 포함한 PDF 순서다.

함께 읽을 파일: [src/main/java/org/example/simpleback/SimpleBackApplication.java](<src/main/java/org/example/simpleback/SimpleBackApplication.java>) · [src/main/java/org/example/simpleback/ui/UserController.java](<src/main/java/org/example/simpleback/ui/UserController.java>) · [src/main/java/org/example/simpleback/app/UserService.java](<src/main/java/org/example/simpleback/app/UserService.java>)

### DB 클라이언트와 DB 서버 구분하기

DBeaver 같은 클라이언트는 SQL을 보내고 결과를 보여 주며 실제 데이터 저장과 쿼리 실행은 DB 서버가 맡는다. 연결에는 호스트·포트·데이터베이스·계정 등 서로 다른 정보가 필요하다. 관리형 DB를 사용해도 애플리케이션의 스키마·쿼리·권한 설계는 남아 있다.

**예시로 이해하기:** 연결 실패 시 서버에 도달하지 못하는 문제와 인증 실패, 존재하지 않는 DB 선택을 구분한다. localhost는 실행 중인 환경 자신을 가리키므로 컨테이너 안과 호스트에서 같은 문자열이 같은 서버를 뜻한다고 가정하면 안 된다.

근거: 301-2 데이터베이스 실습 — [7쪽](<../260629_ex/새 폴더/7-9/301-2_데이터베이스_실습.pdf#page=7>) · [11쪽](<../260629_ex/새 폴더/7-9/301-2_데이터베이스_실습.pdf#page=11>) · [16쪽](<../260629_ex/새 폴더/7-9/301-2_데이터베이스_실습.pdf#page=16>) · [18쪽](<../260629_ex/새 폴더/7-9/301-2_데이터베이스_실습.pdf#page=18>)

### 설정 파일의 값과 실제 적용값 구분하기

같은 설정 키가 여러 출처에 있으면 우선순위에 따라 최종 값이 정해진다. 프로파일은 환경별 설정을 선택하고 ConfigurationProperties는 관련 값을 타입으로 묶는다. 파일에 값이 적혀 있다는 사실만으로 그 값이 실행 시 사용된다고 판단하면 설정 오류를 놓칠 수 있다.

**예시로 이해하기:** 개발 포트를 바꿨는데 반영되지 않으면 활성 프로파일과 환경 변수·실행 인자를 함께 확인한다. .env 파일도 존재만으로 모든 실행 도구에 자동 적용되는 것은 아니므로 import나 로딩 구성을 읽는다. 비밀 값 자체보다 어떤 설정 키가 어디서 공급되는지를 TIL에 남긴다.

근거: 401-2 application-yml과 외부 설정 — [10쪽](<../260629_ex/새 폴더/8-4/401-2_application-yml과_외부_설정.pdf#page=10>) · [13쪽](<../260629_ex/새 폴더/8-4/401-2_application-yml과_외부_설정.pdf#page=13>) · [17쪽](<../260629_ex/새 폴더/8-4/401-2_application-yml과_외부_설정.pdf#page=17>) · [28쪽](<../260629_ex/새 폴더/8-4/401-2_application-yml과_외부_설정.pdf#page=28>) · [32쪽](<../260629_ex/새 폴더/8-4/401-2_application-yml과_외부_설정.pdf#page=32>) · [34쪽](<../260629_ex/새 폴더/8-4/401-2_application-yml과_외부_설정.pdf#page=34>)

### MVC와 계층형 설계의 역할 차이

MVC는 입력 제어·데이터·화면의 역할을 나누고, 계층형 설계는 웹 처리·업무 규칙·저장소 접근의 책임을 나눈다. 따라서 MVC와 Controller–Service–Repository 구조를 함께 사용할 수 있다. 클린 아키텍처에서는 업무 규칙이 외부 구현을 직접 참조하지 않도록 의존 방향을 조정한다.

**예시로 이해하기:** 컨트롤러는 “요청이 어떤 형식인가”, 서비스는 “이 작업이 허용되는가”, 저장소는 “어떻게 읽고 쓰는가”를 맡도록 생각한다. 외부 AI 제공자를 교체할 때 요청 API까지 바꿔야 한다면 제공자 전용 타입이 경계를 넘는지 살펴본다.

근거: 232 소프트웨어 아키텍처 패턴 — [4쪽](<../260629_ex/새 폴더/7-2/232_소프트웨어_아키텍처_패턴.pdf#page=4>) · [12쪽](<../260629_ex/새 폴더/7-2/232_소프트웨어_아키텍처_패턴.pdf#page=12>) · [15쪽](<../260629_ex/새 폴더/7-2/232_소프트웨어_아키텍처_패턴.pdf#page=15>) · [19쪽](<../260629_ex/새 폴더/7-2/232_소프트웨어_아키텍처_패턴.pdf#page=19>)

<!-- pdf-til-supplement:end -->

<!-- infra-pdf-20260914:start -->
## TIL 부연 설명 — 9월 인프라 PDF

기존 실습을 새로 추가된 PDF와 연결해 풀어 쓴 설명이다. 페이지 번호는 표지를 포함한 PDF 순서이며, 아래 개념 예시는 실제 실행 결과와 구분한다.

### 이미지 빌드와 컨테이너 실행은 다른 단계

현재 [Dockerfile](<Dockerfile>)은 Gradle/JDK 단계에서 `bootJar`를 실행하고, JRE 단계에는 `/workspace/build/libs/*-SNAPSHOT.jar`를 `app.jar`로 복사한다. 소스보다 `build.gradle`·`settings.gradle`을 먼저 복사해 의존성 처리 레이어를 재사용하려는 구조다. 다만 `gradle dependencies --no-daemon || true`는 그 명령의 실패를 무시하므로 이 단계 통과가 최종 빌드 성공을 뜻하지 않는다.

멀티 스테이지는 컴파일에 필요한 도구와 운영 시 필요한 실행 파일을 분리하는 방식이다. 앞 단계에서 만든 파일 중 `COPY --from`으로 선택한 것만 다음 단계로 옮긴다. `docker build`는 이미지를 만들며 웹 서버를 계속 실행해 두는 명령은 아니다. 실제 서비스는 그 이미지로 컨테이너를 생성·실행할 때 시작된다. 빌드 성공 후에도 런타임의 DB 접속·환경변수·포트 문제로 시작에 실패할 수 있다.

PDF 근거: 이미지 빌드·볼륨·네트워크 — [4쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=4>) · [5쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=5>) · [6쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=6>) · [7쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=7>)

### 설정·공개 포트·저장 데이터의 경계

이 Dockerfile의 `ENV PORT=8080`만으로 Spring의 `server.port`가 자동 변경된다고 단정할 수 없다. 애플리케이션 설정에서 PORT를 읽는지 확인한다.

`EXPOSE`는 사용 포트를 이미지에 명시하는 것이며 호스트 포트를 실제로 여는 동작은 아니다. `-p 호스트포트:컨테이너포트` 또는 Compose의 `ports`가 두 포트를 연결한다. 컨테이너 안의 `localhost`는 그 컨테이너 자신이므로 별도 DB 컨테이너를 찾는 주소로 사용할 수 없다. 같은 사용자 정의 네트워크에 연결된 컨테이너는 이름으로 상대를 찾는 구성을 사용할 수 있다.

이미지에는 실행 코드를 두고 환경별 값은 실행 시 전달하면 같은 이미지를 여러 환경에서 사용할 수 있다. 업로드·DB 파일처럼 재생성 후에도 남아야 하는 데이터는 컨테이너 쓰기 레이어와 분리한다. 볼륨은 영속 저장의 수단이며 백업 자체를 대신하지는 않는다.

PDF 근거: 이미지 빌드·볼륨·네트워크 — [10쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=10>) · [11쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=11>) · [12쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=12>) · [13쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=13>) · [14쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=14>)

### GHCR 발행과 서비스 배포를 구분하기

현재 [.github/workflows/docker-publish.yml](<.github/workflows/docker-publish.yml>)은 `main` push를 계기로 checkout → Buildx 준비 → GHCR 로그인 → 이미지명 소문자화 → 이미지 빌드·push를 수행한다. `contents: read`는 소스 접근에, `packages: write`는 패키지 발행에 연결된다. 워크플로의 `GITHUB_TOKEN`과 로컬 터미널에서 사용하는 로그인 자격 증명은 적용되는 실행 환경이 다르다.

현재 발행 태그는 `latest`이며 태그는 같은 이름으로 다른 이미지 내용을 가리킬 수 있다. “latest를 사용했다”만으로 어떤 코드를 실행했는지 재현하기 어려우므로 실행 기록에는 해당 커밋이나 이미지 digest를 연결해 생각한다. 이 워크플로에는 운영 서버의 컨테이너를 교체하는 단계가 없으므로 GHCR push 성공을 서비스 갱신 완료라고 해석하지 않는다. 로컬 이미지에 태그를 붙이는 일도 레지스트리에 업로드하는 push와는 별개다.

PDF 근거: GHCR와 GitHub Actions — [4쪽](<../260629_ex/새 폴더/9-8/03-2_Docker_Registry_GHCR과_GitHub_Actions.pdf#page=4>) · [7쪽](<../260629_ex/새 폴더/9-8/03-2_Docker_Registry_GHCR과_GitHub_Actions.pdf#page=7>) · [9쪽](<../260629_ex/새 폴더/9-8/03-2_Docker_Registry_GHCR과_GitHub_Actions.pdf#page=9>) · [10쪽](<../260629_ex/새 폴더/9-8/03-2_Docker_Registry_GHCR과_GitHub_Actions.pdf#page=10>) · [12쪽](<../260629_ex/새 폴더/9-8/03-2_Docker_Registry_GHCR과_GitHub_Actions.pdf#page=12>) · [25쪽](<../260629_ex/새 폴더/9-8/03-2_Docker_Registry_GHCR과_GitHub_Actions.pdf#page=25>)

<!-- infra-pdf-20260914:end -->
