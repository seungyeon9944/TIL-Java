## 1. Django vs Spring Boot 핵심 차이점
구분|Django (Python)|Spring Boot (Java)
|---|------|------|
|철학|**Batteries Included.** 필요한 기능이 이미 다 들어있음.|**Modular.** 필요한 기능을 선택해서 조립함.|
|언어 특성|동적 타이핑 (실행 시점에 타입 결정). 유연함.|정적 타이핑 (컴파일 시점에 타입 결정). 엄격함.
|설정 방식|`settings.py`에서 중앙 집중 관리.|**Annotation** (`@`) 기반 설정이 주를 이룸.
|ORM|Django ORM (매우 직관적).|Spring Data JPA / Hibernate (강력하지만 초기 설정 복잡).
|아키텍처|MVT (Model-View-Template)|MVC (Model-View-Controller)

## 2. Django 개발자를 위한 Spring Boot 매핑 가이드

주요 컴포넌트 매핑
- `urls.py` → `@Controller` / `@RestController`: URL 경로를 특정 함수와 연결하는 역할입니다. Spring에서는 클래스나 메서드 위에 `@GetMapping("/path")` 같은 어노테이션을 붙여 처리합니다.

- `models.py` → `@Entity`: 데이터베이스 테이블과 매핑되는 클래스입니다.

- `views.py` **(로직)** → `@Service`: Django는 View에 비즈니스 로직을 많이 넣지만, Spring은 Service 계층에 로직을 모으는 것이 관례입니다.

- **Admin 페이지**: Spring Boot에는 기본 Admin이 없습니다. (직접 만들거나 라이브러리를 써야 합니다.)

## 3. 단기 공부 전략
시간이 부족한 상황이므로 "Java 언어 공부"보다는 "Spring 사용법"에 집중하세요.

1. **어노테이션(Annotation) 위주로**:  `@SpringBootApplication`, `@RestController`, `@Service`, `@Repository`, `@Entity`, `@Autowired`가 각각 무엇을 의미하는지

2. **Spring Initializr 활용**: start.spring.io에서 프로젝트 구조를 생성하는 법부터 익히세요. (Dependencies: Spring Web, Spring Data JPA, Lombok, H2 Database/MySQL 추천)

3. **Lombok 사용**: Java의 불편한 점(Getter, Setter 노가다)을 Python처럼 편하게 줄여주는 라이브러리입니다. 프로젝트 시작 시 필수로 추가하세요.

4. **REST API 구현에 집중**: 앱과 웹을 동시에 지원해야 하므로, HTML을 렌더링하는 방식보다는 JSON을 주고받는 **RestAPI** 서버 구축 방식을 먼저 익히는 것이 효율적입니다.