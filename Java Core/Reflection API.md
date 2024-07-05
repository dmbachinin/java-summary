# СОДЕРЖАНИЕ:

## [Java Reflection API](#java-reflection-api-1)
- [Методы для работы с классами](#методы-для-работы-с-классами)
    - [Методы получения объекта Class<?>](#методы-получения-объекта-class)
    - [Методы объекта Class<?>](#методы-объекта-class)
- [Методы для работы с конструкторами](#методы-для-работы-с-конструкторами)
- [Методы для работы с методами](#методы-для-работы-с-методами)
- [Методы для работы с полями](#методы-для-работы-с-полями)
- [Методы для работы с аннотациями](#методы-для-работы-с-аннотациями)
- [ClassLoader - основные понятия и методы](#classloader---основные-понятия-и-методы)
    - [Иерархия ClassLoader](#иерархия-classloader)
    - [Как работает ClassLoader](#как-работает-classloader)
    - [Различия между Class.forName и ClassLoader.loadClass](#различия-между-classforname-и-classloaderloadclass)
    - [Основные методы ClassLoader](#основные-методы-classloader)
    - [Влияние ClassLoader на загрузку классов](#влияние-classloader-на-загрузку-классов)
    - [Особенности работы методов getResource и getResourceAsStream](#особенности-работы-методов-getresource-и-getresourceasstream)

## [Reflections](#reflections-1)
- [Подключение через Maven](#подключение-через-maven)
- [Оснонвые методы](#оснонвые-методы)


## Java Reflection API

`Java Reflection API` — это мощный механизм, который позволяет программам на Java исследовать и манипулировать своим собственным кодом (например, классы, методы, поля) во время выполнения. Используя Reflection API, можно динамически создавать объекты, вызывать методы, изменять поля и получать информацию о классах и их структурах

**Где используется Java Reflection API**
1. **Фреймворки и библиотеки**:
- Широко используется в популярных фреймворках, таких как Spring, Hibernate, JUnit, для динамического внедрения зависимостей, создания прокси-объектов, управления транзакциями и многого другого.

2. **Системы плагинов**:
- В системах, где требуется динамическое подключение и отключение модулей или плагинов, Reflection API позволяет загружать и взаимодействовать с ними без жесткой привязки к конкретным классам.

3. **Инструменты разработки**:
- IDE и другие инструменты разработки используют Reflection для предоставления функций рефакторинга, автозаполнения, анализа кода и генерации документации.

4. **Тестирование**:
- В тестовых фреймворках Reflection позволяет создавать тесты, которые могут взаимодействовать с закрытыми частями классов, обеспечивая более глубокое тестирование.

5. **Системы управления данными**:
- Используется в ORM системах для маппинга объектов на базы данных, а также для сериализации и десериализации объектов в различные форматы данных.

### Методы для работы с классами

#### Методы получения объекта Class<?>

`Class<?>` — это обобщенный (generic) тип в Java, представляющий метаданные о классе. В Java Class является встроенным классом, который используется для представления классов и интерфейсов во время выполнения. Обобщенный тип <?> используется для указания того, что Class может быть любого типа.

```java
    // Получение объекта Class<?>
    Class<?> clazz = Class.forName("com.example.MyClass"); // Возвращает объект Class, соответствующий указанному имени класса
    //  * вызывает ClassNotFoundException - если класс не найден
    Class<?> clazz = String.class; // Получения объекта Class во время компиляции
    Class<?> clazz = obj.getClass(); // Получение объекта Class из экземпляра объекта
```

#### Методы объекта Class<?>
```java
    String className = clazz.getName(); // Возвращает имя класса или интерфейса, представленного данным объектом Class
    Class<?> superClass = clazz.getSuperclass(); // Возвращает суперкласс (базовый класс) данного класса

    // Package - представляет пакет, в котором находится класс или интерфейс. Пакеты используются для группировки связанных классов и интерфейсов, а также для управления видимостью и организации кода

    Package pkg = clazz.getPackage(); // Возвращает пакет, к которому принадлежит класс
    String packageName = pkg.getName(); // Возвращает имя пакета
    String specVersion = pkg.getSpecificationVersion(); // Возвращает версию спецификации пакета
    String specTitle = pkg.getSpecificationTitle(); // Возвращает название спецификации пакета
    String specVendor = pkg.getSpecificationVendor(); // Возвращает имя поставщика спецификации пакета
    String implTitle = pkg.getImplementationTitle(); // Возвращает название реализации пакета
    String implVersion = pkg.getImplementationVersion(); // Возвращает версию реализации пакета
    String implVendor = pkg.getImplementationVendor(); // Возвращает имя поставщика реализации пакета
    boolean sealed = pkg.isSealed(); // Проверяет, запечатан ли пакет
    boolean sealed = pkg.isSealed(url); // Проверяет, запечатан ли пакет по указанному URL
    boolean compatible = pkg.isCompatibleWith("1.1"); // Проверяет, совместима ли версия пакета с указанной версией
    Annotation annotation = pkg.getAnnotation(MyAnnotation.class); // Возвращает аннотацию указанного типа, если она присутствует на этом пакете
    Annotation[] declaredAnnotations = pkg.getDeclaredAnnotations(); // Возвращает все аннотации, непосредственно объявленные на этом пакете
    //  * Информация о пакете в Java берется из метаданных, содержащихся в файлах JAR (Java Archive), которые включают манифест (MANIFEST.MF)
    //  * Если проект не содержит MANIFEST.MF, то методы будут возвращать null или false

    //  Аннотации к пакетам

    // Для аннотирования пакета необходимо создать файл package-info.java в соответствующей директории пакета и указать аннотацию в этом файле
    @Deprecated //  Аннотация, указывающая, что весь пакет com.example устарел
    package com.example;
    
    // Создание пользовательской аннотации
    @Target(ElementType.PACKAGE)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface MyCustomAnnotation {
        String value();
    }
    // Пример содержимого файла package-info.java
    @MyCustomAnnotation("This is a custom annotation for the package")
    package com.example;

    import my.annotations.MyCustomAnnotation;
```

### Методы для работы с конструкторами
```java
    Constructor<?> constructor = clazz.getConstructor(String.class, int.class); // Возвращает публичный конструктор с указанными типами параметров
    Constructor<MyClass> constructor = clazz.getConstructor(String.class, int.class); // Пример получения объекта конструктор
    Constructor<MyClass> constructor = clazz.getDeclaredConstructor(String.class, int.class); // Возвращает конструктор с указанными типами параметров, включая приватные и защищенные конструкторы
    Constructor<?>[] constructors = clazz.getConstructors(); // Возвращает массив всех публичных конструкторов класса
    Constructor<?>[] constructors = clazz.getDeclaredConstructors(); // Возвращает массив объектов Constructor, представляющих все объявленные конструкторы класса, включая приватные

    boolean accessible = constructor.isAccessible(); // Проверяет, доступен ли конструктор (игнорирует ли он проверку доступа)
    constructor.setAccessible(true); // Устанавливает доступность конструктора, позволяя или запрещая доступ к нему независимо от модификатора доступа
    Class<?>[] parameterTypes = constructor.getParameterTypes(); // Возвращает массив объектов Class, представляющих типы параметров конструктора
    int modifiers = constructor.getModifiers(); // Возвращает модификаторы доступа конструктора
    Class<?> declaringClass = constructor.getDeclaringClass(); // Возвращает объект Class, представляющий класс, в котором объявлен этот конструктор
    Type[] genericParameterTypes = constructor.getGenericParameterTypes(); // Возвращает массив объектов Type, представляющих обобщенные типы параметров конструктора
    Annotation[][] parameterAnnotations = constructor.getParameterAnnotations(); // Возвращает двумерный массив объектов Annotation, представляющих аннотации параметров конструктора

    MyClass instance = constructor.newInstance("John", 30); // Создает новый экземпляр класса, используя этот конструктор
```

### Методы для работы с методами
```java
    Method method = clazz.getMethod("sayWord", String.class); // Возвращает публичный метод с указанным именем и типами параметров
    Method method = clazz.getDeclaredMethod("privateMethod", String.class); // Возвращает метод с указанным именем и типами параметров, включая приватные и защищенные методы
    Method[] methods = clazz.getMethods(); // Возвращает массив всех публичных методов класса (в том числе все унаследованные методы)
    Method[] methods = clazz.getDeclaredMethods(); // Возвращает массив объектов Method, представляющих все объявленные методы класса, включая приватные (не возвращает методы, унаследованные от супер класса)

    boolean accessible = method.isAccessible(); // Проверяет, доступен ли метод (игнорирует ли он проверку доступа)
    method.setAccessible(true); // Устанавливает доступность метода, позволяя или запрещая доступ к нему независимо от модификатора доступа
    Class<?>[] parameterTypes = method.getParameterTypes(); // Возвращает массив объектов Class, представляющих типы параметров метода
    Class<?> returnType = method.getReturnType(); // Возвращает объект Class, представляющий тип возвращаемого значения метода
    int modifiers = method.getModifiers(); // Возвращает модификаторы доступа метода
    Class<?> declaringClass = method.getDeclaringClass(); // Возвращает объект Class, представляющий класс, в котором объявлен этот метод
    String methodName = method.getName(); // Возвращает имя метода
    Annotation[] annotations = method.getDeclaredAnnotations(); // Возвращает массив всех аннотаций, присутствующих на этом методе
    Type[] genericParameterTypes = method.getGenericParameterTypes(); // Возвращает массив объектов Type, представляющих обобщенные типы параметров метода
    Annotation[][] parameterAnnotations = method.getParameterAnnotations(); // Возвращает двумерный массив объектов Annotation, представляющих аннотации параметров метода
    boolean isVarArgs = method.isVarArgs(); // Проверяет, является ли метод varargs (принимающим переменное количество аргументов)

    method.invoke(instance); // Вызывает метод на указанном объекте с переданными аргументами
```

### Методы для работы с полями
```java
    Field field = clazz.getField("publicFieldName"); // Возвращает публичное поле с указанным именем
    Field field = clazz.getDeclaredField("privateFieldName"); // Возвращает поле с указанным именем, включая приватные и защищенные поля
    Field[] fields = clazz.getFields(); // Возвращает массив всех публичных полей класса
    Field[] fields = clazz.getDeclaredFields(); // Возвращает массив объектов Field, представляющих все объявленные поля класса, включая приватные

    boolean accessible = field.isAccessible(); // Проверяет, доступно ли поле (игнорирует ли оно проверку доступа)
    boolean isHasAnnotation = field.isAnnotationPresent(Annotation.class); // Провеяет присутствует ли данная аннотация на данном поле
    Class<?> fieldType = field.getType(); // Возвращает объект Class, представляющий тип поля
    int modifiers = field.getModifiers(); // Возвращает модификаторы доступа поля
    String fieldName = field.getName(); // Возвращает имя поля
    Class<?> declaringClass = field.getDeclaringClass(); // Возвращает объект Class, представляющий класс, в котором объявлено это поле
    Annotation[] annotations = field.getDeclaredAnnotations(); // Возвращает массив всех аннотаций, присутствующих на этом поле

    field.setAccessible(true);  // Позволяет получить доступ к приватному полю
    String name = (String) field.get(instance); // Возвращает значение поля из указанного объекта
    field.set(instance, "Alice"); // Устанавливает значение поля в указанном объект
```

### Методы для работы с аннотациями
```java
    Deprecated deprecated = MyClass.class.getAnnotation(Deprecated.class); // Возвращает аннотацию указанного типа, если она присутствует на данном элементе
    Deprecated deprecated = myMethod.getAnnotation(Deprecated.class); // Возвращает аннотацию указанного типа, если она присутствует на данном методе
    Deprecated deprecated = myField.getAnnotation(Deprecated.class); // Возвращает аннотацию указанного типа, если она присутствует на данном поле
    Deprecated deprecated = myConstructor.getAnnotation(Deprecated.class); // Возвращает аннотацию указанного типа, если она присутствует на данном конструкторе
```

### ClassLoader - основные понятия и методы

`ClassLoader`- это абстрактный класс, который используется для загрузки классов в JVM (Java Virtual Machine). Он играет важную роль в Java Runtime Environment, управляя процессом загрузки классов и обеспечивая изоляцию классов для разных приложений и модулей

#### Иерархия ClassLoader
1. `Bootstrap ClassLoader`: Загружает базовые классы Java из JDK (например, классы из java.lang и java.util).
2. `Extension ClassLoader`: Загружает классы из расширений (JAR-файлы, находящиеся в каталоге lib/ext).
3. `System ClassLoader (Application ClassLoader)`: Загружает классы из classpath приложения.

#### Как работает ClassLoader
1. `Проверка кэша классов`: JVM сначала проверяет, загружен ли класс уже в кэш классов. Если класс уже загружен, возвращается этот загруженный класс.
2. `Делегирование родителю`: Если класс не найден в кэше, метод делегирует запрос родительскому ClassLoader. Это делегирование происходит рекурсивно вверх по иерархии ClassLoader вплоть до Bootstrap ClassLoader.
3. `Загрузка класса`: Если родительский ClassLoader не может найти класс, текущий ClassLoader пытается загрузить его самостоятельно.
Инициализация класса: Если флаг resolve установлен в true, загруженный класс инициализируется с помощью вызова resolveClass.

#### Различия между Class.forName и ClassLoader.loadClass
1. Делегирование загрузки классов:
- `Class.forName`: Делегирует загрузку классов системному загрузчику классов по умолчанию.
- `ClassLoader.loadClass`: Позволяет использовать конкретный загрузчик классов, предоставляя больше контроля над процессом загрузки. Это особенно важно в контекстах, где необходимо загружать классы из специфических источников или с определенной изоляцией.
2. Инициализация класса:
- `Class.forName`: Загружает и инициализирует класс сразу же (выполняет статические блоки инициализации).
- `ClassLoader.loadClass`: Загружает класс, но не инициализирует его сразу. Инициализация происходит только при необходимости (например, при вызове конструктора или статического метода).

#### Основные методы ClassLoader
```java
    ClassLoader classLoader = clazz.getClassLoader(); // Возвращает загрузчик класса, который загрузил данный класс
    //  * Гарантирует, что класс будет загружен в том же контексте, что и исходный класс. Это важно для обеспечения корректной видимости и доступности зависимостей
    Class<?> clazz = classLoader.loadClass("com.example.MyClass"); // Загружает класс с указанным именем
    Class<?> clazz = classLoader.loadClass("com.example.MyClass", true); // Загружает класс с указанным именем и разрешает его, если флаг resolve установлен в true
    //  * resolve - флаг, указывающий, нужно ли сразу инициализировать класс после его загрузки. Если true, то вызов resolveClass будет произведен после загрузки класса
    //  * Метод resolveClass используется для связывания (или разрешения) загруженного класса, что включает проверку и подготовку всех ссылок на другие классы, используемые загружаемым классом
    URL resource = classLoader.getResource("config.properties"); // Ищет ресурс с указанным именем и возвращает URL к ресурсу
    InputStream inputStream = classLoader.getResourceAsStream("config.properties"); // Ищет ресурс с указанным именем и возвращает поток ввода для чтения ресурса
    Enumeration<URL> resources = classLoader.getResources("META-INF/MANIFEST.MF"); // Ищет все ресурсы с указанным именем и возвращает их в виде перечисления URL
    ClassLoader parent = classLoader.getParent(); // Возвращает родительский ClassLoader
    classLoader.setDefaultAssertionStatus(true); // Устанавливает статус утверждения по умолчанию для всех классов, загруженных этим загрузчиком
    classLoader.setPackageAssertionStatus("com.example", true); // Устанавливает статус утверждения для всех классов в указанном пакете
    classLoader.setClassAssertionStatus("com.example.MyClass", true); // Устанавливает статус утверждения для указанного класса
```
#### Влияние ClassLoader на загрузку классов

1. **Контекст загрузки классов**:
- Когда вы используете clazz.getClassLoader(), вы получаете загрузчик классов, который использовался для загрузки данного класса. Этот загрузчик может быть использован для загрузки других классов и ресурсов в том же контексте.
- Например, если класс был загружен системным загрузчиком классов (System ClassLoader), то другие классы, загружаемые этим загрузчиком, будут также загружаться в системный контекст.

2. **Изоляция и видимость классов**:
- Различные загрузчики классов могут загружать разные версии одного и того же класса, обеспечивая изоляцию между ними. Это особенно важно в контейнерах сервлетов, приложениях OSGi и других модульных системах.
- Классы, загруженные одним загрузчиком классов, могут быть невидимы для классов, загруженных другим загрузчиком, если они находятся в разных иерархиях загрузки.

3. **Безопасность и контроль**:
- Загрузчики классов могут управлять доступом к классам и ресурсам, обеспечивая безопасность приложения. Например, пользовательский загрузчик классов может загружать классы из зашифрованных файлов или проверять подписи классов.

#### Особенности работы методов getResource и getResourceAsStream

Методы `getResource` и `getResourceAsStream` используют classpath для поиска ресурсов. Они ищут ресурсы относительно корня classpath, что позволяет находить файлы независимо от их физического расположения в файловой системе. Это делает код более универсальным и позволяет загружать ресурсы из различных источников, таких как директории и JAR-файлы, указанные в classpath.

Когда вы вызываете `getResource` или `getResourceAsStream`, путь, который вы указываете, интерпретируется относительно корня classpath. Это означает, что если у вас есть файл, находящийся в поддиректории, вы должны указать его путь относительно корня classpath

**Пример формирования classpath:**
```sh
java -cp bin:lib/mylib.jar com.example.MyClass # В данном примере мы указваем, что classpath содержит дирректорию bin и lib/mylib.jar
```
**Поиск работает следующим образом**:
1. JVM постепенно посещает каждую дирректорию, указанную в classpath (в порядке перечисления). Так в примере он зайдет сначала в bin, потом в lib/mylib.jar, при этом JVM будет посещать JAR-файлы, указанные в classpath, и искать в них ресурсы и классы так же, как и в директориях
2. Пытается найти файл, в указанной папке по указанному пути (как будто папка это корень системы)
3. Если файл найден, то поиск прекращается, если нет, то JVM переходит в следующую дирректорию classpath

**Особенности:**

1. **Кроссплатформенность**:
- Эти методы абстрагируют путь к ресурсу, позволяя искать ресурсы независимо от операционной системы. Например, они корректно обрабатывают пути с обратными и прямыми слешами.

2. **Поддержка различных источников**:
- Эти методы могут искать ресурсы не только в файловой системе, но и внутри JAR-файлов, WAR-файлов или даже в сетевых источниках. Это особенно важно для Java-приложений, которые могут быть упакованы в архивы.

3. **Классовая иерархия загрузчиков**:
- Методы getResource и getResourceAsStream используют иерархию загрузчиков классов для поиска ресурсов. Это означает, что они могут искать ресурсы в контексте текущего загрузчика класса и его родителей, что повышает вероятность нахождения нужного ресурса.

4. **Изоляция и безопасность**:
- Использование загрузчиков классов для загрузки ресурсов обеспечивает изоляцию и безопасность. Ресурсы могут быть загружены только теми загрузчиками классов, которые имеют к ним доступ, что предотвращает несанкционированный доступ.

## Reflections

Библиотека `Reflections` предоставляет множество методов для работы с рефлексией, которые значительно упрощают задачу поиска классов, методов, полей и аннотаций в проекте.

### Подключение через Maven
```java
    <dependency>
        <groupId>org.reflections</groupId>
        <artifactId>reflections</artifactId>
        <version>0.10.2</version>
    </dependency>
```

### Оснонвые методы
```java
    import org.reflections.Reflections;
    import org.reflections.scanners.Scanners;

    Reflections reflections = new Reflections("com.example", Scanners); // Инициализация Reflections для указанного пакета c указанными сканерами 

    // Scanners — это утилиты в библиотеке Reflections, которые используются для определения, какие элементы будут сканироваться в процессе анализа. Эти параметры задают, какие типы данных будут собираться и анализироваться библиотекой Reflections
    //  Вот основные сканеры, которые можно использовать:
    //  Scanners.SubTypes - Сканирует и находит все подклассы и реализации интерфейсов
    //  Scanners.TypesAnnotated - Сканирует и находит все классы, аннотированные определенными аннотациями
    //  Scanners.MethodsAnnotated - Сканирует и находит все методы, аннотированные определенными аннотациями
    //  Scanners.FieldsAnnotated - Сканирует и находит все поля, аннотированные определенными аннотациями

    Reflections reflections = new Reflections("com.example"); // Инициализация Reflections для указанного пакета c настройками по умолчанию
    // Сканеры по умолчанию: Scanners.SubTypes, Scanners.TypesAnnotated

    Reflections reflections = new Reflections(packageName, new SubTypesScanner(false)) // Данная инициализация помогает найти все классы в указанном пакете при вызове getSubTypesOf


    Set<Class<? extends MyInterface>> subTypes = reflections.getSubTypesOf(MyInterface.class); // Ищет все подклассы указанного класса или интерфейса
    Set<Class<?>> allClasses = reflections.getSubTypesOf(Object.class); // Получить все классы из пакета 
    Set<Class<?>> annotatedClasses = reflections.getTypesAnnotatedWith(MyAnnotation.class); // Ищет все классы, аннотированные указанной аннотацией
    Set<Method> annotatedMethods = reflections.getMethodsAnnotatedWith(MyMethodAnnotation.class); // Ищет все методы, аннотированные указанной аннотацией
    Set<Field> annotatedFields = reflections.getFieldsAnnotatedWith(MyFieldAnnotation.class); // Ищет все поля, аннотированные указанной аннотацией
    Set<Constructor> annotatedConstructors = reflections.getConstructorsAnnotatedWith(MyConstructorAnnotation.class); // Ищет все конструкторы, аннотированные указанной аннотацией
    Set<String> resources = reflections.getResources(Pattern.compile(".*\\.xml")); // Ищет все ресурсы (файлы) в classpath, соответствующие указанному шаблону
```