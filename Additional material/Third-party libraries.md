# Сторонние библиотеки

```plantext
    В данном файле приведены сторонние библиотеки java, которые могут пригодиться в работе
```

- [Сторонние библиотеки](#сторонние-библиотеки)
  - [JCommander](#jcommander)
  - [JCDP](#jcdp)
  - [json (org.json)](#json-orgjson)
    - [Основные классы](#основные-классы)
    - [Основные методы](#основные-методы)
    - [Примеры использования](#примеры-использования)
      - [Чтение JSON из файла](#чтение-json-из-файла)
      - [Создание и модификация JSON-объектов](#создание-и-модификация-json-объектов)
      - [Чтение и модификация JSON-массивов](#чтение-и-модификация-json-массивов)

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

- <https://repo1.maven.org/maven2/com/beust/jcommander/1.72/jcommander-1.72.jar>
- <http://www.java2s.com/Code/Jar/j/Downloadjcommanderjar.htm>

**Документация**:

- <https://jcommander.org/>

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

- <https://repo1.maven.org/maven2/com/diogonunes/JCDP/4.0.0/JCDP-4.0.0.jar>
- <https://mavenlibs.com/jar/file/com.diogonunes/JCDP>

**Документация**:

- <https://dialex.github.io/JColor/index-all.html>

## json (org.json)

Библиотека `org.json` (также известная как JSON-java) является простой и лёгкой библиотекой для работы с JSON в Java. Она предоставляет классы для работы с JSON-объектами и массивами, позволяя легко парсить, создавать и модифицировать JSON-данные.

### Основные классы

```java
JSONObject; // Представляет JSON-объект, который содержит пары "ключ-значение".
JSONArray; // Представляет JSON-массив, который содержит упорядоченные значения.
JSONTokener; // Разбирает JSON-данные из строки или потока.
```

### Основные методы

```java
JSONObject.put(String key, Object value); // Добавляет или обновляет пару "ключ-значение".
JSONObject.getString(String key); // Возвращает значение по ключу в виде строки.
JSONObject.getInt(String key); // Возвращает значение по ключу в виде целого числа.
JSONArray.put(Object value); // Добавляет значение в массив.
JSONArray.getJSONObject(int index); // Возвращает объект JSON по индексу.
```

**Подключение для Maven:**

```xml
    <dependency>
        <groupId>com.diogonunes</groupId>
        <artifactId>JCDP</artifactId>
        <version>20210307</version>
    </dependency>
```

### Примеры использования

#### Чтение JSON из файла

```java
import org.json.JSONObject;
import java.nio.file.Files;
import java.nio.file.Paths;

public class ReadJsonExample {
    public static void main(String[] args) {
        try {
            // Чтение содержимого файла в строку
            String content = new String(Files.readAllBytes(Paths.get("config.json")));
            
            // Создание JSONObject из строки
            JSONObject json = new JSONObject(content);
            
            // Извлечение данных
            String botToken = json.getString("bot_token");
            String apiKey = json.getString("api_key");

            // Вывод данных
            System.out.println("Bot Token: " + botToken);
            System.out.println("API Key: " + apiKey);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### Создание и модификация JSON-объектов

```java
import org.json.JSONObject;

public class CreateJsonExample {
    public static void main(String[] args) {
        // Создание нового JSON-объекта
        JSONObject json = new JSONObject();
        
        // Добавление пар "ключ-значение"
        json.put("bot_token", "YOUR_BOT_TOKEN_HERE");
        json.put("api_key", "YOUR_API_KEY_HERE");

        // Вывод JSON-объекта
        System.out.println(json.toString());

        // Изменение значения по ключу
        json.put("api_key", "NEW_API_KEY_HERE");

        // Вывод обновленного JSON-объекта
        System.out.println(json.toString());
    }
}
```

#### Чтение и модификация JSON-массивов

```java
import org.json.JSONArray;
import org.json.JSONObject;

public class JsonArrayExample {
    public static void main(String[] args) {
        // Создание нового JSON-массива
        JSONArray jsonArray = new JSONArray();

        // Создание и добавление JSON-объектов в массив
        JSONObject json1 = new JSONObject();
        json1.put("name", "Alice");
        json1.put("age", 30);

        JSONObject json2 = new JSONObject();
        json2.put("name", "Bob");
        json2.put("age", 25);

        jsonArray.put(json1);
        jsonArray.put(json2);

        // Вывод JSON-массива
        System.out.println(jsonArray.toString());

        // Чтение данных из JSON-массива
        for (int i = 0; i < jsonArray.length(); i++) {
            JSONObject jsonObject = jsonArray.getJSONObject(i);
            String name = jsonObject.getString("name");
            int age = jsonObject.getInt("age");
            System.out.println("Name: " + name + ", Age: " + age);
        }

        // Модификация JSON-объекта в массиве
        jsonArray.getJSONObject(0).put("age", 31);

        // Вывод обновленного JSON-массива
        System.out.println(jsonArray.toString());
    }
}
```
