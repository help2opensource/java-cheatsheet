
# Spring Boot Cheat Sheet

## 1. Spring Boot Setup
- **Create a Spring Boot Application:**
  ```bash
  spring init --dependencies=web,data-jpa,h2 my-app
  ```

- **Main Application Class:**
  ```java
  @SpringBootApplication
  public class MyApplication {
      public static void main(String[] args) {
          SpringApplication.run(MyApplication.class, args);
      }
  }
  ```

## 2. Application Properties (application.properties)
- **Server Configuration:**
  ```properties
  server.port=8081
  server.servlet.context-path=/api
  ```

- **Database Configuration (H2):**
  ```properties
  spring.datasource.url=jdbc:h2:mem:testdb
  spring.datasource.driverClassName=org.h2.Driver
  spring.datasource.username=sa
  spring.datasource.password=password
  spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
  ```

## 3. REST API Endpoints
- **Controller Example:**
  ```java
  @RestController
  @RequestMapping("/api")
  public class MyController {
      
      @GetMapping("/greeting")
      public String greeting() {
          return "Hello, World!";
      }
  }
  ```

## 4. Dependency Injection
- **Injecting a Service into a Controller:**
  ```java
  @RestController
  public class MyController {

      private final MyService myService;

      @Autowired
      public MyController(MyService myService) {
          this.myService = myService;
      }
      
      @GetMapping("/service")
      public String getServiceMessage() {
          return myService.getMessage();
      }
  }
  ```

- **Service Example:**
  ```java
  @Service
  public class MyService {
      public String getMessage() {
          return "Service message";
      }
  }
  ```

## 5. Spring Boot Actuator
- **Enable Actuator Endpoints:**
  ```properties
  management.endpoints.web.exposure.include=health,info
  ```

- **Common Endpoints:**
  - `/actuator/health`
  - `/actuator/info`
  - `/actuator/metrics`

## 6. Spring Data JPA
- **Repository Example:**
  ```java
  @Repository
  public interface MyRepository extends JpaRepository<MyEntity, Long> {
  }
  ```

- **Entity Example:**
  ```java
  @Entity
  public class MyEntity {
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;
      private String name;
  }
  ```

## 7. Spring Security
- **Basic Authentication Configuration:**
  ```java
  @Configuration
  public class SecurityConfig extends WebSecurityConfigurerAdapter {

      @Override
      protected void configure(HttpSecurity http) throws Exception {
          http
              .authorizeRequests()
              .antMatchers("/api/**").authenticated()
              .and()
              .httpBasic();
      }

      @Override
      protected void configure(AuthenticationManagerBuilder auth) throws Exception {
          auth.inMemoryAuthentication()
              .withUser("user").password(passwordEncoder().encode("password")).roles("USER");
      }

      @Bean
      public PasswordEncoder passwordEncoder() {
          return new BCryptPasswordEncoder();
      }
  }
  ```

## 8. Spring Boot Profiles
- **Set Active Profile:**
  ```properties
  spring.profiles.active=dev
  ```

- **Define Profile-Specific Properties (application-dev.properties):**
  ```properties
  spring.datasource.url=jdbc:mysql://localhost:3306/devdb
  ```

## 9. Customizing Spring Boot Banner
- **Disable Banner:**
  ```properties
  spring.main.banner-mode=off
  ```

- **Custom Banner (banner.txt):**
  ```
  ________  ____   __   __
  \_____  \ \   \ /  \_/  /
   ______)  ) \    |    /
  ```

## 10. Testing
- **JUnit Test Example:**
  ```java
  @SpringBootTest
  class MyServiceTests {

      @Autowired
      private MyService myService;

      @Test
      void testServiceMessage() {
          assertEquals("Service message", myService.getMessage());
      }
  }
  ```

## 11. Spring Boot Maven Plugin
- **Running the Application:**
  ```bash
  mvn spring-boot:run
  ```

- **Building the Jar:**
  ```bash
  mvn clean package
  ```

## 12. Customizing Error Pages
- **Create a custom error page (resources/templates/error.html):**
  ```html
  <html>
      <body>
          <h1>An error occurred</h1>
      </body>
  </html>
  ```
