# ScriptUtils

```plantext
    ScriptUtils — это утилитарный класс в Spring Framework, который предназначен для выполнения SQL-скриптов
```

- [ScriptUtils](#scriptutils)
  - [ScriptUtils Основная информация](#scriptutils-основная-информация)
    - [Основные методы `ScriptUtils`](#основные-методы-scriptutils)
    - [Пример использования `ScriptUtils` в Spring](#пример-использования-scriptutils-в-spring)

## ScriptUtils Основная информация

`ScriptUtils` — это утилитарный класс в Spring Framework, который предназначен для выполнения SQL-скриптов. Он предоставляет методы, позволяющие легко загружать и выполнять SQL-скрипты в рамках работы с базой данных. `ScriptUtils` особенно полезен при выполнении операций с базой данных, таких как создание схемы, заполнение таблиц тестовыми данными и т.д.

`ScriptUtils` полезен для выполнения операций инициализации базы данных, управления схемами и предварительного заполнения данных. Он особенно удобен в интеграционных тестах и при разработке приложений, где требуется часто мигрировать схему базы данных.

**Основные возможности `ScriptUtils`**

- **Выполнение SQL-скриптов из различных источников:**
  `ScriptUtils` может выполнять скрипты, находящиеся в ресурсах проекта, строках, потоках и других источниках.
- **Поддержка различных диалектов SQL:**
  `ScriptUtils` поддерживает выполнение скриптов с разными диалектами SQL и управляет обработкой комментариев и разделителей команд.
- **Управление транзакциями:**
  Вы можете использовать `ScriptUtils` для выполнения скриптов в рамках транзакции, что позволяет откатывать изменения, если возникнет ошибка.

**Важные моменты:**

- **Управление транзакциями:** В зависимости от конфигурации транзакций, выполнение скрипта может быть частью транзакции. Если скрипт содержит ошибки, вы можете откатить изменения.
  
- **Обработка ошибок:** `ScriptUtils` бросает исключения, если выполнение скрипта не удается, поэтому важно обрабатывать их для корректного завершения работы приложения.

- **Поддержка различных диалектов:** `ScriptUtils` автоматически распознает команды, комментарии и разделители, что делает его гибким инструментом для работы с различными базами данных.

### Основные методы `ScriptUtils`

Вот некоторые ключевые методы, которые предоставляет `ScriptUtils`:

```java
Connection connection = dataSource.getConnection(); // Объект `Connection`, который используется для выполнения SQL
Resource resource = new ClassPathResource("schema.sql"); // Объект `Resource`, представляющий скрипт, который нужно выполнить
ScriptUtils.executeSqlScript(connection, resource); // Выполняет SQL-скрипт, расположенный в указанном ресурсе, используя предоставленное соединение

String script = "CREATE TABLE example (id INT PRIMARY KEY, name VARCHAR(100));";
ScriptUtils.executeSqlScript(connection, script); // Выполняет SQL-скрипт, представленный в виде строки
```

### Пример использования `ScriptUtils` в Spring

Предположим, у вас есть SQL-скрипт для создания таблицы и заполнения её данными, и вы хотите выполнить этот скрипт при инициализации приложения.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.ClassPathResource;
import org.springframework.jdbc.datasource.DataSourceUtils;
import org.springframework.jdbc.datasource.init.ScriptUtils;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Configuration
public class DatabaseConfig {

    @Autowired
    private DataSource dataSource;

    @Bean
    public void initDatabase() {
        try (Connection connection = DataSourceUtils.getConnection(dataSource)) {
            ScriptUtils.executeSqlScript(connection, new ClassPathResource("schema.sql"));
        } catch (SQLException e) {
            // Обработка ошибок при выполнении SQL-скрипта
            e.printStackTrace();
        }
    }
}
```
