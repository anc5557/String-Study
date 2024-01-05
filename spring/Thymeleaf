# 기초
## 1. Thymeleaf란 무엇인가?

- **Thymeleaf**는 서버 사이드 Java 템플릿 엔진으로, 웹 및 독립형 환경 모두에서 XML/XHTML/HTML5 템플릿을 처리하는 데 사용됩니다.
- Thymeleaf의 주요 목표는 웹 개발을 위한 우아하고 자연스러운 템플릿 생성을 가능하게 하는 것입니다. 특히, Thymeleaf는 **스프링 프레임워크**와 잘 통합되어 있어, 스프링 어플리케이션에서 매우 유용합니다.

## 2. Thymeleaf의 핵심 특징

1. **자연 템플릿**: Thymeleaf 템플릿은 웹 브라우저에서 그대로 열어도 정상적으로 보여지는 유효한 HTML 파일입니다. 이는 디자이너와 개발자 간의 협업을 용이하게 합니다.
2. **표준화된 문법**: Thymeleaf는 Spring EL과 유사한 표현 언어를 사용하여 템플릿 내에서 로직을 처리합니다.
3. **통합 지원**: 스프링 프레임워크와의 높은 수준의 통합을 제공합니다. 이를 통해 스프링의 기능들, 예를 들어 국제화(i18n), 봄 데이터 처리 등을 쉽게 사용할 수 있습니다.
    

## 3. 스프링 부트와의 통합

- **의존성 추가**: 스프링 부트 프로젝트에 Thymeleaf를 사용하려면, 먼저 `spring-boot-starter-thymeleaf` 의존성을 `pom.xml`이나 `build.gradle`에 추가해야 합니다.
- **자동 설정**: 스프링 부트는 Thymeleaf와 관련된 대부분의 설정을 자동으로 처리합니다. 예를 들어, Thymeleaf 템플릿을 `src/main/resources/templates` 디렉토리에 두면 스프링 부트가 자동으로 이를 인식합니다.
- **Controller와의 결합**: 스프링 MVC Controller에서 Thymeleaf 템플릿을 반환하기 위해 `@Controller` 어노테이션을 사용한 클래스 내에서 `return "templateName";`과 같은 방식으로 템플릿 이름을 반환합니다.

# 기본 설정
## 1. 의존성 추가

