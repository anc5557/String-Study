# 스프링에서 JWT 사용하기

## 1. JWT의 기본 개념

- **JWT (JSON Web Token)**: JSON 형식의 페이로드를 포함하는, URL 안전 문자열로 인코딩된 토큰입니다.
- **헤더(Header)**: 토큰의 타입(JWT)과 사용된 해시 알고리즘의 정보를 담고 있습니다.
- **페이로드(Payload)**: 토큰에 담을 클레임(claim) 정보가 포함됩니다. 클레임은 엔티티(일반적으로 사용자)와 추가 데이터를 의미합니다.
- **서명(Signature)**: 토큰의 유효성을 검증하기 위해 사용됩니다.

## 2. 의존성 추가

JWT를 사용하기 위해 `java-jwt` 라이브러리를 추가합니다.

```xml
<!-- Maven의 경우 --> 
<dependency>     
	<groupId>com.auth0</groupId>     
	<artifactId>java-jwt</artifactId>     
	<version>버전을 확인하세요</version> 
</dependency>
```


## 3. 예시
### 프로젝트 구조

간단한 예시를 위해 프로젝트 구조는 다음과 같이 설정합니다.
- `JwtUtil` - JWT 생성 및 검증 유틸리티 클래스
- `SecurityConfig` - 스프링 시큐리티 설정
- `MyUserDetailsService` - 사용자 정보를 로드하는 서비스
- `JwtAuthenticationFilter` - JWT 인증을 위한 필터
- `JwtAuthorizationFilter` - JWT 권한 부여를 위한 필터
- `AuthenticationController` - 인증 관련 컨트롤러

### JwtUtil 클래스

```java
import com.auth0.jwt.JWT;
import com.auth0.jwt.algorithms.Algorithm;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class JwtUtil {

    private final String SECRET_KEY = "your-secret-key"; // 실제 서비스에서는 보안을 위해 복잡한 키 사용

    public String generateToken(String username) {
        return JWT.create()
                .withSubject(username)
                .withExpiresAt(new Date(System.currentTimeMillis() + 10 * 60 * 1000)) // 10분 유효
                .sign(Algorithm.HMAC256(SECRET_KEY));
    }

    public boolean validateToken(String token) {
        try {
            JWT.require(Algorithm.HMAC256(SECRET_KEY))
                    .build()
                    .verify(token);
            return true;
        } catch (Exception e) {
            return false;
        }
    }

    public String getUsernameFromToken(String token) {
        return JWT.require(Algorithm.HMAC256(SECRET_KEY))
                .build()
                .verify(token)
                .getSubject();
    }
}
```

### SecurityConfig 클래스

```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeRequests()
                .antMatchers("/auth/**").permitAll()
                .anyRequest().authenticated()
            .and()
            .addFilter(new JwtAuthenticationFilter(authenticationManager()))
            .addFilter(new JwtAuthorizationFilter(authenticationManager()));
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

### MyUserDetailsService 클래스

```java
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.ArrayList;

@Service
public class MyUserDetailsService implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 여기서는 예시로 하드코딩된 사용자 정보를 제공합니다. 실제로는 데이터베이스에서 사용자 정보를 조회해야 합니다.
        return new User("user", "password", new ArrayList<>());
    }
}
```

### JwtAuthenticationFilter 클래스

```java
import javax.servlet.Filter;

// 코드 생략: JWT 인증 로직 구현
```

### JwtAuthorizationFilter 클래스

```java
import javax.servlet.Filter;

// 코드 생략: JWT 권한 부여 로직 구현
```

### AuthenticationController 클래스

```java
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AuthenticationController {

    private final JwtUtil jwtUtil;

    public AuthenticationController(JwtUtil jwtUtil) {
        this.jwtUtil = jwtUtil;
    }

    @PostMapping("/auth/login")
    public String login(@RequestBody User user) {
        // 사용자 인증 로직 (예: 데이터베이스에서 사용자 확인)
        // 인증 성공 시 JWT 토큰 생성 및 반환
        return jwtUtil.generateToken(user.getUsername());
    }
}
```


- `SECRET_KEY`는 보안을 위해 안전하게 관리해야 합니다. (예: 환경 변수 사용)
- JWT 토큰의 유효시간과 보안 설정은 프로젝트의 요구사항에 맞게 조정해야 합니다.
