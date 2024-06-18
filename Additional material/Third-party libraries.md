```
    В данном файле приведены сторонние библиотеки java, которые могут пригодиться в работе
```

# Cодержание

- [JCommander](#jcommander) - **Обработка параметров командой строки**
- [JCDP](#jcdp) - **Цветная печать в терминале**

## JCommander

`JCommander` - это библиотека для обработки аргументов командной строки в Java. Она предоставляет удобные средства для определения и разбора параметров командной строки, позволяя легко создавать приложения с гибкими и понятными интерфейсами командной строки.

**Подключение для Maven:**
```xml
    <dependency>
        <groupId>com.beust</groupId>
        <artifactId>jcommander</artifactId>
        <version>1.82</version>
    </dependency>
```

**Пример использования JCommander:**

```java
import com.beust.jcommander.JCommander;
import com.beust.jcommander.Parameter;

public class MyApp {
    @Parameter(names = {"-username", "-u"}, description = "User's name")
    private String username;

    @Parameter(names = {"-password", "-p"}, description = "User's password")
    private String password;

    @Parameter(names = {"-verbose", "-v"}, description = "Enable verbose mode")
    private boolean verbose = false;

    public static void main(String[] args) {
        MyApp app = new MyApp();
        JCommander.newBuilder()
                .addObject(app)
                .build()
                .parse(args);

        app.run();
    }

    public void run() {
        System.out.println("Username: " + username);
        System.out.println("Password: " + password);
        System.out.println("Verbose mode: " + verbose);
    }
}
```

**Ссылки для скачивания**: 
* https://repo1.maven.org/maven2/com/beust/jcommander/1.72/jcommander-1.72.jar
* http://www.java2s.com/Code/Jar/j/Downloadjcommanderjar.htm

**Документация**:
* https://jcommander.org/

## JCDP

`JCDP` (Java Colored Debug Printer) — это библиотека для Java, которая позволяет разработчикам выводить на консоль цветной текст. Эта библиотека упрощает форматирование сообщений для отладки и логгирования, делая вывод более читабельным и структурированным.

**Подключение для Maven:**
```xml
    <dependency>
        <groupId>com.diogonunes</groupId>
        <artifactId>JCDP</artifactId>
        <version>2.0.3.1</version>
    </dependency>
```

**Пример использования JCDP:**
```java
import com.diogonunes.jcdp.color.api.Ansi;
import com.diogonunes.jcdp.color.ColoredPrinter;
import com.diogonunes.jcdp.color.api.Ansi.FColor;
import com.diogonunes.jcdp.color.api.Ansi.BColor;

public class JCDPExample {
    public static void main(String[] args) {
        // Создаем экземпляр ColoredPrinter с базовой конфигурацией
        ColoredPrinter cp = new ColoredPrinter.Builder(1, false)
            .foreground(FColor.WHITE).background(BColor.BLUE)   // Цвет текста и фона
            .build();

        // Печать сообщения с конфигурацией по умолчанию
        cp.println("This is a default message.");

        // Печать сообщения с измененными цветами текста и фона
        cp.print("This is a message with different colors.", Ansi.Attribute.NONE, FColor.RED, BColor.YELLOW);

        // Печать сообщения с жирным текстом
        cp.println(" This is a bold message.", Ansi.Attribute.BOLD, FColor.GREEN, BColor.NONE);
        
        // Закрываем принтер
        cp.clear();
    }
}
```

**Ссылки для скачивания**: 
* https://repo1.maven.org/maven2/com/diogonunes/JCDP/4.0.0/JCDP-4.0.0.jar
* https://mavenlibs.com/jar/file/com.diogonunes/JCDP

**Документация**:
* https://dialex.github.io/JColor/index-all.html

## JDBC (PostgreSQL)

`JDBC (Java Database Connectivity)` — это стандартный интерфейс API в языке Java, который позволяет взаимодействовать с реляционными базами данных. JDBC предоставляет набор классов и интерфейсов для подключения к базе данных, выполнения SQL-запросов и обработки результатов.

Для работы с PostgreSQL через JDBC вам потребуется драйвер PostgreSQL JDBC. Этот драйвер позволяет Java-приложению взаимодействовать с базой данных PostgreSQL.

По сути JDBC - это стандарт, который реализует каждая база данных, это нужно для того, чтобы не зависеть от конкретной реализации подключенной БД. Также это нужно для того, чтобы для взаимодейстивя с любой базой данных был определен одинаковый интерфейс

**Подключение для Maven:**
```xml
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.2.5</version>
    </dependency>
```

Интерфейс JDBC предоставляет множество методов для выполнения различных операций с базами данных. Основные интерфейсы и их методы включают:

```java
    // Интерфейс `DriverManager` - Этот интерфейс управляет набором драйверов баз данных
    static Connection getConnection(String url, String user, String password) // Устанавливает соединение с базой данных по указанному URL, имени пользователя и паролю.

    // Интерфейс `Connection` - Этот интерфейс представляет соединение с конкретной базой данных
    Statement createStatement(); // Создает объект `Statement` для отправки SQL-запросов в базу данных.
    PreparedStatement prepareStatement(String sql); // Создает объект `PreparedStatement` для отправки предварительно скомпилированных SQL-запросов.
    CallableStatement prepareCall(String sql); // Создает объект `CallableStatement` для вызова хранимых процедур.
    void setAutoCommit(boolean autoCommit); // Устанавливает режим автокоммита транзакций.
    void commit(); // Выполняет явный коммит текущей транзакции.
    void rollback(); // Откатывает текущую транзакцию.
    void close(); // Закрывает соединение с базой данных.

    // Интерфейс `Statement` - Этот интерфейс используется для выполнения SQL-запросов в базу данных
    ResultSet executeQuery(String sql); // Выполняет SQL-запрос и возвращает объект `ResultSet`, содержащий результаты запроса.
    int executeUpdate(String sql); // Выполняет SQL-запрос (например, `INSERT`, `UPDATE`, `DELETE`) и возвращает количество затронутых строк.
    boolean execute(String sql); // Выполняет любой SQL-запрос. Возвращает `true`, если результатом является `ResultSet`.
    void close(); // Закрывает объект `Statement`.

    // Интерфейс `PreparedStatement` - Этот интерфейс расширяет `Statement` и используется для выполнения предварительно скомпилированных SQL-запросов с параметрами
    void setInt(int parameterIndex, int x); // Устанавливает значение параметра типа `int`.
    void setString(int parameterIndex, String x); // Устанавливает значение параметра типа `String`.
    void setDate(int parameterIndex, Date x); // Устанавливает значение параметра типа `Date`.
    ResultSet executeQuery(); // Выполняет SQL-запрос и возвращает объект `ResultSet`.
    int executeUpdate(); // Выполняет SQL-запрос и возвращает количество затронутых строк.
    boolean execute(); // Выполняет любой SQL-запрос. Возвращает `true`, если результатом является `ResultSet`.
    void close(); // Закрывает объект `PreparedStatement`.

    // Интерфейс `CallableStatement` - Этот интерфейс расширяет `PreparedStatement` и используется для вызова хранимых процедур
    void registerOutParameter(int parameterIndex, int sqlType); // Регистрирует выходной параметр.
    boolean wasNull(); // Проверяет, было ли значение последнего прочитанного параметра SQL NULL.
    int getInt(int parameterIndex); // Получает значение выходного параметра типа `int`.
    String getString(int parameterIndex); // Получает значение выходного параметра типа `String`.

    // Интерфейс `ResultSet` - Этот интерфейс используется для получения результатов SQL-запросов
    boolean next(); // Перемещает курсор на одну строку вперед из текущей позиции.
    int getInt(int columnIndex); // Возвращает значение указанного столбца как `int`.
    String getString(int columnIndex); // Возвращает значение указанного столбца как `String`.
    Date getDate(int columnIndex); // Возвращает значение указанного столбца как `Date`.
    boolean wasNull(); // Проверяет, было ли значение последней прочитанной колонки SQL NULL.
    void close(); // Закрывает объект `ResultSet`.
```
### Пример использования основных методов:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCExample {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/mydatabase";
        String user = "myusername";
        String password = "mypassword";

        try (Connection connection = DriverManager.getConnection(url, user, password);
             Statement statement = connection.createStatement();
             PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM mytable WHERE id = ?");
             ResultSet resultSet = statement.executeQuery("SELECT * FROM mytable")) {

            // Использование Statement
            while (resultSet.next()) {
                System.out.println("Column1: " + resultSet.getString(1) + " Column2: " + resultSet.getString(2));
            }

            // Использование PreparedStatement
            preparedStatement.setInt(1, 1);
            try (ResultSet preparedResultSet = preparedStatement.executeQuery()) {
                while (preparedResultSet.next()) {
                    System.out.println("Column1: " + preparedResultSet.getString(1) + " Column2: " + preparedResultSet.getString(2));
                }
            }

        } catch (SQLException e) {
            System.out.println("Connection failure: " + e.getMessage());
        }
    }
}
```