Gradle을 사용하는 스프링 부트 프로젝트에서는 `build.gradle` 파일에 Thymeleaf 관련 의존성을 추가해야 합니다. `spring-boot-starter-thymeleaf` 의존성을 추가하면 됩니다. 이 의존성은 스프링 부트와 Thymeleaf의 통합을 간편하게 해줍니다.

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    // 다른 필요한 의존성들...
}
```

## 2. 템플릿 파일 위치

스프링 부트는 `src/main/resources/templates` 디렉토리를 Thymeleaf 템플릿 파일들의 기본 위치로 사용합니다. 여기에 HTML 파일을 작성하면 됩니다.

## 3. application.properties 설정 (선택적)

`application.properties` 또는 `application.yml` 파일을 사용하여 Thymeleaf와 관련된 추가 설정을 할 수 있습니다. 이는 선택적입니다. 예를 들어, 템플릿 캐싱을 비활성화하는 것은 개발 중에 유용할 수 있습니다.

```properties
spring.thymeleaf.cache=false  # 개발 시 템플릿 캐시 비활성화
```

## 4. Controller 설정

Thymeleaf 템플릿을 반환하려면, 스프링 MVC 컨트롤러 내에서 해당 템플릿의 이름을 반환하면 됩니다.

```java
@Controller
public class MyController {
    @GetMapping("/greeting")
    public String greeting(Model model) {
        model.addAttribute("name", "World");
        return "greeting";  // src/main/resources/templates/greeting.html을 가리킴
    }
}
```

# 템플릿 기본 문법
## 1. 변수 표현식 (Variable Expressions)

**\$*{...}**: 모델 속성에 접근할 때 사용합니다. 예를 들어, 모델에 `name`이라는 속성이 있을 경우, `${name}`으로 접근할 수 있습니다.

  ```html
  <p>Hello, ${name}!</p>
  ```

## 2. 선택 변수 표현식 (Selection Variable Expressions)

- **\**{...}**: 선택된 객체에 대한 표현식입니다. `th:object`로 설정된 객체에 접근할 때 사용합니다.

  ```html
  <div th:object="${myBean}">
    <p>Name: <span th:text="*{name}"></span></p>
  </div>
  ```

## 3. 메시지 표현식 (Message Expressions)

- **#{...}**: 국제화(i18n) 기능을 사용할 때 사용합니다. 메시지 소스에서 메시지를 가져오는 데 사용됩니다.

  ```html
  <p th:text="#{welcome.message}"></p>
  ```

## 4. 링크 URL 표현식 (Link URL Expressions)

- **@{...}**: URL을 생성할 때 사용합니다. 스프링 MVC의 `@RequestMapping`과 유사하게 동작합니다.

  ```html
  <a th:href="@{/login}">Login</a>
  ```

## 5. 조각 표현식 (Fragment Expressions)

- **~{...}**: 템플릿 조각을 재사용할 때 사용합니다.

  ```html
  <div th:insert="~{fragments/header :: header}"></div>
  ```

## 6. 속성 설정

- **th:text, th:value, th:href 등**: Thymeleaf는 HTML 태그의 속성을 동적으로 설정할 수 있도록 `th:*` 형식의 속성을 제공합니다.

  ```html
  <span th:text="${name}">Name</span>
  <input type="text" th:value="${value}" />
  ```

## 7. 조건문과 반복문

- **th:if, th:unless, th:switch, th:case**: 조건문을 처리합니다.
- **th:each**: 반복문을 처리합니다. 리스트나 배열과 같은 컬렉션을 순회할 때 사용합니다.

  ```html
  <div th:if="${condition}">
    Condition is true.
  </div>
  <ul>
    <li th:each="item : ${list}" th:text="${item}"></li>
  </ul>
  ```


# 조건문과 반복문
## 1. 조건문

### (1)  `th:if`와 `th:unless`

- `th:if`: 조건이 참(true)일 때 해당 HTML 요소를 표시합니다.
- `th:unless`: 조건이 거짓(false)일 때 해당 HTML 요소를 표시합니다.

```html
<div th:if="${condition}">
  이 부분은 'condition'이 참일 때만 보여집니다.
</div>
<div th:unless="${condition}">
  이 부분은 'condition'이 거짓일 때만 보여집니다.
</div>
```

### (2) `th:switch`와 `th:case`

- 여러 조건을 처리할 때 사용합니다.

```html
<div th:switch="${userRole}">
  <p th:case="'admin'">관리자 계정</p>
  <p th:case="'user'">사용자 계정</p>
  <p th:case="*">알 수 없는 계정</p>
</div>
```

## 2. 반복문

### (1) `th:each`

- 컬렉션(리스트, 배열 등)을 순회하며 반복적인 요소를 생성할 때 사용합니다.

```html
<ul>
  <li th:each="item : ${items}" th:text="${item}">
    여기에는 반복되는 아이템이 들어갑니다.
  </li>
</ul>
```

- `th:each`에서는 `item`이 각 요소를 나타내고, `${items}`는 순회할 컬렉션입니다. 이 구조는 `for(item : items)`와 유사합니다.

### (2) 추가 팁

- **변수 상태 정보**: 반복문 내에서 특별한 변수 상태(`status`)를 사용할 수 있습니다. 이를 통해 현재 반복의 인덱스, 첫 번째/마지막 요소 여부 등을 확인할 수 있습니다.

```html
<ul>
  <li th:each="item, stat : ${items}" th:text="${stat.index} + '. ' + ${item}">
    여기에는 인덱스와 함께 아이템이 표시됩니다.
  </li>
</ul>
```

- `stat` 변수는 현재 반복에 대한 상태 정보를 제공합니다(`index`, `count`, `first`, `last`, `even`, `odd` 등).

아래 HTML 코드는 Thymeleaf를 사용하여 목록 `items`를 순회하면서 각 아이템과 함께 그 아이템의 인덱스, 홀수/짝수 여부, 첫 번째/마지막 요소 여부를 표시하는 예시입니다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Example</title>
</head>
<body>
    <ul>
        <li th:each="item, stat : ${items}" 
            th:text="'Index: ' + ${stat.index} + ', Item: ' + ${item} + 
                     ', Even? ' + ${stat.even} + ', Odd? ' + ${stat.odd} + 
                     ', First? ' + ${stat.first} + ', Last? ' + ${stat.last}">
            <!-- 여기에 각 아이템과 관련 상태 정보가 표시됩니다 -->
        </li>
    </ul>
</body>
</html>
```

