# СОДЕРЖАНИЕ:

## [Maven](#maven)
   - [Основные функции Maven](#основные-функции-maven)
   - [Структура проекта Maven](#структура-проекта-maven)
   - [Команды Maven](#команды-maven)
      - [Основные команды](#основные-команды)
      - [Дополнительные команды](#дополнительные-команды)
      - [Профили и активация команд](#профили-и-активация-команд)
      - [Плагины и команды плагинов](#плагины-и-команды-плагинов)
      - [Общие опции командной строки](#общие-опции-командной-строки)
   - [Структура POM файла](#структура-pom-файла)
      - [Основные теги](#основные-теги)
      - [Управление зависимостями](#управление-зависимостями)
      - [Управление сборкой](#управление-сборкой)
      - [Профили](#профили)
## [Плагины для сборки](#плагины-для-сборки-1)
   - [maven-compiler-plugin](#maven-compiler-plugin-orgapachemavenplugins) - **Компилирует исходный код Java**
   - [maven-surefire-plugin](#maven-surefire-plugin-orgapachemavenplugins) - **Запускает тесты во время фазы test**
   - [maven-jar-plugin](#maven-jar-plugin-orgapachemavenplugins) - **Упаковывает скомпилированные классы и ресурсы в JAR файл**
   - [maven-assembly-plugin](#maven-assembly-plugin-orgapachemavenplugins) - **Создает архивы, включающие зависимости и другие ресурсы**
   - [maven-dependency-plugin](#maven-dependency-plugin-orgapachemavenplugins) - **Управление зависимостями, например, копирование зависимостей в определенные директории**
   - [maven-clean-plugin](#maven-clean-plugin-orgapachemavenplugins) - **Очищает директорию сборки**
   - [maven-site-plugin](#maven-site-plugin-orgapachemavenplugins) - **Создает сайт проекта на основе информации из POM и отчетов**
   - [maven-checkstyle-plugin](#maven-checkstyle-plugin-orgapachemavenplugins) - **Проверяет стиль кода на соответствие правилам**
   - [maven-enforcer-plugin](#maven-enforcer-plugin-orgapachemavenplugins) - **Налагает ограничения и проверяет, соответствуют ли они правилам**
   - [maven-war-plugin](#maven-war-plugin-orgapachemavenplugins) - **Используется для создания веб-архивов (WAR)**
   - [appassembler-maven-plugin](#appassembler-maven-plugin-orgcodehausmojo) - **Генерация скриптов для запуска проектов**
   - [maven-shade-plugin](#maven-shade-plugin-orgapachemavenplugins) - **Позволяет укаповать проект в fat jar (uber jar)**
      - [Что такое META-INF?](#что-такое-meta-inf)
         - [Для чего нужен каталог META-INF?](#для-чего-нужен-каталог-meta-inf)
         - [Основные файлы и подкаталоги в META-INF](#основные-файлы-и-подкаталоги-в-meta-inf)
         - [Кто создает каталог META-INF?](#кто-создает-каталог-meta-inf)
         - [Использование каталога META-INF](#использование-каталога-meta-inf)
      - [Для чего исключать подписи в блоке `filters`](#для-чего-исключать-подписи-в-блоке-filters)
      - [Типы параметров `implementation` в конфигурации `transformer`](#типы-параметров-implementation-в-конфигурации-transformer)

## [Дополнительная информация](#дополнительная-информация-1)
   - [Семантическое версионирование](#семантическое-версионирование-semantic-versioning-semver)

# Maven

[ДОКУМЕНТАЦИЯ](https://maven.apache.org/guides/index.html)

`Maven` — это инструмент для управления проектами и сборки на базе Java, разработанный Apache Software Foundation. Он упрощает процесс сборки и управления зависимостями, а также предоставляет стандартизированную структуру проекта.  
`Maven` использует декларативный подход к сборке проектов (мы говорим, что мы хотим, "собери проект", например). Исопльзует язык XML.\
`Maven` помогает в подлкючении сторонних библиотек, нам достаточно указать, что мы хотим подлючить. После чего Maven сам будет искать данную библиотеку сначала на локальном компьютере, если он ее не найдет, то он будет пытаться найти ее на удаленном репозитории ([Maven central](https://mvnrepository.com))

### Основные функции Maven:

1. **Управление зависимостями:**
   Maven позволяет легко управлять зависимостями проекта. Все зависимости указываются в файле `pom.xml`, и Maven автоматически загружает их из центрального репозитория Maven или других настроенных репозиториев.

2. **Компиляция:**
   Maven автоматически обрабатывает компиляцию исходного кода проекта и его ресурсов. Он использует стандартную структуру каталогов, что упрощает организацию проекта.

3. **Тестирование:**
   Maven поддерживает интеграцию с различными фреймворками для тестирования, такими как JUnit и TestNG. Он позволяет запускать тесты и генерировать отчеты о тестировании.

4. **Сборка артефактов:**
   Maven может создавать различные типы артефактов, такие как JAR, WAR и EAR файлы, которые можно использовать для развёртывания приложений.

5. **Плагины:**
   Maven имеет модульную архитектуру, которая позволяет использовать плагины для выполнения различных задач. Существует множество плагинов, которые могут расширять возможности Maven, например, для создания документации, анализа кода, деплоя и т.д.

### Структура проекта Maven:

Maven использует стандартную структуру каталогов:
- `src/main/java` — исходные коды Java.
- `src/main/resources` — ресурсы, используемые в приложении.
- `src/test/java` — тестовые классы Java.
- `src/test/resources` — ресурсы для тестов.

### Команды Maven:

Maven предоставляет множество команд для выполнения различных задач по сборке, тестированию, упаковке и развертыванию проектов. Вот основные команды `mvn`, которые часто используются:

#### Основные команды:

```shell
   mvn clean # Удаляет скомпилированные файлы и другие артефакты, созданные в процессе сборки. Полезно для очистки проекта перед новой сборкой
   mvn compile # Компилирует исходный код проекта
   mvn test # Запускает тесты, используя тестовый фреймворк, указанный в проекте (например, JUnit)
   mvn package # Упаковывает скомпилированный код в JAR или WAR файл, в зависимости от конфигурации проекта
   mvn install # Устанавливает собранный артефакт в локальный репозиторий Maven. Это позволяет другим проектам использовать этот артефакт в качестве зависимости
   mvn deploy # Деплоит собранный артефакт в удаленный репозиторий, который может быть доступен другим разработчикам

   mvn clean install # Комбинирует команды `clean` и `install`
```

#### Дополнительные команды:
```shell
   mvn validate # Проверяет корректность проекта и конфигурации POM
   mvn verify # Выполняет все проверки и интеграционные тесты, чтобы убедиться, что проект готов к упаковке
   mvn site # Генерирует сайт проекта с документацией на основе информации, предоставленной в POM-файле
   mvn dependency:resolve # Разрешает все зависимости проекта и загружает их в локальный репозиторий, если это необходимо
   mvn dependency:tree # Выводит дерево зависимостей проекта
   mvn eclipse:eclipse # Генерирует файлы конфигурации для Eclipse IDE
   mvn idea:idea # Генерирует файлы конфигурации для IntelliJ IDEA
   mvn help:describe -Dcmd=<command> # Описывает указанную команду Maven, предоставляя информацию о ее использовании и опциях (mvn help:describe -Dcmd=compile)

   # Создание нового Maven проекта через командную строку
   mvn archetype:generate -DgroupId=com.example -DartifactId=my-project -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   #  archetype:generate: Команда для генерации нового проекта на основе archetype.
   #  -DgroupId=com.example: Задает groupId вашего проекта (например, com.example).
   #  -DartifactId=my-project: Задает artifactId вашего проекта (например, my-project).
   #  -DarchetypeArtifactId=maven-archetype-quickstart: Указывает, какой archetype использовать. maven-archetype-quickstart - это простой шаблон для проекта Java.
   #  -DinteractiveMode=false: Отключает интерактивный режим, чтобы создать проект без # дополнительных запросов.
```

#### Профили и активация команд:

Maven позволяет использовать профили для настройки различных конфигураций сборки. Профили можно активировать с помощью команды `-P`.

```shell
mvn clean install -Pdev # Эта команда активирует профиль с идентификатором `dev`, что позволяет изменять конфигурацию сборки в зависимости от среды
```

#### Плагины и команды плагинов:

Maven имеет модульную архитектуру, которая позволяет использовать плагины для расширения функциональности. Плагины добавляют дополнительные команды.

```shell
mvn surefire:test # Плагин Surefire используется для выполнения тестов
```

#### Общие опции командной строки:
```shell
   # `-D` Устанавливает системные свойства, которые могут быть использованы в POM-файле
   mvn clean install -DskipTests=true # Эта команда пропускает тесты во время сборки
   mvn clean install -e # `-e` Включает детализированное отображение ошибок
   mvn clean install -X # `-X` Включает отладочный режим, предоставляя более подробную информацию о процессе сборки
   mvn clean install -pl module-name # `-pl` Собирает только указанные модули в многомодульном проекте
```

### Структура POM файла

Файл `pom.xml` в Maven содержит множество тегов, каждый из которых играет свою роль в описании и конфигурации проекта. Рассмотрим основные теги и их назначение:

#### Основные теги:
```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <!-- Корневой элемент POM-файла. Включает в себя всю конфигурацию проекта -->
    <modelVersion>4.0.0</modelVersion> <!-- Указывает версию модели POM, которую использует Maven. Обычно это `4.0.0` -->

    <groupId>com.example</groupId> <!-- Определяет уникальный идентификатор для группы, к которой принадлежит проект. Обычно соответствует домену компании, написанному в обратном порядке -->
    <artifactId>my-app</artifactId> <!-- Уникальный идентификатор для проекта в рамках группы. Это имя проекта или библиотеки -->
    <version>1.0-SNAPSHOT</version> <!-- Определяет версию проекта. Обычно следуют правилам семантического версионирования -->
    <packaging>jar</packaging> <!-- Указывает тип артефакта, который будет создан. Возможные значения: `jar`, `war`, `ear`, `pom`. По умолчанию — `jar` -->

    <name>My Application</name> <!-- Человеко-читаемое имя проекта -->
    <description>A simple application</description> <!-- Краткое описание проекта -->
    <url>http://www.example.com/my-app</url> <!-- URL-адрес, связанный с проектом -->

    <properties> <!-- Используется для определения переменных, которые могут быть использованы в других частях файла -->
        <maven.compiler.source>1.8</maven.compiler.source> <!-- Эта переменная указывает на версию исходного кода Java, которую будет использовать компилятор (В данном на Java 8) -->
        <maven.compiler.target>1.8</maven.compiler.target> <!-- Эта переменная указывает на целевую версию платформы Java, для которой компилируется код (В данном случае означает, что скомпилированный код будет совместим с Java 8)-->
    </properties>

</project>
```

#### Управление зависимостями

Тег  `<dependency>` в Maven используется для указания зависимостей, которые необходимы вашему проекту. Это позволяет Maven автоматически загружать и включать эти зависимости во время сборки проекта.

```xml
   <!-- ... -->
    <dependencies> <!-- Корневой элемент для всех зависимостей проекта -->
        <dependency> <!-- Описывает одну зависимость -->
            <groupId>junit</groupId> <!-- Уникальный идентификатор организации или проекта, который выпускает библиотеку -->
            <artifactId>junit</artifactId> <!-- Уникальный идентификатор конкретного артефакта (библиотеки или модуля) внутри данной группы -->
            <version>4.13.1</version> <!-- Это версия артефакта, который требуется проекту -->
            <scope>test</scope> <!-- Область применения (scope). Это элемент, который определяет область видимости и использование зависимости -->
        </dependency>
    </dependencies>
   <!-- ... -->
```

**Возможные значения для <scope> в <dependency>**

1. `compile`: Зависимость требуется для компиляции и доступна во всех фазах проекта (по умолчанию).
2. `provided`: Зависимость требуется для компиляции, но будет предоставлена контейнером (например, сервером приложений) во время выполнения.
3. `runtime`: Зависимость не требуется для компиляции, но нужна для запуска и тестирования.
4. `test`: Зависимость требуется только для компиляции и запуска тестов.
5. `system`: Зависимость предоставляется локально и не загружается из репозитория Maven.
6. `import`: Используется для импорта зависимостей из BOM-файла (Bill of Materials).

#### Управление сборкой:

Тег `<build>` в Maven используется для определения конфигурации сборки проекта. Он включает в себя информацию о плагинах, которые должны быть использованы, о том, как эти плагины должны быть настроены, а также другую конфигурацию, связанную с процессом сборки проекта.

```xml
   <!-- ... -->
   <build> <!-- Содержит информацию, связанную со сборкой проекта -->

      <resources>
         <resource>
            <directory>src/main/resources</directory> <!-- Копирование директории с ресурсами в папку сборки -->
            <includes>
               <include>**/*</include>
            </includes>
         </resource>
      </resources>

      <plugins> <!-- Корневой элемент для всех плагинов сборки -->
         <plugin> <!-- Описывает один плагин -->
               <groupId>org.apache.maven.plugins</groupId> <!-- Групповой идентификатор. Это уникальный идентификатор группы или организации, которая разрабатывает плагин -->
               <artifactId>maven-compiler-plugin</artifactId> <!-- Артефактный идентификатор. Это уникальный идентификатор самого плагина -->
               <version>3.8.1</version> <!-- Версия. Это версия плагина -->
               <configuration> <!-- Конфигурация. Этот элемент позволяет вам настроить параметры плагина -->
                  <source>${maven.compiler.source}</source> <!-- Пример использования переменной, объявленной в <properties> -->
                  <target>${maven.compiler.target}</target>
               </configuration>
         </plugin>
      </plugins>
   </build>
   <!-- ... -->
   <build>
      <plugins>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <executions> <!-- Корневой элемент для всех разновидностей конфигураций -->
               <execution> <!--  Описывает одну конфигурацию для выполнения плагина -->
                     <id>default-compile</id> <!-- Идентификатор выполнения. Уникальный идентификатор для конкретного выполнения -->
                     <phase>compile</phase> <!-- Фаза сборки Maven, в которой будет выполнено это действие (validate, compile, test, package, verify, install, deploy и другие) -->
                     <goals> <!--  Цели (goals), которые будут выполнены в рамках этого выполнения -->
                        <goal>compile</goal> <!-- Этот тег указывает на конкретную цель, которую должен выполнить плагин. В данном примере указана цель compile, которая отвечает за компиляцию исходного кода. Повышает читаемость для дргуих разработчиков -->
                     </goals>
                     <configuration> <!-- Дополнительная конфигурация, специфичная для данного выполнения -->
                        <source>1.8</source>
                        <target>1.8</target>
                     </configuration>
               </execution>
               <execution> <!--  Пример другой конфигурации для компиляции тестов -->
                     <id>default-test-compile</id>
                     <phase>test-compile</phase>
                     <goals>
                        <goal>testCompile</goal>
                     </goals>
                     <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                     </configuration>
               </execution>
            </executions>
         </plugin>
      </plugins>
   </build>
   <!-- ... -->
```

#### Профили:

`Профили сборки в Maven` позволяют определять различные конфигурации для сборки проекта, которые могут быть активированы в зависимости от условий. Это полезно, когда вам нужно собирать проект с разными параметрами для разных сред (например, для разработки, тестирования или производства).

```xml
   <!-- ... -->
   <profiles> <!-- Корневой элемент, содержащий один или несколько профилей -->
      <profile> <!-- Описывает один профиль сборки. В данном примере это профиль с идентификатором dev -->
         <id>dev</id> <!-- Уникальный идентификатор профиля. В данном случае, это dev -->
         <activation> <!-- Условия активации профиля. Профиль будет активирован, если условия внутри этого элемента выполняются -->
               <property> <!-- Определяет условие активации на основе системного свойства. В данном случае, профиль активируется, если свойство env имеет значение dev -->
                  <name>env</name> <!-- Имя свойства, на основании которого будет происходить активация профиля -->
                  <value>dev</value> <!-- Значение свойства, при котором профиль будет активирован -->
               </property>
         </activation>
         <properties> <!-- Задает свойства, которые будут доступны в проекте, когда профиль активен -->
               <env>development</env> <!-- Задается свойство env со значением development -->
         </properties>
      </profile>
   </profiles>
   <!-- ... -->
```

**Активация профилей**

1. Через командную строку:

```shell
   mvn clean install -Denv=dev # задали переменную env=dev
```

2. Через файл `settings.xml`:

```xml
   <!-- ... -->
   <settings>
      <profiles>
         <profile>
            <id>dev</id>
            <properties>
                  <env>dev</env>
            </properties>
         </profile>
      </profiles>
      <activeProfiles>
         <activeProfile>dev</activeProfile>
      </activeProfiles>
   </settings>
   <!-- ... -->
```

#### Управление репозиториями:

Теги `<repositories>` и `<repository>` в Maven используются для указания репозиториев, из которых Maven будет загружать зависимости и плагины. Эти теги позволяют настроить дополнительные репозитории помимо стандартного центрального репозитория Maven, что может быть полезно, если вы используете зависимости из частных или сторонних репозиториев.

```xml
   <!-- ... -->
   <repositories> <!-- Корневой элемент, содержащий один или несколько элементов <repository> -->
      <repository> <!-- Описывает один репозиторий, из которого Maven будет загружать зависимости -->
         <id>central</id> <!-- Уникальный идентификатор репозитория. Этот идентификатор используется для различения репозиториев в конфигурации Maven -->
         <url>https://repo.maven.apache.org/maven2</url> <!-- URL репозитория, указывающий на его местоположение. В данном примере это URL центрального репозитория Maven -->
         <releases> <!-- Используется для настройки поведения Maven при работе с релизными артефактами, которые считаются стабильными версиями -->
            <enabled>true</enabled> <!-- Включает или отключает загрузку релизных артефактов из этого репозитория. По умолчанию true -->
            <updatePolicy>always</updatePolicy> <!-- Определяет, как часто Maven проверяет обновления для артефактов -->
            <checksumPolicy>warn</checksumPolicy> <!-- Определяет, как Maven обрабатывает проверки контрольных сумм -->
         </releases>
         <snapshots> <!-- Используется для настройки поведения Maven при работе со снапшотами — артефактами, которые находятся в стадии разработки и могут часто меняться -->
            <enabled>false</enabled>
         </snapshots>
      </repository>
      <repository>
         <id>private-repo</id>
         <url>https://private.repo.com/maven2</url>
         <releases>
            <enabled>true</enabled>
         </releases>
         <snapshots>
            <enabled>true</enabled>
         </snapshots>
         <authentication> <!-- Используется для указания учетных данных для доступа к приватным репозиториям -->
            <username>myUser</username> <!-- Имя пользователя для доступа к репозиторию -->
            <password>myPassword</password> <!-- Пароль для доступа к репозиторию. Можно использовать шифрование паролей для повышения безопасности -->
         </authentication>
         <proxy> <!-- Определяет настройки прокси-сервера, который Maven будет использовать для доступа к репозиториям -->
            <id>example-proxy</id> <!-- Уникальный идентификатор прокси -->
            <active>true</active> <!-- Указывает, активен ли прокси. По умолчанию true -->
            <protocol>http</protocol> <!-- Протокол, используемый прокси (http или https) -->
            <host>proxy.example.com</host> <!-- Хост прокси-сервера -->
            <port>8080</port> <!-- Порт прокси-сервера -->
            <username>proxyUser</username> <!-- Имя пользователя для доступа к прокси-серверу -->
            <password>proxyPassword</password> <!-- Пароль для доступа к прокси-серверу -->
            <nonProxyHosts>www.google.com|*.example.com</nonProxyHosts> <!-- Список хостов, которые не должны проходить через прокси. Можно использовать шаблоны с `*` -->
        </proxy>
      </repository>
   </repositories>
   <!-- ... -->
```

**Возможные значения для тега `<updatePolicy>`**:
   - `always`: Maven всегда проверяет наличие обновлений.
   - `daily`: Maven проверяет обновления раз в день.
   - `interval:X`: Maven проверяет обновления каждые X минут.
   - `never`: Maven никогда не проверяет наличие обновлений.

**Возможные значения для тега `<checksumPolicy>`**:
   - `fail`: Сборка завершится ошибкой, если контрольная сумма не совпадает.
   - `warn`: Maven выдаст предупреждение, если контрольная сумма не совпадает.
   - `ignore`: Maven игнорирует контрольные суммы.

## Плагины для сборки

### maven-compiler-plugin (org.apache.maven.plugins)

Назначение: `Компилирует исходный код Java`.

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                  <source>1.8</source> <!-- Указывает версию исходного кода Java -->
                  <target>1.8</target> <!-- Указывает целевую версию платформы Java -->
                  <compilerArguments>-Xlint:unchecked</compilerArguments> <!-- Дополнительные аргументы компилятора -->
                  <showWarnings>true</showWarnings> <!-- Показывать ли предупреждения компилятора -->
            </configuration>
         </plugin>
      </plugins>
   </build>
```

### maven-surefire-plugin (org.apache.maven.plugins)

Назначение: `Запускает тесты во время фазы test`

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-surefire-plugin</artifactId>
               <version>2.22.2</version>
               <configuration>
                  <includes> <!-- Список тестовых классов для включения -->
                     <include>**/*Test.java</include>
                  </includes>
                  <excludes> <!-- Список тестовых классов для исключения -->
                     <exclude>**/SomeTest.java</exclude>
                  </excludes>
                  <forkCount>1</forkCount> <!-- Количество процессов для запуска тестов -->
                  <reuseForks>true</reuseForks> <!-- Повторное использование процессов для тестов -->
                  <parallel>methods</parallel> <!-- Запуск тестов параллельно на уровне методов, классов или обоих -->
                  <threadCount>4</threadCount> <!--  Количество потоков для параллельного выполнения тестов -->
               </configuration>
         </plugin>
      </plugins>
   </build>
```

### maven-jar-plugin (org.apache.maven.plugins)

Назначение: `Упаковывает скомпилированные классы и ресурсы в JAR файл`

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.2.0</version>
            <configuration>
                  <archive> <!-- Конфигурация архива JAR -->
                     <manifest> <!-- Настройки манифеста JAR файла -->
                        <mainClass>com.example.Main</mainClass> <!-- Главный класс для выполнения -->
                     </manifest>
                  </archive>
                  <classifier>sources</classifier> <!-- Классификатор для JAR файла (например, sources для исходных кодов) -->
            </configuration>
         </plugin>
      </plugins>
   </build>
```

### maven-assembly-plugin (org.apache.maven.plugins)

Назначение: `Создает архивы (ZIP, TAR, JAR и т.д.), включающие зависимости и другие ресурсы`

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-assembly-plugin</artifactId>
            <version>3.3.0</version>
            <configuration>
                  <descriptorRefs> <!-- Ссылки на предопределенные дескрипторы сборки -->
                     <descriptorRef>jar-with-dependencies</descriptorRef>
                  </descriptorRefs>
                  <appendAssemblyId>false</appendAssemblyId> <!-- Добавлять ли идентификатор сборки к имени файла -->
                  <archive> <!-- Настройки архива -->
                     <manifest>
                        <mainClass>com.example.Main</mainClass>
                     </manifest>
                  </archive>
            </configuration>
            <executions>
                  <execution>
                     <id>make-assembly</id>
                     <phase>package</phase>
                     <goals>
                        <goal>single</goal>
                     </goals>
                  </execution>
            </executions>
         </plugin>
      </plugins>
   </build>
```

### maven-dependency-plugin (org.apache.maven.plugins)

Назначение: `Управление зависимостями, например, копирование зависимостей в определенные директории`

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <version>3.1.2</version>
            <executions>
                  <execution>
                     <id>copy-dependencies</id>
                     <phase>package</phase>
                     <goals>
                        <goal>copy-dependencies</goal>
                     </goals>
                     <configuration>
                        <outputDirectory>${project.build.directory}/lib</outputDirectory> <!-- Директория, в которую копируются зависимости -->
                        <includeScope>compile</includeScope> <!-- Включение зависимостей определенного уровня (compile, runtime, test и т.д.) -->
                        <includeTypes>jar</includeTypes> <!-- Включение зависимостей определенного типа (jar, war и т.д.) -->
                     </configuration>
                  </execution>
            </executions>
         </plugin>
      </plugins>
   </build>
```

### maven-clean-plugin (org.apache.maven.plugins)

Назначение: `Очищает директорию сборки (удаляет артефакты предыдущих сборок)`

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-clean-plugin</artifactId>
               <version>3.1.0</version>
               <configuration>
                  <filesets> <!-- Наборы файлов для удаления -->
                     <fileset>
                           <directory>${project.build.directory}</directory> <!-- Директория для очистки -->
                           <includes> <!-- Включение файлов для удаления -->
                              <include>**/*.jar</include>
                           </includes>
                           <excludes> <!-- Исключение файлов из удаления -->
                              <exclude>**/*.class</exclude>
                           </excludes>
                     </fileset>
                  </filesets>
               </configuration>
         </plugin>
      </plugins>
   </build>
```

### maven-site-plugin (org.apache.maven.plugins)

Назначение: `Создает сайт проекта на основе информации из POM и отчетов`

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-site-plugin</artifactId>
               <version>3.9.1</version>
               <configuration>
                  <outputDirectory>${project.build.directory}/site</outputDirectory> <!-- Директория, в которую будет сгенерирован сайт -->
                  <reportPlugins> <!-- Плагины для генерации отчетов -->
                     <plugin>
                           <groupId>org.apache.maven.plugins</groupId>
                           <artifactId>maven-project-info-reports-plugin</artifactId>
                           <version>3.1.1</version>
                     </plugin>
                  </reportPlugins>
               </configuration>
         </plugin>
      </plugins>
   </build>
```

### maven-checkstyle-plugin (org.apache.maven.plugins)

Назначение: `Проверяет стиль кода на соответствие правилам`

Этот плагин обычно используется для автоматической проверки кода в процессе сборки, что помогает поддерживать стандарты качества кода в проекте. Конфигурационный файл Checkstyle (checkstyle.xml) определяет правила, которым должен соответствовать код, и может быть настроен в соответствии с требованиями вашего проекта

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-checkstyle-plugin</artifactId>
               <version>3.1.1</version>
               <configuration>
                  <configLocation>checkstyle.xml</configLocation> <!-- Указывает местоположение конфигурационного файла Checkstyle. Это может быть относительный путь к файлу в вашем проекте или URL -->
                  <suppressionsFileExpression>checkstyle-suppressions.xml</suppressionsFileExpression>  <!-- Определяет путь к файлу подавления предупреждений. Этот файл содержит правила подавления предупреждений для конкретных участков кода -->
                  <encoding>UTF-8</encoding> <!-- Указывает кодировку, используемую для чтения исходных файлов -->
                  <outputFile>${project.build.directory}/checkstyle-result.xml</outputFile> <!-- Указывает файл, в который будет записан результат выполнения плагина -->
                  <failOnViolation>true</failOnViolation> <!-- Определяет, должна ли сборка завершаться с ошибкой, если найдены нарушения стиля кода. По умолчанию true -->
                  <linkXRef>false</linkXRef> <!-- Указывает, должны ли результаты включать ссылки на исходные файлы. По умолчанию true -->
                  <consoleOutput>true</consoleOutput> <!-- Определяет, будет ли вывод проверок выведен на консоль. По умолчанию false -->
               </configuration>
               <executions>
                  <execution>
                     <phase>validate</phase>
                     <goals>
                           <goal>check</goal>
                     </goals>
                  </execution>
               </executions>
         </plugin>
      </plugins>
   </build>
```

### maven-enforcer-plugin (org.apache.maven.plugins)

Назначение: `Налагает ограничения и проверяет, соответствуют ли они правилам (например, версия Maven, JDK и т.д.).`

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-enforcer-plugin</artifactId>
               <version>3.0.0-M3</version>
               <executions>
                  <execution>
                     <id>enforce-versions</id>
                     <goals>
                           <goal>enforce</goal>
                     </goals>
                     <configuration>
                           <rules>
                              <requireMavenVersion> <!-- Проверка версии Maven -->
                                 <version>[3.6.0,)</version>
                              </requireMavenVersion>
                              <requireJavaVersion> <!-- Проверка версии JDK -->
                                 <version>[1.8,)</version>
                              </requireJavaVersion>
                              <bannedDependencies> <!-- Запрещение использования определенных зависимостей -->
                                 <searchTransitive>true</searchTransitive> <!-- Указывает, нужно ли искать транзитивные зависимости -->
                                 <includes> <!-- Указывает список запрещенных зависимостей -->
                                       <include>com.example:deprecated-library</include>
                                 </includes>
                              </bannedDependencies>
                              <requirePluginVersions> <!-- Проверяет версии плагинов -->
                                 <banSnapshots>true</banSnapshots> <!-- Указывает, запрещены ли снапшоты плагинов -->
                              </requirePluginVersions>
                           </rules>
                           <fail>true</fail> <!-- Указывает, должна ли сборка завершаться с ошибкой, если найдены нарушения -->
                     </configuration>
                  </execution>
               </executions>
         </plugin>
      </plugins>
   </build>
```

### maven-war-plugin (org.apache.maven.plugins)

Назначение: Используется для создания веб-архивов (WAR) из проектов Maven. Эти архивы WAR содержат все необходимые компоненты веб-приложения, такие как сервлеты, файлы JSP, статические ресурсы и зависимости.

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-war-plugin</artifactId>
               <version>3.3.1</version>
               <configuration>
                  <warSourceDirectory>src/main/webapp</warSourceDirectory> <!-- Директория, содержащая исходные файлы веб-приложения (HTML, JSP, JS, CSS и т.д.) -->
                  <webXml>src/main/webapp/WEB-INF/web.xml</webXml> <!-- Путь к файлу web.xml, который используется для конфигурации сервлетов и других компонентов веб-приложения -->
                  <outputDirectory>${project.build.directory}</outputDirectory> <!--  Директория, куда будет помещен сгенерированный WAR файл -->
                  <archiveClasses>false</archiveClasses> <!-- Флаг, указывающий, должны ли классы быть включены в архив -->
                  <failOnMissingWebXml>true</failOnMissingWebXml> <!-- Флаг, указывающий, должна ли сборка завершаться с ошибкой, если файл web.xml отсутствует -->
                  <webResources> <!-- Определяет дополнительные ресурсы, которые должны быть включены в WAR. Каждый ресурс можно настроить для включения или исключения определенных файлов -->
                     <resource>
                           <directory>src/main/additional-resources</directory>
                           <targetPath>WEB-INF/classes</targetPath>
                           <includes>
                              <include>**/*.properties</include>
                           </includes>
                     </resource>
                  </webResources>
                  <packagingExcludes>**/exclude/**</packagingExcludes> <!-- Указывает файлы или директории, которые нужно исключить из WAR -->
                  <packagingIncludes>**/include/**</packagingIncludes> <!-- Указывает файлы или директории, которые нужно включить в WAR -->
               </configuration>
         </plugin>
      </plugins>
   </build>
```

### appassembler-maven-plugin (org.codehaus.mojo)

Плагин `appassembler-maven-plugin` от `org.codehaus.mojo` используется для создания дистрибутивов Java приложений. Он автоматически генерирует скрипты для запуска приложений на различных платформах, таких как Windows и Unix, что упрощает развертывание и запуск Java программ.

**Пример подключения**
```xml
   <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>appassembler-maven-plugin</artifactId>
      <version>2.1.0</version>
      <configuration>
         <assembleDirectory>target</assembleDirectory> <!-- Указывает директорию, в которой будут созданы скрипты и другие необходимые файлы для запуска приложения. В данном примере это директория target -->
         <programs> <!--  Определяет список программ, для которых будут созданы скрипты -->
               <program> <!-- Описывает одну программу, для которой создается скрипт -->
                  <mainClass>ru.AutoTrackingBot.Program.Main</mainClass> <!-- Главный класс Java-приложения, который будет запускаться -->
                  <name>AutoTrackingBot</name> <!-- Имя программы, которое будет использовано для создание скрипта -->
               </program>
         </programs>
      </configuration>
      <executions>
         <execution>
               <phase>package</phase> <!-- Указывает фазу сборки Maven, в которую будет выполнен плагин. В данном случае это package -->
               <goals>
                  <goal>assemble</goal>
               </goals>
         </execution>
      </executions>
   </plugin>
```

### maven-shade-plugin (org.apache.maven.plugins)

Назначение: `Позволяет укаповать проект в fat jar (uber jar)`

`Maven Shade Plugin` — мощный плагин для Apache Maven, который используется для упаковки всех зависимостей проекта в один исполняемый JAR файл. Он поддерживает множество настроек и конфигураций, которые позволяют адаптировать процесс создания fat JAR файла под ваши нужды. Рассмотрим подробнее параметры настройки Maven Shade Plugin.

**Пример подключения**
```xml
   <build>
      <plugins>
         <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-shade-plugin</artifactId>
               <version>3.2.4</version>
               <executions>
                  <execution>
                     <phase>package</phase>
                     <goals>
                           <goal>shade</goal>
                     </goals>
                     <configuration>
                           <transformers> <!-- Используются для модификации содержимого JAR файла, например, для изменения манифеста или объединения ресурсов -->
                              <!-- Изменение манифеста -->
                              <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer"> <!-- Изменяет файл манифеста JAR. В данном примере устанавливает главный класс -->
                                 <mainClass>com.example.Main</mainClass> 
                              </transformer>
                              <!-- Объединение свойств -->
                              <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"> <!-- Объединяет ресурсы с одинаковыми именами. Например, полезно для объединения файлов свойств Spring -->
                                 <resource>META-INF/spring.handlers</resource> 
                              </transformer>
                              <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                                 <resource>META-INF/spring.schemas</resource>
                              </transformer>
                           </transformers>

                           <!-- Настройка фильтров -->
                           <filters> <!-- Позволяют исключать или включать определенные файлы из зависимостей -->
                              <filter>
                                 <artifact>*:*</artifact> <!-- Задает шаблон для включения/исключения артефактов -->
                                 <excludes> <!-- Исключает файлы из включаемых артефактов. В данном примере исключаются файлы с цифровыми подписями -->
                                       <exclude>META-INF/*.SF</exclude> <!-- Исключает все файлы с расширением .SF в каталоге META-INF. Файлы .SF содержат подписи JAR файла -->
                                       <exclude>META-INF/*.DSA</exclude> <!-- Исключает все файлы с расширением .DSA в каталоге META-INF. Файлы .DSA содержат цифровые подписи и сертификаты -->
                                       <exclude>META-INF/*.RSA</exclude> <!-- Исключает все файлы с расширением .RSA в каталоге META-INF. Файлы .RSA также используются для хранения цифровых подписей и сертификатов -->
                                 </excludes>
                              </filter>
                           </filters>

                           <!-- Настройка включаемых и исключаемых артефактов -->
                           <artifactSet> <!-- Позволяет указать конкретные артефакты (зависимости), которые должны быть включены или исключены из итогового JAR файла -->
                              <includes> <!-- Указывает артефакты, которые должны быть включены -->
                                 <include>com.example:my-dependency</include>
                              </includes>
                              <excludes>
                                 <exclude>org.unwanted:unwanted-dependency</exclude>
                              </excludes> <!-- Указывает артефакты, которые должны быть исключены -->
                           </artifactSet>

                           <!-- Настройка переименования пакетов -->
                           <relocations> <!-- Позволяет переименовывать (relocate) пакеты, чтобы избежать конфликтов между зависимостями -->
                              <relocation>
                                 <pattern>org.apache.commons</pattern> <!-- Паттерн для пакетов, которые нужно переименовать -->
                                 <shadedPattern>com.example.shaded.org.apache.commons</shadedPattern> <!-- Новое имя для переименованных пакетов. В данном примере все пакеты org.apache.commons будут переименованы в com.example.shaded.org.apache.commons -->
                              </relocation>
                           </relocations>
                     </configuration>
                  </execution>
               </executions>
         </plugin>
      </plugins>
   </build>
```

### Что такое META-INF?

`META-INF` — это специальный каталог в JAR файлах, который содержит метаданные о содержимом архива. Эти метаданные используются различными инструментами и библиотеками для обеспечения правильной работы JAR файла. Каталог `META-INF` стандартизирован и поддерживается JVM и многими фреймворками и инструментами.

#### Для чего нужен каталог META-INF?

Каталог `META-INF` служит для хранения различных метаданных, которые могут быть использованы во время выполнения, сборки или проверки целостности JAR файла. Эти метаданные могут включать информацию о версиях, цифровых подписях, конфигурациях сервисов и многое другое.

#### Основные файлы и подкаталоги в META-INF

1. **MANIFEST.MF**: Файл манифеста JAR файла.
   - **Содержимое**: Манифест содержит ключи и значения, которые описывают метаданные JAR файла, такие как версии, главный класс, пути к зависимостям и др.
   - **Пример содержимого**:
     ```plaintext
     Manifest-Version: 1.0
     Main-Class: com.example.Main
     Class-Path: lib/dependency1.jar lib/dependency2.jar
     ```

2. **LICENSE** и **NOTICE**: Лицензионные файлы.
   - **Содержимое**: Информация о лицензии и уведомления о правах, которые должны быть включены в дистрибутив JAR файла.
   - **Пример содержимого LICENSE**:
     ```plaintext
     Apache License
     Version 2.0, January 2004
     http://www.apache.org/licenses/
     ```

3. **services/**: Каталог для Java SPI (Service Provider Interface).
   - **Содержимое**: Файлы с именами интерфейсов и классами, реализующими эти интерфейсы. Используется для регистрации сервисов в Java.
   - **Пример содержимого**: Файл `META-INF/services/java.sql.Driver`, содержащий строку `com.mysql.cj.jdbc.Driver`.

4. **spring.schemas** и **spring.handlers**: Файлы, используемые Spring Framework.
   - **Содержимое**: Содержат информацию о пользовательских пространств имен XML и обработчиках для них.
   - **Пример содержимого spring.handlers**:
     ```plaintext
     http\://www.springframework.org/schema/mycustom=com.example.MyCustomNamespaceHandler
     ```

5. **Файлы цифровых подписей**: `*.SF`, `*.DSA`, `*.RSA`
   - **Содержимое**: Файлы, связанные с цифровыми подписями, обеспечивающими целостность и подлинность JAR файла.
   - **Пример содержимого**: `META-INF/SIGNATURE.SF`, содержащий информацию о подписанных файлах и хэш-значения.

#### Кто создает каталог META-INF?

Каталог `META-INF` и его содержимое обычно создаются инструментами сборки и пакетирования, такими как Maven, Gradle, Ant, а также Java Development Kit (JDK) утилитами, такими как `jar` и `javac`.

- **Maven**: При сборке проекта Maven создает JAR файл и автоматически добавляет файл `MANIFEST.MF` в каталог `META-INF`.
- **Gradle**: Похожим образом, Gradle создает JAR файл и добавляет необходимые метаданные в `META-INF`.
- **JDK**: Утилита `jar` создает `META-INF` и файл `MANIFEST.MF`, если они не предоставлены вручную.

#### Где находится каталог META-INF?

Каталог `META-INF` находится в корне JAR файла. Когда вы распаковываете JAR файл, вы увидите этот каталог наряду с другими пакетами и классами.

#### Пример структуры JAR файла

```
my-app.jar
│
├── META-INF
│   ├── MANIFEST.MF
│   ├── LICENSE
│   ├── NOTICE
│   ├── services
│   │   └── java.sql.Driver
│   ├── spring.handlers
│   ├── spring.schemas
│   ├── SIGNATURE.SF
│   ├── SIGNATURE.DSA
│   └── SIGNATURE.RSA
│
├── com
│   └── example
│       ├── Main.class
│       └── MyCustomNamespaceHandler.class
└── lib
    ├── dependency1.jar
    └── dependency2.jar
```

#### Использование каталога META-INF

1. **Манифест JAR файла**: Указывается главный класс для запуска приложения, пути к зависимостям и другая информация.
2. **Лицензионные файлы**: Включают информацию о лицензии, которая обязательна для распространения ПО.
3. **Java SPI**: Позволяет регистрировать и находить реализации интерфейсов динамически во время выполнения.
4. **Spring Framework**: Используется для конфигурации кастомных пространств имен и обработчиков.
5. **Цифровые подписи**: Обеспечивают безопасность и целостность JAR файла.


### Для чего исключать подписи в блоке `filters`
`Цифровые подписи (файлы .SF, .DSA, .RSA)` создаются для обеспечения целостности и подлинности JAR файла. При создании fat JAR файла эти файлы подписей часто становятся некорректными, так как содержимое JAR файла изменяется. Поэтому их исключают, чтобы избежать проблем с некорректными подписями и гарантировать, что итоговый JAR файл можно будет использовать без ошибок, связанных с проверкой цифровых подписей.

### Типы параметров `implementation` в конфигурации `transformer`
```xml

   <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer"> <!-- Изменение манифеста JAR файла (например, установка основного класса) -->
      <mainClass>com.example.Main</mainClass> <!-- Этот трансформер устанавливает указанный основной класс в манифесте JAR файла, что позволяет запустить JAR как исполняемый файл -->
   </transformer>
 
   <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"> <!-- Объединение ресурсов с одинаковыми именами из различных JAR файлов -->
      <resource>META-INF/spring.handlers</resource> <!-- Этот трансформер объединяет файлы `META-INF/spring.handlers` из всех зависимостей в один файл, который будет включен в итоговый JAR -->
   </transformer>

   <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/> <!-- Этот трансформер объединяет все файлы, находящиеся в `META-INF/services`, чтобы обеспечить корректную работу механизма сервисов Java -->

   <transformer implementation="org.apache.maven.plugins.shade.resource.DontIncludeResourceTransformer"> <!--  Исключение определенных ресурсов из итогового JAR файла -->
         <resource>META-INF/DEPENDENCIES</resource> <!-- Этот трансформер исключает файл `META-INF/DEPENDENCIES` из итогового JAR -->
   </transformer>
```

## Дополнительная информация

### Семантическое версионирование (Semantic Versioning, SemVer)

`Семантическое версионирование (Semantic Versioning, SemVer)` — это система версионирования программного обеспечения, которая помогает разработчикам управлять зависимостями и обновлениями в проекте. Семантическое версионирование имеет формат `MAJOR.MINOR.PATCH`, где каждая часть версии имеет свое значение.

### Основные правила семантического версионирования

1. **Формат версии:**
   Версия должна быть в формате `MAJOR.MINOR.PATCH`, например, `1.0.0`.

2. **Смысл частей версии:**
   - **MAJOR**: Основная версия. Увеличивается, когда вносятся изменения, несовместимые с предыдущими версиями.
   - **MINOR**: Дополнительная версия. Увеличивается, когда добавляется новый функционал, который совместим с предыдущими версиями.
   - **PATCH**: Исправление. Увеличивается, когда вносятся исправления ошибок, совместимые с предыдущими версиями.

3. **Начальная версия:**
   Изначально версия начинается с `0.1.0`. Версия `0.y.z` считается для начальной разработки, и все может измениться в любой момент. Версия `1.0.0` является первой стабильной версией.

4. **Изменения версии:**
   - Если вносится изменение, несовместимое с предыдущими версиями, увеличивается основная версия (`MAJOR`).
   - Если добавляется новая функциональность, совместимая с предыдущими версиями, увеличивается дополнительная версия (`MINOR`).
   - Если вносятся исправления ошибок, совместимые с предыдущими версиями, увеличивается версия исправлений (`PATCH`).

5. **Дополнительные обозначения:**
   - **Pre-release версии**: используются для предварительных выпусков, обозначаются с помощью дефиса и дополнительного идентификатора, например, `1.0.0-alpha`, `1.0.0-beta.2`.
   - **Build metadata**: используется для дополнительной информации о сборке, обозначается с помощью знака `+`, например, `1.0.0+001`.

### Примеры использования

1. **Обновление исправлений:**
   Если текущая версия `1.0.0` и в коде исправлена ошибка, новая версия будет `1.0.1`.

2. **Добавление функциональности:**
   Если текущая версия `1.0.0` и добавлена новая функциональность, которая совместима с предыдущей версией, новая версия будет `1.1.0`.

3. **Внесение несовместимых изменений:**
   Если текущая версия `1.0.0` и в коде внесены изменения, несовместимые с предыдущими версиями, новая версия будет `2.0.0`.

### Рекомендации

- **Фиксированные зависимости:** Для стабильной работы приложения рекомендуется фиксировать зависимости до определенного `MAJOR` уровня, например, использовать `^1.0.0` для npm, что означает любые совместимые версии до `2.0.0`.
- **Документация изменений:** При обновлении версий важно документировать изменения, особенно для `MAJOR` и `MINOR` версий, чтобы пользователи знали, какие изменения внесены и как это может повлиять на их код.

### Заключение
`Семантическое версионирование` — это четкая и понятная система, которая помогает управлять версиями программного обеспечения и зависимостями. Придерживаясь правил SemVer, разработчики могут обеспечить предсказуемость и стабильность своих проектов и библиотек.