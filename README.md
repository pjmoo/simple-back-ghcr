# simple-back

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
