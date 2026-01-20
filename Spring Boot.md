## Spring Boot
- **독립 실행형** 스프링 어플리케이션 생성
- **Tomcat 내장**하고있어, WAR 파일 배포할 필요 x
- 'starter' **종속성** 제공 → 빌드 구성 단순화
- 스프링, **3rd-파티** 라이브러리의 버전 자동으로 관리
- 상태 확인 기능 제공
- XML에 대한 요구사항 X 

---

## Spring Boot Project
- STS 이용해서 Spring Starter Project 생성
    - 의존성 추가
    - Spring Boot DevTools (변경 시 자동 재시작 등)
    - Spring Web MVC를 사용하기 위하여 추가
    - Next or Finish

### Spring Boot Project 구조
![STS 구조](https://i.imgur.com/43eIotO.png)
- `src/main/java` : Java 코드를 작성하는 폴더
- `src/main/resources` : 어플리케이션의 리소스 파일을 저장하는 폴더
- `static` : CSS, JavaScript, Image 등 정적 파일 저장하는 폴더
- `templates` : 동적으로 생성되는 HTML 템플릿을 저장하는 폴더 (Thymeleaf, FreeMaker, 등)
- `application.properties` : 어플리케이션의 설정파일
- `src/test/java` : 테스트 코드를 저장하는 폴더
- `HelloSpringBootApplication.java` : 어플리케이션을 시작할 수 있는 main method가 존재하는 클래스
- `HelloSpringBootApplicationTests.java` : JUnit Test 클래스
- `target` : Maven 빌드 시 결과물이 저장되는 폴더
- `HELP.md` : Spring Boot 도움말
- `mvnw, mvnw.cmd` : Maven Wrapper의 실행 스크립트
- `pom.xml` : Maven 프로젝트의 설정파일

### Spring Boot Project 실행
static/index.html
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>HelloSpringBoot</title>
</head>
<body>
    <h2>Hello Spring Boot</h2>
</body>
</html>
```
파일 만들고 프로젝트 → 우 클릭 → **Run As → Spring Boot App**으로 실행 / 혹은 main 메서드 실행

---

## Spring Boot JSP
- HelloController 생성
```
package com.ssafy.hello.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("/hello")
    public String hello(Model model){
        model.addAttribute("msg", "Hello Boot");
        return "hello"; // view 이름: /WEB-INF/views/hello.jsp
    }
}
```
- 기존 Spring에서 사용하던 폴더 구조를 생성 후 hello.jsp 생성
```
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>HelloJSP</title>
</head>
<body>
    <h2>HelloJSP</h2>
    <p>${msg}</p>
</body>
</html>
```

- application.properties에 ViewResolver 설정 등록
```
spring.application.name=HelloSpringBoot

# JSP path
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

- Spring Boot Bean 확인

- JSP를 위한 설정 추가 (pom.xml)
```
<dependency>
    <groupId>jakarta.servlet.jsp.jstl</groupId>
    <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
</dependency>
<dependency>
    <groupId>org.glassfish.web</groupId>
    <artifactId>jakarta.servlet.jsp.jstl</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```

---

## Spring Boot MVC 실습
- 등록요청 처리
```
//등록요청 처리 1
@PostMapping("/write")
public String write(HttpServletRequest request){
    String title = request.getParameter("title");
    String content = request.getParameter("content");
    //...
    Board boardd = new Board(title, content);
    return null;
}
```
```
//등록요청 처리 2
@PostMapping("/write")
public String write(@RequestParam("title")String title, @RequestParam("content") String content){
    Board board = new Board(title, content);
    return null;
}
```
```
//등록요청 처리  3
@PostMapping("/write")
public String write(@ModelAttribute Board board){
    //등록하는 서비스메서드 호출
    list.add(board);
    return "redirect:list"; //정보가 남아있기 때문에 
}

@GetMapping("/list")
public String list(Model model){
    //정석: 전체목록을 가져오는 서비스메서드 호출해서 바구니에 결과 넣기 !
    model.addAttribute("boardList", list);
    return "/board/list"; //WEB-INF/board/list.jsp
}
```
- list.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="jakarta.tags.core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>게시글 목록</title>
</head>
<body>
    <h2>글제목 리스트</h2>
    <ul>
        <c:forEach items="${boardList}" var="board">
            <li>${board.title}</li>
        <c:forEach>
    </ul>
    <a href="/writeform">게시글 등록</a>
</body>
</html>
```
