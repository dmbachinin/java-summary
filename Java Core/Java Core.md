# Полезные темы

- [Полезные темы](#полезные-темы)
  - [Аннотации](#аннотации)
    - [Оснонвные аннотации](#оснонвные-аннотации)
    - [Написание своих аннотаций](#написание-своих-аннотаций)
      - [Мета-аннотации](#мета-аннотации)
      - [Пример написания собственных аннотаций](#пример-написания-собственных-аннотаций)
    - [AbstractProcessor](#abstractprocessor)
      - [Оснонвные методы](#оснонвные-методы)
      - [Пример процессора аннотаций](#пример-процессора-аннотаций)
      - [Обеспечение правильной работы процессоров аннотаций](#обеспечение-правильной-работы-процессоров-аннотаций)
  - [Логирование](#логирование)
  - [Работа с файловой системой](#работа-с-файловой-системой)
  - [Ввод и вывод данных](#ввод-и-вывод-данных)
    - [FileInputStream](#fileinputstream)
      - [Описание FileInputStream](#описание-fileinputstream)
      - [Основные методы FileInputStream](#основные-методы-fileinputstream)
    - [java.io.Reader](#javaioreader)
      - [Основные методы Reader](#основные-методы-reader)
    - [InputStreamReader](#inputstreamreader)
      - [Констркуторы InputStreamReader](#констркуторы-inputstreamreader)
    - [FileReader](#filereader)
      - [Конструкторы FileReader](#конструкторы-filereader)
    - [BufferedReader](#bufferedreader)
      - [Конструкторы BufferedReader](#конструкторы-bufferedreader)
      - [Как работает буферизация в BufferedReader](#как-работает-буферизация-в-bufferedreader)
    - [Методы BufferedReader и их работа с буфером](#методы-bufferedreader-и-их-работа-с-буфером)
      - [Заключение](#заключение)
    - [FileOutputStream](#fileoutputstream)
      - [Описание FileOutputStream](#описание-fileoutputstream)
      - [Основные методы FileOutputStream](#основные-методы-fileoutputstream)
    - [java.io.Writer](#javaiowriter)
      - [Основные методы Writer](#основные-методы-writer)
    - [OutputStreamWriter](#outputstreamwriter)
      - [Констркуторы OutputStreamWriter](#констркуторы-outputstreamwriter)
    - [FileWriter](#filewriter)
      - [Конструкторы FileWriter](#конструкторы-filewriter)
    - [BufferedWriter](#bufferedwriter)
      - [Конструкторы BufferedWriter](#конструкторы-bufferedwriter)
  - [Работа с URL](#работа-с-url)
    - [Описание основных методов класса URL](#описание-основных-методов-класса-url)
    - [Конструкторы класса URL](#конструкторы-класса-url)
    - [Методы для получения частей URL](#методы-для-получения-частей-url)
    - [Методы для работы с соединением](#методы-для-работы-с-соединением)
    - [HttpURLConnection](#httpurlconnection)
      - [Методы настройки запроса](#методы-настройки-запроса)
      - [Методы отправки данных](#методы-отправки-данных)
      - [Методы чтения ответа](#методы-чтения-ответа)
    - [Пример использования класса URL](#пример-использования-класса-url)
    - [Пример использования openConnection()](#пример-использования-openconnection)
    - [Описание](#описание)
  - [Stream API](#stream-api)

## Аннотации

`Аннотации в Java` — это специальные метаданные, которые можно добавлять к коду для предоставления дополнительной информации компилятору, инструментам разработки или самому приложению во время выполнения. Аннотации могут использоваться для различных целей, таких как указание компилятору на необходимость выполнения определенных проверок, создание документации или использование в различных фреймворках.

### Оснонвные аннотации

```java
    Для методов

    @Override // Данная аннотация показывает, что мы хотим переопределить метод, который был в родительском классе
    //  * Если переопределение не будет происходить, то компилятор укажет на проблемы

    @Deprecated /** Тут пишется, что лучше использовать вместо него*/ //Означает, что класс или метод устарел и не рекомендуется к использованию

    @SuppressWarnings({"unchecked", "deprecation"}) // Временно отключает предупреждения компилятора на подозрительные места в коде.
    /* Все типы отключаемых предупреждений:
        all - Отключает все предупреждения
        unchecked - Отключает предупреждения о небезопасных операциях с необобщенными типами
        deprecation - Отключает предупреждения об использовании устаревших (deprecated) методов или классов
        serial - Отключает предупреждения о классе, который должен иметь serialVersionUID (для сериализуемых классов)
        fallthrough - Отключает предупреждения о пропущенных операторах break в конструкциях switch
        unused - Отключает предупреждения о неиспользуемых локальных переменных или параметрах
        rawtypes - Отключает предупреждения о необобщенных типах (Raw types)
        cast - Отключает предупреждения о небезопасных приведениях типов
        divzero - Отключает предупреждения о делении на ноль
        empty - Отключает предупреждения о пустых блоках кода
        finally - Отключает предупреждения о блоках finally, которые не завершаются нормально
        hiding - Отключает предупреждения о сокрытии полей суперкласса
        incomplete-switch - Отключает предупреждения о неполных конструкциях switch
        nls - Отключает предупреждения, связанные с проблемами локализации (non-nls)
        null - Отключает предупреждения о возможных значениях null
        static-access - Отключает предупреждения о некорректном доступе к статическим членам класса
        synthetic-access - Отключает предупреждения о доступе к синтетическим членам класса
        restriction - Отключает предупреждения об использовании запрещенных или ограниченных API
        resource - Отключает предупреждения об использовании ресурсов, которые не закрываются 
    */
    
    @SafeVarargs // Обозначает, что использование параметров переменной длины (varargs) безопасно

    // Для интерфейсов

    @FunctionalInterface // Означает, что данный интерфейс реализует 1 абстрактный метод
    // * При этом дефолтные методы и статические методы не идут в счет

    // Для переменных

    @Nullable 
    String nullText = null; // Означает, что данная переменная может принять значение null
    @NonNull
    String notNullText = "Hi"; // Означает, что данная переменная не может принять значение null

    @Size(min = 1, max = 100)
    private String description; //  Используются в Bean Validation (например, Hibernate Validator) для указания ограничений на значения полей
```

### Написание своих аннотаций

Для создания собственной аннотации в Java используется ключевое слово `@interface`. Аннотации могут иметь элементы, которые работают как параметры аннотации.

#### Мета-аннотации

Java предоставляет несколько мета-аннотаций, которые используются для настройки аннотаций:

```java
    @Retention(RetentionPolicy.RUNTIME) // Указывает, как долго аннотация сохраняется (SOURCE, CLASS, RUNTIME)
    //  SOURCE - Аннотации с политикой SOURCE видны только на этапе компиляции и отбрасываются компилятором. Эти аннотации не включаются в скомпилированный байт-код. (Используются для предоставления информации компилятору или для инструментов разработки. Например, @Override)
    //  CLASS - Аннотации с политикой CLASS сохраняются в скомпилированном байт-коде, но не доступны во время выполнения через рефлексию. Они видны только на этапе компиляции и загрузки класса (Используются для аннотаций, которые должны быть доступны для инструментов обработки байт-кода или других инструментов на этапе загрузки класса, но не нужны во время выполнения.)
    // RUNTIME - Аннотации с политикой RUNTIME сохраняются в байт-коде и доступны во время выполнения через рефлексию. Это позволяет программам считывать аннотации и использовать их для динамического поведения (Используются для аннотаций, которые должны быть доступны во время выполнения для выполнения задач, таких как валидация, конфигурация или внедрение зависимостей.)
    
    @Target(ElementType.METHOD) // Указывает, к чему может применяться аннотация (типы элементов: классы, методы, поля и т.д.)
    /* Возможные значения для ElementType
        TYPE - Классы, интерфейсы (включая аннотации), перечисления
        FIELD - Поля (включая перечисляемые константы)
        METHOD - Методы
        PARAMETER - Параметры методов
        CONSTRUCTOR - Конструкторы
        LOCAL_VARIABLE - Локальные переменные
        ANNOTATION_TYPE - Другие аннотации
        PACKAGE - Пакеты
        TYPE_PARAMETER - Параметры обобщенных типов
        TYPE_USE - Любое использование типов
    */

    @Inherited // Указывает, что аннотация может наследоваться подклассами

    @Documented // Указывает, что аннотация должна быть включена в Javadoc
```

#### Пример написания собственных аннотаций

```java
    // Пример создания собственной аннотации
    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;

    // Указываем, что аннотация доступна во время выполнения
    @Retention(RetentionPolicy.RUNTIME)
    // Указываем, что аннотация может применяться к методам
    @Target(ElementType.METHOD)
    public @interface MyAnnotation {
        String value(); // Если не указывать значение по умолчанию, то необходимо обязательно указать значение
        int number() default 0; // Элемент с значением по умолчанию
    }
    // Пример использования аннотации
    public class Example {
        @MyAnnotation(value = "Test", number = 5)
        public void myMethod() {
            System.out.println("MyMethod is called");
        }
    }
    // Пример получения значений из аннотации
    import java.lang.reflect.Method;

    public class AnnotationReader {
        public static void main(String[] args) throws Exception {
            Method method = Example.class.getMethod("myMethod"); // возвращает объект Method, который представляет собой указанный метод публичного доступа (включая унаследованные методы). В данном случае, мы получаем метод myMethod из класса Example

            if (method.isAnnotationPresent(MyAnnotation.class)) { // Проверяет, присутствует ли аннотация @MyAnnotation на методе myMethod
                MyAnnotation annotation = method.getAnnotation(MyAnnotation.class); // Возвращает экземпляр аннотации @MyAnnotation, если она присутствует на данном методе. Если аннотация отсутствует, возвращается null.
                System.out.println("Value: " + annotation.value()); // Возвращает значение из аннотации
                System.out.println("Number: " + annotation.number());
            }
        }
    }

    // Пример использования аннотации к переменным
    public class User {
        @Validate(message = "Name cannot be empty")
        private String name;

        public User(String name) {
            this.name = name;
        }
    }
    // Чтение и обработка аннотаций
    import java.lang.reflect.Field;

    public class Validator {
        public static void validate(Object obj) throws Exception {
            Field[] fields = obj.getClass().getDeclaredFields(); // Возвращает массив Field, представляющий все поля, объявленные в классе, включая приватные и защищенные поля, но не включая унаследованные поля

            for (Field field : fields) {
                if (field.isAnnotationPresent(Validate.class)) { //  Проверяет, присутствует ли указанная аннотация на данном поле
                    field.setAccessible(true); // Позволяет получить доступ к приватным полям для чтения и записи
                    Validate annotation = field.getAnnotation(Validate.class); // Возвращает экземпляр аннотации, если она присутствует на данном элементе
                    Object value = field.get(obj); // Возвращает значение поля из указанного объекта. В данном случае это значение поля для объекта

                    if (value == null || (value instanceof String && ((String) value).isEmpty())) {
                        throw new Exception(annotation.message());
                    }
                }
            }
        }

        public static void main(String[] args) {
            try {
                User user = new User("");
                validate(user);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
```

### AbstractProcessor

`AbstractProcessor` в Java является абстрактным классом, который предоставляет основу для создания аннотаций процессоров. Процессоры аннотаций используются для обработки аннотаций во время компиляции кода. Этот механизм позволяет разработчикам анализировать и изменять исходный код на основе аннотаций.

#### Оснонвные методы

```java
    @Override
    public synchronized void init(ProcessingEnvironment processingEnv) { // Вызывается один раз при инициализации процессора
        super.init(processingEnv);         // Инициализация процессора
    }
    //  * Используется для настройки процессора и получения ProcessingEnvironment, который предоставляет доступ к различным утилитам, таким как Filer, Messager и Elements

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) { // Основной метод для обработки аннотаций. Метод принимает два параметра: набор аннотаций для обработки и окружение текущего раунда обработки (RoundEnvironment)
        for (Element element : roundEnv.getElementsAnnotatedWith(ExampleAnnotation.class)) {
            // Обработка аннотированных элементов
        }
        return true; // Возвращает true, если аннотации были обработаны этим процессором, иначе false
    }
    //  * Вызывается для каждой компиляционной единицы, которая содержит аннотированные элементы

    @Override
    public Set<String> getSupportedAnnotationTypes() { // Возвращает набор типов аннотаций, которые поддерживаются этим процессором
        return Collections.singleton(ExampleAnnotation.class.getCanonicalName());
    }
    //  * Может быть заменен аннотацией @SupportedAnnotationTypes

    @Override
    public SourceVersion getSupportedSourceVersion() { // Возвращает версию исходного кода Java, поддерживаемую этим процессором
        return SourceVersion.latestSupported();
    }
    //  * Может быть заменен аннотацией @SupportedSourceVersion

    @Override
    public Set<String> getSupportedOptions() { // Возвращает набор имен опций конфигурации, поддерживаемых этим процессором
        return Collections.singleton("debug");
    }
    //  * Может быть заменен аннотацией @SupportedOptions
```

#### Пример процессора аннотаций

```java
    import com.example.ExampleAnnotation;
    import javax.annotation.processing.AbstractProcessor;
    import javax.annotation.processing.Processor;
    import javax.annotation.processing.RoundEnvironment;
    import javax.annotation.processing.SupportedAnnotationTypes;
    import javax.annotation.processing.SupportedSourceVersion;
    import javax.lang.model.SourceVersion;
    import javax.lang.model.element.Element;
    import javax.lang.model.element.TypeElement;
    import java.io.IOException;
    import java.io.Writer;
    import java.util.Set;
    import javax.tools.Diagnostic;
    import javax.tools.JavaFileObject;

    @SupportedAnnotationTypes("com.example.ExampleAnnotation") // Определяет пользовательскую аннотацию, которая будет обрабатываться процессором.
    @SupportedSourceVersion(SourceVersion.RELEASE_8) // Указывает, что процессор поддерживает версию исходного кода Java 8 (или более позднюю)
    public class ExampleProcessor extends AbstractProcessor {

        @Override
        public synchronized void init(ProcessingEnvironment processingEnv) { // Инициализация процессора
            super.init(processingEnv);
        }

        @Override
        public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
            for (Element element : roundEnv.getElementsAnnotatedWith(ExampleAnnotation.class)) { // Проходит по всем элементам, аннотированным ExampleAnnotation. Метод getElementsAnnotatedWith возвращает набор элементов, которые помечены данной аннотацией в текущем раунде компиляции
                String className = element.getSimpleName().toString(); // Получает простое имя аннотированного элемента и преобразует его в строку. Это имя будет использоваться для создания имени нового Java-файла
                try {
                    JavaFileObject file = processingEnv.getFiler().createSourceFile("Generated" + className); // Создает новый файл с именем Generated + className (например, если класс называется MyClass, то файл будет называться GeneratedMyClass.java). Метод createSourceFile из объекта Filer используется для создания нового исходного файла Java.
                    try (Writer writer = file.openWriter()) { // Запись содержимого в файл
                        writer.write("package com.example;\n");
                        writer.write("public class Generated" + className + " {\n");
                        writer.write("    // This class is generated by ExampleProcessor\n");
                        writer.write("}\n");
                    }
                } catch (IOException e) {
                    processingEnv.getMessager().printMessage(Diagnostic.Kind.ERROR, e.toString());
                }
            }
            return true; // Аннотации обработаны этим процессором
        }
    }
```

#### Обеспечение правильной работы процессоров аннотаций

`maven-compiler-plugin` используется для компиляции Java-кода. Настройки этого плагина могут быть изменены для поддержки аннотаций и указания процессоров аннотаций. Вот пример настройки:

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <executions>
                    <execution>
                        <phase>compile</phase>
                        <goals>
                            <goal>compile</goal>
                        </goals>
                        <configuration>
                            <annotationProcessors>
                                <annotationProcessor>
                                    edu.school.htmlProcessor.HtmlProcessor <!-- Путь к прцессору -->
                                </annotationProcessor>
                            </annotationProcessors>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>    
```

Библиотека `auto-service` от Google используется для автоматической регистрации процессоров аннотаций. Она позволяет упростить процесс создания файлов META-INF/services, которые необходимы для регистрации процессоров аннотаций.
**Поключение через Maven**

```xml
    <dependencies>
        <dependency>
            <groupId>com.google.auto.service</groupId>
            <artifactId>auto-service</artifactId>
            <version>1.0-rc7</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```

**Пример использования**:

```java

import com.google.auto.service.AutoService;

@AutoService(Processor.class) // Автоматическая регистрация процессора аннотаций
@SupportedAnnotationTypes("com.example.ExampleAnnotation")
@SupportedSourceVersion(SourceVersion.RELEASE_8)
public class ExampleProcessor extends AbstractProcessor {...}
```

## Логирование

```java
    import java.util.logging.*; - импорт библиотеки логгера

    private static final Logger LOGGER = Logger.getLogger(ClassName.class.getName()); - стандартное создание логгера для текущего класса
    * В качетстве имя логгера передается имя текущего класса

    Запись логов

    LOGGER.log(Level.INFO, "[Text]"); - запись лога
    * Уровни:
    SEVERE - на данном уровне логируются серьезные ошибки программы (ужас ужас)
    WARNING - логируются предупреждения
    INFO - просто информация
    CONFIG - логирование конфигурационных параметров
    FINE   |
    FINER  | - для детального логирования, что программа делает
    FINEST |

    LOGGER.warning("[Text]"); - у логгера есть возможность показывать сообщения в консоль определенного уровня
    * Для каждого уровня есть одноименный метод
    LOGGER.setLeval(Level.[level]); - установить для логгера уровень, чтобы сообщения более низких уровней игнорировались
    * По у молчанию стоит уровень INFO

    LOGGER.log(Level.[level], "{0}", [param]); - сопособ подстановки ОДНОГО параметра в строку сообщения
    LOGGER.log(Level.[level], "{0} {1}", new Object[] {param1, param2}); - способ подстановки нескольких параметров в строку
    LOGGER.log(Level.[level], "[Text]", e); - способ передачи ошибки при логировании

    Обработчики сообщения - определяе, куда будет записано сообщение

    java.until.logging.ConsoleHandler - Выводит в консоль
    java.until.logging.FileHandler - Выводит в файл
    java.until.logging.SocketHandler - Отправляет по сети

    LOGGER.addHandler([handler]); - добавление хендлера к логгеру

    Форматы вывода - определяет в каком формате сообщения записываются в лог

    java.until.logging.SimpleFormatter - формат записи в виде текста
    java.until.logging.XMLFormatter - формат записи в виде XML файла

    * Формат определяется у хендлера:
    Handler handler; - объявление
    handler.setFormatter([formatter]); - определения формата записи
    handler.setLevel(Level.[level]); - определения уровня

    logging.properties - файл, который можно создать в проекте на уровне папки src, в котором можно прописать конфигурацию логирования по умолчанию
    * Пример:
    .level=ALL - разрешает видеть записи на всех уровнях
    .handler=java.until.logging.ConsoleHandler - определение обработчика
    .java.until.logging.ConsoleHandler.level=ALL - определение уровня обработчика
    ** Чтобы запустить программу с настройками логирования через файл, необходимо при запуске программы передать параметр:
    -Djava.until.logging.config.file=logging.properties
```

## Работа с файловой системой

```java
    import java.nio.file.Path;
    import java.nio.file.Paths;
    import java.nio.file.Files;
    import java.io.File - класс, который отечает за работу с файлами
    // * Немного устареший класс

    // ДОКУМЕНТАЦИЯ: https://javarush.com/groups/posts/2275-files-path

    File javaFile = new File("[Path]"); // создание экземпляра файла через передачу пути конструктору
    // * Адрес директории или файла может записываться по разному, в зависимости от системы:
    Windows: "C:\\dir1\\bin\\java.java"
    Unix: "usr/bin/ls"
    // * В объявлении пути к файлу можно использовать как абсолютный так и относительные пути

    File.separator // содержит символ для разделения адреса (зависит от операционной системы, например в Windows = "\\")
    File.separatorChar // тоже самое, что и File.separator, только не типа String, а Char

    File.pathSeparator // содержит символ для разделения набора путей (зависит от операционной системы, например Windows = ";")
    File.pathSeparatorChar // тоже самое, что и File.pathSeparator, только не типа String, а Char

    javaFile.isAbsolute(); // проверяет, задан ли путь к файлу абсолютным путем
    javaFile.getAbsolutePath(); // получить абсолютный путь для файла

    javaFile.getPath(); // получить полный адрес файла ("C:\\dir1\\bin\\java.java")
    javaFile.getName(); // получить имя файла ("java.java")
    javaFile.getParent(); // получить путь до директории с файлом ("C:\\dir1\\bin")

    javaFile.getCanonicalPath(); // привести путь файла к конаническому виду, т.е. получить релальный путь до файла (без учета ссылок с система)
    // * Если привести пути 2//х файлов к каноническому виду, то можно сравнить их как строки и понять один и тот же это файл или нет

    javaFile.exists(); // проверяет, существует ли файл
    javaFile.isFile(); // проверяет, является ли файл именно файлом
    javaFile.isDirectory(); // проверяет, является ли файл директорией

    javaFile.length(); // возвращает размер файла в байтах
    javaFile.lastModified(); // возвращает время последнеего изменения в миллисекундах с 1970 года
    // * Если файла не существует то данный методы будут возвращать 0

    javaFile.list(); // возвращает массив строк файлов данной директории (если файл директория)
    javaFile.listFiles(); // возвращает массив файлов данной директории (если файл директория)
    // * Если директории не существует, то вернется null

    javaFile.createNewFile(); // создает файл в системе, по указанному адресу (адрес указывается при инициализации)
    // * Если файл создан, то возвращает true, если файл уже существует, то false
    // * Может выбрасить #Проверяемые# ошибку (IOException), поэтому ее нужно указать в имени функции через throws IOException
    javaFile.mkdir(); // создает директорию (только 1)
    javaFile.mkdirs(); // создает структуру из директорий (любого размера)

    javaFile.delete(); // удаляет данный файл или директорию(она должна быть пуста)

    java.nio.file.Path // класс, который отечает за работу с файлами
    // * Более современная версия, которая бросает исключения при ошибке работы с файлами

    Path path = Paths.get("[Path]"); // объявление пути к файлу
    File fromPath = path.toFile(); // конвертировать путь в файл
    Path fromFile = javaFile.toPath(); // конвертировать файл в путь

    Path java = Paths.get("[Path]"); // пример создания пути к файлу
    java.isAbsolute(); // проверяет, задан ли путь к файлу абсолютным путем
    java.getFileName(); // возвращает имя файла
    java.getParent(); // получить путь до директории, где хранится файл
    java.getNameCount(); // возвращает количество компонентов пути
    java.getName([idx]); // получить имя компонента пути по индексу
    java.startWith("[Path]"); // проверка на то, начинается ли путь с заданного пути

    Files.move(Path sourcePath, Path destinationPath, StandardCopyOption.REPLACE_EXISTING); // Перемещение файла
    Files.exists(java); // проверка сущетсвования файла по данному пути
    Files.size(java); // возвращает размер файла в байтах
    Files.getLastModifiedTime(java).toMillis(); // возвращает время последнеего изменения в миллисекундах с 1970 года
    Files.copy(java, Paths.get("[newPath]"), [option]); // копирует файл в указанную директорию
    // * Набор опций содержится в классе StandardCopyOption:
    StandardCopyOption.REPLACE_EXISTING // опция, которая отвечает за замену файла при копировании, если одноименный файл уже существует
    Files.createDirectoriy(path); // создает директорию (только 1)
    Files.createDirectories(path); // создает структуру из директорий (любого размера)
```

## Ввод и вывод данных

`java.io.InputStream` - базовый абстрактный класс, который обеспечивает поток байт, из которых можно читать и информацию
`java.io.OutputStream` - базовый абстрактный класс, который обеспечивает поток байт, в который можно записывать информацию

В Java классы `FileInputStream` и `FileOutputStream` используются для чтения и записи данных из файлов в байтовом формате. Эти классы являются частью пакета `java.io` и позволяют работать с файлами на низком уровне.

### FileInputStream

#### Описание FileInputStream

`FileInputStream` используется для чтения байтов из файла. Он наследует класс `InputStream` и предоставляет методы для чтения данных из файла.

#### Основные методы FileInputStream

``` java
int read() // Читает один байт данных из входного потока. Возвращает данных в виде целого числа от 0 до 255. Возвращает -1, если достигнут  файла.
int read(byte[] b) // Читает байты из потока и помещает их в массив `b`. ащает количество прочитанных байтов или -1, если достигнут конец файла.
int read(byte[] b, int off, int len)//  Читает до `len` байтов из потока ещает их в массив `b`, начиная с смещения `off`. Возвращает количество танных байтов или -1, если достигнут конец файла.
void close() // Закрывает поток и освобождает все системные ресурсы, связанные с этим потоком.
```

### java.io.Reader

`java.io.Reader` - отвечает за чтение потока символов (абстрактный класс)

#### Основные методы Reader

```java
    int read() throws IOException // Читает один символ и возвращает его в виде целого числа. Возвращает -1, если достигнут конец потока
    int read(char[] cbuf) throws IOException // Читает символы в массив cbuf и возвращает количество фактически прочитанных символов. Возвращает -1, если достигнут конец потока.
    int read(char[] cbuf, int offset, int length) throws IOException // Читает до length символов в массив cbuf, начиная с позиции offset. Возвращает количество фактически прочитанных символов или -1, если достигнут конец потока.
    long skip(long n) throws IOException // Пропускает n символов в потоке. Возвращает количество фактически пропущенных символов.
    boolean ready() throws IOException // Проверяет, готов ли поток для чтения без блокировки. Возвращает true, если поток готов, иначе false.
    boolean markSupported() // Проверяет, поддерживает ли поток операцию mark (отметку). Возвращает true, если поддерживает, иначе false.
    void mark(int readAheadLimit) throws IOException // Помечает текущую позицию в потоке, чтобы можно было вернуться к ней позже с помощью метода reset. Параметр readAheadLimit определяет, сколько символов можно прочитать перед тем, как отметка станет недействительной.
    void reset() throws IOException // Возвращает поток к позиции, отмеченной последним вызовом метода mark
    void close() throws IOException // Закрывает поток и освобождает все связанные с ним ресурсы.
```

### InputStreamReader

`InputStreamReader` Преобразует байтовые потоки (InputStream) в символьные потоки (Reader). Поддерживает указание кодировки для чтения данных. Является наследником для [Reader](#javaioreader) и реализует его методы.

#### Констркуторы InputStreamReader

```java
    InputStreamReader fileReader = new InputStreamReader(new FileInputStream("in.txt"), StandardCharset.UTF_8); // Пример инициализации InputStreamReader

    InputStreamReader(InputStream in) // Создает InputStreamReader, использующий кодировку по умолчанию.
    InputStreamReader(InputStream in, String charsetName) // Создает InputStreamReader, использующий указанную кодировку.
    InputStreamReader(InputStream in, Charset charset) // Создает InputStreamReader, использующий указанный объект Charset.
    InputStreamReader(InputStream in, CharsetDecoder dec) // Создает InputStreamReader, использующий указанный CharsetDecoder.

    OutputStreamWriter fileWriter = new OutputStreamWriter(new FileInputStream("in.txt"), StandardCharset.UTF_8); |
```

### FileReader

Класс `FileReader` наследует все методы от класса [InputStreamReader](#inputstreamreader), а также от его родителя [Reader](#javaioreader). Вот основные методы, которые можно использовать с объектами FileReader:

Особенности FileReader:

- Предназначен для чтения текстовых файлов.
- Автоматически использует кодировку по умолчанию для вашей системы (обычно UTF-8).
- Подходит для простых случаев чтения текстовых файлов, но не предоставляет буферизацию.

#### Конструкторы FileReader

```java
    FileReader(String fileName) // Создает объект FileReader, который будет читать из файла с указанным именем
    FileReader(File file) // Создает объект FileReader, который будет читать из указанного объекта File
    FileReader(FileDescriptor fd) // Создает объект FileReader, который будет читать из указанного дескриптора файла
```

### BufferedReader

`BufferedReader` в Java - это класс, который добавляет буферизацию к потоку символов, что значительно улучшает производительность при чтении данных из источника, такого как файл или сеть. Буферизация помогает уменьшить количество операций ввода-вывода, группируя их в более крупные блоки, что делает чтение данных более эффективным.

#### Конструкторы BufferedReader

```java
    BufferedReader(Reader in) // Создает буферизированный поток чтения с буфером по умолчанию (8192 символов)
    BufferedReader(Reader in, int sz) // Создает буферизированный поток чтения с указанным размером буфера
    BufferedReader(InputStreamReader in) // Создает буферизированный поток чтения для InputStreamReader

    // Пример
    BufferedReader reader = new BufferedReader(new FileReader("example.txt"));
    BufferedReader reader = new BufferedReader(new FileReader("example.txt"), 16384);
```

`BufferedReader` наследует методы класса [Reader](#javaioreader) и добавляет свои собственные:

```java
    String line = reader.readLine(); // Читает одну строку текста. Возвращает null, если достигнут конец потока
```

#### Как работает буферизация в BufferedReader

Буферизация в классе `BufferedReader` используется для повышения производительности при чтении данных из символьных потоков, таких как файлы или сетевые соединения. Буферизация помогает уменьшить количество операций ввода-вывода (I/O), которые могут быть медленными и ресурсоемкими, за счет чтения больших блоков данных за один раз и сохранения их во внутреннем буфере для последующего использования.

1. **Инициализация буфера**:
   - При создании объекта `BufferedReader` создается внутренний буфер фиксированного размера (по умолчанию 8192 символа или 8 КБ). Размер буфера можно изменить, передав нужное значение в конструктор.

2. **Чтение данных в буфер**:
   - Когда вы вызываете метод `read` или `readLine` на `BufferedReader`, он сначала проверяет, есть ли данные в буфере. Если буфер пуст, `BufferedReader` считывает большой блок данных из исходного потока (например, файла) и заполняет буфер.
   - Это чтение большого блока данных вместо отдельных символов или строк значительно снижает количество обращений к исходному потоку.

3. **Обслуживание запросов из буфера**:
   - После заполнения буфера данные читаются из него до тех пор, пока он не опустеет. Это означает, что последующие вызовы методов `read` или `readLine` могут обрабатываться быстро, так как данные уже находятся в памяти и не требуется выполнение операций I/O.
   - Когда буфер опустеет, `BufferedReader` снова считывает следующий блок данных из исходного потока и заполняет буфер.

### Методы BufferedReader и их работа с буфером

1. **String readLine()**:
   - Читает одну строку текста, завершающуюся символом новой строки (`\n`), возвратом каретки (`\r`) или их комбинацией (`\r\n`).
   - Если строка полностью находится в буфере, она извлекается и возвращается.
   - Если строка пересекает границу буфера, `BufferedReader` считывает дополнительные данные в буфер, чтобы завершить чтение строки.

2. **int read()**:
   - Читает один символ из буфера и возвращает его в виде целого числа.
   - Если буфер пуст, `BufferedReader` заполняет его, считывая данные из исходного потока.

3. **int read(char[] cbuf, int off, int len)**:
   - Читает до `len` символов в массив `cbuf`, начиная с позиции `off`.
   - Если запрашиваемые данные находятся в буфере, они извлекаются из него.
   - Если запрашиваемые данные превышают размер оставшегося буфера, `BufferedReader` заполняет буфер дополнительными данными и продолжает чтение.

#### Заключение

Буферизация в `BufferedReader` значительно улучшает производительность ввода-вывода, уменьшая количество операций чтения из основного источника данных. Это достигается за счет использования внутреннего буфера, который позволяет считывать большие блоки данных за один раз и обслуживать последующие запросы из памяти, а не из исходного потока. Это делает `BufferedReader` особенно полезным для работы с текстовыми файлами и другими символьными потоками, где частое чтение данных может быть накладным.

### FileOutputStream

#### Описание FileOutputStream

`FileOutputStream` используется для записи байтов в файл. Он наследует класс `OutputStream` и предоставляет методы для записи данных в файл.

#### Основные методы FileOutputStream

```java
void write(int b) // Записывает один байт данных, указанный аргументом `b`, в выходной поток.
void write(byte[] b) // Записывает массив байтов `b` в выходной поток.
void write(byte[] b, int off, int len) // Записывает `len` байтов из массива `b`, начиная с смещения `off`, в выходной поток.
void close() // Закрывает поток и освобождает все системные ресурсы, связанные с этим потоком.
```

### java.io.Writer

`java.io.Writer` - отвечает за вывод данных в потоке символов (абстрактный класс)

#### Основные методы Writer

```java
    void write(int c) throws IOException // Записывает один символ
    void write(char[] cbuf) throws IOException // Записывает массив символов cbuf
    void write(char[] cbuf, int offset, int length) throws IOException // Записывает часть массива символов cbuf, начиная с позиции offset до количества символов length
    void write(String str) throws IOException // Записывает строку str
    void write(String str, int offset, int length) throws IOException // Записывает часть строки str, начиная с позиции offset до количества символов length
    void flush() throws IOException // Очищает буфер и записывает все буферизованные данные в целевой поток
    void close() throws IOException // Закрывает поток и освобождает все связанные с ним ресурсы
```

### OutputStreamWriter

`OutputStreamWriter` в Java - это класс, который преобразует символьные потоки в байтовые потоки, используя указанную кодировку. Он является мостом между потоками символов (Writer) и потоками байтов (OutputStream). Является наследником для [Writer](#javaiowriter) и реализует его методы.

#### Констркуторы OutputStreamWriter

```java

    OutputStreamWriter(OutputStream out); // Создает OutputStreamWriter, используя кодировку по умолчанию
    OutputStreamWriter(OutputStream out, String charsetName); // Создает OutputStreamWriter с указанной кодировкой
    OutputStreamWriter(OutputStream out, Charset charset):; // Создает OutputStreamWriter с указанным объектом Charset
    CharsetEncoder encoder = StandardCharsets.UTF_8.newEncoder();
    OutputStreamWriter(OutputStream out, CharsetEncoder encoder); // Создает OutputStreamWriter с указанным CharsetEncoder
```

### FileWriter

`FileWriter` в Java - это класс, который используется для записи символьных данных в файл. Он является подклассом класса [OutputStreamWriter](#outputstreamwriter) и предназначен для упрощения записи в файлы. FileWriter работает с файлами, используя кодировку по умолчанию для системы.

#### Конструкторы FileWriter

```java
   FileWriter(String fileName) // Создает объект FileWriter, который записывает в файл с указанным именем. Если файл существует, он будет перезаписан
   FileWriter(String fileName, boolean append) // Создает объект FileWriter, который записывает в файл с указанным именем. Если append равно true, данные будут добавлены в конец файла, если файл существует
   FileWriter(File file) // Создает объект FileWriter, который записывает в указанный объект File. Если файл существует, он будет перезаписан
   FileWriter(File file, boolean append) // Создает объект FileWriter, который записывает в указанный объект File. Если append равно true, данные будут добавлены в конец файла, если файл существует
   FileWriter(FileDescriptor fd) // Создает объект FileWriter, который записывает в указанный дескриптор файла.
```

### BufferedWriter

`BufferedWriter` в Java - это класс, который добавляет буферизацию к символьному потоку для более эффективной записи данных. Буферизация позволяет уменьшить количество операций ввода-вывода (I/O), которые являются относительно медленными и ресурсоемкими, за счет группировки нескольких операций записи в одну крупную операцию.

#### Конструкторы BufferedWriter

```java
BufferedWriter(Writer out) // Создает BufferedWriter с буфером по умолчанию (8192 символов).
BufferedWriter(Writer out, int sz) // Создает BufferedWriter с указанным размером буфера.
```

`BufferedWriter` наследует методы класса [Writer](#javaiowriter) и добавляет свои собственные:

```java
    void newLine() // Записывает символ(ы) новой строки. Этот метод обеспечивает переносимость, так как символы новой строки зависят от платформы.
```

## Работа с URL

Класс `URL` в Java представляет собой унифицированный указатель ресурса (URL), который является ссылкой на ресурс в Интернете. Этот класс находится в пакете java.net и позволяет работать с URL-адресами, предоставляя методы для их анализа и использования.

Класс `URL` в Java представляет собой унифицированный указатель ресурса (URL), который является ссылкой на ресурс в Интернете. Этот класс находится в пакете `java.net` и позволяет работать с URL-адресами, предоставляя методы для их анализа и использования.

### Описание основных методов класса URL

Класс `URL` в Java предоставляет несколько конструкторов для создания URL-объектов. Каждый из этих конструкторов принимает разные параметры, которые представляют собой различные части URL. Давайте рассмотрим эти конструкторы и параметры подробнее.

### Конструкторы класса URL

```java
  URL url = new URL("https://www.example.com:8080/path/to/resource?query=java#section"); // Этот конструктор принимает строку `spec`, которая представляет собой полный URL-адрес в виде строки
  //  * spec: Полная спецификация URL в виде строки, включая протокол, хост, порт (необязательно), путь, строку запроса (необязательно) и якорь (необязательно)

  URL url = new URL("https", "www.example.com", 8080, "/path/to/resource?query=java#section"); // Этот конструктор позволяет создавать URL-объект из отдельных компонентов: протокол, хост, порт и файл (путь)
  //  - protocol: Протокол URL (например, "http", "https", "ftp").
  //  - host: Хост URL, доменное имя или IP-адрес (например, "www.example.com").
  //  - port: Порт URL (например, 80 для HTTP, 443 для HTTPS). Если порт не указан, используйте -1.
  //  - file: Путь к ресурсу, включая строку запроса и якорь (например, "/path/to/resource?query=java#section")

  URL url = new URL("https", "www.example.com", "/path/to/resource?query=java#section"); // Этот конструктор похож на предыдущий, но без указания порта. По умолчанию используется порт по умолчанию для указанного протокола (например, 80 для HTTP, 443 для HTTPS)

  URL base = new URL("https://www.example.com/path/to/"); // Этот конструктор позволяет создавать URL-объект на основе базового URL (контекста) и относительной спецификации URL
  URL url = new URL(base, "resource?query=java#section");
```

### Методы для получения частей URL

``` java
    URL url = new URL("https://www.example.com:8080/path/to/resource?query=java#section");
    url.getProtocol(); // Возвращает протокол URL (Output: https)
    url.getHost(); // Возвращает хост URL. (Output: www.example.com)
    url.getPort(); // Возвращает порт URL или -1, если порт не указан. (Output: 8080)
    url.getPath(); // Возвращает путь URL. (Output: /path/to/resource)
    url.getQuery(); // Возвращает строку запроса URL или null, если запрос отсутствует. (Output: query=java)
    url.getRef() // Возвращает якорь (реф) URL или null, если якорь отсутствует. (Output: section)
    url.getFile() // Возвращает строку файла URL (путь и запрос). (Output: /path/to/resource?query=java)
```

### Методы для работы с соединением

```java
    openStream(); // Открывает соединение и возвращает `InputStream` для чтения данных из ресурса.
    try {
        URL url = new URL("https://www.example.com");
        try (InputStream in = url.openStream();
                Scanner scanner = new Scanner(in)) {
            // ...
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
    
    openConnection(); // Возвращает объект `URLConnection` для более детального управления соединением.
    try {
        URL url = new URL("https://www.example.com");
        URLConnection connection = url.openConnection();
        try (BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()))) {
            // ...
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
```

### HttpURLConnection

`HttpURLConnection` является подклассом URLConnection, который используется для управления HTTP соединениями. Он предоставляет дополнительные методы для настройки и работы с HTTP-запросами и ответами.

#### Методы настройки запроса

```java
    URL url = new URL("https://jsonplaceholder.typicode.com/posts");
    HttpURLConnection connection = (HttpURLConnection) url.openConnection(); // Открывает связь к ресурсу

    connection.setRequestMethod(String method); // Этот метод устанавливает HTTP-метод для запроса, который может быть одним из следующих: "GET", "POST", "PUT", "DELETE", "HEAD", "OPTIONS", "TRACE".
    connection.setRequestProperty(String key, String value); //  устанавливает значение определенного заголовка запроса. Это может быть полезно для установки заголовков, таких как "Content-Type", "User-Agent", "Accept", и других. (Например, connection.setRequestProperty("User-Agent", "Mozilla/5.0"))
    connection.setDoOutput(boolean doOutput); // Указывает, будет ли использоваться поток вывода (необходимо для методов "POST" и "PUT").

    connection.disconnect(); // Закрывает соединение и освобождает все связанные ресурсы.
```

#### Методы отправки данных

```java
    URL url = new URL("https://jsonplaceholder.typicode.com/posts");
    HttpURLConnection connection = (HttpURLConnection) url.openConnection();

    try (OutputStream os = connection.getOutputStream()) { // Возвращает поток вывода, через который можно отправлять данные на сервер.
        byte[] input = data.getBytes("utf-8");
        os.write(input, 0, input.length);
    }
```

#### Методы чтения ответа

```java
    URL url = new URL("https://jsonplaceholder.typicode.com/posts");
    HttpURLConnection connection = (HttpURLConnection) url.openConnection();
    
    getInputStream(); // Возвращает поток ввода для чтения данных из ответа сервера.
    try (BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()))) {
        String inputLine;
        StringBuilder response = new StringBuilder();
        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        System.out.println(response.toString());
    }    
    
   int responseCode = connection.getResponseCode(); //  Возвращает код ответа HTTP от сервера.
    getResponseMessage(): Возвращает сообщение ответа HTTP.
```

### Пример использования класса URL

```java
import java.net.URL;
import java.net.MalformedURLException;
import java.io.InputStream;
import java.io.IOException;
import java.util.Scanner;

public class URLExample {
    public static void main(String[] args) {
        try {
            // Создание объекта URL
            URL url = new URL("https://www.example.com:80/path/to/resource?query=java#section");

            // Получение различных частей URL
            System.out.println("Protocol: " + url.getProtocol());
            System.out.println("Host: " + url.getHost());
            System.out.println("Port: " + url.getPort());
            System.out.println("Path: " + url.getPath());
            System.out.println("Query: " + url.getQuery());
            System.out.println("Ref: " + url.getRef());

            // Открытие соединения и чтение данных
            try (InputStream in = url.openStream();
                 Scanner scanner = new Scanner(in)) {
                System.out.println("Content:");
                while (scanner.hasNextLine()) {
                    System.out.println(scanner.nextLine());
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
    }
}
```

### Пример использования openConnection()

```java
import java.net.URL;
import java.net.HttpURLConnection;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class URLConnectionExample {
    public static void main(String[] args) {
        try {
            // Создание объекта URL
            URL url = new URL("https://www.example.com");

            // Открытие соединения
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");

            // Получение кода ответа
            int responseCode = connection.getResponseCode();
            System.out.println("Response Code: " + responseCode);

            // Чтение данных из ответа
            try (BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()))) {
                String inputLine;
                StringBuilder content = new StringBuilder();
                while ((inputLine = in.readLine()) != null) {
                    content.append(inputLine);
                }
                System.out.println("Content: " + content.toString());
            } catch (IOException e) {
                e.printStackTrace();
            }

            // Закрытие соединения
            connection.disconnect();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### Описание

1. **Создание URL-объекта**:
   - Создаем объект `URL` с заданным URL-адресом.

2. **Открытие соединения**:
   - Открываем соединение с использованием `openConnection()` и приводим его к `HttpURLConnection`.
   - Устанавливаем метод запроса `GET`.

3. **Получение ответа**:
   - Получаем код ответа сервера с помощью `getResponseCode()`.
   - Читаем данные из входного потока соединения с использованием `BufferedReader`.

4. **Закрытие соединения**:
   - Закрываем соединение с помощью `disconnect()`.

Класс `URL` является мощным инструментом для работы с URL-адресами и соединениями в Java, предоставляя высокоуровневый интерфейс для взаимодействия с интернет-ресурсами.

## Stream API

```java
    Stream - последовательность элементов, потенциально бесконечная, с возможностью применять к ней многоэтапные преобразования

    java.until.stream; - пакет, в котором хранится весь Stream API

    IntStream stream = IntStream.iterate([startValue], [lambdaFunction]);
    * startValue - начальное значение, с которого начинается последовательность
    * lambdaFunction -  функция, которая определяет как меняется следующий элемент последовательности в сравнении с предыдущим:
    n -> n+1 - следующий элемент будет на 1 больше предыдущего

    * Структура кода Stream состоит из 3-х частей:
    1. Объявление самой последовательности (IntStream stream = IntStream.iterate([startValue], [lambdaFunction]))
    2. Выполнение какого то количества преобразований последовательности (filter, limit, map)
    3. Терминальная операция, которая позволяет получить полезный результат от стрима (sum)

    Способы получения Stream-а:

    1. Можно получить из любой #Коллекции# с использовованием команды stream:
    Set<Type> set = ...;
    Stream<Type> stream = set.stream;
    2. Можно получить из потока символов (необходимо закрывать stream.close()):
    BufferedReader reader = ...;
    Stream<String> stream = reader.lines();
    3. Можно получить последовательность из директорий на диске (необходимо закрывать stream.close()):
    Path path = ...;
    Stream<Path> stream = Files.list(path); - возвращает содержимое директорий на один уровень
    Stream<Path> stream = Files.walk(path); - рекурсивно обойдет и поддиректории тоже
    4. Можно получить из строки:
    IntStream chars = "[String]".chars();
    5. Можно генерировать при помощи какой то функции:
    DoubleStream randomNumbers = DoubleStream.generate(Math::random);
    * В метод DoubleStream.generate([Supplier]) - передается метод, который реализует интерфейс Supplier, т.е. ничего не получает, но что то отдает
    6. Можно получить итерированием какой то функции:
    IntStream stream = IntStream.iterate(0, n -> ++n);
    7. Можно получить из последовательности чисел:
    IntStream stream = IntStream.range(0, 100); - получение чисел от 0 до 99
    IntStream stream = IntStream.rangeClosed(0, 100); - получение чисел от 0 до 100
    8. Можно получить из конкатинации 2-х других стримов:
    IntStream newStream = IntStream.concat(stream1, stream2);
    9. Можно получить пустой стрим:
    IntStream newStream = IntStream.empty();
    10. Можно получить стрим из массива:
    double[] array = ...;
    DoubleStream stream = Arrays.stream(array);
    11. Можно явно перечислить все элементы стрима:
    IntStream newStream = IntStream.of(2,4,5,6,7,8,9,10);

    Преобразование последовательностей:

    stream.filter(n -> {condition}); - использование фильтрации последовательности (к каждому элементу последовательности применяется данная функция и проверяет, выполняется ли условие)
    * n -> n % 5 == 0 - пример написания условия фильтрации
    stream.skip([count]); - позволяет пропустить count значений
    stream.limit([count]); - оставляет первые count элементов последовательности
    stream.map(n -> n*n); - применяет какую то функцию к каждому элементу последовательности
    stream.mapToObj(Integer::toString); - преобразует все элементы последовательности к другому типу
    stream.flatMapToInt(s -> [return Stream]);- похожа на map, только принимает функцию, возвращающую Stream, после превращения всех элементов в набор Stream-ов конкатинирует их и получается один Stream
    stream.distinct(); - убирает дубликаты элементов
    stream.sorted(); - сортирует последовательность по возрастанию
    * Если последовательность состоит из объектов, то можно передать в данную функцию компаратор

    stream.peek(System.out::println); - позволяет посмотреть какие значения сейчас в последовательсности
    * Принимает интерфейс Consumer

    Терминальные операции: - которые запускают выполенние стрима
    * После искользоваться терминальной операции стрим перестает быть действительным и не может использоваться дальше

    stream.forEach(System.out::println); - приминяет функцию к каждому элементу последовательности
    * Принимает интерфейс Consumer
    OptionalInt result = stream.findFirst(); - возвращает первый элемент последовательности, который может быть null, поэтому тип возвращаемого значения OptionalInt
    Int result = stream.findAny(); - возвращает первый элемент последовательности кроме null
    boolean bool = stream.allMatch(s -> [condition]); - проверяет удволетворяют ли все элементы последовательности данному условию
    boolean bool = stream.anyMatch(s -> [condition]); - проверяет удволетворяет ли хотя бы один элемент последовательности данному условию
    boolean bool = stream.noneMatch(s -> [condition]); - проверяет НЕ удволетворяют ли все элементы последовательности данному условию

    stream.min() | stream.max();; - возвращает минимальный или максимальный элемент последовательности
    * Если последовательность не числовая, то принимает Comparator.comparing(...);
    stream.sum(); - возвращает сумму элементов последовательности
    stream.count(); - возвращает количетсво элементов последовательности

    stream.collect(Сollectors.to[List | Set | ...]); - преобразует последовательность в какой то тип
    * преобразует не только в коллекции, все стандартные варианты хранятся в Сollectors
    stream.reduce([defaultValue], [BinaryOperator<T>]); - производит свертку всех элементов (последовательно применяет ко всем парам элементов какую то функцию, пока не останится один элемент)


    stream.close(); - закрытие стрима, обязательная команда, если стрим работает с какими то системными ресурсами, например, с файлами
```
