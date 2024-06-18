```
    В данном файле собрана информация по работе с базами данных через java
```

# Cодержание

- [java.sql](#javasql)
    - [Основные интерфейсы и методы java.sql](#основные-интерфейсы-и-методы-javasql)
    - [Типы Date, Time, Timestamp в java.sql](#типы-date-time-timestamp-в-javasql)
- [JDBC (PostgreSQL)](#jdbc-postgresql)


## java.sql

Пакет `java.sql` является частью стандартной библиотеки Java и предоставляет интерфейсы и классы для работы с реляционными базами данных через `JDBC (Java Database Connectivity)`. `JDBC` позволяет разработчикам выполнять SQL-запросы, управлять транзакциями и взаимодействовать с базами данных независимо от конкретного СУБД.

`JDBC (Java Database Connectivity)` — это стандартный интерфейс API в языке Java, который позволяет взаимодействовать с реляционными базами данных. JDBC предоставляет набор классов и интерфейсов для подключения к базе данных, выполнения SQL-запросов и обработки результатов.

По сути JDBC - это стандарт, который реализует каждая база данных, это нужно для того, чтобы не зависеть от конкретной реализации подключенной БД. Также это нужно для того, чтобы для взаимодейстивя с любой базой данных был определен одинаковый интерфейс

### Основные интерфейсы и методы java.sql

```java
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.SQLException;
    import java.sql.Statement;
    import javax.sql.DataSource;

    // DriverManager - Этот интерфейс управляет набором драйверов баз данных
    Connection driverManagerConnection = DriverManager.getConnection("jdbc:postgresql://localhost:5432/mydatabase", "myusername", "mypassword"); // Устанавливает соединение с базой данных

    // Connection - Этот интерфейс представляет соединение с конкретной базой данных
    Connection connection = DriverManager.getConnection("jdbc:postgresql://localhost:5432/mydatabase", "myusername", "mypassword");

    connection.createStatement(); // Создает объект Statement для отправки SQL-запросов
    connection.prepareStatement(String sql); // Создает объект PreparedStatement для предварительно скомпилированных SQL-запросов
    connection.commit(); // Фиксирует текущую транзакцию
    connection.rollback(); // Откатывает текущую транзакцию
    connection.close(); // Закрывает соединение

    // Statement - Этот интерфейс используется для выполнения SQL-запросов в базу данных
    Statement statement = connection.createStatement(); // Создает объект `Statement` для отправки SQL-запросов в базу данных
    ResultSet resultSet = statement.executeQuery("SELECT * FROM mytable"); // Выполняет SQL-запрос и возвращает объект ResultSet
    int rowsAffected = statement.executeUpdate("UPDATE mytable SET column = value"); // Выполняет SQL-запрос и возвращает количество затронутых строк
    statement.close(); // Закрывает объект Statement

    // PreparedStatement - Этот интерфейс расширяет `Statement` и используется для выполнения предварительно скомпилированных SQL-запросов с параметрами
    PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM mytable WHERE id = ?"); // Создает объект `PreparedStatement` для отправки предварительно скомпилированных SQL-запросов
    preparedStatement.setInt(1, 1); // Устанавливает значение параметра типа int
    ResultSet preparedResultSet = preparedStatement.executeQuery(); // Выполняет SQL-запрос и возвращает объект ResultSet
    int preparedRowsAffected = preparedStatement.executeUpdate(); // Выполняет SQL-запрос и возвращает количество затронутых строк
    preparedStatement.close(); // Закрывает объект PreparedStatement

    // CallableStatement - Этот интерфейс расширяет `PreparedStatement` и используется для вызова хранимых процедур
    CallableStatement callableStatement = connection.prepareCall("{call myStoredProcedure(?, ?)}"); // Создает объект `CallableStatement` для вызова хранимых процедур.
    callableStatement.setInt(1, 1); // Устанавливает значение входного параметра типа int
    callableStatement.registerOutParameter(2, java.sql.Types.VARCHAR); // Регистрирует выходной параметр
    callableStatement.execute(); // Выполняет хранимую процедуру
    boolean isNull = callableStatement.wasNull(); // Проверяет, было ли значение последнего прочитанного параметра SQL NULL
    int output = callableStatement.getInt(2); // Получает значение выходного параметра типа int
    String output = callableStatement.getString(2); // Возвращает значение выходного параметра как String
    callableStatement.close(); // Закрывает объект CallableStatement

    // ResultSet - Этот интерфейс используется для получения результатов SQL-запросов
    ResultSet resultSet = statement.executeQuery("SELECT * FROM mytable");
    while (resultSet.next()) { // Перемещает курсор на одну строку вперед из текущей позиции
        String column1 = resultSet.getString(1); // Возвращает значение указанного столбца как String
        int column2 = resultSet.getInt(2); // Возвращает значение указанного столбца как int
        Date column3 = resultSet.getDate(3); // // Возвращает значение указанного столбца как Date
    }
     boolean isNull = resultSet.wasNull(); // Проверяет, было ли значение последней прочитанной колонки SQL NULL
    resultSet.close(); // Закрывает объект ResultSet

    // DataSource - это интерфейс, который предоставляет способ получения соединений с базой данных. В отличие от использования драйвера JDBC напрямую с помощью DriverManager, использование DataSource имеет несколько преимуществ, включая управление пулом соединений, распределение нагрузки, кэширование соединений и более гибкую конфигурацию
    DataSource dataSource = new HikariDataSource(); // Пример с использованием HikariCP
    Connection dsConnection = dataSource.getConnection(); // Возвращает новое соединение с базой данных
    Connection dsConnectionWithCredentials = dataSource.getConnection("myusername", "mypassword"); // Возвращает новое соединение с базой данных с указанными учетными данными

    // SQLException
    try {
        Connection connection = DriverManager.getConnection("jdbc:invalid:url");
    } catch (SQLException e) {
        int errorCode = e.getErrorCode(); // Возвращает код ошибки
        String sqlState = e.getSQLState(); // Возвращает SQLState связанный с исключением
        SQLException nextException = e.getNextException(); // Возвращает следующую SQLException в цепочке
        e.printStackTrace();
    }
```

### Типы Date, Time, Timestamp в java.sql

```java
    import java.sql.Date;
    import java.sql.Time;
    import java.sql.Timestamp;

    // Date
    Date sqlDate = new Date(System.currentTimeMillis()); // Создание объекта Date на основе текущего времени
    Date specificDate = Date.valueOf("2023-06-18"); // Создание объекта Date на основе строки в формате "yyyy-MM-dd"
    long timeInMillis = sqlDate.getTime(); // Получение значения времени в миллисекундах с 1 января 1970 года
    System.out.println(sqlDate); // Вывод даты в формате "yyyy-MM-dd"

    // Преобразование между java.util.Date и java.sql.Date
    java.util.Date utilDate = new java.util.Date();
    Date sqlDateFromUtilDate = new Date(utilDate.getTime()); // Преобразование java.util.Date в java.sql.Date
    java.util.Date utilDateFromSqlDate = new java.util.Date(sqlDate.getTime()); // Преобразование java.sql.Date в java.util.Date

    // Time
    Time sqlTime = new Time(System.currentTimeMillis()); // Создание объекта Time на основе текущего времени
    Time specificTime = Time.valueOf("12:34:56"); // Создание объекта Time на основе строки в формате "HH:mm:ss"
    long timeInMillisTime = sqlTime.getTime(); // Получение значения времени в миллисекундах с 1 января 1970 года
    System.out.println(sqlTime); // Вывод времени в формате "HH:mm:ss"

    // Преобразование между java.util.Date и java.sql.Time
    java.util.Date utilTime = new java.util.Date();
    Time sqlTimeFromUtilDate = new Time(utilTime.getTime()); // Преобразование java.util.Date в java.sql.Time
    java.util.Date utilDateFromSqlTime = new java.util.Date(sqlTime.getTime()); // Преобразование java.sql.Time в java.util.Date

    // Timestamp
    Timestamp sqlTimestamp = new Timestamp(System.currentTimeMillis()); // Создание объекта Timestamp на основе текущего времени
    Timestamp specificTimestamp = Timestamp.valueOf("2023-06-18 12:34:56.789"); // Создание объекта Timestamp на основе строки в формате "yyyy-MM-dd HH:mm:ss.fffffffff"
    long timeInMillisTimestamp = sqlTimestamp.getTime(); // Получение значения времени в миллисекундах с 1 января 1970 года
    int nanos = sqlTimestamp.getNanos(); // Получение наносекундной части времени
    System.out.println(sqlTimestamp); // Вывод метки времени в формате "yyyy-MM-dd HH:mm:ss.fffffffff"

    // Преобразование между java.util.Date и java.sql.Timestamp
    java.util.Date utilTimestamp = new java.util.Date();
    Timestamp sqlTimestampFromUtilDate = new Timestamp(utilTimestamp.getTime()); // Преобразование java.util.Date в java.sql.Timestamp
    java.util.Date utilDateFromSqlTimestamp = new java.util.Date(sqlTimestamp.getTime()); // Преобразование java.sql.Timestamp в java.util.Date

    // Изменить формат вывода
    SimpleDateFormat dateFormatter = new SimpleDateFormat("dd-MM-yyyy", Locale.getDefault());
    SimpleDateFormat timeFormatter = new SimpleDateFormat("HH:mm:ss", Locale.getDefault());
    SimpleDateFormat timestampFormatter = new SimpleDateFormat("dd-MM-yyyy HH:mm:ss.SSS", Locale.getDefault());
    //  * Locale.getDefault() - возвращает текущую локаль по умолчанию для среды выполнения Java. Локаль определяет настройки для форматирования даты, времени, чисел и сообщений в зависимости от географического региона и языка

    // Форматирование Date
    String formattedDate = dateFormatter.format(sqlDate);
    System.out.println("Formatted Date: " + formattedDate);

    // Форматирование Time
    String formattedTime = timeFormatter.format(sqlTime);
    System.out.println("Formatted Time: " + formattedTime);

    // Форматирование Timestamp
    String formattedTimestamp = timestampFormatter.format(sqlTimestamp);
    System.out.println("Formatted Timestamp: " + formattedTimestamp);
```

## JDBC(PostgreSQL)

Для работы с PostgreSQL через JDBC вам потребуется драйвер PostgreSQL JDBC. Этот драйвер позволяет Java-приложению взаимодействовать с базой данных PostgreSQL.

**Подключение для Maven:**
```xml
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.2.5</version>
    </dependency>
```

## HikariCP

`HikariCP` — это высокопроизводительный JDBC пул соединений для Java. Он создан для обеспечения надежности, устойчивости и высокой производительности. HikariCP стал одним из самых популярных решений для управления соединениями с базами данных благодаря своей скорости и простоте настройки.

### Основные преимущества HikariCP:
1. `Высокая производительность`: HikariCP известен своей минимальной латентностью и высокой пропускной способностью.
2. `Низкое потребление ресурсов`: Он эффективно управляет соединениями, что снижает нагрузку на ресурсы.
3. `Надежность`: HikariCP устойчив к различным сбоям и проблемам с базой данных.
4. `Простота настройки`: Легкость в конфигурации и интеграции с популярными ORM-библиотеками, такими как Hibernate.

### Основные функции HikariCP:
- Быстрая проверка и валидация соединений.
- Поддержка различных механизмов изоляции транзакций.
- Конфигурируемое время жизни и тайм-аут соединений.
- Управление размером пула соединений и авто-расширение.

**Подключение для Maven:**
```xml
    <dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP</artifactId>
        <version>5.0.1</version>
    </dependency>
```

### Конфигурация HikariCP

Для настройки `HikariCP` с использованием XML-конфигурации, вам потребуется создать и настроить файл конфигурации Spring, в котором будет описана настройка источника данных (DataSource). Обычно такой файл называется `applicationContext.xml` или `datasource-config.xml`, и он размещается в директории src/main/resources вашего проекта.

#### XML-конфигурация HikariCP:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource">
        <property name="driverClassName" value="org.postgresql.Driver"/>
        <property name="jdbcUrl" value="jdbc:postgresql://localhost:5432/mydatabase"/>
        <property name="username" value="myusername"/>
        <property name="password" value="mypassword"/>
        <property name="maximumPoolSize" value="10"/>
    </bean>
</beans>

```

#### Программная конфигурация HikariCP:

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import javax.sql.DataSource;

public class DataSourceConfig {

    public DataSource getDataSource() {
        HikariConfig config = new HikariConfig();
        config.setDriverClassName("org.postgresql.Driver");
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydatabase"); //  Устанавливает URL подключения к базе данных
        config.setUsername("myusername"); // Задают учетные данные для подключения к базе данных
        config.setPassword("mypassword"); //
        config.setMaximumPoolSize(10); // Устанавливает максимальный размер пула соединений

        return new HikariDataSource(config); // Создается экземпляр HikariDataSource с использованием конфигурации HikariConfig. Этот объект dataSource будет использоваться для получения соединений с базой данных
    }
}
```

### 4. Пример использования HikariCP

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class HikariCPExample {

    public static void main(String[] args) {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydatabase");
        config.setUsername("myusername");
        config.setPassword("mypassword");
        config.setMaximumPoolSize(10);

        DataSource dataSource = new HikariDataSource(config);

        try (Connection connection = dataSource.getConnection();
             Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery("SELECT * FROM mytable")) {

            while (resultSet.next()) {
                System.out.println("Column1: " + resultSet.getString(1) + " Column2: " + resultSet.getString(2));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```