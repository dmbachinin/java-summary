# JdbcTemplate

``` plantext
    JdbcTemplate — это класс в Spring Framework, предназначенный для упрощения работы с базой данных через JDBC (Java Database Connectivity)
```

- [JdbcTemplate](#jdbctemplate)
  - [JdbcTemplate основная информация](#jdbctemplate-основная-информация)
    - [Основные функции JdbcTemplate](#основные-функции-jdbctemplate)
    - [Отличия от обычного JDBC](#отличия-от-обычного-jdbc)
  - [Подключение JdbcTemplate](#подключение-jdbctemplate)
    - [Подключение через Maven](#подключение-через-maven)
  - [Методы JdbcTemplate](#методы-jdbctemplate)
    - [execute](#execute)
    - [update](#update)
    - [queryForObject](#queryforobject)
    - [query](#query)
    - [batchUpdate](#batchupdate)
    - [queryForList](#queryforlist)
    - [queryForMap](#queryformap)
    - [queryForRowSet](#queryforrowset)
    - [call](#call)
  - [RowMapper](#rowmapper)
    - [Пример создания `RowMapper`](#пример-создания-rowmapper)
    - [Использование `RowMapper`](#использование-rowmapper)
  - [NamedParameterJdbcTemplate](#namedparameterjdbctemplate)
    - [Пример использования ParameterJdbcTemplate](#пример-использования-parameterjdbctemplate)
  - [KeyHolder](#keyholder)
    - [Пример исопльзования KeyHolder](#пример-исопльзования-keyholder)

## JdbcTemplate основная информация

`JdbcTemplate` — это класс в Spring Framework, предназначенный для упрощения работы с базой данных через JDBC (Java Database Connectivity). Он предоставляет множество удобных методов для выполнения SQL-запросов, обновлений, а также для извлечения и обработки данных из базы данных. Использование `JdbcTemplate` позволяет сократить количество стандартного кода, который обычно требуется при работе с JDBC.

### Основные функции JdbcTemplate

1. **Упрощение выполнения SQL-запросов**: `JdbcTemplate` автоматически управляет открытием и закрытием соединений, а также выполнением запросов.
2. **Избавление от шаблонного кода**: Управляет созданием и закрытием `Statement` и `ResultSet`, избавляя разработчика от рутинной работы по обработке исключений и закрытию ресурсов.
3. **Поддержка исключений времени выполнения**: Преобразует проверяемые исключения JDBC в непроверяемые, упрощая обработку ошибок и интеграцию с другими компонентами Spring.
4. **Поддержка обратных вызовов**: Предоставляет различные методы обратного вызова (`RowMapper`, `ResultSetExtractor`), которые помогают в обработке данных из базы данных.

### Отличия от обычного JDBC

1. **Обработка исключений**: В JDBC требуется обрабатывать `SQLException`, которое является проверяемым исключением. `JdbcTemplate` обрабатывает эти исключения внутри и выбрасывает непроверяемые исключения (`DataAccessException`), что упрощает управление ошибками.
2. **Управление ресурсами**: В JDBC разработчик должен явно открывать и закрывать соединения, запросы и результаты, что делает код более громоздким и подверженным утечкам ресурсов при неправильной обработке. `JdbcTemplate` автоматизирует эти процессы.
3. **Упрощение кода**: `JdbcTemplate` предлагает широкий спектр методов для запросов, обновлений, вставки и удаления данных, что значительно сокращает количество кода, необходимого для выполнения этих операций.
4. **Интеграция с Spring**: `JdbcTemplate` является частью экосистемы Spring и предоставляет лучшую интеграцию с другими компонентами Spring, такими как транзакционное управление.

В целом, `JdbcTemplate` является мощным инструментом для тех, кто использует Spring Framework и хочет сократить количество стандартного кода, связанного с взаимодействием с базой данных через JDBC.

## Подключение JdbcTemplate

### Подключение через Maven

```xml
    <!-- Spring Context для поддержки аннотаций и ApplicationContext -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.25.RELEASE</version>
    </dependency>

    <!-- Spring JDBC для работы с JdbcTemplate -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.39</version>
    </dependency>

    <!-- Драйвер базы данных для PostgreSQL -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.2.5</version>
    </dependency>
```

## Методы JdbcTemplate

### execute

**Назначение:** Выполняет произвольный SQL-запрос, не возвращающий результат (например, `DDL` команды)

```java
jdbcTemplate.execute("CREATE TABLE users (id INT, name VARCHAR(50))");
```

### update

**Назначение:** Выполняет SQL-запросы, которые изменяют данные в базе, такие как `INSERT`, `UPDATE` или `DELETE`. Возвращает количество измененных строк.

```java
String sql = "UPDATE users SET name = ? WHERE id = ?";
int rowsAffected = jdbcTemplate.update(sql, "John Doe", 1);
```

**Перегрузки метода:**

- `update(String sql)`
- `update(String sql, Object... args)`
- `update(String sql, PreparedStatementSetter pss)`

### queryForObject

**Назначение:** Выполняет SQL-запрос и возвращает один объект. Если результатом выполнения запроса является несколько строк, возвращается только первая. Обычно используется для выборок, возвращающих одиночное значение.

```java
String sql = "SELECT name FROM users WHERE id = ?";
String name = jdbcTemplate.queryForObject(sql, new Object[]{1}, String.class);
```

**Перегрузки метода:**

- `queryForObject(String sql, Class<T> requiredType)`
- `queryForObject(String sql, RowMapper<T> rowMapper)`
- `queryForObject(String sql, Object[] args, Class<T> requiredType)`
- `queryForObject(String sql, Object[] args, RowMapper<T> rowMapper)`

### query

**Назначение:** Выполняет SQL-запрос и возвращает список объектов, сопоставленных с результатом выполнения запроса. Используется для выборок, возвращающих несколько строк.

```java
String sql = "SELECT id, name FROM users";
List<User> users = jdbcTemplate.query(sql, new RowMapper<User>() {
    @Override
    public User mapRow(ResultSet rs, int rowNum) throws SQLException {
        User user = new User();
        user.setId(rs.getInt("id"));
        user.setName(rs.getString("name"));
        return user;
    }
});
```

**Перегрузки метода:**

- `query(String sql, RowMapper<T> rowMapper)`
- `query(String sql, Object[] args, RowMapper<T> rowMapper)`
- `query(String sql, ResultSetExtractor<T> rse)`
- `query(String sql, PreparedStatementSetter pss, RowMapper<T> rowMapper)`

### batchUpdate

**Назначение:** Выполняет пакетное обновление данных. Этот метод полезен для массовых вставок, обновлений или удалений.

```java
String sql = "INSERT INTO users (id, name) VALUES (?, ?)";
List<Object[]> batchArgs = new ArrayList<>();
batchArgs.add(new Object[]{1, "John"});
batchArgs.add(new Object[]{2, "Jane"});
int[] updateCounts = jdbcTemplate.batchUpdate(sql, batchArgs);
```

**Перегрузки метода:**

- `batchUpdate(String sql, BatchPreparedStatementSetter pss)`
- `batchUpdate(String sql, List<Object[]> batchArgs)`

### queryForList

**Назначение:** Выполняет SQL-запрос и возвращает результаты в виде списка. Каждый элемент списка представляет собой `Map`, где ключи — имена столбцов, а значения — соответствующие значения столбцов.

```java
String sql = "SELECT * FROM users";
List<Map<String, Object>> users = jdbcTemplate.queryForList(sql);
```

**Перегрузки метода:**

- `queryForList(String sql)`
- `queryForList(String sql, Object[] args)`

### queryForMap

**Назначение:** Выполняет SQL-запрос и возвращает результат в виде `Map`, где ключи — это имена столбцов, а значения — соответствующие значения. Подходит для запросов, возвращающих одну строку.

```java
String sql = "SELECT * FROM users WHERE id = ?";
Map<String, Object> user = jdbcTemplate.queryForMap(sql, 1);
```

### queryForRowSet

**Назначение:** Выполняет SQL-запрос и возвращает результат в виде `SqlRowSet` — кросс-платформенного интерфейса, похожего на `ResultSet`.

```java
String sql = "SELECT id, name FROM users";
SqlRowSet rowSet = jdbcTemplate.queryForRowSet(sql);
```

### call

**Назначение:** Используется для вызова хранимых процедур в базе данных.

```java
String procedureCall = "{call my_stored_procedure(?, ?)}";
List<SqlParameter> parameters = Arrays.asList(
    new SqlParameter(Types.VARCHAR),
    new SqlOutParameter("out_param", Types.INTEGER)
);
Map<String, Object> result = jdbcTemplate.call(new CallableStatementCreator() {
    public CallableStatement createCallableStatement(Connection con) throws SQLException {
        CallableStatement cs = con.prepareCall(procedureCall);
        cs.setString(1, "input_param");
        cs.registerOutParameter(2, Types.INTEGER);
        return cs;
    }
}, parameters);
```

## RowMapper

`RowMapper` — это интерфейс в Spring, который используется для сопоставления каждой строки результата SQL-запроса с объектом пользовательского класса. Он предназначен для упрощения преобразования данных из базы данных в объекты Java, которые затем могут быть использованы в приложении.

**Метод интерфейса `RowMapper`**

```java
public interface RowMapper<T> {
    T mapRow(ResultSet rs, int rowNum) throws SQLException;
}
```

Цель `RowMapper` — извлечь данные из `ResultSet` и преобразовать их в экземпляры нужного вам класса. Это делается для каждой строки результата запроса.

`RowMapper` работает следующим образом:

1. **SQL-запрос** выполняется через `JdbcTemplate`, и возвращается `ResultSet` — набор данных, представляющий строки результата.
2. **`RowMapper`** извлекает данные из каждой строки `ResultSet` и преобразует их в объект.
3. **JdbcTemplate** собирает все объекты, возвращаемые `RowMapper`, в список (или другой контейнер), который затем передается обратно вызывающему коду.

### Пример создания `RowMapper`

Предположим, у нас есть класс `User`:

```java
    public class User {
        private int id;
        private String name;
        private String email;

        // Геттеры и сеттеры

        public User() {}

        public User(int id, String name, String email) {
            this.id = id;
            this.name = name;
            this.email = email;
        }

        // toString(), equals(), hashCode() и другие методы
    }
    // Теперь создадим `RowMapper` для этого класса:
    public class UserRowMapper implements RowMapper<User> {

        @Override
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
            User user = new User();
            user.setId(rs.getInt("id"));
            user.setName(rs.getString("name"));
            user.setEmail(rs.getString("email"));
            return user;
        }
    }
```

### Использование `RowMapper`

```java
    // Использование UserRowMapper 
    public List<User> getAllUsers() {
        String sql = "SELECT id, name, email FROM users";
        return jdbcTemplate.query(sql, new UserRowMapper());
    }

    // Альтернативный вариант: Лямбда-выражения
    String sql = "SELECT id, name, email FROM users";
    List<User> users = jdbcTemplate.query(sql, (rs, rowNum) -> {
        return new User(
            rs.getInt("id"),
            rs.getString("name"),
            rs.getString("email")
        );
    });
```

## NamedParameterJdbcTemplate

`NamedParameterJdbcTemplate` — это один из компонентов Spring, используемый для работы с базами данных. Это расширенная версия `JdbcTemplate`, предоставляющая дополнительные возможности для работы с параметризованными SQL-запросами.

Основные понятия и особенности `NamedParameterJdbcTemplate`:

**Параметризация SQL-запросов:**

- В классическом `JdbcTemplate` параметры в SQL-запросах могут быть указаны через подстановочные знаки (`?`), а затем их значения передаются через массивы или списки параметров.
- В `NamedParameterJdbcTemplate` используются именованные параметры, что упрощает чтение и сопровождение кода. Вместо `?` используются имена параметров, что делает SQL-запросы более понятными.

**Преимущества `NamedParameterJdbcTemplate`**

- **Повышенная читаемость кода:** SQL-запросы с именованными параметрами легче читать и понимать, особенно в больших проектах.
- **Удобство работы с параметрами:** Не нужно помнить порядок параметров, как в случае с `?`. Использование именованных параметров снижает вероятность ошибок при передаче значений.
- **Поддержка сложных типов данных:** Легче работать с коллекциями и другими сложными типами данных, передаваемыми в запросы.

**Использование с аннотацией `@NamedParameter`**

В случае использования Spring Data JDBC, вы можете использовать именованные параметры непосредственно в репозиториях, аннотируя методы репозиториев с помощью `@NamedParameter`.

### Пример использования ParameterJdbcTemplate

Рассмотрим пример использования `NamedParameterJdbcTemplate`:

```java
String sql = "SELECT * FROM users WHERE id = :id AND status = :status"; // `:id` и `:status` — именованные параметры.

Map<String, Object> params = new HashMap<>(); // используется для хранения значений параметров, где ключом является имя параметра, а значением — его значение
params.put("id", 1);
params.put("status", "active");

List<User> users = namedParameterJdbcTemplate.query(sql, params, new UserRowMapper());
```

## KeyHolder

`KeyHolder` — это специальный объект, который может содержать сгенерированные ключи (например, автоинкрементные значения ID).

За реализацию `KeyHolder` отвечает класс `GeneratedKeyHolder`.

### Пример исопльзования KeyHolder

```java
    import org.springframework.jdbc.support.GeneratedKeyHolder;
    import org.springframework.jdbc.support.KeyHolder;

    @Override
    public void save(User entity) {
        String sql = "INSERT INTO users(email, password_hash) VALUES (:email, :password_hash)";
        Map<String, Object> params = new HashMap<>();
        params.put("email", entity.getEmail());
        params.put("password_hash", entity.getPasswordHash());

        // Создаем объект KeyHolder для хранения сгенерированного ключа
        KeyHolder keyHolder = new GeneratedKeyHolder();

        // Выполняем вставку и сохраняем сгенерированный ключ в keyHolder
        namedParameterJdbcTemplate.update(sql, 
            new MapSqlParameterSource(params), // Вместо обычной Map, здесь используется MapSqlParameterSource, которая совместима с KeyHolder
            keyHolder, 
            new String[] {"id"} // массив строк с именами столбцов, для которых вы хотите получить сгенерированные значения. "id" — это имя столбца в таблице, который содержит сгенерированный идентификатор
        );
        Number generatedId = keyHolder.getKey(); // вернет сгенерированный идентификатор ID из keyHolder
        if (generatedId != null) {
            // Устанавливаем сгенерированный ID в entity
            entity.setId(generatedId.longValue());
        }
    }
```
