<https://spring.io/projects/spring-security#overview>

# 1. 스프링 보안의 기본 개념
스프링 보안(Spring Security)은 스프링 기반 애플리케이션에서 인증과 권한 부여를 관리하는 강력한 보안 프레임워크입니다. 스프링 보안을 사용하면 애플리케이션의 보안을 강화하고, 보안 관련 작업을 쉽게 처리할 수 있습니다.

- **인증(Authentication)**: 사용자가 누구인지 확인하는 과정입니다. 일반적으로 사용자 이름과 비밀번호를 통해 이루어집니다.
- **권한 부여(Authorization)**: 인증된 사용자가 어떤 자원에 접근할 수 있는지 결정합니다.
- **보안 필터 체인**: HTTP 요청에 대한 보안 처리를 담당하는 일련의 필터들입니다.

# 2. 의존성 추가
스프링 부트 프로젝트에 스프링 보안을 추가하기 위해 `spring-boot-starter-security` 의존성을 추가합니다.

```xml
<!-- Maven의 경우 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

# 3. 기본 설정
스프링 보안은 기본적으로 모든 요청에 대해 인증을 요구합니다. 사용자 정의 보안 설정을 제공하려면 `WebSecurityConfigurerAdapter`를 상속받는 클래스를 생성하고 설정을 재정의합니다.

```java
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/", "/home").permitAll() // 홈페이지와 같은 특정 경로는 모든 사용자에게 허용
                .anyRequest().authenticated() // 나머지 경로는 인증 필요
            .and()
            .formLogin()
                .loginPage("/login") // 사용자 정의 로그인 페이지
                .permitAll()
            .and()
            .logout()
                .permitAll();
    }
}
```

# 4. 인증 관리자 설정
사용자 인증을 관리하기 위해 `UserDetailsService` 인터페이스를 구현하고, 사용자 정보를 반환하는 방법을 정의합니다.

```java
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

@Service
public class MyUserDetailsService implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 사용자 정보를 조회하여 반환하는 로직 구현
        return new User("user", "password", new ArrayList<>());
    }
}
```

# 5. 비밀번호 인코딩
보안상의 이유로 비밀번호는 항상 인코딩된 형태로 저장되어야 합니다. `PasswordEncoder`를 구현하여 비밀번호를 인코딩합니다.

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

# 6. CSRF 보호
스프링 보안은 기본적으로 CSRF(Cross-Site Request Forgery) 공격을 방지합니다. 필요한 경우 CSRF 보호를 비활성화할 수도 있습니다.

```java
http.csrf().disable();
```



