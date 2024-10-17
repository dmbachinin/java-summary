# Spring Data REST

```plantext
    Spring Data REST — это проект, который автоматически преобразует репозитории Spring Data в полноценные RESTful API. Основное назначение Spring Data REST — упростить создание REST API, предоставляя автоматически сгенерированные конечные точки для сущностей, управляемых через Spring Data JPA
```

- [Spring Data REST](#spring-data-rest)
  - [Основные возможности Spring Data REST](#основные-возможности-spring-data-rest)
  - [Когда использовать Spring Data REST?](#когда-использовать-spring-data-rest)
  - [Подкючение Spring Data REST в Spring Boot?](#подкючение-spring-data-rest-в-spring-boot)
    - [Подкючение Spring Data через Maven](#подкючение-spring-data-через-maven)
    - [Создание сущности и репозитория](#создание-сущности-и-репозитория)
    - [Адреса для обращения](#адреса-для-обращения)
    - [Встроенная поддержка сортировки, пагинации и фильтрации](#встроенная-поддержка-сортировки-пагинации-и-фильтрации)
  - [Расширение и настройка Spring Data REST](#расширение-и-настройка-spring-data-rest)
    - [Переопределение методов](#переопределение-методов)
    - [Создание пользовательских методов](#создание-пользовательских-методов)

## Основные возможности Spring Data REST

1. **Автоматическое создание REST API**:
   - Spring Data REST автоматически создает RESTful интерфейс для ваших репозиториев, предоставляя конечные точки для стандартных CRUD-операций (Create, Read, Update, Delete).
   - Например, если у вас есть репозиторий для сущности `User`, Spring Data REST создаст API для создания пользователей, получения списка пользователей, обновления и удаления пользователей.

2. **HATEOAS поддержка**:
   - Spring Data REST поддерживает **HATEOAS** (Hypermedia as the Engine of Application State) — это стиль создания REST API, который позволяет клиентам взаимодействовать с ресурсами через предоставленные ссылки.
   - Каждая запись в REST API будет содержать ссылки (`_links`) на связанные ресурсы или возможные действия (например, ссылки для редактирования, удаления или перехода к связанным сущностям).

3. **Автоматическая пагинация, сортировка и фильтрация**:
   - Spring Data REST автоматически добавляет поддержку пагинации и сортировки для запросов. Это полезно при работе с большим количеством данных.
   - Вы можете использовать параметры запроса `?page`, `?size` и `?sort` для управления пагинацией и сортировкой результатов.

4. **Встроенные функции валидации и обработки ошибок**:
   - Spring Data REST автоматически обрабатывает валидацию данных (если вы используете `@Valid` и другие аннотации валидации) и возвращает соответствующие HTTP-ответы при возникновении ошибок.

5. **Расширяемость**:
   - Несмотря на автоматическую генерацию API, Spring Data REST можно легко настроить и расширить, добавив свои методы или переопределив поведение по умолчанию.

## Когда использовать Spring Data REST?

1. **Быстрая разработка прототипов**:
   - Если вам нужно быстро создать REST API для сущностей вашего приложения и предоставить стандартные CRUD-операции, Spring Data REST отлично подходит для этих задач.
   - Вы избавляетесь от необходимости писать код контроллеров, сервисов и репозиториев вручную — все это создается автоматически.

2. **Приложения с простыми CRUD API**:
   - Для приложений, где основная задача — предоставление простого CRUD-интерфейса для управления сущностями, Spring Data REST обеспечивает быстрый и легкий способ реализации этих операций.

3. **API с поддержкой HATEOAS**:
   - Если ваше приложение строится на принципах HATEOAS, Spring Data REST упрощает добавление гипермедийных ссылок в ваши API, делая API более самоописательным и легким в навигации.

## Подкючение Spring Data REST в Spring Boot?

### Подкючение Spring Data через Maven

В вашем `pom.xml` файле нужно добавить зависимость на Spring Data REST:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>
```

### Создание сущности и репозитория

Создайте сущность и репозиторий, как если бы вы использовали Spring Data JPA.

**Пример сущности:**

```java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String firstName;
    private String lastName;
    private String email;

    // Геттеры и сеттеры
}
```

**Пример репозитория:**

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

### Адреса для обращения

После добавления зависимости и создания сущности с репозиторием, Spring Data REST автоматически создаст REST API для этой сущности.

Теперь, когда вы запустите приложение Spring Boot, будут доступны следующие конечные точки:

- **GET** `/users` — получить список всех пользователей.
- **GET** `/users/{id}` — получить пользователя по идентификатору.
- **POST** `/users` — создать нового пользователя.
- **PUT** `/users/{id}` — обновить данные пользователя.
- **DELETE** `/users/{id}` — удалить пользователя.

### Встроенная поддержка сортировки, пагинации и фильтрации

Spring Data REST автоматически поддерживает сортировку, фильтрацию и пагинацию для ваших API. Например:

- Параметры для сортировки:

  ```plantext
  GET /users?sort=firstName,desc
  ```

- Параметры для пагинации:
  
  ```plantext
  GET /users?page=0&size=5
  ```

- Фильтрация по полям:
  Если вы добавите метод в репозиторий для фильтрации, например, `findByLastName`, Spring Data REST автоматически предоставит эту функцию как фильтр.

Пример метода в репозитории:

```java
List<User> findByLastName(@Param("lastName") String lastName);
```

Теперь вы можете вызвать API с фильтрацией:

```plantext
GET /users/search/findByLastName?lastName=Doe
```

## Расширение и настройка Spring Data REST

Несмотря на то, что Spring Data REST автоматически генерирует стандартные API для CRUD-операций, вы можете настраивать и расширять поведение.

### Переопределение методов

Вы можете переопределить стандартные методы, такие как сохранение или удаление данных. Например, добавив проверку перед сохранением:

```java
import org.springframework.data.rest.core.annotation.RepositoryEventHandler;
import org.springframework.stereotype.Component;
import org.springframework.data.rest.core.event.BeforeCreateEvent;

@Component
@RepositoryEventHandler(User.class)
public class UserEventHandler {

    @HandleBeforeCreate
    public void handleBeforeCreate(User user) {
        // Логика перед созданием

 пользователя
    }
}
```

### Создание пользовательских методов

Вы можете добавлять свои методы в репозитории и делать их доступными через API:

```java
@Query("SELECT u FROM User u WHERE u.email = :email")
User findByEmail(@Param("email") String email);
```

Теперь запрос будет доступен по адресу `/users/search/findByEmail?email=example@example.com`
