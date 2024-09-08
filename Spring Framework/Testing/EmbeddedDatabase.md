# EmbeddedDatabase

```plantext
    В данном файле собрана информация про EmbeddedDatabase - in-memory database для использования в приложении или тестах
```

- [EmbeddedDatabase](#embeddeddatabase)
  - [EmbeddedDatabase Основная информация](#embeddeddatabase-основная-информация)
  - [Использование в тестах](#использование-в-тестах)
    - [Методы EmbeddedDatabaseBuilder](#методы-embeddeddatabasebuilder)
    - [Пример использования](#пример-использования)

## EmbeddedDatabase Основная информация

`EmbeddedDatabase` в Spring — это интерфейс, который позволяет вам создавать встроенную базу данных в памяти (in-memory database) для использования в приложении или тестах. Это удобный способ работы с базой данных, не требующий установки и настройки внешнего сервера базы данных. Встроенные базы данных идеально подходят для тестирования и небольших приложений, где требуется быстрая и лёгкая база данных.

**Основные особенности EmbeddedDatabase:**

- `In-Memory`: База данных существует только в оперативной памяти и полностью теряется после завершения работы приложения.
- `Легковесность`: Используется минимальное количество ресурсов, и база данных запускается очень быстро.
- `Поддержка нескольких баз данных`: Spring поддерживает несколько типов встроенных баз данных, таких как HSQLDB, H2 и Derby.

## Использование в тестах

### Методы EmbeddedDatabaseBuilder

`EmbeddedDatabaseBuilder` — это класс в Spring, который используется для создания встроенной базы данных (Embedded Database) на основе конфигурации, предоставленной разработчиком.

```java
    new EmbeddedDatabaseBuilder()
        .setType(EmbeddedDatabaseType.H2); // Этот метод задает тип базы данных, которую нужно использовать.
        // Доступные типы определяются перечислением EmbeddedDatabaseType, которое включает: 
        //  H2: Использует базу данных H2.
        //  HSQL: Использует базу данных HSQLDB.
        //  DERBY: Использует базу данных Apache Derby.
        .addScript("schema.sql"); // Этот метод добавляет SQL-скрипт, который будет выполнен при создании базы данных. Обычно используется для добавления скриптов создания схемы (например, schema.sql)
        .addScripts("schema.sql", "data.sql", "another_script.sql"); // Этот метод позволяет добавить несколько SQL-скриптов сразу, переданных как массив строк
        .continueOnError(true); // Этот метод определяет, должен ли процесс выполнения скрипта продолжаться при возникновении ошибки. Если установлено значение true, ошибки будут игнорироваться, и выполнение скрипта продолжится. Если false (по умолчанию), выполнение будет остановлено при первой ошибке.
        .ignoreFailedDrops(true); // Этот метод указывает, следует ли игнорировать ошибки, возникающие при выполнении SQL-команд DROP, если объект, который пытаются удалить, не существует. Это полезно, когда ваши скрипты содержат команды DROP TABLE IF EXISTS или подобные, и вы не хотите, чтобы ошибки прерывали выполнение скриптов.
        .generateUniqueName(true); // Этот метод указывает, должен ли Spring генерировать уникальное имя для базы данных. Если установлено true, имя базы данных будет уникальным для каждого запуска приложения, что может быть полезно для тестирования параллельно выполняемых тестов, чтобы избежать конфликтов между ними
        .setName("myTestDb"); // Этот метод задает имя базы данных. Если не указано, база данных получит стандартное имя testdb (или уникальное имя, если включен generateUniqueName)
        .build(); // Этот метод завершает создание и возвращает экземпляр EmbeddedDatabase. Это фактический шаг создания базы данных на основе всех предыдущих настроек.
```

### Пример использования

```java
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.jdbc.datasource.embedded.EmbeddedDatabase;
    import org.springframework.jdbc.datasource.embedded.EmbeddedDatabaseBuilder;
    import org.springframework.jdbc.datasource.embedded.EmbeddedDatabaseType;

    @Configuration
    public class DataSourceConfig {

        @Bean
        public EmbeddedDatabase dataSource() {
            return new EmbeddedDatabaseBuilder()
                    .setType(EmbeddedDatabaseType.HSQL) // Можно использовать H2, HSQL или DERBY
                    .addScript("schema.sql") // SQL-скрипт для создания схемы
                    .addScript("data.sql") // SQL-скрипт для заполнения базы данными
                    .build();
        }
    }
```
