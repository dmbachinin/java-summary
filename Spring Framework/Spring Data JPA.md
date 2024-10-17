# Spring Data JPA

```plantext
    Spring Data JPA — это надстройка над Spring ORM, которая предоставляет механизм репозиториев (Repository) для автоматической генерации запросов. Это позволяет быстро создать DAO-слой без необходимости написания стандартных операций (CRUD). Spring Data JPA также предоставляет механизмы для создания сложных запросов с помощью метода именования или аннотаций, а также поддержку QueryDSL и Specification
```

- [Spring Data JPA](#spring-data-jpa)
  - [Spring Data JPA основная иформация](#spring-data-jpa-основная-иформация)
  - [Покдлючение к проекту Spring Boot](#покдлючение-к-проекту-spring-boot)
    - [Подключение через Maven](#подключение-через-maven)
    - [Настройка файла application.properties](#настройка-файла-applicationproperties)
  - [JpaRepository](#jparepository)
    - [Подключение JpaRepository в проект](#подключение-jparepository-в-проект)
    - [Основные методы JpaRepository](#основные-методы-jparepository)
    - [Пагинация в Spring Data JPA](#пагинация-в-spring-data-jpa)
      - [Класс Pageable и Page](#класс-pageable-и-page)
    - [Сортировка в Spring Data JPA](#сортировка-в-spring-data-jpa)
      - [Класс Sort](#класс-sort)
    - [Механизм Query Creation (Создание запросов по именам методов)](#механизм-query-creation-создание-запросов-по-именам-методов)
      - [Ключевые слова запросов](#ключевые-слова-запросов)
      - [Операторы для условий](#операторы-для-условий)
      - [@Query](#query)

## Spring Data JPA основная иформация

`Spring Data JPA` — это высокоуровневый модуль Spring, который предназначен для упрощения работы с JPA (Java Persistence API) в Spring-приложениях. Он предоставляет удобные инструменты для создания репозиториев данных и автоматизирует выполнение большинства стандартных операций с базой данных. Благодаря этому, разработчики могут сосредоточиться на бизнес-логике, не тратя время на реализацию шаблонного кода для операций с базой данных.

**Для чего нужна Spring Data JPA?**

1. **Упрощение работы с базой данных**:
   - Основное назначение Spring Data JPA — это снижение объема шаблонного кода для выполнения стандартных операций с базой данных, таких как создание, чтение, обновление и удаление записей (CRUD).
   - Благодаря Spring Data JPA, можно создавать интерфейсы репозиториев, которые автоматически получают реализацию в рантайме, не требуя ручной реализации методов для работы с базой данных.

2. **Автоматическая генерация запросов**:
   - Spring Data JPA позволяет создавать запросы на основе имен методов (Derived Queries). Например, метод `findByName(String name)` автоматически будет выполнять поиск в базе данных по полю `name`.
   - Это позволяет быстро и удобно создавать простые запросы, не погружаясь в детали SQL.

3. **Расширенные возможности работы с JPA**:
   - Spring Data JPA поддерживает сложные запросы с использованием аннотаций `@Query`, что позволяет писать JPQL (Java Persistence Query Language) или даже нативные SQL-запросы.
   - Также поддерживается создание динамических запросов через `Specifications` и `QueryDSL`.

4. **Легкость интеграции с Spring Boot**:
   - В Spring Boot `spring-boot-starter-data-jpa` автоматически конфигурирует необходимые бины для работы с базой данных. Это позволяет быстро начать работу с JPA и сократить количество ручной конфигурации.

## Покдлючение к проекту Spring Boot

### Подключение через Maven

```xml
    <!-- Покдлючение стартового пакета для работы с Data JPA-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Connector -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.3.3</version>
    </dependency>
```

### Настройка файла application.properties

```properties
    spring.datasource.url=jdbc:postgresql://db:5432/trade
    spring.datasource.driverClassName=org.postgresql.Driver
    spring.datasource.username=username
    spring.datasource.password=password
    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true
```

## JpaRepository

`JpaRepository` — это интерфейс, предоставляемый библиотекой Spring Data JPA, который значительно упрощает работу с JPA (Java Persistence API) в приложениях на основе Spring. Он наследуется от нескольких базовых интерфейсов, предоставляя множество методов для стандартных операций с базой данных, таких как сохранение, удаление и поиск сущностей

JpaRepository наследуется от нескольких других интерфейсов, предоставляя доступ ко множеству стандартных методов:

- `Repository`: базовый интерфейс, предоставляемый Spring Data, не содержит методов.
- `CrudRepository`: добавляет основные методы для CRUD-операций (например, save, findAll, deleteById).
- `PagingAndSortingRepository`: расширяет CrudRepository, добавляя методы для работы с пагинацией и сортировкой.
- `JpaRepository`: расширяет `PagingAndSortingRepository` и добавляет дополнительные методы для работы с JPA.

### Подключение JpaRepository в проект

```java
    import org.springframework.data.jpa.repository.JpaRepository;
    import org.springframework.stereotype.Repository;

    @Repository
    public interface UserRepository extends JpaRepository<User, Long> {
        // User — это сущность, с которой работает репозиторий
        // Long — тип идентификатора (ID) сущности

        // Здесь можно добавлять кастомные методы для поиска
        List<User> findByName(String name);
    }
```

Spring автоматически предоставляет реализацию этого интерфейса в рантайме, что избавляет от необходимости ручного написания кода для доступа к данным

### Основные методы JpaRepository

```java
    // CRUD методы

    <S extends T> S save(S entity); // Cохраняет или обновляет сущность в базе данных. Если сущность уже существует (определяется по ID != 0), она обновляется, иначе создается новая запись.
    Optional<T> findById(ID id); // Находит сущность по её ID. Возвращает Optional, чтобы избежать NullPointerException при отсутствии записи.
    List<T> findAll(); // Возвращает все записи из таблицы.
    Iterable<T> findAllById(Iterable<ID> ids); // Возвращает все записи по указанным ID.
    void deleteById(ID id); // Удаляет запись по её ID.
    void delete(T entity); // Удаляет переданную сущность.
    void deleteAll(); // Удаляет все записи из таблицы.
    void deleteAll(Iterable<? extends T> entities); // Удаляет все указанные сущности.

    // Методы для работы с коллекциями

    List<T> findAll(Sort sort); // Возвращает все записи с возможностью сортировки.
    Page<T> findAll(Pageable pageable); // Возвращает записи с поддержкой пагинации. Используется для извлечения данных частями, например, по страницам.

    // Методы для работы с количеством записей

    long count(); // Возвращает количество записей в таблице.
    boolean existsById(ID id); // Проверяет, существует ли запись с указанным ID.
```

### Пагинация в Spring Data JPA

Пагинация позволяет извлекать данные по частям (страницами), что особенно полезно, если нужно показывать данные в интерфейсе или обрабатывать их по частям. Вместо того, чтобы загружать все записи из базы данных сразу, можно запрашивать только определенное количество записей за один запрос

#### Класс Pageable и Page

`Pageable` — это интерфейс, который определяет информацию о номере страницы и размере страницы (количестве записей на странице).

**Для создания объекта Pageable используется реализация PageRequest:**

```java
    import org.springframework.data.domain.PageRequest;
    import org.springframework.data.domain.Pageable;

    Pageable pageable = PageRequest.of(pageNumber, pageSize);
```

`Page<User>` — это интерфейс, предоставляющий дополнительные методы для работы с результатами пагинации

**Пример использования пагинации в репозитории:**

```java
    @Repository
    public interface UserRepository extends JpaRepository<User, Long> {
        Page<User> findByName(String name, Pageable pageable); // Создание метода с пагинацией
    }

    Pageable pageable = PageRequest.of(0, 5); // Первая страница, 5 записей на странице
    Page<User> usersPage = userRepository.findByName("John", pageable);

    List<User> users = usersPage.getContent(); // Получаем записи
    long totalElements = usersPage.getTotalElements(); // Общее количество записей с именем "John"
    int totalPages = usersPage.getTotalPages(); // Общее количество страниц
    boolean isLast = usersPage.isLast(); // Проверяет, является ли страница последней
    boolean isFirst = usersPage.isFirst(); // Проверяет, является ли страница первой
```

### Сортировка в Spring Data JPA

Сортировка позволяет упорядочить результаты запроса по одному или нескольким полям. Это важно для отображения данных в определенном порядке

#### Класс Sort

Класс `Sort` в Spring Data JPA используется для описания сортировки данных в запросах. Он позволяет определить порядок сортировки (по возрастанию или убыванию) по одному или нескольким полям

```java
    // 1. Создание сортировки по одному полю по возрастанию
    Sort sortByNameAsc = Sort.by("name").ascending();
    Sort sortByName = Sort.by("name"); // По умолчанию по возрастанию
    // Эквивалентно: ORDER BY name ASC

    // 2. Создание сортировки по одному полю по убыванию
    Sort sortByNameDesc = Sort.by("name").descending();
    // Эквивалентно: ORDER BY name DESC

    // 3. Создание сортировки по нескольким полям
    Sort sortByMultipleFields = Sort.by("name").ascending()
                                    .and(Sort.by("age").descending());
    // Эквивалентно: ORDER BY name ASC, age DESC

    // 4. Использование статического метода для создания сложной сортировки
    Sort complexSort = Sort.by(
            Sort.Order.asc("name"),
            Sort.Order.desc("age")
    );
    // Эквивалентно: ORDER BY name ASC, age DESC

    // 5. Вложенные поля (сортировка по полям вложенных объектов)
    Sort sortByNestedField = Sort.by("address.city").ascending();
    // Эквивалентно: ORDER BY address.city ASC

    // 6. Сортировка с использованием NullHandling
    Sort sortByNullHandling = Sort.by(
            Sort.Order.asc("name").nullsLast(),
            Sort.Order.desc("age").nullsFirst()
    );
    // Эквивалентно: ORDER BY name ASC NULLS LAST, age DESC NULLS FIRST

    // Примеры использования методов Order
    Sort.Order order = Sort.Order.by("name"); // Создание объекта Sort.Order для поля "name"
    order = order.ignoreCase(); // Игнорировать регистр при сортировке
    order = order.with(Sort.Direction.ASC); // Задать направление сортировки (возрастание)
    order = order.nullsFirst(); // Учитывать null-значения в начале
    order = order.nullsLast(); // Учитывать null-значения в конце

    // Создание сортировки на основе настроенного Order
    Sort sortWithCustomOrder = Sort.by(order);
```

### Механизм Query Creation (Создание запросов по именам методов)

Механизм, который позволяет JpaRepository в Spring Data JPA интерпретировать методы по их названиям, называется **Query Creation (Создание запросов по именам методов)**. Это одна из ключевых возможностей Spring Data JPA, которая автоматически генерирует SQL-запросы на основе названий методов, написанных в репозитории. Этот подход избавляет разработчика от необходимости вручную писать запросы (SQL или JPQL), предоставляя удобный способ работы с базой данных

**Правила формирования методов:**

Для того чтобы Spring Data JPA правильно интерпретировал название метода, необходимо следовать определенным правилам:

1. **Ключевое слово запроса**:
   - Название метода должно начинаться с одного из ключевых слов: `find`, `get`, `read`, `query`, `count`, `delete`, `exists`. Например, `findBy`, `deleteBy`, `countBy`.

2. **Поле сущности**:
   - После ключевого слова идет название поля, по которому будет выполняться операция. **Название поля должно точно совпадать с именем поля в классе сущности**.
   - Пример: если в сущности есть поле `email`, метод может быть назван `findByEmail(String email)`.

3. **Операторы для условий**:
   - В названии метода могут быть логические операторы, такие как `And`, `Or`, которые соединяют несколько условий.
   - Пример: `findByFirstNameAndLastName(String firstName, String lastName)`.

4. **Сортировка**:
   - Если необходимо задать порядок сортировки прямо в названии метода, можно использовать `OrderBy`.
   - Пример: `findByAgeGreaterThanOrderByLastNameDesc(int age)` — поиск всех пользователей старше указанного возраста и сортировка по фамилии в порядке убывания.

#### Ключевые слова запросов

Отличный вопрос! Действительно, в Spring Data JPA присутствует множество ключевых слов (`find`, `get`, `read`, `query`, `count`, `delete`, `exists`), которые на первый взгляд могут казаться избыточными, поскольку они выполняют схожие функции. Однако каждое из этих ключевых слов имеет свои особенности и предназначение, что обеспечивает гибкость и читаемость кода. Давайте подробно рассмотрим, почему так много ключевых слов и для чего каждое из них предназначено.

**Основные ключевые слова и их предназначение:**

| Ключевое слово    | Назначение                                                                 | Пример метода                                   | Пример SQL-запроса                                     |
|-------------------|-----------------------------------------------------------------------------|-------------------------------------------------|--------------------------------------------------------|
| `findBy`          | Поиск записей по условию                                                    | `findByEmail(String email)`                     | `SELECT * FROM users WHERE email = ?;`                  |
| `getBy`           | Получение записи по условию, часто предполагает существование записи      | `getByUsername(String username)`                | `SELECT * FROM users WHERE username = ?;`               |
| `readBy`          | Чтение записей по условию, аналогично `findBy`                             | `readByLastName(String lastName)`               | `SELECT * FROM users WHERE last_name = ?;`              |
| `queryBy`         | Выполнение запроса по условию, может подразумевать более сложный запрос     | `queryByAgeGreaterThan(int age)`                 | `SELECT * FROM users WHERE age > ?;`                     |
| `countBy`         | Подсчет количества записей, удовлетворяющих условию                        | `countByAgeGreaterThan(int age)`                 | `SELECT COUNT(*) FROM users WHERE age > ?;`              |
| `deleteBy`        | Удаление записей по условию                                                | `deleteByEmail(String email)`                    | `DELETE FROM users WHERE email = ?;`                     |
| `removeBy`        | Удаление записей по условию, аналогично `deleteBy`                         | `removeByLastName(String lastName)`              | `DELETE FROM users WHERE last_name = ?;`                  |
| `existsBy`        | Проверка существования записей, удовлетворяющих условию                    | `existsByUsername(String username)`              | `SELECT CASE WHEN COUNT(*) > 0 THEN TRUE ELSE FALSE END FROM users WHERE username = ?;` |

```java
    // find...By
    List<User> findByEmail(String email); // Используется для поиска записей по определенному условию (полю или нескольким полям)
    // SELECT * FROM users WHERE email = ?;

    // get...By
    User getByUsername(String username); // Работает аналогично find...By, используется для получения записей по условию
    // SELECT * FROM users WHERE username = ?;

    // read...By
    List<User> readByLastName(String lastName); // Еще один вариант поиска записей, аналогичный find...By и get...By
    // SELECT * FROM users WHERE last_name = ?;

    // query...By
    List<User> queryByAgeGreaterThan(int age); // Aналогично предыдущим ключевым словам, предназначено для поиска записей, но с акцентом на выполнение запроса
    // SELECT * FROM users WHERE age > ?;

    // count...By
    long countByAgeGreaterThan(int age); // Подсчитывает количество записей, удовлетворяющих условию
    // SELECT COUNT(*) FROM users WHERE age > ?;

    // delete...By
    void deleteByEmail(String email); // Удаляет записи, удовлетворяющие условию
    // DELETE FROM users WHERE email = ?;

    // remove...By
    void removeByLastName(String lastName); // Aналогично delete...By, удаляет записи по определенному условию
    // DELETE FROM users WHERE last_name = ?;

    // exists...By
    boolean existsByUsername(String username); // Проверяет, существуют ли записи, удовлетворяющие указанному условию
    // SELECT CASE WHEN COUNT(*) > 0 THEN TRUE ELSE FALSE END FROM users WHERE username = ?;
```

#### Операторы для условий

```java
    // And
    List<User> findByFirstNameAndLastName(String firstName, String lastName); // Оператор And объединяет два или более условия, которые должны выполняться одновременно (логическое "И")
    // SELECT * FROM users WHERE first_name = ? AND last_name = ?;

    // Or
    List<User> findByFirstNameOrLastName(String firstName, String lastName); // Оператор Or объединяет два условия, из которых хотя бы одно должно быть выполнено (логическое "ИЛИ")
    // SELECT * FROM users WHERE first_name = ? OR last_name = ?;

    // Equals или Is
    List<User> findByAgeEquals(int age); // По умолчанию, если оператор не указан, Spring Data JPA использует оператор равенства (=)
    List<User> findByAge(int age);  // Оператор `Equals` можно опустить
    // SELECT * FROM users WHERE age = ?;

    // GreaterThan: ищет записи, у которых значение больше указанного
    // GreaterThanEqual: ищет записи, у которых значение больше или равно указанному
    List<User> findByAgeGreaterThan(int age);
    // SELECT * FROM users WHERE age > ?;

    // LessThan: ищет записи, у которых значение меньше указанного
    // LessThanEqual: ищет записи, у которых значение меньше или равно указанному
    List<User> findByAgeLessThan(int age);
    // SELECT * FROM users WHERE age < ?;

    // Between
    List<User> findByAgeBetween(int startAge, int endAge); // Оператор Between используется для поиска записей, у которых значение поля находится в пределах указанного диапазона
    // SELECT * FROM users WHERE age BETWEEN ? AND ?;

    // Not
    List<User> findByAgeNot(int age); // Оператор Not используется для создания условий, противоположных указанным значениям. Может комбинироваться с другими операторами, такими как Equals, Like, In и т.д.
    // SELECT * FROM users WHERE age <> ?;

    // Операторы для работы с null значениями
    List<User> findByEmailIsNull(); // Оператор IsNull используется для поиска записей, где указанное поле имеет значение null
    // SELECT * FROM users WHERE email IS NULL;

    List<User> findByEmailIsNotNull(); // Оператор IsNotNull ищет записи, где поле не является null
    // SELECT * FROM users WHERE email IS NOT NULL;

    // Like
    List<User> findByNameLike(String pattern); // Оператор Like используется для поиска записей, где поле соответствует указанному шаблону. В шаблоне можно использовать подстановочные символы (%)
    // SELECT * FROM users WHERE name LIKE ?;

    // NotLike
    List<User> findByNameNotLike(String pattern); // Оператор NotLike ищет записи, которые не соответствуют указанному шаблону
    // SELECT * FROM users WHERE name NOT LIKE ?;

    // StartingWith / EndingWith / Containing
    List<User> findByNameStartingWith(String prefix); // StartingWith: проверяет, начинается ли строка с указанного текста.
    // SELECT * FROM users WHERE name LIKE ? + '%';   -- StartingWith

    List<User> findByNameEndingWith(String suffix); // EndingWith: проверяет, заканчивается ли строка указанным текстом.
    // SELECT * FROM users WHERE name LIKE '%' + ?;   -- EndingWith

    List<User> findByNameContaining(String infix); // Containing: проверяет, содержит ли строка указанную подстроку.
    // SELECT * FROM users WHERE name LIKE '%' + ? + '%'; -- Containing

    // IgnoreCase
    List<User> findByNameIgnoreCase(String name); // Этот оператор используется для игнорирования регистра символов при поиске строк
    // SELECT * FROM users WHERE LOWER(name) = LOWER(?);

    // In
    List<User> findByAgeIn(List<Integer> ages); // Оператор In используется для поиска записей, у которых значение поля содержится в списке значений
    // SELECT * FROM users WHERE age IN (?, ?, ...);

    // NotIn
    List<User> findByAgeNotIn(List<Integer> ages); // Оператор NotIn работает противоположно, возвращая записи, у которых значение не содержится в списке
    // SELECT * FROM users WHERE age NOT IN (?, ?, ...);

    // True
    List<User> findByIsActiveTrue(); // Оператор True ищет записи, где булевое поле равно true
    // SELECT * FROM users WHERE is_active = TRUE;

    // False
    List<User> findByIsActiveFalse(); //  Оператор False ищет записи, где булевое поле равно false
    // SELECT * FROM users WHERE is

    // OrderBy
    List<User> findByLastNameOrderByAgeAsc(String lastName); // Оператор OrderBy указывает порядок сортировки записей по одному или нескольким полям. Можно комбинировать с Asc (по возрастанию) и Desc (по убыванию)
    // SELECT * FROM users WHERE last_name = ? ORDER BY age ASC;

    // First / Top - Эти операторы возвращают первую (или несколько первых) записей в результате выполнения запроса. Используются для возврата ограниченного количества данных
    User findFirstByOrderByAgeDesc(); // Для первой записи
    // SELECT * FROM users ORDER BY age DESC LIMIT 1;  -- First

    List<User> findTop3ByOrderByAgeDesc(); // Для первых нескольких записей
    // SELECT * FROM users ORDER BY age DESC LIMIT 3;  -- Top 3
```

#### @Query

Аннотация `@Query` в Spring Data JPA используется для явного определения запросов (как JPQL, так и нативных SQL-запросов) в репозиториях. Она позволяет разработчикам точно контролировать, какой запрос будет выполнен, когда возможностей автоматической генерации запросов по имени метода недостаточно или когда требуется использовать более сложные запросы

**Примеры использования @Query:**

```java
    @Query("SELECT u FROM User u WHERE u.name = :name")
    List<User> findByName(@Param("name") String name); // Это JPQL-запрос, который выбирает объект User из базы данных на основе значения поля name
    // SELECT * FROM users WHERE name = ?;

    @Query(value = "SELECT * FROM users WHERE email = ?1", nativeQuery = true) // Этот параметр указывает, что запрос написан на чистом SQL, а не на JPQL (Java Persistence Query Language), который работает с объектами сущностей
    User findByEmailNative(String email); // Это нативный SQL-запрос, который работает напрямую с таблицами и полями базы данных
    // SELECT * FROM users WHERE email = ?;

    @Modifying // Используется для того, чтобы указать, что запрос изменяет данные (выполняет UPDATE или DELETE)
    @Transactional // Методы, использующие @Modifying, должны также быть аннотированы @Transactional, так как они изменяют данные
    @Query("UPDATE User u SET u.email = :email WHERE u.id = :id")
    int updateUserEmailById(@Param("id") Long id, @Param("email") String email); // С помощью @Query можно выполнять операции обновления или удаления данных
    // UPDATE users SET email = ? WHERE id = ?;

    @Query(value = "SELECT * FROM users ORDER BY registration_date DESC LIMIT 10", nativeQuery = true)
    List<User> findLatestUsers(); // Вы можете использовать @Query для запроса с лимитом количества возвращаемых записей

    @Query("SELECT COUNT(u) FROM User u WHERE u.isActive = true") // Этот запрос подсчитывает количество активных пользователей
    long countActiveUsers(); // Можно также использовать агрегатные функции в JPQL-запросах для подсчета, нахождения максимальных, минимальных или средних значений
    // SELECT COUNT(*) FROM users WHERE is_active = TRUE;
```