여기서 `${items}`는 컨트롤러에서 모델에 추가된 목록 객체입니다. `th:each`로 이 목록을 순회하며, 각 반복에 대한 정보는 `stat` 객체를 통해 접근합니다. 

예를 들어, `items` 목록에 "사과", "바나나", "체리"가 들어있다고 가정하면, 웹 페이지에는 다음과 같이 출력될 것입니다:

1. Index: 0, Item: 사과, Even? true, Odd? false, First? true, Last? false
2. Index: 1, Item: 바나나, Even? false, Odd? true, First? false, Last? false
3. Index: 2, Item: 체리, Even? true, Odd? false, First? false, Last? true
4. 
# 폼 처리
Thymeleaf에서 폼 처리는 주로 사용자 입력을 수집하고 서버에 전송하는 데 사용됩니다.

## 1. 폼 생성

Thymeleaf에서 폼을 생성할 때 `th:action`과 `th:object`를 사용합니다. `th:action`은 폼 데이터가 전송될 URL을 지정하고, `th:object`는 폼 데이터가 바인딩될 객체를 지정합니다.

```html
<form th:action="@{/submitForm}" th:object="${formObject}" method="post">
    <!-- 폼 필드들... -->
</form>
```

## 2. 폼 필드 바인딩

`th:field`를 사용하여 폼 필드를 모델 객체의 속성에 바인딩합니다. 이를 통해 폼 데이터를 서버로 전송할 때 해당 모델 객체의 속성으로 자동 매핑됩니다.

```html
<input type="text" th:field="*{username}" />
<input type="password" th:field="*{password}" />
```

여기서 `*{username}`과 `*{password}`는 `formObject` 객체의 `username`과 `password` 속성에 각각 바인딩됩니다.

## 3. 폼 검증 오류 표시

폼 검증 오류를 표시하기 위해 `th:errors`를 사용합니다. 이 태그는 해당 필드에 오류가 있을 때만 내용을 표시합니다.

```html
<span th:if="${#fields.hasErrors('username')}" th:errors="*{username}">오류 메시지</span>
```

## 4. 폼 제출

폼을 제출하면, `th:action`에 지정된 URL로 `POST` 요청이 전송됩니다. 스프링 컨트롤러에서 이 요청을 처리할 수 있도록 매핑합니다.

```java
@PostMapping("/submitForm")
public String submitForm(@ModelAttribute("formObject") FormObject formObject, BindingResult bindingResult, Model model) {
    // 폼 데이터 처리 로직
    return "resultPage";
}
```

## 5. Select, Checkbox, Radio 버튼

- **Select**: `th:field`를 사용하여 선택된 항목을 모델 속성에 바인딩합니다.

  ```html
  <select th:field="*{selectedOption}">
    <option th:each="option : ${options}" th:value="${option}" th:text="${option}"></option>
  </select>
  ```

- **Checkbox**와 **Radio 버튼**: 체크박스와 라디오 버튼도 `th:field`로 모델 속성에 바인딩할 수 있습니다.

  ```html
  <input type="checkbox" th:field="*{agreeTerms}" />
  <input type="radio" th:field="*{gender}" value="Male" />
  <input type="radio" th:field="*{gender}" value="Female" />
  ```


# 스프링MVC 통합 기능
스프링 MVC는 웹 애플리케이션 개발을 위한 강력한 프레임워크이며, Thymeleaf는 이와 통합되어 효율적인 웹 페이지 렌더링과 데이터 처리를 가능하게 합니다.

## 1. 스프링 MVC 개요

- **스프링 MVC**는 Model-View-Controller 아키텍처를 따르며, 웹 애플리케이션 개발에 필요한 여러 기능을 제공합니다.
- **Controller**: 사용자의 요청을 처리하고, 모델을 조작하거나 데이터를 뷰로 전달하는 역할을 합니다.
- **Model**: 데이터와 비즈니스 로직을 처리합니다. 컨트롤러와 뷰 사이에서 데이터를 전달하는 데 사용됩니다.
- **View**: 사용자에게 데이터를 표시하는 역할을 합니다. Thymeleaf는 뷰 레이어에서 주로 사용됩니다.

