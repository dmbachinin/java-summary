# Java OpenAPI

```plantext
    Java предоставляет множество инструментов и библиотек для интеграции с OpenAPI. Они позволяют генерировать спецификации OpenAPI из Java-кода, создавать серверы и клиентов на основе существующих спецификаций, а также тестировать API
```

- [Java OpenAPI](#java-openapi)
  - [Design First (Генерация кода из спецификации)](#design-first-генерация-кода-из-спецификации)
    - [Типы генераторов для java](#типы-генераторов-для-java)
  - [Настройка плагина OpenAPI Generator для Maven](#настройка-плагина-openapi-generator-для-maven)
    - [Генератор Spring Boot](#генератор-spring-boot)
  - [Пример настройки генератора Spring Boot](#пример-настройки-генератора-spring-boot)

## Design First (Генерация кода из спецификации)

Генерация кода из спецификации OpenAPI позволяет автоматически создавать клиентские библиотеки, серверные заглушки и модели данных.

### Типы генераторов для java

| Generator Name       | Назначение                               | Особенности                                  |
|----------------------|------------------------------------------|---------------------------------------------|
| **`java`**           | Клиент на Java                          | Поддерживает разные HTTP-библиотеки: RestTemplate, OkHttp, Feign. |
| **`spring`**         | Сервер на Spring Boot                   | Создаёт контроллеры, готовые к интеграции с API. |
| **`kotlin`**         | Клиент на Kotlin                        | Совместим с HTTP-клиентами Kotlin.          |
| **`kotlin-spring`**  | Сервер на Kotlin с Spring Boot          | Поддерживает кодогенерацию для Spring на Kotlin. |
| **`jaxrs-jersey`**   | Сервер с использованием JAX-RS (Jersey) | Подходит для RESTful сервисов на Java EE.   |
| **`jaxrs-cxf`**      | Сервер на Apache CXF                    | Поддержка JAX-RS с Apache CXF.              |
| **`micronaut`**      | Сервер на Micronaut                     | Лёгкий серверный фреймворк для микросервисов. |

---

## Настройка плагина OpenAPI Generator для Maven

### Генератор Spring Boot

Генератор `spring` в OpenAPI Generator предоставляет множество настроек для конфигурации генерируемого кода. Эти настройки позволяют изменять поведение генерации, структуру кода, использовать различные паттерны проектирования и интегрироваться с конкретными библиотеками.

```xml
    <build>
        <plugins>
            <plugin>
                   <groupId>org.openapitools</groupId>
                    <artifactId>openapi-generator-maven-plugin</artifactId>
                    <version>6.6.0</version>
                    <executions>
                        <execution>
                            <id>generate-sources</id>
                            <goals>
                                <goal>generate</goal> <!-- Фаза запуска плагина -->
                            </goals>
                            <configuration>
                                <inputSpec>src/main/resources/openapi.yaml</inputSpec> <!-- Путь к спецификации OpenAPI -->
                                <generatorName>spring-boot</generatorName> <!-- Генератор для Java -->
                                <output>target/generated-sources/openapi</output> <!-- Папка для сгенерированного кода -->
                                
                                <!-- Настраивают структуру пакетов -->
                                <apiPackage>com.example.api</apiPackage> <!-- Указывает пакет, в котором будут размещены клиентские классы, реализующие вызовы API -->
                                <modelPackage>com.example.model</modelPackage> <!-- Указывает пакет, в котором будут размещены модели данных, используемые в запросах и ответах -->
                                <invokerPackage>com.example.invoker</invokerPackage> <!-- Указывает пакет для вспомогательных (утилитных) классов, таких как HTTP-клиенты, сериализация, десериализация и обработка исключений -->
                                
                                <configOptions> <!-- Предоставляет дополнительные настройки, специфичные для выбранного генератора (в данном случае для генератора java). Они задают поведение и архитектуру сгенерированного кода -->
                                    
                                    <library>resttemplate</library> <!-- Указывает, какая библиотека HTTP-клиента будет использоваться -->
                                    <!-- Возможные занчения:
                                    resttemplate (по умолчанию): Использует RestTemplate, который является стандартной HTTP-библиотекой в Spring Framework.
                                    feign: Генерация клиента с использованием Feign, удобного для микросервисной архитектуры.
                                    okhttp-gson: Использует OkHttp и GSON для HTTP-запросов и обработки JSON.
                                    webclient: Использует WebClient из Spring WebFlux для асинхронных запросов -->
                                    
                                    <dateLibrary>java8</dateLibrary> <!--Указывает, какую библиотеку использовать для работы с датами -->
                                    <!-- Возможные значения: 
                                    java8: Использует классы из пакета java.time, такие как LocalDateTime и ZonedDateTime.
                                    java8-localdatetime: Аналогично java8, но с более простыми датами.
                                    threetenbp: Библиотека для работы с датами в проектах, где Java 8 недоступна -->

                                    <java8>false</java8> <!-- Указывает, использовать ли Java 8. Значение false предполагает поддержку более новых версий Java (например, Java 11) -->
                                    <sourceFolder>src/gen/java/main</sourceFolder> <!-- Указывает путь, где будут размещены сгенерированные исходные файлы -->
                                    <openApiNullable>false</openApiNullable> <!-- Указывает, поддерживать ли nullable-объекты OpenAPI -->
                                </configOptions>

                                <skipOverwrite>true</skipOverwrite> <!-- Если true, существующие файлы не будут перезаписаны -->
                                <verbose>true</verbose> <!-- Включает подробный вывод логов -->
                                <generateSupportingFiles>false</generateSupportingFiles>                                 <!-- Указывает, нужно ли генерировать вспомогательные файлы (например, файлы сборки, конфигурации) -->
                                <generateApiDocumentation>false</generateApiDocumentation>                                 <!-- Указывает, нужно ли генерировать документацию API -->
                                <generateApis>false</generateApis> <!-- Указывает, нужно ли генерировать API-классы (контроллеры). Значение false отключает их генерацию -->
                            </configuration>
                            
                            
                            <!-- Позволяет задать глобальные параметры, которые влияют на процесс генерации кода. Эти параметры применяются ко всем аспектам генерации, включая обработку спецификации OpenAPI -->
                            <globalProperties>
                                <modelFilesConstrainedTo>User,Order</modelFilesConstrainedTo> <!-- Указывает, какие модели должны быть сгенерированы. Используется для ограничения генерации моделей, если спецификация содержит много схем, но вы хотите генерировать только определённые -->
                                <skipFormModel>false</skipFormModel> <!-- Указывает, нужно ли игнорировать модели, связанные с формами (в спецификациях OpenAPI они могут быть связаны с параметрами типа formData) -->
                                <skipValidateSpec>false</skipValidateSpec> <!-- Указывает, нужно ли проверять спецификацию OpenAPI на соответствие стандарту. -->
                                <debugOperations>true</debugOperations> <!-- Включает или отключает режим отладки операций API -->
                                <debugModels>false</debugModels> <!-- Включает или отключает режим отладки для моделей данных -->


                                <apis>all</apis> <!-- Определяет, нужно ли генерировать API-классы. all — Генерируются все API-классы. none — API-классы не генерируются-->
                                <models>all</models> <!-- Определяет, нужно ли генерировать модели данных.  all — Генерируются все модели. none — Модели не генерируются-->
                                <supportingFiles>none</supportingFiles> <!-- Определяет, нужно ли генерировать вспомогательные файлы (например, README.md, файлы конфигурации, скрипты сборки). Может принимать значение all, none -->

                                <serverVariables> <!-- Указывает значения для переменных серверов, определённых в спецификации OpenAPI -->
                                    <!-- Формат: Ключ-значение -->
                                    <basePath>/api/v1</basePath>
                                    <host>localhost</host>
                                </serverVariables>

                                <ignoreFileOverride>${basedir}/.openapi-generator-ignore</ignoreFileOverride> <!-- Указывает путь к файлу .openapi-generator-ignore, который содержит правила для игнорирования определённых файлов при генерации -->

                            </globalProperties>

                        </execution>
                    </executions>
            </plugin>
        </plugins>
    </build>
```

**Основные настройки генератора `spring`:**

| **Настройка**            | **Описание**                                                                 | **Значения по умолчанию / возможные значения**                                  |
|---------------------------|-----------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| **`dateLibrary`**         | Указывает библиотеку для работы с датами.                                   | `java8` (по умолчанию), `java8-localdatetime`, `threetenbp`, `legacy`          |
| **`interfaceOnly`**       | Генерировать только интерфейсы без реализации контроллеров.                 | `true`, `false` (по умолчанию)                                                 |
| **`delegatePattern`**     | Включить делегаты для реализации методов.                                   | `true`, `false` (по умолчанию)                                                 |
| **`useTags`**             | Разделить эндпоинты по тегам, генерируя отдельные интерфейсы для каждого.   | `true`, `false` (по умолчанию)                                                 |
| **`singleContentTypes`**  | Генерировать методы с одним типом контента вместо списка.                   | `true`, `false` (по умолчанию)                                                 |
| **`reactive`**            | Генерировать код с поддержкой реактивного программирования (Spring WebFlux).| `true`, `false` (по умолчанию)                                                 |
| **`useBeanValidation`**   | Добавить аннотации `@Valid` и `@NotNull` для валидации данных.              | `true` (по умолчанию), `false`                                                 |
| **`useOptional`**         | Использовать `Optional` для необязательных параметров.                      | `true`, `false` (по умолчанию)                                                 |
| **`skipDefaultInterface`**| Не генерировать интерфейсы, только классы-контроллеры.                      | `true`, `false` (по умолчанию)                                                 |
| **`useSpringController`** | Генерировать аннотации Spring (`@RestController`) вместо JAX-RS.            | `true` (по умолчанию), `false`                                                 |
| **`serializableModel`**   | Сделать сгенерированные модели `Serializable`.                              | `true`, `false` (по умолчанию)                                                 |
| **`title`**               | Указать заголовок проекта для генерации.                                    | Любое текстовое значение                                                       |
| **`springBoot`**          | Генерация Spring Boot приложения (включает Spring Boot аннотации).         | `true`, `false` (по умолчанию)                                                 |
| **`basePackage`**         | Базовый пакет для всех классов (API, модели, утилиты).                      | По умолчанию не указано                                                        |
| **`apiPackage`**          | Пакет для API-классов (контроллеров).                                       | По умолчанию: `io.swagger.api`                                                 |
| **`modelPackage`**        | Пакет для моделей данных.                                                   | По умолчанию: `io.swagger.model`                                               |
| **`configPackage`**       | Пакет для конфигурационных классов.                                         | По умолчанию: `io.swagger.configuration`                                       |
| **`enumUnknownDefaultCase`** | Добавить `UNKNOWN` как значение по умолчанию для `enum`.                 | `true`, `false` (по умолчанию)                                                 |

---

## Пример настройки генератора Spring Boot

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <plugin>
                <groupId>org.openapitools</groupId>
                <artifactId>openapi-generator-maven-plugin</artifactId>
                <version>5.1.0</version>
                <executions>
                    <execution>
                        <id>base</id>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                        <configuration>
                            <inputSpec>${basedir}/src/main/openapi/pprb-selfemployedb2b.yaml</inputSpec>
                            <output>${basedir}/target/generated-sources</output>
                            <generatorName>spring</generatorName>
                            <library>spring-boot</library>
                            <generateSupportingFiles>false</generateSupportingFiles> 
                            <generateApiDocumentation>false</generateApiDocumentation>
                            <generateApis>false</generateApis>
                            <modelPackage>ru.trade.helper.web.model</modelPackage>
                            <configOptions>
                                <java8>false</java8>
                                <dateLibrary>java8</dateLibrary>
                                <sourceFolder>src/gen/java/main</sourceFolder>
                                <openApiNullable>false</openApiNullable>
                            </configOptions>
                            <globalProperties>
                                <skipFormModel>false</skipFormModel>
                            </globalProperties>
                        </configuration>
                    </execution>
            </plugin>
        </plugins>
    </build>
```
