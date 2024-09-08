# Cодержание

- [Cодержание](#cодержание)
  - [Подключение через Maven](#подключение-через-maven)

## Подключение через Maven

**[Модули Spring для Maven](https://mvnrepository.com/search?q=Spring)**

**Пример подключения:**

```xml
<!-- Spring Context для поддержки аннотаций и ApplicationContext -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.9</version>
</dependency>

<!-- Spring JDBC для работы с JdbcTemplate -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.9</version>
</dependency>

<!-- Spring Security для обеспечения безопасности -->
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-core</artifactId>
    <version>5.8.1</version>
</dependency>

<!-- Spring Test для использования Spring в тестах -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.9</version>
    <scope>test</scope>
</dependency>
```
