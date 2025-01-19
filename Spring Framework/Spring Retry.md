# Spring Retry

```plantext
    Spring Retry — это модуль Spring, который предоставляет механизмы для обработки ошибок путем повторения операции в случае ее неудачи. Это полезно, например, при взаимодействии с ненадежными внешними сервисами, которые могут временно быть недоступны
```

- [Spring Retry](#spring-retry)
  - [Подключение через Maven](#подключение-через-maven)
  - [Включение Spring Retry через @EnableRetry](#включение-spring-retry-через-enableretry)
  - [@Retryable](#retryable)
    - [Принцип работы @Retryable](#принцип-работы-retryable)

## Подключение через Maven

```xml
    <dependency>
        <groupId>org.springframework.retry</groupId>
        <artifactId>spring-retry</artifactId>
        <version>1.3.4</version> <!-- Убедитесь, что версия актуальна -->
    </dependency>
    <dependency>
        <groupId>org.springframework.retry</groupId>
        <artifactId>spring-retry-spring-boot</artifactId>
        <version>2.0.2</version> <!-- Для интеграции с Spring Boot -->
    </dependency>
```

## Включение Spring Retry через @EnableRetry

```java
    import org.springframework.context.annotation.Configuration;
    import org.springframework.retry.annotation.EnableRetry;

    @Configuration
    @EnableRetry
    public class AppConfig {
    }
```

## @Retryable

**Параметры:**

- `value` — указывает тип исключений, для которых применяется повторение.
- `maxAttempts` — максимальное количество попыток.
- `backoff` — настройки задержки между попытками:
- `delay` — фиксированная задержка (в миллисекундах).
- `multiplier` — коэффициент увеличения задержки (например, экспоненциальная задержка).

```java
    import org.springframework.retry.annotation.Retryable;
    import org.springframework.stereotype.Service;

    @Service
    public class MyService {

        @Retryable(value = {RuntimeException.class}, maxAttempts = 3, backoff = @Backoff(delay = 2000))
        public void performAction() {
            System.out.println("Attempting action...");
            // Искусственно выбрасываем исключение
            throw new RuntimeException("Operation failed!");
        }

        @Recover // В этом случае, если все попытки завершатся неудачно, будет вызван метод recover
        public void recover(RuntimeException e) {
            System.out.println("Recovery logic after all attempts failed: " + e.getMessage());
        }
    }
```

### Принцип работы @Retryable

**Как это работает?**

1. Когда вызывается метод, аннотированный @Retryable, Spring создаёт прокси-объект вокруг этого метода.
2. При выбросе указанного исключения выполнение метода повторяется в соответствии с параметрами maxAttempts и backoff.
3. Если количество попыток исчерпано, но метод всё ещё завершился с исключением, Spring вызывает метод, помеченный @Recover, если он есть.
4. **Если метод @Recover отсутствует, исключение будет выброшено наружу**.
