# JUnit

`JUnit` — это широко используемая библиотека для модульного тестирования в языке программирования Java. Она позволяет разработчикам писать и выполнять тесты для проверки функциональности отдельных модулей программного обеспечения, обеспечивая тем самым их корректность и устойчивость к изменениям.

- [JUnit](#junit)
  - [Подключение с использованием Maven](#подключение-с-использованием-maven)
  - [Aннотации JUnit](#aннотации-junit)
    - [Подробнее про @ParameterizedTest](#подробнее-про-parameterizedtest)
    - [Assertions](#assertions)
    - [Запуск тестов](#запуск-тестов)

## Подключение с использованием Maven

```xml
    <properties>
        <junit.jupiter.version>5.8.1</junit.jupiter.version> <!--  Задаем версии JDK и JUnit Jupiter, которые будут использоваться в проекте -->
    </properties>

    <dependencies>
        <!-- JUnit Jupiter API  - Библиотека, содержащая API для написания тестов с использованием JUnit 5 -->
        <dependency>
            <groupId>org.junit.jupiter</groupId> 
            <artifactId>junit-jupiter-api</artifactId>
            <version>${junit.jupiter.version}</version>
            <scope>test</scope>
        </dependency>

        <!-- JUnit Jupiter Engine -  Библиотека, обеспечивающая выполнение тестов с использованием JUnit 5 -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>${junit.jupiter.version}</version>
            <scope>test</scope>
        </dependency>

        <!-- JUnit Jupiter Params - Библиотека, предоставляющая поддержку параметризованных тестов в JUnit 5 -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-params</artifactId>
            <version>${junit.jupiter.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId> <!-- Плагин Maven для запуска тестов. Включает конфигурацию для запуска всех тестов, имена которых заканчиваются на *Test или *TestSuite -->
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0-M5</version>
                <configuration>
                    <includes>
                        <include>**/*Test.java</include>
                        <include>**/*TestSuite.java</include>
                    </includes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## Aннотации JUnit

```java
    // Основные аннотации
    @Test // Обозначает метод как тестовый. Такой метод будет выполнен как тестовый случай
    @BeforeEach // Обозначает метод, который будет выполняться перед каждым тестовым методом. Используется для подготовки состояния, необходимого для тестов
    @AfterEach // Обозначает метод, который будет выполняться после каждого тестового метода. Используется для очистки после тестов
    @BeforeAll // Обозначает метод, который будет выполнен один раз перед всеми тестовыми методами в текущем классе. Метод должен быть статическим
    @AfterAll // Обозначает метод, который будет выполнен один раз после всех тестовых методов в текущем классе. Метод должен быть статическим

    // Дополнительные аннотации
    @DisplayName("Test addition operation") // Позволяет задать пользовательское имя для тестового метода или класса, которое будет отображаться в отчетах и выводе
    @Disabled("Disabled until bug #123 is fixed") // Отключает тестовый метод или класс, предотвращая его выполнение
    @Tag("fast") // Помечает тестовый метод или класс тегом, который можно использовать для фильтрации тестов при их запуске
    @Nested // Позволяет создавать вложенные тестовые классы, что помогает структурировать тесты и группировать их по функциональным блокам (Применяется к классам)
    @RepeatedTest(5) // Обозначает метод, который должен быть выполнен несколько раз
    
    @ParameterizedTest // Обозначает метод, который должен быть выполнен с различными наборами данных
    @ValueSource(strings = {"apple", "banana", "cherry"})
    void parameterizedTest(String fruit) {}

    @TestFactory // Используется для создания динамических тестов, возвращая коллекцию или поток динамических тестов
    @TestFactory
    Collection<DynamicTest> dynamicTests() {
        return Arrays.asList(
            DynamicTest.dynamicTest("Test 1", () -> assertEquals(2, 1 + 1)),
            DynamicTest.dynamicTest("Test 2", () -> assertEquals(4, 2 * 2))
        );
    }

    @TestInstance(TestInstance.Lifecycle.PER_CLASS) // Управляет жизненным циклом экземпляров тестовых классов. По умолчанию создается новый экземпляр для каждого метода, но можно изменить это поведение

```

### Подробнее про @ParameterizedTest

Аннотация `@ParameterizedTest` в **JUnit 5** позволяет выполнять один и тот же тестовый метод с разными входными данными. Это особенно полезно для тестирования одной и той же логики с различными значениями, чтобы убедиться, что код работает корректно во всех случаях.

**Основные аннотации и источники данных**:

```java
    // @ValueSource

    @ValueSource(strings = {"apple", "banana", "cherry"}) // Эта аннотация используется для предоставления набора простых значений для параметризованного теста
    //  * Поддерживает следующие типы данных: short, byte, int, long, float, double, char, boolean, и String

    enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }
    @ParameterizedTest
    @EnumSource(Day.class) // Используется для передачи значений из перечисления (enum)
    void testEnumSource(Day day) {assertNotNull(day);}

    //  @CsvSource

    @ParameterizedTest
    @CsvSource({
        "apple, 1",
        "banana, 2",
        "cherry, 3"
    }) // Используется для передачи значений в виде CSV (Comma-Separated Values)
    void testCsvSource(String fruit, int number) {
        assertNotNull(fruit);
        assertTrue(number > 0);
    }

    //  @MethodSource

    static Stream<Arguments> provideStringsForTest() {
        return Stream.of(
            Arguments.of("apple", 5),
            Arguments.of("banana", 6),
            Arguments.of("cherry", 6)
        );
    }

    @ParameterizedTest
    @MethodSource("provideStringsForTest") // Используется для передачи значений из методов, которые возвращают Stream, Iterable или массив
    void testLength(String fruit, int expectedLength) {
        assertEquals(expectedLength, fruit.length());
    }

     //  @ArgumentsSource

    static class CustomArgumentsProvider implements ArgumentsProvider {
        @Override
        public Stream<? extends Arguments> provideArguments(ExtensionContext context) {
            return Stream.of(
                Arguments.of("apple", 1),
                Arguments.of("banana", 2)
            );
        }
    }

    @ParameterizedTest
    @ArgumentsSource(CustomArgumentsProvider.class)
    void testWithArgumentsSource(String fruit, int number) {
        assertNotNull(fruit);
        assertTrue(number > 0);
    }

    //  @CsvFileSource

    @ParameterizedTest
    @CsvFileSource(resources = "/data.csv", numLinesToSkip = 1)
    //  * numLinesToSkip - Указывает, что первая строка (заголовок) должна быть пропущена. Параметр позволяет пропустить какие то первые строки файла, будь это зоголовок, комментарий или метаданные.
    void testLength(String fruit, int expectedLength) {
        assertEquals(expectedLength, fruit.length());
    }

```

### Assertions

`JUnit 5` предоставляет множество методов утверждений (Assertions), которые используются для проверки условий в тестах. Эти утверждения помогают проверить, соответствует ли фактический результат выполнения кода ожидаемому. **Если условие утверждения не выполняется, тест считается неудачным (failed)**.

**Основные методы Assertions**:

```java
    assertEquals(expected, actual); // Проверяет, что два значения равны
    assertNotEquals(unexpected, actual); // Проверяет, что два значения не равны
    assertTrue(condition); // Проверяет, что условие истинно (true)
    assertFalse(condition); // Проверяет, что условие ложно (false)
    assertNull(object); // Проверяет, что объект равен null
    assertNotNull(object); // Проверяет, что объект не равен null
    assertArrayEquals(expectedArray, actualArray); // Проверяет, что два массива равны

    assertSame(expected, actual); // Проверяет, что два объекта ссылаются на один и тот же объект
    String expected = "JUnit 5"; String actual = str; // Пример такой ссылки

    assertNotSame(unexpected, actual); // Проверяет, что два объекта не ссылаются на один и тот же объект
    //  * Ко всем методам выше можно добавить параметр сообщения в случае провала теста (3 параметром)

    assertThrows(ExceptionType.class, () -> { // Проверяет, что выполнение блока кода выбрасывает указанное исключение
        // Код, в котором должно вызваться исключение ExceptionType.class
    });

    assertDoesNotThrow(() -> { // Проверяет, что выполнение блока кода не выбрасывает исключений
        // Код, в котором не должно возникнуть исключений
    });

    assertTimeout(Duration.ofSeconds(2), () -> { // Проверяет, что выполнение блока кода укладывается в указанное время
        // Код, котроый долже завершиться за указанный таймер (в данном случае 2 секунды)
    });

    assertTimeoutPreemptively(Duration.ofSeconds(2), () -> { // Проверяет, что выполнение блока кода укладывается в указанное время, прерывая выполнение, если время истекло
        // Код, котроый долже завершиться за указанный таймер (в данном случае 2 секунды)
    });
```

**Комбинированные и сложные утверждения**:

```java
    assertAll("Заголовок для группы утверждений",
        () -> assertEquals(2, 1 + 1),
        () -> assertTrue(true),
        () -> assertNotNull(new Object())
    ); // Позволяет выполнить несколько утверждений в одном тесте и сообщает о всех сбоях сразу

    assertIterableEquals(expectedIterable, actualIterable); // Проверяет, что два Iterable содержат одинаковые элементы в одинаковом порядке

    assertLinesMatch(expectedLines, actualLines); // Проверяет, что два списка строк совпадают с учетом шаблонов (regex)
    public List<String> generateReport() {
        return Arrays.asList(
            "Report for June 2024",
            "====================",
            "Total Sales: $5000",
            "Total Expenses: $3000",
            "Net Profit: $2000"
        );
    }
    List<String> expectedReport = List.of(
        "Report for June 2024",
        "====================",
        "Total Sales: \\$\\d+",
        "Total Expenses: \\$\\d+",
        "Net Profit: \\$\\d+"
    );

    assertLinesMatch(expectedReport, generateReport()); // Пример сравнения массивово строк с шаблонами
```

### Запуск тестов

Вы можете запускать тесты различными способами:

- Через IDE, просто щелкнув правой кнопкой мыши на классе или методе и выбрав "Run".
- `mvn clean compile test` - для Maven.

JUnit является мощным инструментом для обеспечения качества кода и надежности приложений. Его простота и интеграция с другими инструментами делают его одним из наиболее популярных выборов для тестирования в Java.
