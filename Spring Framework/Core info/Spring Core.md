# Spring Core

```plantext
    Spring Core — это основа всего Spring Framework. Он предоставляет базовую инфраструктуру для создания приложений с использованием принципов инверсии управления (IoC) и внедрения зависимостей (DI). Основная задача Spring Core — управлять жизненным циклом объектов (бинов) и их зависимостями, помогая разработчикам сосредоточиться на логике приложения, не беспокоясь о "склеивании" компонентов
```

- [Spring Core](#spring-core)
  - [Основные компоненты Spring Core](#основные-компоненты-spring-core)
    - [BeanDefinitionReader](#beandefinitionreader)
    - [BeanFactoryPostProcessor](#beanfactorypostprocessor)
    - [BeanPostProcessor](#beanpostprocessor)
    - [ContextListener](#contextlistener)
    - [ClassPathBeanDefinitionScanner](#classpathbeandefinitionscanner)
    - [BeanFactory](#beanfactory)

## Основные компоненты Spring Core

Все эти классы играют разные роли на различных этапах жизненного цикла бина в Spring:

- **`BeanDefinitionReader`** — отвечает за чтение конфигурации бинов и создание их метаданных.
- **`BeanFactoryPostProcessor`** — позволяет модифицировать метаданные бинов перед их созданием.
- **`BeanPostProcessor`** — вмешивается в процесс создания бинов, изменяя их состояние до и после инициализации.
- **`ContextListener`** — реагирует на события контекста приложения.
- **`ClassPathBeanDefinitionScanner`** — автоматически находит и регистрирует бины на основе аннотаций.
- **`BeanFactory`** — основной контейнер, управляющий созданием и жизненным циклом бинов.

Вы можете внедрять свои классы для расширения поведения Spring на каждом из этих этапов, создавая собственные реализации `BeanFactoryPostProcessor`, `BeanPostProcessor`, `ApplicationListener` и других компонентов.

### BeanDefinitionReader

**Назначение:**

`BeanDefinitionReader` — это интерфейс, который отвечает за чтение метаданных бинов из различных источников конфигурации (например, XML-файлов, аннотаций и т.д.) и их регистрацию в Spring IoC контейнере в виде `BeanDefinition` объектов. Эти объекты содержат полное описание бина, его класс, зависимости, методы инициализации и уничтожения.

**Основные реализации:**

- `XmlBeanDefinitionReader` — считывает конфигурацию бинов из XML-файлов.
- `AnnotatedBeanDefinitionReader` — считывает конфигурацию, основанную на аннотациях.
- `PropertiesBeanDefinitionReader` — используется для конфигурации бинов через файлы свойств (properties).

**Этап создания бина:**  

`BeanDefinitionReader` участвует на самом начальном этапе, когда происходит загрузка конфигурации и регистрация метаданных бинов. Это один из первых шагов в жизненном цикле контейнера.

**Расширение:**  
Вы можете реализовать свой `BeanDefinitionReader`, если хотите добавить поддержку нестандартных форматов конфигурации.

### BeanFactoryPostProcessor

**Назначение:**

`BeanFactoryPostProcessor` — это интерфейс, который позволяет изменять метаданные всех бинов (их `BeanDefinition`) перед тем, как контейнер создаст экземпляры этих бинов. Это мощный инструмент для вмешательства в процесс конфигурации бинов.

**Пример использования:**

Можно изменить значение некоторых свойств бина, динамически добавлять новые бины или изменять зависимости до того, как они будут инициализированы.

**Основные реализации:**

- `PropertySourcesPlaceholderConfigurer` — популярная реализация, которая подставляет значения свойств из внешних источников (например, `application.properties`) в конфигурацию бинов.
  
**Этап создания бина:**  
Работает после того, как контейнер прочитал конфигурацию и создал объекты `BeanDefinition`, но до создания самих бинов.

**Расширение:**

Вы можете внедрить собственный `BeanFactoryPostProcessor`, чтобы модифицировать или настраивать бины до их создания. Для этого достаточно создать класс, который реализует этот интерфейс, и зарегистрировать его в контексте.

Пример:

```java
@Component
public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) {
        BeanDefinition beanDefinition = beanFactory.getBeanDefinition("myBean");
        beanDefinition.getPropertyValues().add("name", "ChangedName");
    }
}
```

### BeanPostProcessor

**Назначение:**  

`BeanPostProcessor` — это интерфейс, который позволяет вмешиваться в жизненный цикл каждого бина сразу после его создания, но до его инициализации, а также после инициализации.

**Основные методы:**

- `postProcessBeforeInitialization(Object bean, String beanName)` — вызывается до метода инициализации бина.
- `postProcessAfterInitialization(Object bean, String beanName)` — вызывается после метода инициализации.

**Пример использования:**

Можно использовать для автоматического проксирования бинов, внедрения аспектов или изменения состояния объекта перед его использованием.

**Этап создания бина:**

Работает после создания бина, но до вызова методов инициализации (`@PostConstruct`, `init-method`).

**Расширение:**

Вы можете создать собственный `BeanPostProcessor`, чтобы модифицировать бины после их создания. Это может быть полезно для кросс-сквозной логики (AOP) или для динамического внедрения логики в бины.

Пример:

```java
@Component
public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) {
        System.out.println("Before initialization of bean: " + beanName);
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        System.out.println("After initialization of bean: " + beanName);
        return bean;
    }
}
```

### ContextListener

**Назначение:**

Контекстные слушатели (`ApplicationListener`) используются для обработки событий в контексте приложения. С их помощью можно реагировать на события, такие как запуск контекста, его завершение или кастомные события, генерируемые внутри приложения.

**Основные реализации:**

- `ContextRefreshedEvent` — событие, которое возникает после полной инициализации контекста.
- `ContextClosedEvent` — событие при завершении контекста.

**Этап создания бина:**

Контекстные слушатели работают на этапе работы приложения, после того как бины были созданы и инициализированы. Они обрабатывают события, возникающие во время работы приложения.

**Расширение:**

Для добавления своих обработчиков событий вы можете реализовать интерфейс `ApplicationListener` и подписаться на нужные события.

Пример:

```java
@Component
public class MyContextListener implements ApplicationListener<ContextRefreshedEvent> {
    @Override
    public void onApplicationEvent(ContextRefreshedEvent event) {
        System.out.println("Context refreshed!");
    }
}
```

### ClassPathBeanDefinitionScanner

**Назначение:**  
`ClassPathBeanDefinitionScanner` — это класс, который сканирует указанные пакеты и автоматически регистрирует бины, аннотированные такими аннотациями, как `@Component`, `@Service`, `@Repository`, `@Controller`.

**Этап создания бина:**

Работает на этапе конфигурации, до создания бинов. Он автоматически находит и регистрирует классы как бины, основываясь на их аннотациях.

**Расширение:**

Вы можете использовать `ClassPathBeanDefinitionScanner`, если хотите сканировать кастомные пакеты в своем приложении.

Пример использования в Java-конфигурации:

```java
@Configuration
public class AppConfig {
    @Bean
    public ClassPathBeanDefinitionScanner scanner() {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(context);
        scanner.scan("com.example"); // Сканируем пакет com.example
        return scanner;
    }
}
```

### BeanFactory

**Назначение:**

`BeanFactory` — это основной интерфейс Spring IoC контейнера, который отвечает за создание, хранение и выдачу бинов по запросу. `ApplicationContext` расширяет `BeanFactory` и добавляет дополнительные функции (например, поддержку событий, AOP и т.д.).

**Основные реализации:**

- `DefaultListableBeanFactory` — одна из самых распространенных реализаций `BeanFactory`, которая используется внутри `ApplicationContext`.
  
**Этап создания бина:**

`BeanFactory` управляет созданием бинов, внедрением зависимостей, инициализацией и уничтожением бинов. Все операции происходят внутри этого контейнера.

**Расширение:**

Можно расширить поведение `BeanFactory`, используя `BeanFactoryPostProcessor`, для изменения метаданных бинов или написания собственного `BeanPostProcessor` для изменения состояния бинов после их создания.
