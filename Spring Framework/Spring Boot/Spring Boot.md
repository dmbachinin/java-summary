# Spring Boot

```plantext
    Spring Boot — это модуль из экосистемы Spring, предназначенный для упрощения разработки, настройки и запуска приложений на Java. Он позволяет создавать независимые, готовые к использованию приложения с минимальной конфигурацией, сокращая количество "рутинного" кода и избавляя от необходимости явной настройки инфраструктуры
```

- [Spring Boot](#spring-boot)
  - [Основные особенности Spring Boot](#основные-особенности-spring-boot)
  - [pom.xml в Spring Boot](#pomxml-в-spring-boot)
    - [Spring Boot Parent (родительский POM)](#spring-boot-parent-родительский-pom)
    - [Использование spring-boot-starter зависимостей](#использование-spring-boot-starter-зависимостей)
    - [Полезные зависимости](#полезные-зависимости)
  - [Конфигурация приложения](#конфигурация-приложения)
    - [@SpringBootApplication](#springbootapplication)
      - [Параметры аннотации @SpringBootApplication](#параметры-аннотации-springbootapplication)
      - [@SpringBootConfiguration](#springbootconfiguration)
      - [@EnableAutoConfiguration](#enableautoconfiguration)
    - [Настройка через application.properties](#настройка-через-applicationproperties)
      - [Cписок параметров для application.properties](#cписок-параметров-для-applicationproperties)
        - [Параметры JPA](#параметры-jpa)
        - [Параметры веб-приложений](#параметры-веб-приложений)
      - [Способы подключения .properties файла](#способы-подключения-properties-файла)
  - [Запуск приложения](#запуск-приложения)
    - [SpringApplication.run](#springapplicationrun)
    - [SpringApplication](#springapplication)

## Основные особенности Spring Boot

**Основные особенности Spring Boot:**

1. `Автоконфигурация`: Spring Boot предлагает механизмы автоматической конфигурации, которые позволяют динамически настраивать приложение в зависимости от присутствующих библиотек и компонентов. Это избавляет разработчиков от ручной настройки конфигурации XML или Java-кода. Например, если в проекте добавлена зависимость к базе данных, Spring Boot автоматически настраивает DataSource.
2. `Встроенный сервер`: Spring Boot включает встроенные серверы (например, Tomcat, Jetty, Undertow), что позволяет запускать веб-приложения без необходимости установки и конфигурации внешнего сервера приложений. Это упрощает тестирование и развертывание.
3. `Микросервисы и монолитные приложения`: Spring Boot идеально подходит для разработки как микросервисов, так и монолитных приложений. Он предлагает удобные инструменты для работы с микросервисной архитектурой, обеспечивая быструю разработку, интеграцию и масштабирование.
4. `Поддержка "production-ready" функций`: Встроенные функции мониторинга, готовность к развертыванию в производственной среде и инструменты управления (например, Spring Boot Actuator) позволяют отслеживать состояние приложения, управлять им и диагностировать ошибки.
5. `Минимальные настройки для начала работы`: Spring Boot позволяет создавать новые проекты с минимальными усилиями. Для этого существует инструмент Spring Initializr, который генерирует скелет проекта на основе выбранных зависимостей (например, JPA, Web, Security).
6. `Соглашения поверх конфигурации`: В Spring Boot используется подход "convention over configuration", что означает, что по умолчанию используется "правильная" конфигурация. Разработчику нужно вмешиваться в настройки только при необходимости изменения поведения по умолчанию.

## pom.xml в Spring Boot

### Spring Boot Parent (родительский POM)

В `Spring Boot` проекты часто наследуются от родительского POM-файла spring-boot-starter-parent, который предоставляет базовые настройки для всего проекта, такие как управление версиями зависимостей, настройки плагинов для сборки и профили для различных окружений

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.11</version> <!-- Наиболее популярная версия -->
        <relativePath/> <!-- необходимая опция для локальной конфигурации -->
    </parent>
```

### Использование spring-boot-starter зависимостей

В Spring Boot существует множество стартовых пакетов (spring-boot-starters), каждый из которых предназначен для упрощения конфигурации и подключения необходимых зависимостей для различных функций.

```xml
    <!-- Используется для создания веб-приложений на основе Spring MVC, REST API, и включает встроенный сервер (по умолчанию — Tomcat) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- Включает: Spring MVC, Jackson для работы с JSON, Встроенный сервер Tomcat -->

    <!-- Используется для работы с базами данных через Spring Data JPA и Hibernate. Это стартовый пакет для проектов, которые используют ORM для взаимодействия с реляционными базами данных -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <!-- Включает: Spring Data JPA, Hibernate (по умолчанию), Поддержку транзакций -->

    <!-- Используется для интеграции безопасности в приложение с помощью Spring Security. Обеспечивает настройку аутентификации и авторизации -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <!-- Включает: Spring Security -->

    <!-- Используется для интеграции шаблонизатора Thymeleaf, который часто применяется для создания серверных HTML-страниц в веб-приложениях -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <!-- Включает: Thymeleaf, Интеграцию с Spring MVC -->

    <!-- Используется для тестирования приложений. Включает в себя необходимые инструменты для написания и запуска тестов -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <!-- Включает: JUnit 5, Mockito, Spring Test, AssertJ, Hamcrest -->

    <!-- Используется для добавления поддержки валидации данных с использованием стандартов Bean Validation API (например, для проверки входных данных в формах или REST-запросах) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <!-- Включает: Hibernate Validator (реализация Bean Validation API) -->

    <!-- Используется для добавления мониторинга и управления приложением. Actuator предоставляет готовые эндпоинты для мониторинга состояния приложения, метрик, информации о сборке и здоровья сервиса -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <!-- Включает: Spring Boot Actuator с различными эндпоинтами для мониторинга приложения -->

    <!-- Это стартовый пакет для подключения логгирования. По умолчанию Spring Boot использует SLF4J и Logback для логгирования. В большинстве случаев эта зависимость включается автоматически и отдельно указывать её не нужно, так как она уже включена в каждый стартер -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-logging</artifactId>
    </dependency>
    <!-- Включает: SLF4J, Logback -->

    <!-- Используется для отправки email сообщений из Spring Boot приложений с использованием JavaMailSender -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-mail</artifactId>
    </dependency>
    <!-- Включает: JavaMailSender для отправки email -->

    <!-- Используется для добавления кэширования в приложение. Поддерживает работу с различными механизмами кэширования (EhCache, Hazelcast и т.д.) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-cache</artifactId>
    </dependency>
    <!-- Включает: Поддержку кэширования через аннотации Spring -->

    <!-- Используется для добавления аспектно-ориентированного программирования (AOP) с помощью Spring AOP -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-aop</artifactId>
    </dependency>
    <!-- Включает: Spring AOP -->

    <!-- Используется для создания реактивных веб-приложений с помощью Spring WebFlux. Это альтернативный подход для создания веб-приложений, основанных на асинхронных потоках -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>
    <!-- Включает: Spring WebFlux, Reactor -->

    <!-- Используется для интеграции с MongoDB. Поддерживает работу с документно-ориентированными базами данных через Spring Data MongoDB -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>
    <!-- Включает: Spring Data MongoDB -->

    <!-- Используется для работы с базами данных Redis через Spring Data Redis. Это полезно для кэширования данных или создания высокопроизводительных приложений -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <!-- Включает: Spring Data Redis, RedisTemplate для работы с Redis -->

    <!-- Используется автоматического создания RESTful интерфейса для ваших репозиториев -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-rest</artifactId>
    </dependency>
```

### Полезные зависимости

```xml
    <!-- Данная утилита мониторит и перезапускает приложение в случае нахождения изменений -->
    <!-- Полезна при разработке, чтобы не перезапускать приложение каждый раз -->
    <!-- !Необходимо провести дополнительные настройки в среде разработки! -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
    </dependency>
```

## Конфигурация приложения

### @SpringBootApplication

Аннотация `@SpringBootApplication` является ключевой аннотацией в Spring Boot, которая служит для упрощения конфигурации и запуска приложений на основе Spring. Она объединяет в себе несколько других аннотаций и предоставляет удобный способ настройки приложения

**@SpringBootApplication объединяет 3 аннотации:**

1. `@SpringBootConfiguration`: Указывает, что это класс конфигурации Spring Boot, который может содержать определения бинов.
2. `@EnableAutoConfiguration`: Включает механизм автоматической конфигурации Spring, который пытается настроить приложение на основе добавленных библиотек (например, если в classpath есть spring-web, будет настроен веб-контекст).
3. `@ComponentScan`: Указывает, что Spring должен сканировать текущий пакет и его подпакеты на наличие компонентов (например, классов, помеченных аннотациями @Component, @Service, @Repository, и т.д.).

#### Параметры аннотации @SpringBootApplication

```java
    @SpringBootApplication(scanBasePackages = "com.example.myapp") // Указывает, какие пакеты нужно просканировать для поиска компонентов. Это может быть полезно, если классы конфигурации и компоненты находятся в разных пакетах
    //  * По умолчанию сканирование происходит в текущем пакете и всех подпакетах

    @SpringBootApplication(scanBasePackageClasses = MyApplication.class) // Позволяет указать классы, пакеты которых нужно просканировать для поиска компонентов. Этот параметр более специфичен и позволяет указать один или несколько классов

    @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class }) // Позволяет исключить определенные классы конфигурации из автоматической конфигурации. Это может быть полезно, если вам нужно переопределить поведение какого-либо компонента или отключить ненужную автоматическую настройку

    @SpringBootApplication(excludeName = { "org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration" }) // Позволяет исключить классы конфигурации по их имени. Это менее распространено, но может быть полезно в некоторых случаях
```

#### @SpringBootConfiguration

Аннотация `@SpringBootConfiguration` в Spring Boot является частью механизма конфигурации приложений и используется для указания, что данный класс является конфигурационным для приложения на основе Spring Boot. Эта аннотация является расширением базовой аннотации `@Configuration` и служит для упрощения и улучшения процесса настройки приложений. Давайте рассмотрим её подробнее

**Основные функции @SpringBootConfiguration:**

1. **Определение конфигурации**:
   - Аннотация @SpringBootConfiguration указывает, что класс содержит определения бинов и других конфигурационных настроек, необходимых для работы приложения Spring Boot.
2. **Автоматическая конфигурация**:
   - Включает в себя автоматическую конфигурацию, которая позволяет Spring Boot автоматически настраивать компоненты на основе зависимостей, находящихся в classpath. Это достигается благодаря дополнительной аннотации @EnableAutoConfiguration, которая интегрирована в @SpringBootConfiguration.
3. **Сканирование компонентов**:
    - Автоматически включает сканирование компонентов в пакет, где находится класс с @SpringBootConfiguration, а также во всех его подпакетах. Это происходит через аннотацию @ComponentScan, что позволяет Spring автоматически находить и регистрировать компоненты (например, сервисы, контроллеры и репозитории).

#### @EnableAutoConfiguration

Аннотация `@EnableAutoConfiguration` в Spring Boot является одной из ключевых аннотаций, которая позволяет автоматически настраивать Spring-приложение на основе зависимостей, добавленных в проект. Она значительно упрощает конфигурацию приложений, позволяя разработчикам сосредоточиться на бизнес-логике, а не на рутинной настройке

**Основные функции @EnableAutoConfiguration:**

1. **Автоматическая конфигурация:**
   - @EnableAutoConfiguration автоматически настраивает компоненты приложения на основе наличия определённых библиотек в classpath. Например, если вы добавите зависимость Spring MVC, Spring Boot автоматически настроит необходимые компоненты для создания веб-приложения.
2. **Обработка конфигураций:**
    - Spring Boot использует специальный механизм под названием "Spring Factories" для загрузки конфигураций. Это происходит через файл META-INF/spring.factories, который находится в каждой зависимости Spring Boot. Этот файл содержит список конфигураций, которые будут автоматически применены.
3. **Поддержка различных настроек:**
   - Автоматическая конфигурация охватывает широкий спектр возможностей, включая поддержку баз данных, безопасности, сообщений, кэша и других технологий. Например, если в вашем проекте присутствует spring-boot-starter-data-jpa, Spring Boot автоматически настраивает EntityManager и другие необходимые компоненты JPA.
4. **Исключение автоматической конфигурации:**
   - Если вам не нужна автоматическая конфигурация для определённых компонентов, вы можете использовать параметр exclude аннотации @EnableAutoConfiguration, чтобы отключить конкретные настройки. Это позволяет избежать конфликтов или настроить приложение вручную.

### Настройка через application.properties

Файл `application.properties (или application.yml)` в Spring Boot используется для настройки различных параметров приложения, включая настройки для JPA, веб-приложений и многих других компонентов. Этот файл позволяет управлять поведением приложения, не внося изменения в код

**Имена параметров в Spring Boot являются стандартными и определены в документации**. Эти имена параметров используются для настройки различных компонентов и функциональностей приложения. Использование стандартных имен позволяет разработчикам легко понимать, как и что настраивается, а также позволяет избежать ошибок при конфигурации

**Параметры настройки JPA:**

```properties
    # Настройка источника данных
    spring.datasource.url=jdbc:mysql://localhost:3306/mydb  # URL базы данных
    spring.datasource.username=root                          # Имя пользователя
    spring.datasource.password=secret                        # Пароль

    # Указание драйвера
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  # Драйвер базы данных

    # Настройки JPA
    spring.jpa.hibernate.ddl-auto=update              # Стратегия обновления схемы (none, update, create, create-drop)
    spring.jpa.show-sql=true                           # Вывод SQL-запросов в консоль
    spring.jpa.properties.hibernate.format_sql=true    # Форматирование SQL-запросов
    spring.jpa.properties.hibernate.hbm2ddl.auto=update  # Стратегия обновления схемы (можно также указать validate, create, create-drop)
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect  # Диалект Hibernate
    spring.jpa.properties.hibernate.jdbc.time_zone=UTC  # Установка часового пояса для JDBC
    spring.jpa.properties.hibernate.enable_lazy_load_no_trans= true  # Разрешение ленивой загрузки без транзакции
```

**Параметры настройки веб-приложений:**

```properties
   # Настройки сервера
    server.port=8080                              # Порт, на котором будет запущен сервер
    server.address=127.0.0.1                       # IP-адрес, на котором будет запущен сервер
    server.servlet.context-path=/myapp              # Контекстный путь приложения

    # Настройки сессий
    server.servlet.session.timeout=30m              # Время жизни сессии
    server.servlet.session.cookie.http-only=true     # Ограничение доступа к cookie для JavaScript

    # Настройки кэширования
    spring.cache.type=simple                        # Тип кэша (none, simple, caffeine, ehcache, redis и т.д.)

    # Настройки кросс-доменных запросов (CORS)
    spring.web.cors.allowed-origin-patterns=*       # Разрешённые источники для CORS
    spring.web.cors.allow-credentials=true           # Разрешить использование учетных данных в CORS
```

#### Cписок параметров для application.properties

[Документация с параметрами properties](https://docs.spring.io/spring-boot/appendix/application-properties/)

##### Параметры JPA

```properties
    spring.datasource.url # URL для подключения к базе данных.
    spring.datasource.username # Имя пользователя для подключения.
    spring.datasource.password # Пароль для подключения.
    spring.datasource.driver-class-name # Класс драйвера для подключения.
    spring.jpa.hibernate.ddl-auto # Устанавливает поведение для управления схемой (options # none, validate, update, create, create-drop).
    spring.jpa.show-sql # Выводить SQL-запросы в консоль.
    spring.jpa.properties.hibernate.dialect # Указывает диалект Hibernate.
    spring.jpa.hibernate.format_sql # Форматировать SQL-запросы в выводе.
    spring.jpa.properties.hibernate.hbm2ddl.auto # Дублирует ddl-auto, используется для конкретных настроек Hibernate.
    spring.jpa.properties.hibernate.enable_lazy_load_no_trans # Разрешает ленивую загрузку без активной транзакции.
    spring.jpa.properties.hibernate.jdbc.time_zone # Установка часового пояса для JDBC.
```

##### Параметры веб-приложений

```properties
    server.port # Порт, на котором приложение будет доступно.
    server.address # IP-адрес, на котором приложение будет доступно.
    server.servlet.context-path # Контекстный путь для приложения.
    server.session.timeout # Время ожидания сессии.
    server.servlet.session.cookie.http-only # Ограничивает доступ к cookie для JavaScript.
    spring.web.cors.allowed-origin-patterns # Разрешает указанные источники для CORS.
    spring.web.cors.allow-credentials # Разрешает использование учетных данных в CORS.
    server.ssl.key-store # Указывает путь к хранилищу ключей для SSL.
    server.ssl.key-store-password # Пароль к хранилищу ключей.
    server.error.whitelabel.enabled # Включает или отключает Whitelabel Error Page.
```

#### Способы подключения .properties файла

В Spring Boot можно указать, какой файл .properties или .yml использовать для конфигурации приложения, несколькими способами. Вот основные методы

1. **Использование профилей Spring**

    Spring Boot позволяет создавать различные конфигурационные файлы для разных профилей. Например, вы можете создать файл `application-dev.properties` для профиля разработки и `application-prod.properties` для производственного профиля. Шаблон для формирования названия файла: `application-{profile}.properties`

    ```bash
        java -jar myapp.jar --spring.profiles.active=dev

        export SPRING_PROFILES_ACTIVE=dev
        java -jar myapp.jar
    ```

2. **Указание файла конфигурации через командную строку**

    Вы также можете указать конкретный файл конфигурации при запуске приложения с помощью параметра --spring.config.location

    ```bash
        java -jar myapp.jar --spring.config.location=classpath:/custom-config.properties # classpath: указывает, что файл должен находиться в classpath приложения

        java -jar myapp.jar --spring.config.location=file:/path/to/your/config.properties # использование абсолютного пути

        java -jar myapp.jar --spring.config.location=classpath:/custom-config.properties,file:/path/to/another-config.properties # можно указать несколько файлов конфигурации, разделив их запятыми
    ```

3. **Использование файла конфигурации по умолчанию**

    Если вы не укажете профиль или файл, Spring Boot будет автоматически загружать следующие файлы конфигурации в порядке их приоритета:
    1. application.properties или application.yml (в корне classpath)
    2. application-{profile}.properties или application-{profile}.yml для активного профиля (например, application-dev.properties)
    3. Конфигурационные файлы в зависимости от окружения (например, в Docker-контейнерах, переменные окружения).

4. **Программная настройка**

    Вы также можете программно настроить источники конфигурации, используя класс SpringApplication

    ```java
        import org.springframework.boot.SpringApplication;
        import org.springframework.boot.autoconfigure.SpringBootApplication;
        import org.springframework.boot.env.PropertiesPropertySourceLoader;
        import org.springframework.core.io.ClassPathResource;

        @SpringBootApplication
        public class MyApplication {
            public static void main(String[] args) {
                SpringApplication app = new SpringApplication(MyApplication.class);
                app.getEnvironment().getPropertySources()
                    .addFirst(new PropertiesPropertySourceLoader().load("custom-config.properties", new ClassPathResource("custom-config.properties")));
                app.run(args);
            }
        }
    ```

## Запуск приложения

Запуск приложения Spring Boot — это простой процесс, который может быть выполнен несколькими способами. Основной класс, который отвечает за запуск приложения, — это класс, помеченный аннотацией @SpringBootApplication.

### SpringApplication.run

**Основной класс приложения:**

```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class MyApplication {
        public static void main(String[] args) {
            SpringApplication.run(MyApplication.class, args); // Cоздает контекст приложения, инициализирует его и загружает все бины, конфигурации и компоненты.
        }
    }
```

### SpringApplication

Класс `SpringApplication` в `Spring Boot` является центральным компонентом, который отвечает за запуск приложения. Он управляет инициализацией контекста приложения, конфигурацией, настройкой встроенного веб-сервера (если это веб-приложение) и многими другими аспектами, связанными с запуском приложения

**Основные методы класса SpringApplication:**

```java
    SpringApplication app = new SpringApplication(MyApplication.class); // Получение объекта SpringApplication
    SpringApplication.run(MyApplication.class, args); // Это основной метод, который запускает приложение. Он принимает класс конфигурации и аргументы командной строки
    app.run(args); // Запуск приложения

    
    app.setBanner((environment, sourceClass, out) -> out.println("Custom Banner")); // Позволяет установить баннер, который будет отображаться при запуске приложения. Можно задать кастомный баннер, используя Banner интерфейс

    app.setAdditionalProfiles("dev"); // Позволяет добавить дополнительные профили, которые будут активированы при запуске приложения

    app.addListeners(new MyApplicationListener()); // Позволяет добавить слушателей событий, которые будут обрабатывать различные события приложения, такие как ApplicationStartedEvent, ApplicationReadyEvent и другие

    // Указываем свою фабрику для создания ApplicationContext
    app.setApplicationContextFactory((webApplicationType) -> {
        if (webApplicationType == WebApplicationType.SERVLET) {
            return new AnnotationConfigApplicationContext(); // Используем AnnotationConfigApplicationContext для SERVLET
        } else {
            // По умолчанию используем стандартный контекст
            return ApplicationContextFactory.DEFAULT.create(webApplicationType);
        }
    }); // Позволяет настроить фабрику, которая будет использоваться для создания экземпляра ApplicationContext — центрального интерфейса в Spring, который управляет жизненным циклом бинов и их взаимосвязями
    // ApplicationContextFactory позволяет вам определить, какой ApplicationContext будет использоваться в зависимости от типа вашего приложения

    app.setSources(Set.of("com.example.myapp.AdditionalConfig")); // Используется для указания дополнительных источников конфигурации для контекста приложения Spring. Это позволяет вам добавлять дополнительные классы конфигурации или ресурсы, которые будут использованы при инициализации контекста приложения
    
    // Отключаем вывод информации о старте в логах
    app.setLogStartupInfo(false); // Управляет тем, будет ли выводиться информация о старте приложения в логах при запуске Spring Boot

    ConfigurableEnvironment environment = app.getEnvironment() // В контексте Spring Boot возвращает объект ConfigurableEnvironment, который является частью Spring Framework и отвечает за управление параметрами окружения, свойствами приложения и профилями

    String customProperty = environment.getProperty("my.custom.property"); // Читаем значение свойства
    environment.setActiveProfiles("dev"); // Устанавливаем активный профиль программно

    // Создаем кастомный источник свойств
    Map<String, Object> customProperties = new HashMap<>();
    customProperties.put("custom.property", "This is a custom property");
    MapPropertySource customPropertySource = new MapPropertySource("customSource", customProperties);

    // Добавляем кастомный источник свойств в окружение
    environment.getPropertySources().addLast(customPropertySource);
```
