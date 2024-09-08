# Database testing

```palntext
    В данном файле собран материал по тестированию баз данных
```

- [Database testing](#database-testing)
  - [Тестирование с использованием HSQLDB](#тестирование-с-использованием-hsqldb)
    - [Подключение зависимости для Maven](#подключение-зависимости-для-maven)
    - [Настройка конфигурации базы данных для тестов](#настройка-конфигурации-базы-данных-для-тестов)

## Тестирование с использованием HSQLDB

Создание `HSQLDB` в режиме тестов позволяет использовать эту базу данных для unit-тестирования вашего приложения, не полагаясь на внешние ресурсы. HSQLDB идеально подходит для этого благодаря своей легковесности и возможности работать в памяти.

### Подключение зависимости для Maven

```xml
    <dependency>
        <groupId>org.hsqldb</groupId> <!-- Это зависимость для HSQLDB (HyperSQL Database), легковесной реляционной базы данных на основе Java. Она используется для создания в памяти баз данных, что идеально подходит для тестирования -->
        <artifactId>hsqldb</artifactId>
        <version>2.6.0</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId> <!-- Эта зависимость предоставляет поддержку JDBC в Spring Framework, упрощая работу с JDBC через такие классы, как JdbcTemplate и NamedParameterJdbcTemplate. Она позволяет легче взаимодействовать с реляционными базами данных -->
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.9</version>
    </dependency>
```

### Настройка конфигурации базы данных для тестов

Создайте конфигурационный класс, который будет использовать HSQLDB в памяти для тестов:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.embedded.EmbeddedDatabase;
import org.springframework.jdbc.datasource.embedded.EmbeddedDatabaseBuilder;
import org.springframework.jdbc.datasource.embedded.EmbeddedDatabaseType;

import javax.sql.DataSource;

@Configuration // Указывает, что класс содержит определения бинов и может быть использован контейнером Spring IoC (Inversion of Control) для генерации бинов на основе этих определений. Это эквивалент XML-конфигурации в Spring, но в виде Java-кода. 
public class DataSourceConfig {

    @Bean // Указывает, что метод будет возвращать объект, который должен быть зарегистрирован в контексте Spring в качестве бина
    public DataSource dataSource() {
        // Создание встраиваемой базы данных HSQLDB
        EmbeddedDatabaseBuilder builder = new EmbeddedDatabaseBuilder();
        EmbeddedDatabase db = builder
                .setType(EmbeddedDatabaseType.HSQL) // Используем HSQLDB
                .addScript("classpath:schema.sql")  // Скрипт для создания схемы
                .addScript("classpath:data.sql")    // Скрипт для начального заполнения данными
                .build();
        return db;
    }
}
```
