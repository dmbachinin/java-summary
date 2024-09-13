# Spring ORM

`Spring ORM` — это модуль в экосистеме Spring, который поддерживает работу с ORM (Object-Relational Mapping) фреймворками. Он предоставляет абстракции для работы с разными ORM-технологиями, такими как Hibernate, JPA, JDO, и iBatis.

- [Spring ORM](#spring-orm)
  - [Основы Spring ORM](#основы-spring-orm)
    - [DAO (Data Access Object)](#dao-data-access-object)
    - [Основные аннотации Spring ORM](#основные-аннотации-spring-orm)
      - [@Transactional](#transactional)
        - [Параметры аннотации @Transactional](#параметры-аннотации-transactional)
      - [@PersistenceContext](#persistencecontext)
      - [@PersistenceUnit](#persistenceunit)
      - [@Repository](#repository)
      - [@Service](#service)
      - [@EnableTransactionManagement](#enabletransactionmanagement)
  - [Hibernate в Spring ORM](#hibernate-в-spring-orm)
    - [Покдключение зависимости Hibernate и Spring ORM](#покдключение-зависимости-hibernate-и-spring-orm)
      - [Подключение через Maven](#подключение-через-maven)
    - [LocalSessionFactoryBean](#localsessionfactorybean)
    - [HibernateTransactionManager](#hibernatetransactionmanager)
    - [Пример настройки через Java-конфигурацию](#пример-настройки-через-java-конфигурацию)

## Основы Spring ORM

`Spring Framework` поддерживает интеграцию с Java Persistence API (JPA) и поддерживает собственный режим гибернации для управления ресурсами, реализаций объектов доступа к данным (DAO) и стратегий транзакций. Например, для Hibernate имеется первоклассная поддержка с несколькими удобными функциями IoC, которые решают многие типичные проблемы интеграции с Hibernate. Вы можете настроить все поддерживаемые функции для инструментов отображения OR (объектно-реляционных) посредством внедрения зависимостей. Они могут участвовать в управлении ресурсами и транзакциями Spring и соответствуют общим иерархиям транзакций Spring и исключений DAO.

Spring Framework упрощает создание ORM DAO благодаря нескольким ключевым преимуществам:

1. **Упрощение тестирования**: Подход IoC Spring позволяет легко заменять компоненты, такие как Hibernate `SessionFactory`, `DataSource` и менеджеры транзакций, что делает тестирование более удобным и гибким.
2. **Единая обработка исключений**: Spring преобразует исключения из ORM-инструментов (например, Hibernate) и JDBC в иерархию `DataAccessException`. Это упрощает обработку исключений и устраняет необходимость работы с проприетарными исключениями.
3. **Общее управление ресурсами**: Spring автоматически управляет конфигурацией ресурсов, таких как `SessionFactory`, `EntityManagerFactory`, `DataSource`, и обеспечивает эффективную обработку транзакций и сессий, привязанных к текущему потоку.
4. **Интегрированное управление транзакциями**: С помощью аннотации `@Transactional` или AOP Spring упрощает управление транзакциями, включая их откат и обработку исключений. Поддерживается интеграция с JTA и локальными транзакциями, а также транзакционная работа с JDBC и ORM в одном контексте.

### DAO (Data Access Object)

`DAO (Data Access Object)` (Объект доступа к данным) — это паттерн проектирования, который используется для разделения логики доступа к данным от бизнес-логики приложения. Он упрощает работу с базами данных, предоставляя интерфейс для выполнения операций чтения, записи, обновления и удаления данных (CRUD — Create, Read, Update, Delete).

В паттерне `DAO (Data Access Object)` могут быть методы, отличные от стандартных CRUD операций (Create, Read, Update, Delete). Хотя CRUD-операции составляют основу любого взаимодействия с базой данных, реальные приложения часто требуют выполнения более сложных или специфичных операций, которые не попадают под эти категории. В таких случаях DAO можно расширить, добавив дополнительные методы для работы с данными

**Пример использования DAO:**

```java
public interface UserDAO {
    //  Стандарный CRUD
    void addUser(User user);
    User getUser(int id);
    List<User> getAllUsers();
    void updateUser(User user);
    void deleteUser(int id);

    // Пример сложных запросов
    List<User> findUsersByRoleAndRegistrationDate(String role, Date startDate, Date endDate);
}
```

### Основные аннотации Spring ORM

Spring ORM добавляет и использует несколько ключевых аннотаций для интеграции с различными ORM-фреймворками, такими как Hibernate или JPA. Эти аннотации помогают управлять транзакциями, внедрять зависимости и упрощать работу с базами данных.

В Spring ORM эти аннотации позволяют легко интегрировать объекты ORM в приложение, управлять транзакциями и работать с базой данных в объектно-ориентированном стиле. Использование таких аннотаций делает код более декларативным, гибким и модульным, что упрощает поддержку и тестирование приложений.

#### @Transactional

**Описание**: Эта аннотация используется для обозначения методов или классов, которые должны быть выполнены в пределах транзакции. Spring автоматически открывает транзакцию, а затем коммитит её по завершению метода. В случае возникновения исключения происходит откат транзакции.

##### Параметры аннотации @Transactional

1. `propagation`

    Параметр `propagation` определяет, как транзакции должны распространяться при вызове метода, который уже выполняется внутри другой транзакции или без неё. Это позволяет контролировать, как новая транзакция будет вести себя в зависимости от существующего контекста

    **Основные значения Propagation**:

    - `REQUIRED (по умолчанию)`: Если существует активная транзакция, метод будет выполнен в её контексте Если  транзакции нет, будет создана новая. Метод выполняется в существующей транзакции, если она уже открыта, иначе создается новая.

    - `REQUIRES_NEW`: Метод всегда будет выполняться в новой транзакции. Если уже существует активная транзакция, она будет приостановлена до завершения новой транзакции. Применяется, когда необходимо выполнить операцию в отдельной транзакции, независимо от текущей.

    - `MANDATORY`: Требуется существование активной транзакции. Если транзакции нет, будет выброшено исключение TransactionRequiredException. Используется в случаях, когда метод должен всегда вызываться в контексте активной транзакции.

    - `SUPPORTS`: Метод не требует транзакции, но если она существует, метод будет выполнен в её контексте. Метод работает в транзакции, если она существует, но также может быть выполнен и без неё.

    - `NOT_SUPPORTED`: Метод не будет выполняться в контексте транзакции. Если транзакция существует, она будет приостановлена на время выполнения метода. Используется для выполнения операций, которые не должны быть частью транзакции, например, чтение большого объема данных.

    - `NEVER`: Запрещает выполнение метода в транзакции. Если метод вызывается в транзакционном контексте, будет выброшено исключение. Подходит для методов, которые не должны выполняться в транзакции по логике.

    - `NESTED`: Создает вложенную транзакцию, которая может быть откатана независимо от внешней транзакции. Вложенная   транзакция использует точку сохранения (savepoint) и может быть откатана без отката всей внешней транзакции. Полезно, если необходимо частично откатить изменения внутри одной транзакции, но не затрагивать другие операции

2. `isolation`: Уровень изоляции транзакции.

    Параметр isolation определяет уровень изоляции транзакции, т.е. как одна транзакция будет взаимодействовать с другими транзакциями при доступе к одним и тем же данным. Это помогает избежать таких проблем, как "грязное чтение", "неповторяемое чтение" и "фантомное чтение".

    **Уровни изоляции в порядке увеличения строгих требований**:

    - `READ_UNCOMMITTED`: Самый низкий уровень изоляции. Транзакция может читать изменения, сделанные другой транзакцией, даже если они ещё не завершены (не зафиксированы). Это приводит к грязному чтению — чтению данных, которые могут быть откатаны.

    - `READ_COMMITTED` (по умолчанию в большинстве БД): Транзакция видит только те изменения, которые были зафиксированы другими транзакциями. Это предотвращает грязное чтение, но неповторяемое чтение (когда данные изменяются между двумя запросами) все ещё возможно.

    - `REPEATABLE_READ`: Гарантирует, что данные, прочитанные в рамках одной транзакции, не изменятся до её завершения, что предотвращает как грязное чтение, так и неповторяемое чтение. Однако возможны фантомные чтения — когда новые строки добавляются в таблицу другой транзакцией, и они становятся видны в следующем запросе.

    - `SERIALIZABLE`: Самый высокий уровень изоляции. Транзакции выполняются последовательно, как если бы они выполнялись одна за другой. Это предотвращает все проблемы — грязное чтение, неповторяемое чтение и фантомные чтения, но значительно снижает производительность

3. `readOnly`: Указывает, что транзакция только для чтения (`true`), что может оптимизировать выполнение.
4. `timeout`: Максимальное время выполнения транзакции(в секундах), после которого она будет прервана.
5. `rollbackFor`: Определяет, какие исключения должны привести к откату транзакции.

**Пример использования**:

```java
    @Transactional(
        propagation = Propagation.REQUIRES_NEW,
        isolation = Isolation.REPEATABLE_READ,
        readOnly = false,
        timeout = 15,
        rollbackFor = {SQLException.class, IOException.class}
    )
    public void transferMoney(Account fromAccount, Account toAccount, BigDecimal amount) throws SQLException, IOException {
        // Логика перевода денег между счетами
    }
```

#### @PersistenceContext

**Описание**: Аннотация, которая используется для внедрения `EntityManager` (при работе с JPA). Это объект, который управляет операциями с базой данных для сущностей.

- **Пример использования**:

```java
  @PersistenceContext
  private EntityManager entityManager;
  
  public User findUser(Long id) {
      return entityManager.find(User.class, id);
  }
```

#### @PersistenceUnit

**Описание**: Используется для внедрения `EntityManagerFactory` — фабрики, создающей экземпляры `EntityManager`. Аннотация полезна, если требуется больше контроля над созданием `EntityManager`.

**Пример использования**:

```java
    @PersistenceUnit
    private EntityManagerFactory entityManagerFactory;

    public EntityManager createEntityManager() {
        return entityManagerFactory.createEntityManager();
    }
```

#### @Repository

**Описание**: Эта аннотация используется для маркировки DAO- или репозиторий-классов, которые работают с базой данных. Она сообщает Spring, что класс содержит логику доступа к данным. Кроме того, эта аннотация автоматически обрабатывает исключения, переводя их в `DataAccessException`.
**Пример использования**:

```java
    @Repository
    public class UserRepository {
        @PersistenceContext
        private EntityManager entityManager;
        
        public void save(User user) {
            entityManager.persist(user);
        }
    }
```

#### @Service

`@Service` — это одна из стереотипных аннотаций в Spring Framework, которая используется для обозначения класса как компонента уровня сервиса в многослойной архитектуре приложения. Она облегчает управление бинами в Spring и их внедрение в другие классы

**Основные задачи аннотации @Service**:

1. `Явное обозначение бизнес-логики`: Делает код более понятным, сигнализируя, что класс реализует бизнес-логику.
2. `Упрощает управление бинами Spring`: Классы, помеченные как @Service, автоматически становятся бинами Spring и управляются контейнером IoC (Inversion of Control — инверсия управления).
3. `Упрощает внедрение зависимостей`: Через аннотацию @Autowired или другие механизмы внедрения зависимостей Spring можно легко внедрить сервис в другие компоненты приложения.

**Пример использования**:

```java
   @Service
    public class StudentServiceImpl implements StudentService {

        @Autowired
        private StudentDao studentDao;  // DAO для работы с базой данных

        @Override
        public void addStudent(Student student) {
            studentDao.save(student);
        }

        @Override
        public Student getStudentById(Long id) {
            return studentDao.findById(id);
        }
    }
```

#### @EnableTransactionManagement

**Описание**: Эта аннотация включает поддержку аннотации `@Transactional` в Spring-приложении. Она указывает фреймворку, что нужно обрабатывать классы и методы с аннотацией `@Transactional` для управления транзакциями.

**Пример использования**:

```java
    @Configuration
    @EnableTransactionManagement
    public class AppConfig {
        // Конфигурация bean-компонентов
    }
```

## Hibernate в Spring ORM

### Покдключение зависимости Hibernate и Spring ORM

#### Подключение через Maven

```xml
    <dependencies>
        <!-- Spring ORM -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>5.3.20</version>
        </dependency>

        <!-- Hibernate Core -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.6.7.Final</version>
        </dependency>

        <!-- PostgreSQL Connector -->
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.3.3</version>
        </dependency>

        <!-- Spring Context and JDBC -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.20</version>
        </dependency>
    </dependencies>
```

### LocalSessionFactoryBean

`LocalSessionFactoryBean` — это специальный фабричный бин в Spring Framework, который используется для настройки и управления объектами SessionFactory из Hibernate. Это важный компонент в интеграции Spring с Hibernate, так как он облегчает создание и настройку SessionFactory, которая необходима для взаимодействия с базой данных через ORM.

`LocalSessionFactoryBean` служит для создания и конфигурации Hibernate SessionFactory в контексте Spring. Он автоматически подстраивает сессии под используемую базу данных и ORM-фреймворк.

**Методы:**

```java
    LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
    sessionFactory.setDataSource(dataSource()); // Используется для указания источника данных (DataSource), который будет использован для подключения к базе данных
    sessionFactory.setPackagesToScan("com.example.entity"); // Используется для указания пакетов, которые Hibernate должен сканировать на предмет аннотированных классов-сущностей (@Entity)

    Properties hibernateProperties = new Properties();
    hibernateProperties.put("hibernate.dialect", "org.hibernate.dialect.PostgreSQLDialect");
    hibernateProperties.put("hibernate.show_sql", "true");
    hibernateProperties.put("hibernate.hbm2ddl.auto", "update");

    sessionFactory.setHibernateProperties(hibernateProperties); // Этот метод позволяет передать различные свойства для настройки Hibernate

    sessionFactory.setAnnotatedClasses(Student.class, Course.class); // Этот метод позволяет вручную указать конкретные классы-сущности, которые должны быть использованы Hibernate

    sessionFactory.setMappingResources("student.hbm.xml", "course.hbm.xml"); // Этот метод используется для указания ресурсов маппинга в виде XML-файлов .hbm.xml. Это необходимо, если вы используете XML-конфигурации для описания маппинга сущностей на таблицы базы данных вместо аннотаций

    sessionFactory.setPhysicalNamingStrategy(new ImprovedNamingStrategy()); // Этот метод позволяет задать стратегию физического именования таблиц и колонок в базе данных. Это полезно, если вы хотите контролировать правила наименования на уровне базы данных (например, преобразование имен сущностей в snake_case)

    sessionFactory.setConfigLocation(new ClassPathResource("hibernate.cfg.xml")); // Этот метод указывает на внешний файл конфигурации Hibernate, например, hibernate.cfg.xml

    Map<String, Object> hibernatePropertiesMap = new HashMap<>();
    hibernatePropertiesMap.put("hibernate.dialect", "org.hibernate.dialect.MySQLDialect");
    hibernatePropertiesMap.put("hibernate.show_sql", true);

    sessionFactory.setHibernatePropertiesMap(hibernatePropertiesMap); // Альтернативный метод для передачи свойств Hibernate в виде Map<String, Object>

    Map<String, Object> eventListeners = new HashMap<>();
    eventListeners.put("postInsert", new PostInsertEventListener());

    sessionFactory.setEventListeners(eventListeners); // Этот метод позволяет задать кастомные обработчики событий Hibernate (например, события создания или удаления сущностей)


    sessionFactory.setEntityInterceptor(Interceptor entityInterceptor); // Этот метод позволяет задать глобальный Hibernate-интерсептор, который будет обрабатывать все действия над сущностями (например, добавление, обновление, удаление). Интерсепторы используются для добавления дополнительных действий или изменений перед выполнением операции с сущностями

    sessionFactory.setJtaTransactionManager(PlatformTransactionManager jtaTransactionManager); // Этот метод используется для указания JTA (Java Transaction API) транзакционного менеджера, если вы используете распределенные транзакции. Он интегрирует Hibernate с JTA для работы с транзакциями, которые затрагивают несколько ресурсов, таких как база данных и очереди сообщений

    sessionFactory.setMultiTenantConnectionProvider(MultiTenantConnectionProvider provider); // Этот метод позволяет настроить поддержку многопользовательской среды (multi-tenancy). Используется для управления несколькими подключениями к базам данных в зависимости от арендатора (tenant)

    sessionFactory.setCurrentTenantIdentifierResolver(CurrentTenantIdentifierResolver resolver); // Используется для настройки механизма разрешения текущего идентификатора арендатора (tenant identifier) в многопользовательской среде

    sessionFactory.setLobHandler(LobHandler lobHandler); // Этот метод используется для работы с большими объектами (LOB) — такими как большие текстовые и бинарные данные. Позволяет указать специальный обработчик LOB, который поддерживает работу с ними в Hibernate
```

### HibernateTransactionManager

`HibernateTransactionManager` — это класс в Spring Framework, который используется для управления транзакциями в приложениях, использующих Hibernate в качестве ORM (Object-Relational Mapping). Этот класс интегрируется с механизмом управления транзакциями Spring и предоставляет поддержку транзакций на уровне Hibernate.

Этот класс расширяет `AbstractPlatformTransactionManager`, который является базовым классом для всех менеджеров транзакций в Spring. `HibernateTransactionManager` управляет транзакциями на уровне `SessionFactory` Hibernate и поддерживает такие важные аспекты транзакционного управления, как:

- Автоматическое начало транзакции перед вызовом метода.
- Коммит транзакции по завершению метода.
- Откат транзакции в случае выброса исключений.

**Методы:**

```java
    HibernateTransactionManager transactionManager = new HibernateTransactionManager();
    transactionManager.setSessionFactory(sessionFactory); // Этот метод связывает менеджер транзакций с фабрикой сессий Hibernate, позволяя управлять транзакциями в контексте работы с базой данных

    TransactionDefinition def = new DefaultTransactionDefinition();
    TransactionStatus status = transactionManager.getTransaction(def); // Начинает транзакцию в соответствии с заданным определением транзакции или использует существующую, если она уже активна
    
    transactionManager.commit(status); // Коммитит (фиксирует) изменения в текущей транзакции. Все изменения, сделанные в рамках этой транзакции, сохраняются в базе данных

    transactionManager.rollback(status); // Откатывает (rollback) транзакцию. Все изменения, сделанные в рамках транзакции, отменяются

    transactionManager.setDataSource(dataSource); // Этот метод позволяет HibernateTransactionManager работать с конкретным DataSource, если вы хотите напрямую использовать источник данных вместо настройки через SessionFactory

    transactionManager.setNestedTransactionAllowed(true); // Разрешает или запрещает вложенные транзакции. По умолчанию вложенные транзакции отключены. Этот метод позволяет их включить, что полезно, когда одна транзакция начинается внутри другой

    transactionManager.setDefaultTimeout(30); // Этот метод задает время ожидания в секундах. Если транзакция не завершится в течение этого времени, будет вызвано исключение, и транзакция будет откатана

    transactionManager.setGlobalRollbackOnParticipationFailure(true); // Определяет, следует ли откатывать всю транзакцию, если произошел сбой внутри одного из участников. Если параметр установлен в true, то при возникновении ошибки во вложенной транзакции вся внешняя транзакция будет откатана

    transactionManager.setRollbackOnCommitFailure(true); // Определяет, следует ли откатывать транзакцию, если при выполнении коммита произошла ошибка

    boolean exists = transactionManager.isExistingTransaction(transactionStatus); // Проверяет, существует ли активная транзакция в текущем контексте

    transactionManager.setValidateExistingTransaction(true); // Если включено, Spring будет проверять, существует ли уже активная транзакция перед началом новой транзакции

```

### Пример настройки через Java-конфигурацию

```java
    @Configuration
    @EnableTransactionManagement // Включает возможность исопльзовать @Transactional
    public class HibernateConfig {

        @Bean
        public DataSource dataSource() { // Определяем DataSource
            DriverManagerDataSource dataSource = new DriverManagerDataSource();
            dataSource.setDriverClassName("org.postgresql.Driver");
            dataSource.setUrl("jdbc:postgresql://localhost:5432/yourdb");
            dataSource.setUsername("yourUsername");
            dataSource.setPassword("yourPassword");
            return dataSource;
        }

        @Bean
        public LocalSessionFactoryBean sessionFactory() {
            LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
            sessionFactory.setDataSource(dataSource());
            sessionFactory.setPackagesToScan("com.example.entity"); // Пакет с сущностями
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
