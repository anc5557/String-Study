# 의존성 주입  이해

의존성 주입(Dependency Injection, DI)은 객체 지향 프로그래밍에서 사용되는 디자인 패턴 중 하나로, 객체의 생성과 사용의 관심을 분리하는 방법입니다. 스프링 프레임워크에서는 DI를 통해 모듈 간의 결합도를 낮추고, 유연성 및 재사용성을 높입니다.

## 1. 의존성 주입의 기본 개념
- **의존성**: 하나의 객체가 다른 객체의 기능을 사용할 때, 사용되는 객체에 대해 '의존'한다고 합니다.
- **주입**: 객체를 직접 생성하는 대신 외부에서 생성된 객체를 받아 사용하는 것을 말합니다. 이를 통해 객체 간의 의존성을 외부에서 관리할 수 있습니다.

## 2. 의존성 주입의 장점
- **낮은 결합도**: 객체가 직접 의존 객체를 생성하지 않고, 외부에서 제공받기 때문에 결합도가 낮아집니다.
- **유연한 코드**: 의존성을 외부에서 주입받기 때문에 프로그램의 유연성이 증가합니다.
- **코드 재사용성 및 유지보수성 향상**: 의존성이 낮은 모듈은 재사용하기 쉽고, 수정이 필요할 때 영향을 미치는 범위가 줄어듭니다.
- **단위 테스트 용이성**: 의존 객체를 Mock 객체 등으로 대체하기 쉬워 테스트가 용이합니다.

## 3. 스프링에서의 의존성 주입
- **스프링 컨테이너**: 스프링에서는 '컨테이너'라는 개념을 통해 객체(Bean)의 생명주기를 관리하고, 의존성을 주입합니다.
- **Bean**: 스프링 컨테이너에 의해 관리되는 객체를 Bean이라고 합니다.
- **의존성 주입 방법**: 주로 `@Autowired` 어노테이션을 사용해 의존성을 주입합니다. 생성자 주입, 세터 주입, 필드 주입 등 여러 방법이 있습니다.

## 4. 의존성 주입의 예시 (Java 코드 예제)
```java
@Component
public class Car {
    private Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }

    // 비즈니스 로직
}

@Component
public class Engine {
    // 엔진 관련 비즈니스 로직
}
```
위 예제에서 `Car` 클래스는 `Engine` 클래스에 의존합니다. `@Autowired`를 사용하여 `Car` 객체가 생성될 때, 스프링 컨테이너가 `Engine` 객체를 자동으로 주입해줍니다.

## 5. 의존성 주입의 범위
- **싱글톤(Singleton)**: 기본적으로 스프링은 Bean을 싱글톤으로 관리합니다. 즉, 애플리케이션 내에 단 하나의 인스턴스만 존재합니다.
- **프로토타입(Prototype)**: 필요할 때마다 새로운 인스턴스를 생성합니다.
- **기타**: 요청(Request), 세션(Session), 전역(Global) 스코프 등 다양한 범위의 Bean을 지원합니다.

의존성 주입은 스프링의 핵심 기능 중 하나로, 객체 지향 설계 원칙을 잘 지키면서 효율적인 코드를 작성하는 데 큰 도움을 줍니다. 초기에는 다소 복잡하게 느껴질 수 있지만, 의존성 주입의 개념과 작동 원리를 이해하고 나면, 스프링을 통한 개발이 훨씬 쉬워질 것입니다.

# 의존성 주입 방법

## 1. 생성자 주입 (Constructor Injection)
- **정의**: 의존성을 객체 생성 시 생성자를 통해 주입하는 방식입니다.
- **장점**:
  - 의존성이 명확하게 드러나며, 불변성을 보장합니다.
  - 순환 참조를 방지할 수 있습니다.
  - 생성 시점에 모든 의존성이 주입되므로 완전한 상태의 객체를 보장합니다.
- **사용 예시**:
  ```java
  @Component
  public class SampleService {
      private final SampleRepository sampleRepository;

      @Autowired
      public SampleService(SampleRepository sampleRepository) {
          this.sampleRepository = sampleRepository;
      }
  }
  ```

## 2. 세터 주입 (Setter Injection)
- **정의**: 세터 메소드를 통해 의존성을 주입하는 방식입니다.
- **장점**:
  - 선택적 의존성을 주입할 수 있습니다. (모든 의존성이 필수는 아님)
  - 런타임 시에 의존성을 변경할 수 있는 유연성을 제공합니다.
- **단점**:
  - 객체가 완전히 초기화되지 않은 상태로 존재할 수 있습니다.
- **사용 예시**:
  ```java
  @Component
  public class SampleService {
      private SampleRepository sampleRepository;

      @Autowired
      public void setSampleRepository(SampleRepository sampleRepository) {
          this.sampleRepository = sampleRepository;
      }
  }
  ```

## 3. 필드 주입 (Field Injection)
- **정의**: 필드(멤버 변수)에 직접 의존성을 주입하는 방식입니다.
- **장점**:
  - 간결한 코드를 작성할 수 있습니다.
- **단점**:
  - 순환 참조의 위험이 있습니다.
  - 테스트하기 어려울 수 있습니다 (필드에 직접 주입하기 때문).
  - 불변성을 보장할 수 없습니다.
- **사용 예시**:
  ```java
  @Component
  public class SampleService {
      @Autowired
      private SampleRepository sampleRepository;
  }
  ```

## 최선의 방법?
일반적으로 **생성자 주입**이 권장되는 방식입니다. 이는 객체의 불변성을 보장하고, 의존성이 명확하게 드러나며, 테스트하기 쉽기 때문입니다. 그러나 특정 상황에 따라 세터 주입이나 필드 주입이 더 적합할 수 있으므로 상황에 따라 적절한 방식을 선택하는 것이 중요합니다.

