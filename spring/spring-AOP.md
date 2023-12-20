# 1. AOP의 기본 개념
스프링 AOP(Aspect-Oriented Programming)는 관심사를 분리하여 모듈화하는 프로그래밍 패러다임입니다. AOP는 반복되는 코드(예: 로깅, 보안, 트랜잭션 관리 등)를 모듈화하여 애플리케이션의 다른 부분에 영향을 미치지 않고 재사용할 수 있도록 해줍니다.
주요 비지니스 로직과 부가기능 인프라 로직으로 나눈다면 반복적되는 인프라 로직을 모듈화 하여 비지니스 로직을 잘보이게 만들고 코드를 줄일 수 있습니다.

- **Aspect**
	- 여러 객체에 걸쳐 있는 관심사(예: 로깅, 보안 등)를 모듈화한 것.
- **Join Point**: **어디에 적용할 것인가**
	- 메소드 실행, 예외 처리 등 프로그램 실행 중 특정 지점.
- **Advice**: **어떤 부가기능**
	- 특정 Join Point에서 Aspect에 의해 취해지는 조치. 예를 들어, 메소드 실행 전/후에 실행되는 코드.
- **Point cut**: **실제 advice가 적용될 지점**
	- 어떤 Join Point를 Advice와 연결할지 정의하는 것. 메서드, 필드, 객체, 생성자 등
- **Target**: **어떤 대상에 적용할 것인가**
	- Advice가 적용되는 객체(클래스)
- **Proxy**
	- AOP 프록시는 Aspect를 적용하기 위해 대상 객체를 감싸는 객체.
- **Weaving**
	- Aspect를 대상 객체에 연결하는 과정.

# 2. 스프링 AOP의 특징
- 스프링 AOP는 프록시 기반의 AOP 구현을 제공합니다.
- 주로 메소드 실행 Join Point만 지원합니다.
- AspectJ의 어노테이션을 사용하여 Aspect를 쉽게 정의할 수 있습니다.

# 3. AOP의 주요 사용 사례
- **로깅 및 감사**: 메소드 호출 시 로그를 기록.
- **보안**: 메소드 실행 전 사용자의 권한 확인.
- **트랜잭션 관리**: 비즈니스 메소드 실행을 트랜잭션으로 관리.
- **오류 처리**: 시스템 전역에서 예외를 적절히 처리.

# 4. AOP 구현 예시
1. **의존성 추가**: `pom.xml` 또는 `build.gradle`에 AOP 관련 의존성 추가.
   ```xml
   <!-- 예시: Maven의 경우 -->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-aop</artifactId>
   </dependency>
   ```
2. **Aspect 정의**:
   ```java
   @Aspect
   @Component
   public class LoggingAspect {
       @Before("execution(* com.example.service.*.*(..))")
       public void logBeforeMethod(JoinPoint joinPoint) {
           System.out.println("Method called: " + joinPoint.getSignature().getName());
       }
   }
   ```
3. **Pointcut 정의**: `@Before`, `@After`, `@Around` 등의 어노테이션을 사용하여 Advice가 적용될 시점과 위치를 정의.

# 5. AOP의 장점과 주의점
- **장점**: 코드 중복 감소, 유지보수성 향상, 비즈니스 로직과 관심사의 분리.
- **주의점**: AOP 사용이 복잡도를 증가시킬 수 있으며, 성능에 영향을 미칠 수 있습니다. 따라서 필요한 곳에 적절하게 사용하는 것이 중요합니다.

스프링 AOP는 애플리케이션 전반에 걸쳐 반복되는 코드를 효율적으로 관리할 수 있게 해주며, 간단한 설정과 어노테이션을 통해 쉽게 구현할 수 있습니다. AOP를 통해 애플리케이션의 각 부분을 잘 분리하고, 각각에 집중하여 개발할 수 있습니다.