## 2. Thymeleaf와 스프링 MVC의 통합

- **의존성 관리**: 스프링 부트를 사용하면 Thymeleaf와 스프링 MVC의 통합이 매우 간단합니다. `spring-boot-starter-thymeleaf` 의존성을 추가하면 스프링 부트가 필요한 설정을 자동으로 처리합니다.
- **뷰 템플릿**: Thymeleaf는 스프링 MVC의 뷰 템플릿으로 사용됩니다. 컨트롤러에서 반환하는 뷰 이름과 일치하는 Thymeleaf 템플릿 파일을 렌더링합니다.

## 3. 컨트롤러에서의 사용

- 스프링 MVC 컨트롤러에서 Thymeleaf 뷰를 반환하기 위해, 컨트롤러 메서드에서 뷰 이름을 문자열로 반환합니다.

```java
@Controller
public class MyController {
    @GetMapping("/greeting")
    public String greeting(Model model) {
        model.addAttribute("name", "World");
        return "greeting"; // Thymeleaf 템플릿 'greeting.html'을 찾음
    }
}
```

## 4. 모델 데이터 전달

- 컨트롤러에서 모델 객체에 데이터를 추가하면, Thymeleaf 템플릿에서 이 데이터에 접근할 수 있습니다.

```java
model.addAttribute("message", "Hello, Thymeleaf!");
```

- Thymeleaf 템플릿에서는 `${message}`와 같은 형식으로 이 데이터를 사용할 수 있습니다.

## 5. 폼 데이터 처리

