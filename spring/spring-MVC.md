# 스프링 MVC의 핵심 구성요소

## 1. 컨트롤러 (Controller)
- 사용자의 요청을 처리하고, 모델 객체를 조작하며, 적절한 뷰를 선택합니다.
- `@Controller` 어노테이션을 사용하여 컨트롤러 클래스를 정의합니다.
- `@RequestMapping` (또는 `@GetMapping`, `@PostMapping` 등)을 사용하여 특정 URL에 메소드를 매핑합니다.
```java
@Controller 
public class HelloController {     
	@RequestMapping("/hello")     
	public String hello(Model model) {      
		model.addAttribute("message", "Hello, Spring MVC!");
		return "hello"; // 뷰의 이름을 반환     
	} 
}
```

## 2. 모델 (Model)
- 데이터와 비즈니스 로직을 담당합니다.
- 컨트롤러가 뷰에 전달할 데이터를 담는 컨테이너 역할을 합니다.
- 스프링 MVC에서는 `Model` 인터페이스를 사용하여 뷰에 데이터를 전달합니다.

## 3. 뷰 (View)
- 사용자에게 정보를 표시합니다.
- 일반적으로 JSP, Thymeleaf와 같은 템플릿 엔진을 사용합니다.
- 컨트롤러는 모델의 데이터를 뷰에 전달하고, 뷰는 이 데이터를 사용하여 사용자에게 HTML 형태로 정보를 렌더링합니다.

# 스프링 MVC의 작동 과정

1. **클라이언트 요청**: 사용자가 웹 브라우저를 통해 특정 URL(예: `/hello`)에 접근합니다.
2. **DispatcherServlet**: 스프링 MVC의 핵심 서블릿으로, 모든 클라이언트 요청을 처음으로 받습니다.
3. **컨트롤러 매핑**: `DispatcherServlet`은 요청을 처리할 적절한 컨트롤러를 찾습니다.
4. **모델 업데이트**: 컨트롤러는 비즈니스 로직을 처리하고 모델 객체를 업데이트합니다.
5. **뷰 선택**: 컨트롤러는 결과를 보여줄 뷰를 선택합니다.
6. **뷰 렌더링**: 선택된 뷰는 모델의 데이터를 사용하여 HTML을 생성하고 클라이언트에게 응답을 반환합니다.

# 예제
스프링 MVC를 이용한 간단한 웹 애플리케이션 예제

## 1. 컨트롤러 (Controller)

컨트롤러는 HTTP 요청을 처리하고, 모델 객체를 만들어 뷰에 전달합니다.

```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("message", "Hello, Spring MVC!");
        return "hello"; // hello.jsp 뷰를 반환
    }
}
```

## 2. 뷰 (View)

`src/main/resources/templates/hello.html` 경로에 다음과 같이 Thymeleaf 템플릿을 사용한 뷰 파일을 생성합니다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
</head>
<body>
    <h1 th:text="${message}">Default Message</h1>
</body>
</html>
```

## 3. 모델 (Model)

모델은 이 경우에 `hello` 메서드의 `Model` 파라미터로 제공됩니다. 이 모델에 "message"라는 이름으로 데이터를 추가합니다. 이 데이터는 뷰에서 `${message}`를 통해 접근할 수 있습니다.

## 4. 스프링 부트 애플리케이션 클래스

스프링 부트 애플리케이션을 시작하기 위한 메인 클래스를 생성합니다.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}
```

### 실행 및 결과 확인

1. 위의 코드로 구성된 스프링 부트 애플리케이션을 실행합니다.
2. 웹 브라우저를 열고 `http://localhost:8080/hello`로 이동합니다.
3. "Hello, Spring MVC!"라는 메시지가 웹 페이지에 표시됩니다.

이 예제는 스프링 MVC의 기본적인 사용법을 보여주며, 각 컴포넌트 간의 상호 작용을 이해하는 데 도움이 됩니다.  
스프링 부트는 내장된 톰캣 서버를 사용하므로, 별도의 웹 서버 설정 없이 애플리케이션을 실행하고 테스트할 수 있습니다.
