# Spring REST

```plantext
Spring REST — это часть Spring MVC, которая специализируется на создании RESTful веб-сервисов, то есть приложений, которые предоставляют API для взаимодействия через HTTP-протокол с использованием стандартных методов (GET, POST, PUT, DELETE и т. д.)
```

- [Spring REST](#spring-rest)
  - [Что такое REST API?](#что-такое-rest-api)
  - [Что такое JSON?](#что-такое-json)
    - [Допустимые типы данных](#допустимые-типы-данных)
    - [Синтаксис JSON](#синтаксис-json)
    - [Недостатки JSON](#недостатки-json)
  - [JSON Mapping](#json-mapping)
  - [Настройка проекта Spring REST API через Java код](#настройка-проекта-spring-rest-api-через-java-код)
    - [Подключение Spring REST через Maven](#подключение-spring-rest-через-maven)
    - [Подключение плагина для создания WAR архива](#подключение-плагина-для-создания-war-архива)
    - [Подключение Hibernate через Maven](#подключение-hibernate-через-maven)
    - [Пример настройки класса конфигурации](#пример-настройки-класса-конфигурации)
  - [Настройка контроллера](#настройка-контроллера)
    - [@RestController](#restcontroller)
  - [Выполнение HTTP-запросов RestTemplate](#выполнение-http-запросов-resttemplate)
    - [Основные методы класса RestTemplate](#основные-методы-класса-resttemplate)
  - [WebClient](#webclient)

## Что такое REST API?

`REST API (Representational State Transfer Application Programming Interface)` — это архитектурный стиль и подход к проектированию веб-сервисов, который использует стандартные протоколы интернета, такие как HTTP, для взаимодействия между клиентом и сервером. RESTful сервисы предоставляют интерфейс, через который клиент может выполнять операции над ресурсами, используя стандартные HTTP методы: GET, POST, PUT, DELETE и другие.

`API (Application Programming Interface)` — это интерфейс для взаимодействия между различными приложениями или системами. REST API — это один из способов построения API с использованием принципов REST.

**Основные принципы REST:**

1. `Клиент-серверная архитектур`а: Клиенты (например, браузеры или мобильные приложения) отправляют запросы на сервер, а сервер отвечает на эти запросы. Клиенты и серверы разделены и взаимодействуют через API.
2. `Без состояния (stateless)`: Каждый запрос от клиента к серверу должен быть самостоятельным и содержать всю информацию, необходимую для его выполнения. Сервер не должен сохранять информацию о состоянии между запросами. Это упрощает масштабирование и надежность системы.
3. `Единообразие интерфейса (uniform interface)`: В REST используются стандартные методы HTTP (GET, POST, PUT, DELETE и т. д.), а ресурсы идентифицируются по URL-адресам (Uniform Resource Identifier, URI). Это упрощает разработку и использование API.
4. `Кэширование`: Ответы на запросы могут быть закэшированы, если это уместно, что улучшает производительность и снижает нагрузку на сервер.
5. `Многоуровневая система`: Клиенты взаимодействуют с API через разные уровни, такие как балансировщики нагрузки или прокси-серверы. REST-сервисы могут поддерживать многоуровневую архитектуру, где клиент не всегда напрямую взаимодействует с сервером.
6. `Ресурсы как основные объекты`: В REST каждая часть данных рассматривается как ресурс. Ресурсы могут быть объектами, коллекциями или документами. Каждый ресурс идентифицируется уникальным URI. Например, ресурс "пользователь" может быть доступен по URI /users/{id}.
7. `Представления ресурсов`: RESTful API предоставляет ресурсы в разных форматах, чаще всего в JSON или XML. Эти данные можно легко парсить на клиентской стороне.

## Что такое JSON?

`JSON (JavaScript Object Notation)` — это текстовый формат обмена данными, который легко читается как человеком, так и компьютером. Он был разработан для простой и удобной передачи данных между клиентом и сервером, особенно в веб-приложениях. Несмотря на то, что JSON был изначально создан на основе синтаксиса JavaScript, он является языконезависимым и поддерживается множеством языков программирования

### Допустимые типы данных

- `Строки`: Любые текстовые данные, заключенные в двойные кавычки, например: "Hello", "World".
- `Числа`: Целые и дробные числа, например: 42, 3.14.
- `Булевые` значения: true или false.
- `Null`: Представляет собой отсутствие значения: null.
- `Массивы`: Упорядоченные списки значений, заключенные в квадратные скобки: [1, 2, 3].
- `Объекты`: Наборы пар "ключ-значение", заключенные в фигурные скобки: {"key1": "value1", "key2": "value2"}

### Синтаксис JSON

```json
    {
        "id": 1,
        "name": "Alice",
        "email": "alice@example.com",
        "age": 28,
        "isStudent": false,
        "address": {
            "city": "New York",
            "zipCode": "10001"
        },
        "courses": ["Mathematics", "Computer Science"],
        "graduationYear": null
    }
```

### Недостатки JSON

- `Отсутствие поддержки сложных типов данных`: JSON не поддерживает такие типы данных, как даты, бинарные данные или ссылки на другие объекты. Даты, например, должны передаваться как строки в формате ISO 8601 или в виде меток времени.
- `Нет поддержки комментариев`: В отличие от других форматов, таких как YAML или XML, JSON не поддерживает комментарии, что может затруднять добавление пояснений или описаний в JSON-документ.
- `Плоские структуры данных`: Хотя JSON поддерживает объекты и массивы, структуры данных могут становиться громоздкими, если требуется передать многоуровневые или взаимосвязанные данные.

## JSON Mapping

`JSON mapping` — это процесс преобразования объектов Java в формат JSON и наоборот. Этот процесс необходим, когда данные в формате JSON отправляются между клиентом и сервером, и требуется их преобразование в объекты Java для обработки на сервере или обратно для отправки на клиент

## Настройка проекта Spring REST API через Java код

### Подключение Spring REST через Maven

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>6.1.6</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.17.2</version>
    </dependency>

    <dependency>
      <groupId>jakarta.servlet</groupId>
      <artifactId>jakarta.servlet-api</artifactId>
      <version>5.0.0</version>
      <scope>provided</scope>
    </dependency>
```

### Подключение плагина для создания WAR архива

```xml
  <build>
    <finalName>spring_rest</finalName>
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
```

### Подключение Hibernate через Maven

```xml
  <!-- Spring ORM -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>6.1.6</version>
  </dependency>

  <!-- Hibernate Core -->
  <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-core</artifactId>
      <version>6.5.2.Final</version>
      <type>pom</type>
  </dependency>

  <!-- PostgreSQL Connector -->
  <dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <version>42.3.3</version>
  </dependency>

  <!-- Зависимости для работы с аннотациями JPA -->
  <dependency>
    <groupId>jakarta.persistence</groupId>
    <artifactId>jakarta.persistence-api</artifactId> 
    <version>3.1.0</version>
  </dependency>
```

### Пример настройки класса конфигурации

```java
  @Configuration
  @ComponentScan(basePackages = "ru.spring_rest.api")
  @EnableWebMvc // Включение Spring MVC
  @EnableTransactionManagement // Включение транзакций
  public class Config {

      @Bean
      public DataSource dataSource() {
          DriverManagerDataSource dataSource = new DriverManagerDataSource();
          dataSource.setDriverClassName("org.postgresql.Driver");
          dataSource.setUrl("jdbc:postgresql://localhost:5432/postgres");
          dataSource.setUsername("dm");
          dataSource.setPassword("");
          return dataSource;
      }

      @Bean
      public LocalSessionFactoryBean sessionFactory() {
          LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
          sessionFactory.setDataSource(dataSource());
          sessionFactory.setPackagesToScan("ru.spring_rest.api.models");
          Properties hibernateProperties = new Properties(); // Параметры для hibernate
          hibernateProperties.put("hibernate.dialect", "org.hibernate.dialect.PostgreSQLDialect");
          hibernateProperties.put("hibernate.show_sql", "true");
          hibernateProperties.put("hibernate.hbm2ddl.auto", "update");
          sessionFactory.setHibernateProperties(hibernateProperties);
          return sessionFactory;
      }

      @Bean
      public HibernateTransactionManager transactionManager() {
          HibernateTransactionManager txManager = new HibernateTransactionManager();
          txManager.setSessionFactory(sessionFactory().getObject());
          return txManager;
      }
    }
```

## Настройка контроллера

### @RestController

`@RestController` — это специальный контроллер, который автоматически объединяет функциональность @Controller и @ResponseBody. Он используется для разработки RESTful веб-сервисов, и все методы в классе, аннотированном @RestController, по умолчанию возвращают данные (например, JSON или XML) в теле HTTP-ответа, а не представления

**Пример:**

```java
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RestController;
  import ru.spring_rest.api.models.Employee;
  import ru.spring_rest.api.services.EmpService;

  import java.util.List;

  @RestController
  @RequestMapping("/api")
  public class MyRESTControllers {

      private EmpService empService;

      @Autowired
      public MyRESTControllers(EmpService empService) {
          this.empService = empService;
      }

      @GetMapping("/emp")
      public List<Employee> showALlEmp() {
          return empService.getAll(); // Автоматически приведет объект в JSON при помощи jackson
      }

      @GetMapping("/emp/{id}")
      public Employee showEmp(@PathVariable("id") Long id) {
          return empService.findById(id);
      }
  }
```

## Выполнение HTTP-запросов RestTemplate

`RestTemplate` — это класс в Spring Framework, который используется для выполнения HTTP-запросов на сторонние веб-сервисы. Он предоставляет удобный и упрощенный способ взаимодействия с RESTful сервисами, позволяя отправлять HTTP-запросы (GET, POST, PUT, DELETE и другие) и получать ответы. RestTemplate обычно используется для интеграции с внешними API или для межсервисного взаимодействия.

**Основные задачи RestTemplate:**

- Отправка HTTP-запросов и получение HTTP-ответов.
- Работа с различными методами HTTP: GET, POST, PUT, DELETE, PATCH и другие.
- Автоматическая сериализация Java-объектов в JSON или XML (и наоборот).
- Работа с URL-адресами, параметрами запросов и заголовками

**RestTemplate считается "устаревающим"** в новых версиях Spring и постепенно заменяется на `WebClient` из Spring WebFlux. `WebClient` предлагает асинхронную поддержку запросов и более гибкую работу с реактивными потоками, хотя RestTemplate по-прежнему широко используется для синхронных операций

### Основные методы класса RestTemplate

```java
  // GET запросы
  Post post = restTemplate.getForObject("https://jsonplaceholder.typicode.com/posts/1", Post.class); // выполняет запрос GET и возвращает объект указанного типа
  ResponseEntity<Post> response = restTemplate.getForEntity("https://jsonplaceholder.typicode.com/posts/1", Post.class); // Возвращает объект ResponseEntity<T>, который содержит не только тело ответа, но и статусный код, и заголовки
  HttpStatus status = response.getStatusCode();
  Post post = response.getBody();

  // POST запросы
  Post newPost = new Post("Title", "Content");
  Post createdPost = restTemplate.postForObject("https://jsonplaceholder.typicode.com/posts", newPost, Post.class); // Отправляет запрос POST и возвращает объект указанного типа в качестве ответа

  ResponseEntity<Post> response = restTemplate.postForEntity("https://jsonplaceholder.typicode.com/posts", newPost, Post.class); // Аналогично postForObject, но возвращает ResponseEntity<T>, включая статус ответа и заголовки

  // PUT запросы
  Post updatedPost = new Post("Updated Title", "Updated Content");
  restTemplate.put("https://jsonplaceholder.typicode.com/posts/1", updatedPost); // Отправляет запрос PUT для обновления ресурса

  // DELETE запросы
  restTemplate.delete("https://jsonplaceholder.typicode.com/posts/1"); // Отправляет запрос DELETE для удаления ресурса

  // Обработка параметров URL
  String url = "https://jsonplaceholder.typicode.com/posts/{id}"; // RestTemplate поддерживает работу с параметрами URL с помощью шаблонов
  Post post = restTemplate.getForObject(url, Post.class, 1); // Здесь параметр {id} будет заменен на значение 1 при выполнении запроса

  // Обработка заголовков
  HttpHeaders headers = new HttpHeaders();
  headers.set("Authorization", "Bearer your-token");

  HttpEntity<String> entity = new HttpEntity<>(headers);
  ResponseEntity<String> response = restTemplate.exchange(
      "https://jsonplaceholder.typicode.com/posts/1", 
      HttpMethod.GET, 
      entity, 
      String.class
  ); // Вы можете передавать кастомные заголовки с помощью HttpHeaders и HttpEntity

  // Метод exchange()
  HttpHeaders headers = new HttpHeaders();
  headers.setContentType(MediaType.APPLICATION_JSON);

  HttpEntity<Post> requestEntity = new HttpEntity<>(newPost, headers);

  ResponseEntity<Post> response = restTemplate.exchange(
      "https://jsonplaceholder.typicode.com/posts",
      HttpMethod.POST,
      requestEntity,
      Post.class
  ); // Этот метод является самым гибким в RestTemplate. Он позволяет вам полностью настраивать HTTP-запрос, включая метод, тело, заголовки, параметры и т.д

  // Метод headForHeaders()
  HttpHeaders headers = restTemplate.headForHeaders("https://jsonplaceholder.typicode.com/posts/1"); // Этот метод выполняет запрос типа HEAD и возвращает заголовки ответа

  // Метод optionsForAllow()
  Set<HttpMethod> options = restTemplate.optionsForAllow("https://jsonplaceholder.typicode.com/posts"); // Этот метод отправляет запрос OPTIONS и возвращает набор поддерживаемых HTTP-методов
```

## WebClient

`WebClient` — это компонент из Spring WebFlux, который предоставляет асинхронную, реактивную альтернативу для отправки HTTP-запросов. В отличие от RestTemplate, который работает в синхронном режиме, WebClient поддерживает асинхронные и реактивные запросы, что делает его более гибким и мощным для современных приложений, особенно если требуется высокая производительность и масштабируемость.

**Основные преимущества WebClient:**

1. Асинхронность и реактивность:
   - WebClient возвращает реактивные типы Mono и Flux, которые позволяют работать с асинхронными операциями и потоками данных.
   - Он не блокирует поток, что позволяет масштабировать приложение и эффективно использовать ресурсы.
2. Поддержка полного спектра HTTP методов:
   - Поддерживаются все HTTP-методы: GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS.
3. Гибкость в настройке:
   - WebClient предоставляет гибкие средства для настройки заголовков, параметров запроса, тела запроса и ответа.
   - Легко интегрируется с OAuth2, OpenID Connect, и другими стандартами безопасности.
4. Поддержка асинхронных потоков данных (streaming):
   - Благодаря использованию реактивного подхода, WebClient позволяет работать с потоками данных, например, при долгосрочных HTTP-соединениях, WebSocket, Server-Sent Events (SSE).
5. Расширяемость:
   - WebClient можно расширять с помощью фильтров, логирования и других средств для изменения поведения запросов или обработки ответов.
   - 