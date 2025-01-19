# Lombok

```plantext
    Lombok — это популярная библиотека для Java, которая значительно упрощает процесс написания кода, сокращая количество шаблонного кода, который разработчики должны вручную писать. Основная цель Lombok — улучшить читабельность и поддерживаемость кода, а также ускорить процесс разработки.
```

- [Lombok](#lombok)
  - [Основные функции Lombok](#основные-функции-lombok)
  - [Подключение Lombok через Maven](#подключение-lombok-через-maven)
  - [Основные аннотации и их функции](#основные-аннотации-и-их-функции)
  - [Дополнительные аннотации](#дополнительные-аннотации)
    - [@Accessors](#accessors)
    - [@With](#with)
    - [@ExtensionMethod](#extensionmethod)
    - [@Data](#data)
    - [@Slf4j](#slf4j)
      - [Добавление SLF4J и реализации логгера](#добавление-slf4j-и-реализации-логгера)
      - [Использование @Slf4j](#использование-slf4j)
  - [lombok.config](#lombokconfig)
    - [Основные настройки](#основные-настройки)

## Основные функции Lombok

1. Автоматическая генерация методов:
   - `@Getter` и `@Setter`: Автоматически генерируют методы доступа (геттеры и сеттеры) для полей класса.
   - `@ToString`: Генерирует метод toString(), который возвращает строковое представление объекта с указанием его полей.
   - `@EqualsAndHashCode`: Генерирует методы equals() и hashCode() для сравнения объектов на основе их полей.
2. Упрощение конструкций:
   - `@AllArgsConstructor` и `@NoArgsConstructor`: Генерируют конструкторы с аргументами и без, соответственно.
   - `@Builder`: Позволяет использовать паттерн «строитель» для создания объектов, что особенно полезно для классов с большим количеством полей.
3. Улучшение обработки исключений:
   - `@SneakyThrows`: Позволяет игнорировать проверяемые исключения, автоматически оборачивая их в непроверяемые, что упрощает обработку.

## Подключение Lombok через Maven

```xml
    <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
        <scope>provided</scope>
    </dependency>

    <!-- Настройка плагина для компиляции lombok -->
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.10.1</version>
        <configuration>
            <source>${java.version}</source>
            <target>${java.version}</target>
            <annotationProcessorPaths>
                <path>
                    <groupId>org.projectlombok</groupId>
                    <artifactId>lombok</artifactId>
                    <version>${lombok.version}</version>
                </path>
            </annotationProcessorPaths>
        </configuration>
    </plugin>
```

## Основные аннотации и их функции

```java
    @Getter // Генерирует геттеры и сеттеры для полей класса
    @Setter // Если указать над именем класса, то будет применен ко всем полям класса
    private String name;

    @ToString(includeFieldNames = true) // Генерирует метод toString(), возвращающий строковое представление объекта
    // includeFieldNames: Включает имена полей в строку
    // exclude: Исключает указанные поля из toString()
    @EqualsAndHashCode // Генерирует методы equals() и hashCode() на основе указанных полей
    // exclude: Исключает указанные поля из equals() и hashCode()
    @NoArgsConstructor // Генерирует конструктор без параметров
    @AllArgsConstructor // Генерирует конструктор с параметрами для всех полей
    public class Person {
        private String name;
        private int age;
    }

    @RequiredArgsConstructor // Используется для автоматической генерации конструктора, который принимает в качестве аргументов только поля, помеченные как final или @NonNull
    public class Person {
        @NonNull
        private final String name;        // Поле должно быть инициализировано
        private final int age;            // Поле должно быть инициализировано
        private String address;           // Необязательное поле

        // Другие методы и логика класса
    }


    @Builder // Позволяет использовать паттерн проектирования "строитель" для создания объектов
    public class Person {
        private String name;
        private int age;
    }
    // Пример вызова:
    Person person = Person.builder().name("Dima").age(21).build();

    @Value // Создает неизменяемый класс (immutable class). Все поля становятся final и private, а также добавляются геттеры и toString(), equals(), hashCode()
    public class Person {
        String name;
        int age;
    }

    @SneakyThrows // Позволяет игнорировать проверяемые исключения, оборачивая их в непроверяемые
    public void doSomething() {
        throw new IOException("Checked exception");
    }

    public void setName(@NonNull String name) { // Генерирует проверку на null для полей или параметров методов. Если значение null, выбрасывается NullPointerException
        this.name = name;
    }

    @Synchronized // Генерирует синхронизированные блоки, аналогичные synchronized в Java
    public void synchronizedMethod() { }

    @Chain // Позволяет создавать методы для цепочечного вызова (chain calls) для сеттеров (person.name("John").age(30))
    public Person setName(String name) {
        this.name = name;
        return this;
    }
```

## Дополнительные аннотации

### @Accessors

`@Accessors` - Позволяет настраивать стиль доступа к полям класса, влияя на то, как генерируются методы геттеров и сеттеров

**Флаги fluent и chain:**

- `fluent`: Если установлен в true, геттеры и сеттеры будут генерироваться без префикса get или set. Например, вместо getName() будет просто name(), и вместо setName(String name) будет name(String name)
- `chain`: Если установлен в true, сеттеры будут возвращать this, что позволяет использовать цепочку вызовов (chain calls). Это позволяет удобно задавать значения полей одного объекта в одной строке
Параметры prefix и suffix
- `prefix`: Позволяет установить префикс для методов доступа. Например, можно установить префикс "is" для логических переменных
- `suffix`: Позволяет установить суффикс для методов доступа, если это необходимо

`@Accessors` может применяться как к классу в целом, так и к отдельным полям. Это позволяет настраивать доступ для различных полей по-разному

**Пример использования:**

```java
    @Getter
    @Setter
    @Accessors(fluent = true, chain = true)
    public class Person {
        private String name;
        private int age;
    }
    Person person = new Person();
    person.name("John").age(30);  // Использование цепочек
    System.out.println(person.name());  // Вывод: John
```

### @With

`@With` - Предназначена для создания методов, которые позволяют создавать копии объектов с изменёнными значениями отдельных полей. Это особенно полезно в случае, когда вы работаете с неизменяемыми объектами (immutable objects), позволяя вам менять значения полей без необходимости изменять сам объект

**Пример использования:**

```java
    @Getter
    public class Person {
        @With
        private final String name;
        @With
        private final int age;

        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
    }

    // Использование
    public class Main {
        public static void main(String[] args) {
            Person person = new Person("Alice", 30);
            Person olderPerson = person.withAge(31);  // Создаем новый объект с измененным возрастом
            
            System.out.println(person.getAge()); // Вывод: 30
            System.out.println(olderPerson.getAge()); // Вывод: 31
        }
    }
```

### @ExtensionMethod

`@ExtensionMethod` - Предназначена для добавления статических методов, которые могут быть использованы как методы экземпляра (instance methods) для классов. Это позволяет разработчикам расширять функциональность существующих классов, не изменяя их исходный код, и делает код более удобочитаемым и лаконичным

**Пример использования:**

```java
    import lombok.experimental.ExtensionMethod;

    class StringUtils {
        public static String addExclamation(String str) {
            return str + "!";
        }
    }

    @ExtensionMethod(StringUtils.class)
    public class Main {
        public static void main(String[] args) {
            String message = "Hello, World";
            // Вызов метода addExclamation как метода экземпляра
            String excitedMessage = message.addExclamation();
            System.out.println(excitedMessage);  // Вывод: Hello, World!
        }
    }
```

### @Data

Аннотация `@Data` в библиотеке Lombok объединяет несколько аннотаций в одну, упрощая создание классов с полями, которые требуют автоматической генерации методов доступа, а также других стандартных методов. Это очень удобно, особенно в Java, где много шаблонного кода

**Идеально подходит для написания моделей в Spring**.

**Основные функции @Data:**

1. **Генерация геттеров и сеттеров**:
   - @Data автоматически генерирует методы доступа (геттеры и сеттеры) для всех полей класса.
2. **Генерация методов toString(), equals(), и hashCode()**:
   - Генерируются стандартные методы toString(), equals(), и hashCode(), которые основываются на полях класса.
3. **Поддержка конструктора**:
   - @Data автоматически создает конструктор с параметрами для всех полей класса (аналогично @AllArgsConstructor).
4. **Поддержка @Value для неизменяемых классов**:
    - Если вы добавите final к полям и используете @Data, он будет вести себя как @Value, создавая неизменяемые объекты

### @Slf4j

`@Slf4j` — это аннотация из библиотеки Lombok, которая автоматически добавляет логгер (логирование) в ваш класс, используя библиотеку SLF4J (Simple Logging Facade for Java). Это позволяет вам писать логирующий код быстрее и чище

#### Добавление SLF4J и реализации логгера

```xml
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>2.0.9</version> <!-- Убедитесь, что версия актуальна -->
    </dependency>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.4.11</version>
    </dependency>
```

#### Использование @Slf4j

```java
    @Slf4j // Автоматически добавляет объект логгера 'log'
    // Аннотация генерирует следующий код внутри класса: private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(Example.class);

    public class Example {

        public static void main(String[] args) {
            log.trace("Trace message");
            log.debug("Debug message");
            log.info("Info message");
            log.warn("Warning message");
            log.error("Error message");
            
            log.debug("User {} logged in at {}", username, LocalDateTime.now()); // Используйте плейсхолдеры для логов, чтобы избежать лишней работы

        }
    }
```

## lombok.config

`lombok.config` — это файл конфигурации библиотеки Lombok, который позволяет управлять поведением этой библиотеки в проекте. Он используется для настройки глобальных и локальных параметров, которые влияют на генерацию кода Lombok'ом, а также помогает задавать правила для разных модулей проекта

**Струтура проекта для lombok.config:**

```plantext
project/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── com/
│   │   │   │   ├── example/
│   │   │   │   │   ├── MyService.java
│   │   │   │   │   ├── App.java
│   │   │   │   │   └── SomeClass.java
│   │   │   │
│   │   │
│   │
│
├── pom.xml
├── lombok.config // Файл должен находится на уровне с pom.xml
```

### Основные настройки

[Документая по настройкам](https://projectlombok.org/features/configuration)

```config
    lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Qualifier // Добавляет возможность перенести аннотацию Qualifier  при использовании @RequiredArgsConstructor
    lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Value

    lombok.log.fieldName=LOG // Меняет названия логгера для @Slf4j
```
