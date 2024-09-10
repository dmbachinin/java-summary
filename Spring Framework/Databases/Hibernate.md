
# Hibernate

```plantext
    В данном файле собрана информация про Hibernate
```

- [Hibernate](#hibernate)
  - [Hibernate основная информация](#hibernate-основная-информация)
  - [Подключение Hibernate](#подключение-hibernate)
    - [Подключение через Maven](#подключение-через-maven)
  - [Конфигурация через hibernate.cfg.xml](#конфигурация-через-hibernatecfgxml)
    - [Исопльзование файла hibernate.properties](#исопльзование-файла-hibernateproperties)
    - [Настройка кэширования в Hibernate](#настройка-кэширования-в-hibernate)
  - [Создание связи между классом и таблицей в базе](#создание-связи-между-классом-и-таблицей-в-базе)
    - [Аннотации Hibernate](#аннотации-hibernate)
      - [@Entity](#entity)
      - [@Table](#table)
      - [@Id](#id)
      - [@GeneratedValue](#generatedvalue)
      - [@Column](#column)
      - [@Transient](#transient)
      - [@Temporal](#temporal)
      - [@Enumerated](#enumerated)
      - [@Lob](#lob)
      - [@OneToOne](#onetoone)
      - [@OneToMany](#onetomany)
      - [@ManyToOne](#manytoone)
      - [@ManyToMany](#manytomany)
      - [@JoinColumn](#joincolumn)
      - [@JoinTable](#jointable)
      - [@Embedded](#embedded)
      - [@Embeddable](#embeddable)
  - [Session](#session)
    - [Создание SessionFactory через Configuration](#создание-sessionfactory-через-configuration)
    - [Контекст персистенции](#контекст-персистенции)
    - [Основные методы Session](#основные-методы-session)
    - [Работа с запросами через Session](#работа-с-запросами-через-session)
      - [Класс Query](#класс-query)

## Hibernate основная информация

`Hibernate` — это популярная библиотека в экосистеме Java, предназначенная для решения задач объектно-реляционного отображения (ORM). Это означает, что `Hibernate` позволяет отображать объекты приложения на таблицы базы данных и автоматически управлять данными между объектной моделью Java (POJO) и базой данных.

**Плюсы Hibernate:**

- Предоставляет технологию ORM
- Регулирует SQL запросы (нам не нужно их писать)
- Уменьшает количество кода для написания
- Использует JDBC за нас

## Подключение Hibernate

### Подключение через Maven

```xml
    <dependency>
        <groupId>org.hibernate.orm</groupId>
        <artifactId>hibernate-core</artifactId> <!-- Для основной функциональности Hibernate -->
        <version>6.5.2.Final</version>
    </dependency>

    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId> <!-- Добавление зависимости для драйвера JDBC  -->
        <version>42.2.5</version>
    </dependency>

    <dependency>
        <groupId>jakarta.persistence</groupId>
        <artifactId>jakarta.persistence-api</artifactId> <!-- Зависимости для работы с аннотациями JPA -->
        <version>3.1.0</version>
    </dependency>
```

## Конфигурация через hibernate.cfg.xml

После добавления зависимостей вам нужно будет настроить `Hibernate`. Это обычно включает в себя создание файла конфигурации `hibernate.cfg.xml`, который должен быть расположен в папке **src/main/resources**.

**Пример конфигурации для PostgreSQL:**

```xml
    <!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

    <hibernate-configuration>
        <session-factory>
            <!-- JDBC Database connection settings -->
            <property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property> <!-- Класс JDBC-драйвера для базы данных -->
            <property name="hibernate.connection.url">jdbc:postgresql://localhost:5432/mydatabase</property> <!-- URL-адрес подключения к базе данных -->
            <property name="hibernate.connection.username">root</property>
            <property name="hibernate.connection.password">password</property>

            <!-- Указывает диалект базы данных, с которой работает Hibernate -->
            <property name="hibernate.dialect">org.hibernate.dialect.PostgreSQLDialect</property> 

            <!-- Стратегия для работы с схемой базы данных -->
            <property name="hibernate.hbm2ddl.auto">update</property>
            <!-- Примеры значений: -->
            <!--
                create — Удаляет старую схему и создаёт её заново на основе текущей модели.

                update — Автоматически обновляет существующую схему базы данных, добавляя новые столбцы или таблицы, но не удаляя старые.
                
                validate — Проверяет, что схема базы данных соответствует объектной модели, но не изменяет её. Если есть несоответствия, будет выброшено исключение.
                
                create-drop - Создаёт схему при запуске приложения и удаляет её при завершении работы
                
                none — не вносит никаких изменений. 
            -->

            <property name="hibernate.current_session_context_class">thread</property> <!-- Настройка контекста для работы с сессиями -->
            <!-- Примеры значений: -->
            <!--
                thread — Контекст сессии привязывается к потоку. Используется в большинстве приложений
                jta — Контекст сессии управляется с помощью JTA (Java Transaction API) в корпоративных приложениях.
                managed — Управление сессиями осуществляется вручную пользователем. 
            -->

            <!-- Указывает классы сущностей (Entity), которые будут маппироваться на таблицы базы данных -->
            <mapping class="com.example.MyEntity"/>
            <mapping resource="com/example/MyEntity.hbm.xml"/> <!-- Может также ссылаться на файлы .hbm.xml, если используется XML-маппинг -->

            <!-- Дополнительные настройки -->
            <property name="hibernate.show_sql">true</property> <!-- Выводит в консоль SQL-запросы, которые выполняет Hibernate -->
            <property name="hibernate.format_sql">true</property> <!-- Форматирует SQL-запросы для удобства чтения -->
            <property name="hibernate.use_sql_comments">true</property> <!-- Добавляет комментарии к сгенерированным SQL-запросам -->

        </session-factory>
    </hibernate-configuration>
```

### Исопльзование файла hibernate.properties

Для более грамотной установки параметром следует исопльзовать файл `hibernate.properties`. В котором можно прописать все необходимые параметры. **Основной файл конфигурации будет искать данный файл в той же папке, в которой находится сам**.

Пример файла `hibernate.properties`:

```property
    hibernate.connection.driver_class=org.postgresql.Driver
    hibernate.connection.url=jdbc:postgresql://localhost:5432/mydatabase
    hibernate.connection.username=myusername
    hibernate.connection.password=mypassword
    hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
    hibernate.show_sql=true
    hibernate.format_sql=true
    hibernate.hbm2ddl.auto=update
```

**Пример конфигурации с файлом свойств**:

```xml
    <!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

    <hibernate-configuration>
        <session-factory>
            <!-- Load properties from hibernate.properties -->
            <property name="hibernate.connection.driver_class">${hibernate.connection.driver_class}</property>
            <property name="hibernate.connection.url">${hibernate.connection.url}</property>
            <property name="hibernate.connection.username">${hibernate.connection.username}</property>
            <property name="hibernate.connection.password">${hibernate.connection.password}</property>

            <property name="hibernate.dialect">${hibernate.dialect}</property>

            <!-- Additional settings from hibernate.properties -->
            <property name="hibernate.show_sql">${hibernate.show_sql}</property>
            <property name="hibernate.format_sql">${hibernate.format_sql}</property>
            <property name="hibernate.hbm2ddl.auto">${hibernate.hbm2ddl.auto}</property>

            <!-- Entity mappings -->
            <mapping class="com.example.MyEntity"/>
        </session-factory>
    </hibernate-configuration>

```

### Настройка кэширования в Hibernate

Свойство `hibernate.cache.region.factory_class` в Hibernate используется для настройки механизма кэширования второго уровня (L2 cache). Кэш второго уровня помогает хранить объекты между сессиями, что улучшает производительность при повторных запросах к базе данных

**Подключение кеширования:**

```xml
    <property name="hibernate.cache.use_second_level_cache">true</property> <!-- Включение кеширования -->
    <property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property> <!-- Определяет класс фабрики регионов для работы с кэшем второго уровня -->
    <property name="net.sf.ehcache.configurationResourceName">/ehcache.xml</property> <!-- Путь к файлу конфигурации ehcache.xml, где задаются параметры работы с Ehcache -->
```

**Возможные значения для hibernate.cache.region.factory_class:**

1. `org.hibernate.cache.ehcache.EhCacheRegionFactory`
   - Используется для интеграции с Ehcache, одним из наиболее популярных решений для кэширования второго уровня в Hibernate.
   - Требует конфигурации через файл `ehcache.xml`, в котором можно настроить параметры кэша, такие как время жизни объектов, количество элементов в кэше и т.д.

2. `org.hibernate.cache.infinispan.InfinispanRegionFactory`
    - Этот класс используется для интеграции с Infinispan, который является распределённой системой кэширования и   ключевого значения.
    - Используется в случаях, когда нужно организовать кэш в кластерной среде или распределённое кэширование.

3. `org.hibernate.cache.redis.SingletonRedisRegionFactory`
    - Интеграция с Redis. Redis — это распределённый in-memory key-value store, который часто используется для  кэширования. Этот тип кэша поддерживает синхронизацию между разными инстансами приложения через Redis.
    - Для работы с Redis могут понадобиться дополнительные библиотеки.

4. `org.hibernate.cache.jcache.JCacheRegionFactory`
   - Использует стандартную спецификацию JCache (JSR-107). JCache предоставляет абстракцию для работы с кэшами в различных провайдерах кэша, таких как Ehcache, Hazelcast и другие.
   - Выбор конкретного JCache провайдера зависит от ваших требований.

5. `org.hibernate.cache.internal.NoCachingRegionFactory`
   - Этот класс используется, если вы не хотите использовать кэширование. В таком случае объекты будут подгружаться из базы данных каждый раз без хранения их в кэше.

## Создание связи между классом и таблицей в базе

Для создания связи между классом и таблицей необходимо создать Entity класс.

`Entity класс` - это класс, который отображает информацию определенной таблицы в Базе Данных, это POJO класс, в котором мы используем Hibernate аннотации.

`POJO (Plain Old Java Object - Простой старый объект Java)` — это обычный объект Java, который не зависит от специфических фреймворков или библиотек. Такие классы являются основой для маппинга объектов в Hibernate.

Основные характеристики POJO классов:

1. `Не зависят от фреймворков`: POJO не содержит специфических аннотаций, интерфейсов или наследования от классов каких-либо библиотек (например, EJB, Spring). Это обычные классы Java.
2. `Содержат только поля, геттеры и сеттеры`: POJO-класс состоит из приватных полей и публичных методов доступа к ним (геттеров и сеттеров).
3. `Конструктор по умолчанию`: В большинстве случаев POJO имеет конструктор по умолчанию (без параметров). Hibernate, например, использует его для создания экземпляров сущностей.
4. `Простота и легкость использования`: POJO-классы являются простыми и не перегружены лишней логикой или зависимостями от внешних библиотек.

### Аннотации Hibernate

В Hibernate используется множество аннотаций для маппинга Java-классов на таблицы базы данных и их столбцы. Эти аннотации позволяют описать различные аспекты структуры базы данных и отношения между сущностями. Вот основные аннотации, которые используются для создания **Entity** классов в Hibernate.

#### @Entity

- Указывает, что класс является сущностью, которая будет маппироваться на таблицу в базе данных.
- Без этой аннотации класс не будет распознан Hibernate как сущность.
  
```java
@Entity
public class User {
    // class body
}
```

#### @Table

- Указывает название таблицы, на которую маппируется сущность.
- Если не указать аннотацию, то таблица по умолчанию будет названа по имени класса.

```java
@Entity
@Table(name = "users")
public class User {
    // class body
}
```

#### @Id

- Указывает поле как первичный ключ таблицы.
  
```java
@Entity
public class User {
    @Id
    private Long id;
    // other fields and methods
}
```

#### @GeneratedValue

- Определяет стратегию генерации значений для первичного ключа.
- Параметр `strategy` может принимать следующие значения:
  - **GenerationType.AUTO**: Hibernate сам выбирает подходящую стратегию (по умолчанию).
  - **GenerationType.IDENTITY**: Использует стратегию автоинкремента (например, для MySQL, PostgreSQL).
  - **GenerationType.SEQUENCE**: Использует последовательности (Sequence) для генерации ключей.
  - **GenerationType.TABLE**: Использует отдельную таблицу для генерации уникальных значений.
- При генерации значения, оно подставляется в соответствующее поле в самом объекте

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id; // После генерации ПК в данное поле будет записано его значение
//  * Если присвоить значение в поле id, то оно все равно будет перезаписано
```

#### @Column

- Используется для настройки маппинга поля класса на колонку таблицы.
- Может задавать имя колонки, длину, уникальность и другие ограничения.

```java
@Column(name = "username", length = 50, nullable = false, unique = true)
private String username;
```

#### @Transient

- Поле, аннотированное этой аннотацией, не будет сохраняться в базу данных. Используется для временных или вычисляемых значений, которые не нужно хранить.

```java
@Transient
private String tempPassword;
```

#### @Temporal

- Применяется для маппинга полей типа `java.util.Date` или `java.util.Calendar`.
- Указывает формат хранения даты:
  - **TemporalType.DATE**: только дата.
  - **TemporalType.TIME**: только время.
  - **TemporalType.TIMESTAMP**: дата и время.

```java
@Temporal(TemporalType.DATE)
private Date birthDate;
```

#### @Enumerated

- Используется для маппинга полей типа `enum`. Может сохранять либо порядковый номер значения, либо его строковое представление.
  
```java
@Enumerated(EnumType.STRING)
private UserRole role;
```

#### @Lob

- Аннотация используется для полей, которые содержат большие объёмы данных, такие как текстовые строки или бинарные данные (BLOB или CLOB).

```java
@Lob
private String biography;
```

#### @OneToOne

- Указывает, что между двумя сущностями существует отношение "один к одному".
- Поля `cascade`, `fetch` и другие параметры позволяют детализировать поведение связи.

```java
@OneToOne(cascade = CascadeType.ALL)
@JoinColumn(name = "profile_id")
private UserProfile profile;
```

#### @OneToMany

- Указывает, что между сущностями существует отношение "один ко многим".
- Используется в случаях, когда одна сущность может быть связана с несколькими экземплярами другой сущности.

```java
@OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
private List<Order> orders;
```

#### @ManyToOne

- Определяет отношение "многие к одному", когда несколько объектов могут ссылаться на один объект.
  
```java
@ManyToOne
@JoinColumn(name = "user_id")
private User user;
```

#### @ManyToMany

- Определяет отношение "многие ко многим". Обычно требует таблицу связывания, которую можно настроить с помощью аннотации `@JoinTable`.

```java
@ManyToMany
@JoinTable(
    name = "user_roles",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "role_id")
)
private Set<Role> roles;
```

#### @JoinColumn

- Используется для указания столбца, который связывает две сущности.
- Обычно применяется в аннотациях `@OneToOne`, `@ManyToOne` и других.

```java
@ManyToOne
@JoinColumn(name = "user_id")
private User user;
```

#### @JoinTable

- Указывает таблицу связывания для отношений `@ManyToMany`.
  
```java
@ManyToMany
@JoinTable(
    name = "user_group",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "group_id")
)
private Set<Group> groups;
```

#### @Embedded

- Указывает, что поле является встраиваемым объектом. Hibernate будет маппировать его поля как отдельные колонки в таблице основной сущности.

```java
@Embedded
private Address address;
```

#### @Embeddable

- Указывает, что класс может быть встроен в другую сущность с использованием аннотации `@Embedded`.

```java
@Embeddable
public class Address {
    private String street;
    private String city;
    // Other fields
}
```

## Session

В Hibernate, `SessionFactory` — это объект, который отвечает за создание и управление сессиями (объектами класса Session), которые используются для взаимодействия с базой данных. `SessionFactory` является потокобезопасным и **обычно создаётся один раз на протяжении всего жизненного цикла приложения**. После его создания он используется для получения сессий.

### Создание SessionFactory через Configuration

Класс `Configuration` в Hibernate используется для настройки и построения объекта SessionFactory. Он загружает конфигурационные параметры и маппинг классов для Hibernate и предоставляет методы для их настройки. Основные задачи, выполняемые с помощью `Configuration`, включают чтение конфигурационных файлов (например, hibernate.cfg.xml), регистрацию сущностей и построение SessionFactory

**Методы класса Configuration:**

```java
    Configuration config = new Configuration().configure(); // По умолчанию загружает hibernate.cfg.xml
    Configuration config = new Configuration().configure("custom.cfg.xml"); // Загружает файл custom.cfg.xml

    config.addAnnotatedClass(User.class); // Используется для регистрации классов, которые маппируются с помощью аннотаций. Этот метод позволяет добавлять сущности без использования XML-файлов маппинга

    config.addPackage("com.example.entities"); // Регистрирует пакеты для поиска классов сущностей. Все классы, аннотированные как @Entity в этом пакете, будут зарегистрированы

    config.addResource("com/example/mappings/User.hbm.xml"); // Добавляет файл маппинга XML вручную. Это полезно, если вы используете отдельные .hbm.xml файлы для маппинга

    Properties props = new Properties();
    props.setProperty("hibernate.dialect", "org.hibernate.dialect.PostgreSQLDialect");
    config.addProperties(props); // Позволяет программно добавлять конфигурационные параметры для Hibernate

    config.setProperty("hibernate.show_sql", "true"); // Позволяет задать конкретное свойство конфигурации. Это эквивалент добавления параметра в XML-файл или в файл свойств
    
    Properties props = config.getProperties(); // Возвращает все текущие настройки конфигурации в виде объекта Properties

    SessionFactory sessionFactory = config.buildSessionFactory(); // Создаёт объект SessionFactory на основе всех настроек и маппингов, указанных в конфигурации

    config.setInterceptor(Interceptor interceptor); // Устанавливает интерсептор для перехвата событий, происходящих в Hibernate (например, до сохранения или после загрузки сущности)

    InputStream stream = new FileInputStream("path/to/hibernate.cfg.xml");
    config.addInputStream(stream); // Загружает конфигурацию из XML-файла через поток InputStream. Это полезно, если вы хотите динамически загружать конфигурацию из нестандартного источника

    config.addCacheableFile("com/example/mappings/User.hbm.xml"); // Добавляет кэшируемый файл маппинга (обычно XML-файл) для загрузки маппингов сущностей. Используется для улучшения производительности за счёт кэширования

    config.setNamingStrategy(NamingStrategy strategy); // Устанавливает стратегию именования для маппинга полей в колонки таблицы (например, для преобразования имен полей в нижний регистр)

    Properties newProps = new Properties();
    newProps.setProperty("hibernate.connection.username", "newUser");
    config.mergeProperties(newProps); // Сливает новые свойства с уже существующими свойствами конфигурации

    config.setSessionFactoryObserver(SessionFactoryObserver observer); // Добавляет наблюдателя (observer), который следит за созданием или закрытием SessionFactory
```

### Контекст персистенции

В контексте Hibernate (и JPA в целом) `контекст персистенции` (или "контекст постоянства") — это пространство или область, в которой Hibernate управляет состоянием объектов. Контекст персистенции определяет, какие объекты отслеживаются системой, и отвечает за синхронизацию изменений этих объектов с базой данных.

Контекст персистенции является основой для механизма управления состояниями объектов в Hibernate. Он создаётся при открытии сессии и закрывается, когда сессия завершается. Все сущности, загруженные в контексте сессии, отслеживаются и сохраняются автоматически

**Ключевые концепции контекста персистенции:**

1. Управляемые объекты (`Managed Entities`)
    - Объект находится в контексте персистенции, если Hibernate отслеживает его изменения. Это называется "управляемым состоянием" (managed state). **Все изменения, сделанные с таким объектом, будут автоматически синхронизированы с базой данных** при вызове метода `flush()` или при завершении транзакции
  
    ```java
        Transaction tx = session.beginTransaction();
        User user = session.find(User.class, 1L);  // Загружаем объект, который становится управляемым
        user.setUsername("NewUsername");  // Изменение управляемого объекта
        tx.commit();  // Изменения автоматически синхронизируются с базой данных
    ```

2. Объекты в "отключённом" состоянии (`Detached Entities`)
    - Если объект покидает контекст персистенции (например, если сессия закрыта или объект был явно "отключён"), он становится "отключённым" (detached). В таком состоянии Hibernate больше не отслеживает его изменения, и любые изменения в объекте не будут синхронизированы с базой данных до тех пор, пока объект не будет снова связан с контекстом сессии (например, через `merge()`)
  
    ```java
        User user = session.find(User.class, 1L);  // Объект управляемый
        session.close();  // Теперь объект находится в отключённом состоянии
        user.setUsername("UpdatedName");  // Это изменение не будет сохранено в базу данных
    ```

3. Новый объект (`Transient`)
    - Объект находится в "переходном" состоянии (transient state), если он был создан в памяти, но не связан с контекстом персистенции и ещё не существует в базе данных
    - Новый объект можно сделать управляемым с помощью метода `persist()` или `merge()`

    ```java
        User newUser = new User();  // Новый объект, не связанный с базой данных (переходное состояние)
        session.persist(newUser);   // Теперь объект становится управляемым
    ```

4. Удалённый объект (`Removed`)
    - Если объект был удалён с помощью метода remove(), он переходит в состояние "удалённого" (removed). При этом он удаляется из базы данных, но остаётся в памяти до завершения транзакции

    ```java
        User user = session.find(User.class, 1L);
        session.remove(user);  // Объект помечен как удалённый и будет удалён при завершении транзакции
    ```

### Основные методы Session

```java

    // Методы Transaction - все операции выполняются в рамках транзакций, даже SELECT

    Transaction tx = session.beginTransaction(); // Начинает новую транзакцию для выполнения операций с базой данных. Возвращает объект Transaction, который можно использовать для управления транзакцией (commit, rollback)
    Transaction tx = session.getTransaction(); // Возвращает текущую активную транзакцию, если она существует
    tx.commit(); // Применяет все изменения, сделанные в базе данных в рамках транзакции. После вызова этого метода все изменения будут зафиксированы в базе данных, и транзакция завершится. Также завершится Session
    tx.rollback(); // Откатывает транзакцию, отменяя все изменения, сделанные в её рамках. Этот метод должен вызываться при возникновении ошибок или исключений, чтобы откатить все изменения, сделанные до этого момента
    boolean isActive = tx.isActive(); // Возвращает true, если транзакция активна, и false, если она завершена или откатана. Это полезно для проверки состояния транзакции
    boolean isParticipating = tx.isParticipating(); // Возвращает true, если транзакция участвует в текущем контексте. Этот метод может использоваться для проверки того, является ли транзакция частью более крупной операции
    TransactionStatus status = tx.getStatus(); // Возвращает текущее состояние транзакции, которое представлено перечислением TransactionStatus. Этот метод полезен для мониторинга хода транзакции
    tx.markRollbackOnly(); // Этот метод помечает текущую транзакцию для отката. Он полезен, если вам нужно указать, что транзакция не должна быть зафиксирована, даже если она ещё не завершена
    boolean wasCommitted = tx.wasCommitted(); // Возвращает true, если транзакция была успешно зафиксирована с помощью commit(), и false в противном случае
    boolean wasRolledBack = tx.wasRolledBack(); // Возвращает true, если транзакция была откатана, и false, если откат не произошёл

    // Методы Session

    session.persist(Object obj); // Сохраняет новый объект в контексте персистенции, добавляя его в базу данных. Работает только с новыми объектами, которые не были сохранены ранее. Не возвращает идентификатор

    User updatedUser = session.merge(detachedUser); // Сливает изменения в "отключённом" объекте с состоянием, находящимся в базе данных. Возвращает управляемый объект (если был отключённый объект), который отслеживается Hibernate.
    //  * Если проинициализировать поле id в объекте, то он будет считаться "отключенным"

    session.remove(user); // Удаляет объект из базы данных. Объект должен быть в "управляемом" состоянии (managed), иначе его нужно сначала загрузить
    session.refresh(user); // Обновляет объект из базы данных, перезаписывая локальные изменения
    session.evict(user); // Удаляет объект из контекста сессии, переводя его в состояние "отключённый" (detached)

    User user = session.find(User.class, 1L); // Загружает объект по его идентификатору. Возвращает null, если объект не найден
    User user = session.get(User.class, 1L); // Загружает объект по его идентификатору. Немедленно выполняет запрос к базе данных и возвращает объект или null, если он не найден
    /* Отличия методов find и get
        get - сразу выполняет SQL-запрос к базе данных для загрузки объекта, даже если объект может быть закэширован в контексте персистенции (сессии)
        find - сначала проверяет, находится ли объект в контексте персистенции (в текущей сессии) или в кэше второго уровня. Если объект уже загружен, он будет возвращён без выполнения нового запроса к базе данных
    */

    User user = session.getReference(User.class, 1L); // Загружает объект по его идентификатору, но возвращает только ссылку на объект (похож на load()), которая может быть инициализирована при доступе к полям. Если объект не найден, будет выброшено исключение EntityNotFoundException

    session.lock(user, LockModeType.OPTIMISTIC); // Устанавливает блокировку для объекта с определённым режимом блокировки (например, READ, WRITE, OPTIMISTIC и т.д.). Это позволяет контролировать доступ к объектам в конкурентных средах

    User user = session.byId(User.class).load(1L); // Возвращает интерфейс для загрузки сущности по её идентификатору, с возможностью указания стратегии блокировки и других параметров

    boolean isContained = session.contains(user); // Проверяет, находится ли объект в контексте персистенции (управляется ли объект сессией)

    session.clear(); // Очищает контекст сессии, удаляя все объекты из неё. При этом изменения, не записанные в базу данных, будут потеряны

    session.flush(); // Принудительно записывает изменения из контекста сессии в базу данных. Обычно вызов flush() происходит автоматически в конце транзакции, но его можно вызвать вручную
```

### Работа с запросами через Session

```java
    Query query = session.createQuery("FROM User WHERE username = :username"); // Создаёт объект запроса HQL (Hibernate Query Language) для выполнения запросов к базе данных
    query.setParameter("username", "JohnDoe");
    List<User> users = query.list();

    Query query = session.createNativeQuery("SELECT * FROM users WHERE id = :id", User.class); // Создаёт объект запроса на чистом SQL для выполнения запросов, написанных на SQL
    query.setParameter("id", 1L);
    User user = (User) query.getSingleResult();
```

#### Класс Query

Класс `Query` в Hibernate используется для выполнения запросов к базе данных с использованием HQL (Hibernate Query Language) или SQL. Этот класс позволяет гибко выполнять выборки, обновления и удаления данных, а также предоставляет возможности для параметризации запросов, управления результатами и обработки кэширования

Для чего нужен класс Query:

Класс Query в Hibernate используется для создания и выполнения запросов к базе данных. С его помощью можно:

- Выполнять запросы для выборки данных (SELECT).
- Выполнять запросы для изменения данных (UPDATE, DELETE).
- Использовать параметры для динамического построения запросов.
- Управлять кэшированием результатов и их постраничной загрузкой (pagination).
- Контролировать конкурентный доступ к данным с помощью блокировок.
  
Различие HQL и SQL-запросов:

- HQL (Hibernate Query Language) работает с сущностями, а не с таблицами базы данных. Он использует имена классов и их поля в запросах.
- SQL-запросы напрямую обращаются к таблицам и столбцам базы данных.

**Основные методы класса Query:**

```java
    Query<User> query = session.createQuery("FROM User WHERE username = :username", User.class); // Используется для выполнения запросов на основе HQL (Hibernate Query Language), который похож на SQL, но работает с объектами, а не с таблицами, поэтому в данном примере User не является таблицей, а это класс User, аналогично username - это имя поля в классе User
    Query query = session.createNativeQuery("SELECT * FROM users WHERE username = :username", User.class); // Используется для выполнения запросов на чистом SQL 
    Query query = session.createQuery("UPDATE User SET username = :newUsername WHERE id = :id");

    query.setParameter("username", "JohnDoe"); // Устанавливает значение для именованного параметра в запросе
    query.setMaxResults(10); // Ограничивает количество возвращаемых результатов
    query.setFirstResult(5);  // Устанавливает начальную позицию для возвращаемых результатов (используется для пагинации) (Пропускаем первые 5 записей)
    query.setCacheable(true); // Указывает, следует ли кэшировать результаты запроса (второй уровень кэширования)
    query.setTimeout(30);  // Устанавливает тайм-аут для выполнения запроса в секундах
    query.setFetchSize(50); // Устанавливает размер выборки (то есть количество записей, которое будет получено за один раз)

    query.setLockMode(LockModeType.PESSIMISTIC_WRITE); // Устанавливает режим блокировки для объекта, возвращаемого запросом (например, для предотвращения конкурентных изменений)
    //  Основные типы блокировок (LockModeType)
    /*
        LockModeType.NONE - Блокировка отсутствует. Это означает, что Hibernate не будет применять никакой блокировки к сущности. Подходит для операций чтения данных, где не важно, что данные могут быть изменены другими транзакциями после их загрузки

        LockModeType.OPTIMISTIC - Используется для реализации оптимистичной блокировки, когда объект блокируется только при его обновлении. Для этого обычно используются версии сущностей. Hibernate при сохранении объекта проверяет, изменилась ли версия объекта с момента его загрузки. Если версия изменилась (например, другой транзакцией), будет выброшено исключение. Подходит для случаев, когда предполагается, что конфликты возникают редко

        LockModeType.OPTIMISTIC_FORCE_INCREMENT - Это также оптимистичная блокировка, но с явным увеличением версии сущности при обновлении. Объект блокируется для других транзакций, и его версия увеличивается принудительно, даже если объект не был изменён. Это полезно для обеспечения того, что другие транзакции не будут работать с устаревшими данными

        LockModeType.PESSIMISTIC_READ - Используется для пессимистичной блокировки на чтение, которая гарантирует, что текущая транзакция может читать данные, но другие транзакции не могут изменять их до завершения текущей транзакции. Этот режим блокировки предотвращает "грязное" или "незавершённое" чтение данных другими транзакциями

        LockModeType.PESSIMISTIC_WRITE - Пессимистичная блокировка на запись. Этот тип блокировки запрещает другим транзакциям как читать, так и изменять данные, пока текущая транзакция не завершится. Используется в тех случаях, когда важно предотвратить любое изменение данных другими пользователями или процессами

        LockModeType.PESSIMISTIC_FORCE_INCREMENT - Это пессимистичная блокировка с принудительным увеличением версии сущности. Когда транзакция завершится, версия объекта будет увеличена, независимо от того, было ли изменение. Полезно, если требуется заблокировать объект на запись и одновременно обеспечить, что версия объекта изменится

        LockModeType.READ - В JPA этот тип используется для оптимистичной блокировки при чтении данных, аналогичен OPTIMISTIC. Он используется в случаях, когда вы хотите прочитать объект и убедиться, что другой процесс не обновил его, пока вы работаете с ним

        LockModeType.WRITE - Этот тип блокировки используется в случаях пессимистичной блокировки для записи. Объект блокируется для других транзакций при записи
    */


    List<User> users = query.getResultList(); // Выполняет запрос и возвращает список результатов
    User user = query.getSingleResult(); // Выполняет запрос и возвращает один результат. Если результатов больше одного, выбрасывается исключение. Если результат отсутствует, может быть возвращён null (в зависимости от настроек)
    Optional<User> userOptional = query.uniqueResultOptional(); // Возвращает объект в виде Optional. Полезно для работы с возможными null результатами
    Stream<User> userStream = query.getResultStream(); // Возвращает результаты запроса в виде стрима (Stream). Полезно для работы с большими наборами данных и ленивой загрузки

    int result = query.executeUpdate();  // Используется для выполнения запросов на обновление или удаление данных. Возвращает количество затронутых строк

    NativeQuery<User> nativeQuery = query.unwrap(NativeQuery.class); // Позволяет "развернуть" объект Query в низкоуровневую реализацию, например, для получения доступа к специфичным для провайдера возможностям
```