- Thymeleaf와 스프링 MVC를 함께 사용하면, 폼 데이터의 송수신과 처리가 매우 간편해집니다.
- `@ModelAttribute`를 사용하여 폼 데이터를 객체에 매핑하고, 이를 컨트롤러에서 처리할 수 있습니다. -> [[#4. 폼 제출]]

## 6. 국제화 및 검증

- 스프링 MVC의 국제화(i18n) 기능과 Thymeleaf의 메시지 표현식을 함께 사용하여 다국어 지원을 구현할 수 있습니다.
- 스프링의 검증 및 데이터 바인딩 기능과 함께 Thymeleaf의 폼 검증 오류 표시 기능을 사용하여 사용자 입력을 검증하고 피드백을 제공할 수 있습니다.

## 7. AJAX 통합

- Thymeleaf는 AJAX와도 잘 통합됩니다. JavaScript와 Thymeleaf를 함께 사용하여 동적인 사용자 경험을 만들 수 있습니다.


# 국제화(i18n)와 지역화(l10n)
## 1. 메시지 소스 설정

- **메시지 소스 파일 생성**: 다양한 언어에 대한 메시지 소스 파일을 생성합니다. 예를 들어, `messages_en.properties`, `messages_ko.properties`와 같이 파일을 만듭니다.
  
```properties
  # messages_en.properties
  welcome.message=Welcome
  
  # messages_ko.properties
  welcome.message=환영합니다
```

- **스프링 부트 설정**: `application.properties` 또는 `application.yml`에 메시지 소스의 기본 이름을 설정합니다.

```properties
spring.messages.basename=messages
```

## 2. LocaleResolver 설정

- **LocaleResolver**: 사용자의 로케일을 결정하는 구성 요소입니다. 스프링 부트는 기본적으로 `AcceptHeaderLocaleResolver`를 사용합니다. 필요한 경우 `SessionLocaleResolver`나 `CookieLocaleResolver`로 변경할 수 있습니다.

  ```java
  @Bean
  public LocaleResolver localeResolver() {
      SessionLocaleResolver slr = new SessionLocaleResolver();
      slr.setDefaultLocale(Locale.KOREAN); // 기본 로케일 설정
      return slr;
  }
  ```

## 3. LocaleChangeInterceptor 설정

- **LocaleChangeInterceptor**: HTTP 요청에 따라 로케일을 변경할 수 있는 인터셉터입니다. 파라미터나 세션을 통해 로케일을 변경할 수 있습니다.

  ```java
  @Bean
  public LocaleChangeInterceptor localeChangeInterceptor() {
      LocaleChangeInterceptor lci = new LocaleChangeInterceptor();
      lci.setParamName("lang"); // 'lang' 파라미터로 로케일 변경
      return lci;
  }
  ```

- 이 인터셉터를 스프링 MVC의 인터셉터로 등록해야 합니다.

## 4. Thymeleaf에서 국제화 사용

- Thymeleaf 템플릿에서 `#{...}` 구문을 사용하여 메시지를 출력합니다.

  ```html
  <p th:text="#{welcome.message}">Default Welcome Message</p>
  ```

## 5. 사용자에 의한 언어 변경

- 사용자가 언어를 변경할 수 있도록 링크나 선택 요소를 제공합니다. 예를 들어, 언어 선택 드롭다운에서 언어를 선택하면 `lang` 파라미터를 통해 로케일을 변경할 수 있습니다.

  ```html
  <a href="?lang=en">English</a> | <a href="?lang=ko">한국어</a>
  ```

이렇게 구성하면 스프링 부트와 Thymeleaf 기반의 웹 애플리케이션에서 다양한 언어와 지역 설정에 맞게 템플릿을 제공할 수 있습니다. 사용자의 언어와 지역 설정에 따라 자동으로 적절한 메시지를 표시하게 됩니다. 

# 레이아웃과 조각
이 기능은 웹 페이지의 여러 부분에서 공통 요소를 재사용할 수 있게 해주어 개발 효율성을 높이고, 코드 중복을 줄이는 데 매우 유용합니다.

## 1. 템플릿 조각 (Template Fragments)

템플릿 조각은 HTML 코드의 일부를 재사용할 수 있게 해주는 Thymeleaf의 기능입니다.
### (1) 조각 정의하기

- 조각을 정의할 때는 `th:fragment` 속성을 사용합니다. 예를 들어, 공통 헤더를 만들 수 있습니다.

  ```html
  <!-- header.html -->
  <div th:fragment="header">
    <h1>공통 헤더</h1>
    <!-- 헤더 내용 -->
  </div>
  ```

### (2) 조각 사용하기

- 다른 템플릿에서 이 조각을 사용하려면 `th:insert` 또는 `th:replace` 속성을 사용합니다.

  ```html
  <!-- home.html -->
  <div th:insert="~{header :: header}"></div>
  ```

## 2. 레이아웃 확장 (Layout Extension)

레이아웃 확장을 사용하면 기본 레이아웃을 정의하고, 다른 페이지에서 이 레이아웃을 확장하여 사용할 수 있습니다.

### (1) 기본 레이아웃 정의하기

- 기본 레이아웃 파일을 만듭니다. 이 파일은 공통 요소(예: 헤더, 푸터)를 포함합니다.

  ```html
  <!-- layout.html -->
  <html>
  <head>
      <title>레이아웃 타이틀</title>
  </head>
  <body>
  <header th:replace="fragments/header :: header"></header>
  <section th:fragment="content">
      <!-- 기본 컨텐츠 -->
  </section>
  <footer th:replace="fragments/footer :: footer"></footer>
  </body>
  </html>
  ```

### (2) 레이아웃 확장하기

- 다른 템플릿에서 이 레이아웃을 확장하여 사용합니다. `th:replace`를 사용하여 `content` 부분을 새로운 내용으로 대체합니다.

  ```html
  <!-- home.html -->
  <html th:replace="layout :: content">
  <head>
      <title>홈 페이지</title>
  </head>
  <body>
  <section>
      <h2>홈 페이지 컨텐츠</h2>
      <!-- 페이지 별 내용 -->
  </section>
  </body>
  </html>
  ```

## 3. 조각과 레이아웃의 장점

- **재사용성**: 헤더, 푸터, 네비게이션 바와 같은 공통 요소를 여러 페이지에서 재사용할 수 있습니다.
- **유지 보수 용이성**: 공통 요소의 변경이 필요할 때, 한 곳에서만 수정하면 모든 페이지에 적용됩니다.
- **일관성**: 모든 페이지에 동일한 레이아웃과 디자인을 적용하여 일관성을 유지할 수 있습니다.

Thymeleaf와 스프링 시큐리티(Security)의 통합은 웹 애플리케이션 보안을 강화하는 중요한 방법입니다. 스프링 시큐리티 6.0.0 이상과 스프링 부트 3.x, 자바 17을 사용한다고 하셨으니, 이 환경에 맞는 예시를 들어 설명드리겠습니다.


# 스프링 시큐리티와 Thymeleaf
## 1. 스프링 시큐리티 의존성 추가

먼저, `build.gradle`에 스프링 시큐리티와 Thymeleaf의 스프링 시큐리티 통합 모듈 의존성을 추가합니다.

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6'
    // 다른 의존성들...
}
```

## 2. 스프링 시큐리티 설정

스프링 시큐리티를 구성하려면 `WebSecurityConfigurerAdapter`를 확장하고 필요한 메소드를 오버라이드합니다.

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/", "/home").permitAll()
                .anyRequest().authenticated()
            .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
            .and()
            .logout()
                .permitAll();
    }
    // ...
}
```

## 3. Thymeleaf에서 스프링 시큐리티 태그 사용

Thymeleaf 템플릿에서 스프링 시큐리티 관련 태그를 사용하여 보안 관련 기능을 구현할 수 있습니다.

### (1) 사용자 인증 정보 표시

```html
<div sec:authorize="isAuthenticated()">
    <p>사용자 이름: <span sec:authentication="name"></span></p>
    <a href="/logout">로그아웃</a>
</div>
<div sec:authorize="!isAuthenticated()">
    <a href="/login">로그인</a>
</div>
```

### (2) 역할 기반 콘텐츠 제어

```html
<div sec:authorize="hasRole('ROLE_ADMIN')">
    <p>관리자 전용 콘텐츠</p>
</div>
```

## 4. CSRF 보호

스프링 시큐리티는 CSRF(Cross-Site Request Forgery) 보호 기능을 기본적으로 활성화합니다. Thymeleaf와 함께 사용할 때는 폼에 CSRF 토큰을 포함시켜야 합니다.

```html
<form action="#" th:action="@{/someAction}" method="post">
    <input type="hidden" th:name="${_csrf.parameterName}" th:value="${_csrf.token}" />
    <!-- 폼 필드 -->
</form>
```

## 5. 보안 헤더

스프링 시큐리티는 보안 헤더를 설정하여 XSS, 클릭재킹 등의 공격을 방지합니다. 이 설정은 `HttpSecurity` 구성에서 조정할 수 있습니다.

```java
http
    .headers()
        .defaultsDisabled()
        .cacheControl();
```

## 6. 패스워드 인코딩

암호화된 패스워드를 처리하기 위해 `PasswordEncoder`를 사용합니다.

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

# 예시) Nav바 로그인여부로 조건부 보여주기

이 예시에서는 사용자가 로그인했을 때와 로그인하지 않았을 때 다른 메뉴 항목을 보여줍니다.

## 1. 네비게이션 바 조각 (fragments/nav.html)

먼저, Thymeleaf 조각 파일(`fragments/nav.html`)을 만들고 `th:fragment`를 사용하여 네비게이션 바를 정의합니다.

```html
<!-- fragments/nav.html -->
<nav th:fragment="navbar">
    <ul>
        <li><a href="/">홈</a></li>
        
        <!-- 로그인 상태일 때 보여질 메뉴 항목 -->
        <li sec:authorize="isAuthenticated()"><a href="/profile">내정보</a></li>
        <li sec:authorize="isAuthenticated()"><a href="/logout">로그아웃</a></li>

        <!-- 비로그인 상태일 때 보여질 메뉴 항목 -->
        <li sec:authorize="!isAuthenticated()"><a href="/login">로그인</a></li>
        <li sec:authorize="!isAuthenticated()"><a href="/signup">회원가입</a></li>
    </ul>
</nav>
```

## 2. 네비게이션 바 사용하기

이제 다른 템플릿에서 이 네비게이션 바를 재사용할 수 있습니다. `th:replace`를 사용하여 원하는 템플릿에 네비게이션 바를 포함시킵니다.

```html
<!-- 예: home.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>홈 페이지</title>
</head>
<body>
    <header th:replace="fragments/nav :: navbar"></header>
    <!-- 페이지의 나머지 내용 -->
</body>
</html>
```

이렇게 설정하면, 사용자의 로그인 상태에 따라 네비게이션 바의 메뉴 항목이 달라집니다. 스프링 시큐리티의 `sec:authorize`를 사용하여 로그인 여부를 확인하고, 이에 따라 "내정보"와 "로그아웃", 혹은 "로그인"과 "회원가입" 링크를 표시합니다. 


