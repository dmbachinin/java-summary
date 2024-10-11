# Spring Security

```plantext
    Spring Security — это мощный и гибкий фреймворк для обеспечения безопасности в приложениях на базе Spring
```

- [Spring Security](#spring-security)
  - [Spring Security Основная информация](#spring-security-основная-информация)
    - [Основные возможности Spring Security](#основные-возможности-spring-security)
    - [Как работает Spring Security](#как-работает-spring-security)
    - [Зачем использовать Spring Security?](#зачем-использовать-spring-security)
  - [Подключение](#подключение)
    - [Подключение через Maven](#подключение-через-maven)
  - [Настройка XML конфигурации Spring](#настройка-xml-конфигурации-spring)
  - [Шифрование паролей](#шифрование-паролей)
    - [PasswordEncoder](#passwordencoder)
      - [BCryptPasswordEncoder](#bcryptpasswordencoder)
      - [Пример использования](#пример-использования)
  - [Настройка Spring Security для Web приложения](#настройка-spring-security-для-web-приложения)
    - [Настройка через SecurityFilterChain (новый подход)](#настройка-через-securityfilterchain-новый-подход)

## Spring Security Основная информация

`Spring Security` — это мощный и гибкий фреймворк для обеспечения безопасности в приложениях на базе Spring. Он предоставляет широкие возможности для аутентификации (проверка подлинности пользователей) и авторизации (управление доступом к ресурсам) в приложениях. Spring Security является частью экосистемы Spring и интегрируется с другими компонентами, такими как Spring MVC и Spring Boot, что делает его одним из самых популярных решений для реализации безопасности в Java-приложениях.

### Основные возможности Spring Security

1. **Аутентификация**:
   - **Механизмы аутентификации**: Spring Security поддерживает различные механизмы аутентификации, такие как базовая аутентификация (Basic Authentication), форма входа (Form-based Authentication), OAuth2, OpenID Connect, LDAP, а также кастомные механизмы.
   - **Менеджеры аутентификации**: Обрабатывают процесс аутентификации, включая проверку учетных данных и управление сессиями.

2. **Авторизация**:
   - **Управление доступом**: Позволяет контролировать доступ к ресурсам на основе ролей и привилегий пользователей.
   - **Выражения безопасности**: Использование выражений, таких как `hasRole()`, `hasAuthority()`, для определения доступа к определенным страницам или методам.

3. **Защита от CSRF**:
   - Spring Security включает защиту от межсайтовых подделок запросов (CSRF), что предотвращает выполнение нежелательных действий на сайте от имени пользователя без его ведома.

4. **Шифрование паролей**:
   - Spring Security предоставляет интерфейс `PasswordEncoder`, который можно использовать для безопасного хранения паролей в базе данных. Как упоминалось ранее, `BCryptPasswordEncoder` — это один из наиболее часто используемых шифровальщиков.

5. **Интеграция с другими фреймворками и стандартами**:
   - Легко интегрируется с Spring MVC, Spring Boot, а также поддерживает стандарты OAuth2, JWT, SAML и другие.

6. **Разделение ответственности**:
   - Spring Security четко разделяет ответственность между аутентификацией и авторизацией, что упрощает управление безопасностью приложения.

### Как работает Spring Security

Spring Security работает через набор фильтров, которые интегрируются в жизненный цикл обработки запросов в приложении Spring. Когда пользователь отправляет запрос к защищенному ресурсу, Spring Security проверяет его с помощью цепочки фильтров безопасности. Эта цепочка фильтров может включать:

- **UsernamePasswordAuthenticationFilter**: Обрабатывает аутентификацию пользователя по логину и паролю.
- **SecurityContextPersistenceFilter**: Управляет контекстом безопасности, который хранит информацию о текущем аутентифицированном пользователе.
- **ExceptionTranslationFilter**: Обрабатывает исключения, возникающие при проверке безопасности, и перенаправляет пользователя на страницу ошибки или логина.

### Зачем использовать Spring Security?

1. **Безопасность**: Spring Security предоставляет готовые решения для обеспечения безопасности на всех уровнях вашего приложения.

2. **Гибкость**: Вы можете легко настроить Spring Security под свои нужды, будь то простая форма входа или сложная схема аутентификации с использованием нескольких факторов.

3. **Масштабируемость**: Spring Security подходит как для небольших, так и для больших приложений, требующих надежной защиты данных и управляемого доступа.

4. **Соответствие стандартам**: Он поддерживает множество современных стандартов безопасности и методов аутентификации, таких как OAuth2, JWT, LDAP, что упрощает интеграцию с внешними системами и услугами.

## Подключение

### Подключение через Maven

```xml
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-core</artifactId> <!-- Данная зависимость может быть учтена в spring-security-web -->
        <version>5.8.1</version> 
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-web</artifactId>
        <version>5.8.1</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-config</artifactId>
        <version>5.8.1</version>
    </dependency>
```

## Настройка XML конфигурации Spring

В отличие от Spring Boot, где конфигурация безопасности может быть автоматически настроена, в обычном Spring-проекте вам нужно вручную настроить конфигурацию через XML или Java.

Создайте файл `spring-security.xml` (или добавьте конфигурацию безопасности в существующий XML-файл конфигурации Spring):

```xml
<beans:beans xmlns="http://www.springframework.org/schema/security"
    xmlns:beans="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/security
        http://www.springframework.org/schema/security/spring-security-5.8.xsd
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Конфигурация HTTP безопасности -->
    <http auto-config="true">
        <intercept-url pattern="/login" access="permitAll"/>
        <intercept-url pattern="/**" access="isAuthenticated()"/>
        <form-login login-page="/login" default-target-url="/home" />
        <logout logout-url="/logout" logout-success-url="/login?logout"/>
    </http>

    <!-- Настройка в памяти хранилища пользователей -->
    <authentication-manager>
        <authentication-provider>
            <user-service>
                <user name="user" password="{noop}password" authorities="ROLE_USER"/>
            </user-service>
        </authentication-provider>
    </authentication-manager>
</beans:beans>
```

## Шифрование паролей

### PasswordEncoder

`PasswordEncoder` — это интерфейс, предоставляемый Spring Security, который определяет методы для шифрования паролей и проверки их соответствия.

**Методы интерфейса:**

```java
String encode(CharSequence rawPassword); // Метод, который принимает необработанный пароль и возвращает его зашифрованную версию.
boolean matches(CharSequence rawPassword, String encodedPassword); // Метод, который сравнивает необработанный пароль с зашифрованной версией и возвращает true, если они совпадают.
boolean upgradeEncoding(String encodedPassword); //Метод, который возвращает true, если пароль зашифрован с использованием устаревшего алгоритма и требует обновления (необязательный метод).
```

PasswordEncoder является абстракцией, которая позволяет вам использовать различные алгоритмы шифрования, не меняя основной логики приложения. Например, вы можете легко переключиться с BCrypt на PBKDF2, SCrypt или другой алгоритм, просто поменяв реализацию PasswordEncoder.

#### BCryptPasswordEncoder

`BCryptPasswordEncoder` — это одна из реализаций интерфейса PasswordEncoder, которая использует алгоритм BCrypt для хеширования паролей. Этот алгоритм был разработан специально для безопасного хранения паролей и обладает рядом преимуществ:

**Основные преимущества BCrypt:**

- `Соль`: BCrypt автоматически генерирует уникальную соль для каждого пароля, что делает атаки с использованием предварительно вычисленных хешей (rainbow table) неэффективными.
- `Регулируемая сложность`: BCrypt позволяет настраивать фактор "сложности" (work factor), который определяет количество итераций хеширования. Это позволяет со временем увеличивать сложность шифрования, чтобы противостоять росту вычислительных мощностей.
- `Безопасность`: BCrypt устойчив к атакам через словари и другим методам подбора паролей благодаря медленному процессу хеширования.

#### Пример использования

```java
    int strength = 12;  // Рабочий фактор в BCryptPasswordEncoder определяется количеством итераций, которое используется для хеширования пароля. Чем выше рабочий фактор, тем больше итераций и, соответственно, тем безопаснее хеш, но и время выполнения будет дольше. (по умолчанию равен 10).
    PasswordEncoder encoder = new BCryptPasswordEncoder(strength); 
    String hashedPassword = encoder.encode("myPassword"); // Хеширование пароля 
    //  * Метод encode генерирует соль и хеширует пароль. Соль хранится вместе с хешем в итоговой строке, возвращаемой BCrypt.

    boolean isPasswordMatch = encoder.matches("userInputPassword", hashedPassword); // Проверка правильности пароля в сравеннии с хешем

```

## Настройка Spring Security для Web приложения

### Настройка через SecurityFilterChain (новый подход)

С новой версией Spring Security (5.7+), конфигурация безопасности выглядит следующим образом

```java
    import org.springframework.context.annotation.Bean;
`import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    // Конфигурация цепочки фильтров безопасности
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests() // Конфигурация авторизации
                .antMatchers("/admin/**").hasRole("ADMIN") // Доступ для ролей ADMIN
                .antMatchers("/user/**").hasRole("USER")   // Доступ для ролей USER
                .antMatchers("/").permitAll()              // Главная страница доступна всем
                .and()
            .formLogin(); // Стандартная форма логина
        return http.build();
    }

    // Определение пользователей в памяти
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withUsername("user")
                .password(passwordEncoder().encode("password"))
                .roles("USER")
                .build();

        UserDetails admin = User.withUsername("admin")
                .password(passwordEncoder().encode("admin"))
                .roles("ADMIN")
                .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    // Шифрование паролей
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```
