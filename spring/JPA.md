# 1. JPA 소개
- **JPA(Java Persistence API)**: 자바 ORM 기술에 대한 API 표준 명세입니다.
- **ORM(Object-Relational Mapping)**: 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑해줍니다.
- **목적**: 데이터베이스 작업을 간소화하고, 개발자가 SQL 대신 객체 중심의 프로그래밍에 집중할 수 있게 도와줍니다.

## 구성
1. **엔티티 (Entity)**: 데이터베이스의 테이블을 Java 클래스로 표현한 것입니다. 이 클래스는 데이터베이스의 구조(테이블, 컬럼 등)를 반영하며, JPA 어노테이션을 사용하여 이 클래스와 데이터베이스 테이블 간의 관계를 매핑합니다.
2. **리포지토리 (Repository)**: 데이터베이스와의 상호작용을 담당합니다. 여기서는 JPA의 쿼리 메소드를 이용해 자주 사용되는 SQL 쿼리를 정의합니다. 이 쿼리 메소드들은 데이터베이스로부터 데이터를 조회하거나 저장하는데 사용됩니다.
3. **서비스 (Service)**: 비즈니스 로직을 수행합니다. 로그인, 회원 가입, 회원 탈퇴와 같은 기능을 구현합니다. 서비스 계층에서는 리포지토리 계층의 메소드를 호출하여 데이터베이스와 상호작용하고, 비즈니스 규칙에 따라 데이터를 처리합니다.
4. **컨트롤러 (Controller)**: HTTP 요청을 받아들이고 처리합니다. 컨트롤러는 사용자의 요청에 따라 적절한 서비스 메소드를 호출하고, 그 결과를 사용자에게 반환합니다. 이는 웹 애플리케이션에서 클라이언트(사용자)와 서버 사이의 상호작용을 관리합니다.

# 2. JPA 설정과 의존성 추가
- **의존성 추가**: 스프링 부트 프로젝트에 JPA를 사용하기 위한 의존성을 추가합니다.
  ```xml
  <!-- 예시: Maven의 경우 -->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  ```
- **application.properties**: 데이터베이스 연결과 JPA 관련 설정을 정의합니다.
  ```text
  spring.datasource.url=jdbc:mysql://localhost:3306/mydb
  spring.datasource.username=myuser
  spring.datasource.password=mypass
  spring.jpa.hibernate.ddl-auto=update
  spring.jpa.show-sql=true
  ```

# 3. 엔티티 클래스 생성
- **엔티티 정의**: 데이터베이스 테이블과 매핑될 자바 클래스(엔티티)를 정의합니다.
  ```java
  @Entity
  public class User {
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;
      private String name;
      // getter, setter, 생성자...
  }
  ```

# 4. 리포지토리 인터페이스 생성
- **리포지토리 인터페이스**: 엔티티에 대한 CRUD 연산을 수행할 인터페이스를 정의합니다.
  ```java
  public interface UserRepository extends JpaRepository<User, Long> {
      // 필요한 쿼리 메소드 정의
  }
  ```

# 5. 서비스 계층 구현
- **서비스 계층**: 리포지토리 인터페이스를 사용하여 비즈니스 로직을 구현합니다.
  ```java
  @Service
  public class UserService {
      private final UserRepository userRepository;

      @Autowired
      public UserService(UserRepository userRepository) {
          this.userRepository = userRepository;
      }

      public List<User> findAllUsers() {
          return userRepository.findAll();
      }
  }
  ```

# 6. 트랜잭션 관리
- **트랜잭션**: 스프링의 `@Transactional` 어노테이션을 사용하여 트랜잭션을 관리합니다.
- **트랜잭션의 범위와 일관성을 보장**하여 데이터 무결성을 유지합니다.

# 7. 쿼리 메소드
- **쿼리 메소드**: 메소드 이름을 기반으로 쿼리를 자동 생성하는 기능을 사용할 수 있습니다.
  ```java
  List<User> findByName(String name);
  ```

# 예시 

스프링 프레임워크와 JPA를 사용하여 회원(Member)과 블로그 댓글(Comment) 관련 기능을 구현하는 예시
## 1. 엔티티 클래스 정의
먼저, 회원(Member)과 댓글(Comment)에 해당하는 엔티티 클래스를 정의합니다.

### Member 엔티티
```java
@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    // 생략: 생성자, getter, setter
}
```

### Comment 엔티티
```java
@Entity
public class Comment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "member_id")
    private Member member;

    private String content;
    // 생략: 생성자, getter, setter
}
```

## 2. 리포지토리 인터페이스
각 엔티티에 대한 JPA 리포지토리 인터페이스를 정의합니다.

### MemberRepository
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    // 필요한 쿼리 메소드 추가
}
```

### CommentRepository
```java
public interface CommentRepository extends JpaRepository<Comment, Long> {
    List<Comment> findByMember(Member member);
}
```

## 3. 서비스 계층
비즈니스 로직을 수행할 서비스 클래스를 구현합니다.

### MemberService
```java
@Service
public class MemberService {
    private final MemberRepository memberRepository;

    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    // 회원 관련 메소드 구현
}
```

### CommentService
```java
@Service
public class CommentService {
    private final CommentRepository commentRepository;

    @Autowired
    public CommentService(CommentRepository commentRepository) {
        this.commentRepository = commentRepository;
    }

    public List<Comment> getCommentsByMember(Member member) {
        return commentRepository.findByMember(member);
    }

    // 기타 댓글 관련 메소드 구현
}
```

