# Spring Cloud

```plantext
    Spring Cloud — это набор инструментов и библиотек для построения распределенных систем и микросервисной архитектуры на базе фреймворка Spring. Основная цель Spring Cloud — помочь разработчикам создавать масштабируемые и устойчивые микросервисы, которые могут взаимодействовать друг с другом, управляться и конфигурироваться централизованно
```

- [Spring Cloud](#spring-cloud)
  - [Зачем нужен Spring Cloud?](#зачем-нужен-spring-cloud)
  - [Основные функции и задачи Spring Cloud](#основные-функции-и-задачи-spring-cloud)
  - [Основные зависимости (артефакты) Spring Cloud](#основные-зависимости-артефакты-spring-cloud)
  - [Eureka Server](#eureka-server)
    - [Зачем нужен Eureka Server?](#зачем-нужен-eureka-server)
    - [Добавление зависимости Maven](#добавление-зависимости-maven)
    - [Использование @EnableEurekaServer](#использование-enableeurekaserver)
    - [Настройка application.properties для Eureka Server](#настройка-applicationproperties-для-eureka-server)
      - [Основные настройки для Eureka Server](#основные-настройки-для-eureka-server)
      - [Настройка логирования для Eureka Server](#настройка-логирования-для-eureka-server)
    - [Запуск Eureka Server](#запуск-eureka-server)
      - [API Eureka Server](#api-eureka-server)
    - [Регистрация сервиса в Eureka?](#регистрация-сервиса-в-eureka)
  - [Eureka Client](#eureka-client)
    - [Подключение Eureka Client](#подключение-eureka-client)
    - [Настройка application.properties для Eureka Client](#настройка-applicationproperties-для-eureka-client)
    - [Использование @EnableEurekaClient](#использование-enableeurekaclient)
  - [Spring Cloud Gateway](#spring-cloud-gateway)
    - [Подключение зависимостей для Cloud Gateway](#подключение-зависимостей-для-cloud-gateway)
    - [Настройка application.properties Cloud Gateway](#настройка-applicationproperties-cloud-gateway)
    - [Настройка маршрутов в Cloud Gateway](#настройка-маршрутов-в-cloud-gateway)
      - [Основные предикаты (Predicates)](#основные-предикаты-predicates)
      - [Основные фильтры в Spring Cloud Gateway](#основные-фильтры-в-spring-cloud-gateway)
  - [Load Balancer в API Gateway](#load-balancer-в-api-gateway)
    - [Типы Load Balancer](#типы-load-balancer)
    - [Алгоритмы балансировки нагрузки](#алгоритмы-балансировки-нагрузки)
    - [Использование Load Balancer в Spring Cloud](#использование-load-balancer-в-spring-cloud)
      - [Настройка балансировщика в Spring Cloud Gateway](#настройка-балансировщика-в-spring-cloud-gateway)
  - [Spring Cloud Config Server](#spring-cloud-config-server)
    - [Подключение Spring Cloud Config Server](#подключение-spring-cloud-config-server)
    - [Основные настройки для Config Server](#основные-настройки-для-config-server)
    - [Использование аннотации @EnableConfigServer](#использование-аннотации-enableconfigserver)
  - [Spring Cloud Config Client](#spring-cloud-config-client)
    - [Подключение Spring Cloud Config Client](#подключение-spring-cloud-config-client)
    - [Настройка приложения клиента](#настройка-приложения-клиента)

## Зачем нужен Spring Cloud?

1. **Масштабируемость**: Spring Cloud помогает легко создавать масштабируемые приложения, где можно добавлять или убирать инстансы микросервисов в зависимости от нагрузки.

2. **Отказоустойчивость**: Инструменты Spring Cloud обеспечивают отказоустойчивость системы за счет резервирования, автоматического восстановления и применения паттернов вроде "Circuit Breaker".

3. **Централизованное управление**: С помощью Spring Cloud Config разработчики могут централизованно управлять конфигурациями всех микросервисов, что упрощает их поддержку и обновление.

4. **Упрощение интеграции**: Благодаря компонентам для обнаружения сервисов и балансировки нагрузки микросервисы могут легко взаимодействовать друг с другом, не требуя статической настройки адресов.

5. **Снижение сложности инфраструктуры**: Spring Cloud помогает решать типичные проблемы микросервисных архитектур, такие как распределенная конфигурация, маршрутизация, отказоустойчивость, трассировка и взаимодействие между сервисами.

Таким образом, Spring Cloud — это ключевой компонент для разработки современных, масштабируемых и отказоустойчивых микросервисных приложений, особенно в среде облачных вычислений.

Spring Cloud включает множество библиотек и инструментов, предназначенных для решения различных задач, таких как управление конфигурацией, балансировка нагрузки, обнаружение сервисов, отказоустойчивость и т.д. Он интегрируется с другими инструментами экосистемы Spring и внешними сервисами, такими как Netflix OSS, Consul, и Kubernetes.

## Основные функции и задачи Spring Cloud

1. **Управление конфигурацией**
   - Используется для централизованного управления конфигурацией всех микросервисов. Это позволяет изменять конфигурации без необходимости перезапускать сервисы.
   - **Spring Cloud Config** — модуль для централизованного хранения и управления конфигурациями.

2. **Обнаружение сервисов**:
   - Позволяет микросервисам находить друг друга без необходимости знать их точные адреса. Это реализуется через регистрацию сервисов в реестре.
   - **Spring Cloud Netflix Eureka** — решение для реализации реестра сервисов и их обнаружения.

3. **Балансировка нагрузки**:
   - Помогает распределять запросы между несколькими инстансами одного сервиса для повышения отказоустойчивости и масштабируемости.
   - **Spring Cloud Netflix Ribbon** — библиотека для клиентской балансировки нагрузки.

4. **Гейтуэй (API Gateway)**:
   - Позволяет управлять запросами, поступающими на микросервисы, выполнять маршрутизацию и обеспечивать безопасность.
   - **Spring Cloud Gateway** — современный модуль для маршрутизации запросов и балансировки нагрузки.

5. **Отказоустойчивость и обработка сбоев**:
   - Включает механизмы резервирования и автоматического восстановления в случае сбоев сервисов.
   - **Spring Cloud Netflix Hystrix** (или Resilience4j в более современных решениях) — библиотека для обработки сбоев, предоставляющая паттерны "Circuit Breaker" (предохранитель), "Bulkhead" и т.д.

6. **Трассировка запросов (Distributed Tracing)**:
   - Позволяет отслеживать цепочки вызовов между микросервисами и анализировать, где возникают задержки или проблемы.
   - **Spring Cloud Sleuth** — интеграция с системами распределенной трассировки, такими как Zipkin.

7. **Шина сообщений (Message Bus)**:
   - Обеспечивает межсервисное взаимодействие через обмен сообщениями и событийную архитектуру.
   - **Spring Cloud Bus** — используется для передачи событий между сервисами через брокеры сообщений (Kafka, RabbitMQ).

## Основные зависимости (артефакты) Spring Cloud

- **spring-cloud-starter-config** — стартер для работы с Spring Cloud Config, который обеспечивает централизованное управление конфигурацией.
- **spring-cloud-starter-netflix-eureka-client** — клиент для работы с Eureka (реестром сервисов).
- **spring-cloud-starter-netflix-eureka-server** — сервер для регистрации и обнаружения сервисов (Eureka).
- **spring-cloud-starter-netflix-ribbon** — балансировка нагрузки с помощью Ribbon.
- **spring-cloud-starter-netflix-hystrix** — для реализации отказоустойчивости с помощью Hystrix.
- **spring-cloud-starter-gateway** — стартер для реализации API Gateway.
- **spring-cloud-starter-sleuth** — стартер для распределенной трассировки.
- **spring-cloud-starter-bus** — стартер для реализации шины событий.
- **spring-cloud-starter-stream-kafka** — стартер для использования Kafka в качестве брокера сообщений.
- **spring-cloud-starter-zipkin** — интеграция с Zipkin для распределенной трассировки.

## Eureka Server

**Eureka Server** — это центральный компонент системы сервисного реестра в Spring Cloud. Он используется для **обнаружения сервисов** (Service Discovery) в распределенной системе, что особенно важно при построении микросервисной архитектуры.

Eureka Server позволяет сервисам регистрироваться и обнаруживать друг друга динамически, что делает его важным элементом для построения отказоустойчивых и масштабируемых приложений. Вместо того чтобы жестко прописывать адреса сервисов, каждый сервис регистрируется в реестре Eureka и сообщает о своем местоположении. Другие сервисы могут найти нужный сервис через этот реестр и взаимодействовать с ним.

### Зачем нужен Eureka Server?

1. **Регистрация сервисов**: Сервисы, участвующие в микросервисной архитектуре, автоматически регистрируются на Eureka Server и сообщают ему свою информацию (например, URL, порт).
2. **Обнаружение сервисов**: Другие сервисы могут находить зарегистрированные сервисы через Eureka Server. Это особенно полезно, когда количество инстансов одного сервиса изменяется динамически (например, при масштабировании).
3. **Отказоустойчивость**: Если один инстанс сервиса выходит из строя, система сможет найти другой инстанс сервиса через реестр.
4. **Централизованное управление**: Eureka Server выполняет роль централизованного узла, где хранится информация о всех активных сервисах.

### Добавление зависимости Maven

Для начала создадим Spring Boot проект и добавим в него зависимость **Eureka Server**. Это можно сделать с помощью Maven, добавив следующую зависимость в `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

Также рекомендуется подключить Spring Cloud BOM для управления версиями зависимостей.

**Spring Cloud BOM (Bill of Materials)** — это специальный механизм, который помогает управлять версиями зависимостей в проектах на базе Spring Cloud. BOM упрощает процесс управления версиями множества библиотек и стартеров, которые используются в приложении

Когда вы создаете приложение с использованием Spring Boot, проект уже наследуется от родительского POM Spring Boot, который содержит BOM для Spring Boot зависимостей. Это означает, что для всех Spring Boot модулей и библиотек версии управляются автоматически.

Но если вы начинаете использовать Spring Cloud, вам нужно подключить еще и Spring Cloud BOM, поскольку Spring Cloud содержит дополнительные зависимости, версии которых не управляются Spring Boot

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version> <!-- Укажите актуальную версию Spring Cloud -->
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### Использование @EnableEurekaServer

Теперь нужно включить функциональность Eureka Server. Это делается через аннотацию `@EnableEurekaServer` в главном классе приложения.

Пример:

```java
package com.example.eurekaserver;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

Аннотация `@EnableEurekaServer` активирует функциональность сервера Eureka, делая этот сервис сервером регистрации.

### Настройка application.properties для Eureka Server

После этого нужно настроить конфигурацию Eureka Server. Обычно для этого создается файл `application.yml` (или `application.properties`), в котором указываются основные параметры для Eureka Server.

Пример конфигурации `application.yml` для Eureka Server:

#### Основные настройки для Eureka Server

```properties
    # Настройки самого сервера

    # Порт, на котором будет запущен Eureka Server (обычно 8761)
    server.port=8761

    # Отключение регистрации самого Eureka сервера как клиента.
    # Это важно для случаев, когда сервер является автономным (он не должен регистрироваться сам в себе).
    eureka.client.register-with-eureka=false
    eureka.client.fetch-registry=false

    # Таймаут для синхронизации с другими Eureka серверами при пустом реестре.
    # Указывается в миллисекундах. Значение 0 означает, что нет задержки.
    eureka.server.wait-time-in-ms-when-sync-empty=0

    # Настройки для клиентов, которые подключаются к Eureka

    # URL для подключения клиентов к Eureka Server.
    # "defaultZone" указывает основной URL для регистрации клиентов. Обычно это URL самого сервера.
    eureka.client.service-url.defaultZone=http://localhost:8761/eureka/

    # Настройки для управления регистрацией клиентов

    # Интервал (в миллисекундах), с которым клиент отправляет heartbeat-сообщения на сервер.
    # Эти сообщения подтверждают, что клиент активен.
    eureka.instance.lease-renewal-interval-in-seconds=30

    # Время (в секундах), через которое инстанс считается недоступным,
    # если от него не поступают heartbeat-сообщения.
    eureka.instance.lease-expiration-duration-in-seconds=90

    # Имя приложения для регистрации клиента в Eureka (для клиента)
    spring.application.name=my-service

    # Уникальный идентификатор инстанса клиента.
    # По умолчанию формируется на основе имени хоста, имени приложения и порта.
    eureka.instance.instance-id=${spring.application.name}:${random.value}

    # Включение меток времени при регистрации клиента.
    # Полезно для устранения ошибок и диагностики проблем с регистрацией.
    eureka.instance.metadata-map.startup-time=${random.int}

    # Указание статуса по умолчанию для клиента при старте.
    # Например, "UP" означает, что инстанс считается активным.
    eureka.instance.initial-status=UP

    # Включение/отключение использования IP-адреса в качестве идентификатора инстанса.
    # Полезно при развертывании в облачной среде, где IP-адрес может меняться.
    eureka.instance.prefer-ip-address=false

    # Настройки балансировки и отказоустойчивости для Eureka Server

    # Включение "self-preservation mode" — режима самосохранения, который предотвращает удаление клиентов,
    # если возникают проблемы с сетью. По умолчанию включен для повышения устойчивости системы.
    eureka.server.enable-self-preservation=true

    # Порог для режима самосохранения (self-preservation mode).
    # Определяет, какой процент клиентов должен отправлять heartbeat,
    # чтобы самосохранение было активировано. Обычно это значение 85%.
    eureka.server.renewal-percent-threshold=0.85

    # Интервал (в секундах), с которым сервер пересматривает состояния клиентов
    # для удаления неактивных инстансов.
    eureka.server.eviction-interval-timer-in-ms=60000

    # Настройки каскадной репликации (если есть несколько Eureka серверов)

    # Включение репликации между несколькими серверами Eureka.
    # Обычно необходимо, если у вас есть несколько инстансов Eureka Server.
    eureka.server.enable-replication=true

    # Таймауты и настройки сетевых взаимодействий

    # Максимальное время ожидания (в миллисекундах) для попытки регистрации клиента в Eureka.
    eureka.client.registration-timeout-in-seconds=30

    # Максимальное количество попыток подключения клиента к серверу.
    # Если указано значение 0, клиент попытается подключиться бесконечно.
    eureka.client.max-attempts=5

    # Включение кэширования реестра на стороне клиента для улучшения производительности.
    eureka.client.registry-fetch-interval-seconds=30

    # Минимальный интервал в миллисекундах между обновлениями кэшированных данных реестра.
    eureka.client.cache-refresh-interval-ms=30000

    # Настройки безопасности и авторизации (если требуется)

    # Включение базовой аутентификации для клиентов, которые обращаются к Eureka Server.
    eureka.client.service-url.defaultZone=http://user:password@localhost:8761/eureka/

    # Дополнительные параметры для клиента

    # Отключение обновления реестра клиентов, если они отключены (например, находятся в процессе завершения).
    eureka.client.should-unregister-on-shutdown=true

    # Отключение поиска других серверов Eureka (например, если ваш сервер автономен).
    eureka.client.should-fetch-registry=false
```

#### Настройка логирования для Eureka Server

```properties
    # Уровень логирования по умолчанию для всего приложения
    logging.level.root=INFO

    # Логирование для компонентов Eureka Server
    # Логирование запросов регистрации/обнаружения клиентов (Eureka Server)
    logging.level.com.netflix.eureka=DEBUG

    # Логирование взаимодействий с клиентами (например, DiscoveryClient)
    logging.level.com.netflix.discovery=DEBUG

    # Логирование взаимодействия Spring Cloud с Eureka
    logging.level.org.springframework.cloud.netflix.eureka=DEBUG
```

### Запуск Eureka Server

После настройки можно запустить приложение как обычное Spring Boot приложение. Eureka Server будет запущен и станет доступен по адресу `http://localhost:8761` (если вы не изменили порт).

Когда сервер будет запущен, на веб-интерфейсе можно будет увидеть страницу с информацией о зарегистрированных сервисах.

#### API Eureka Server

**API Eureka Server** предоставляет набор REST-эндпоинтов, с помощью которых клиенты могут регистрироваться, обновлять свое состояние, удалять свои записи и находить другие сервисы. Эти эндпоинты реализуют все основные функции, которые необходимы для взаимодействия между Eureka Server и клиентами (микросервисами).

API Eureka Server предоставляет мощные возможности для управления микросервисами в распределенной системе. Клиенты могут использовать этот API для регистрации, обновления статуса, получения списка сервисов и удаления своих записей. Этот подход позволяет динамически управлять микросервисами, обеспечивая высокую отказоустойчивость и масштабируемость приложений

Давайте подробнее рассмотрим основные части API Eureka Server, которые доступны по пути `/eureka/`:

```bash
    # Регистрация сервиса (Register Service)
    POST http://localhost:8761/eureka/apps/{applicationName} # Когда клиент-сервис запускается, он должен зарегистрироваться в Eureka Server, чтобы его можно было обнаружить другими сервисами

    # Получение списка всех зарегистрированных приложений (Fetch Registered Services)
    GET http://localhost:8761/eureka/apps # Этот запрос позволяет получить список всех зарегистрированных сервисов и их экземпляров

    # Получение информации о конкретном приложении (Fetch Specific Service)
    GET http://localhost:8761/eureka/apps/{applicationName} # Этот эндпоинт возвращает информацию о всех инстансах конкретного приложения

    # Поддержание "живого" статуса (Send Heartbeat)
    PUT http://localhost:8761/eureka/apps/{applicationName}/{instanceId} # Этот запрос обновляет статус сервиса в реестре, подтверждая, что он работает (heartbeat). Обычно клиенты делают этот запрос автоматически через определенные интервалы времени

    # Изменение статуса сервиса (Update Instance Status)
    PUT http://localhost:8761/eureka/apps/{applicationName}/{instanceId}/status?value={newStatus} # С помощью этого эндпоинта можно обновить статус инстанса сервиса, например, перевести его в режим "DOWN" или "OUT_OF_SERVICE"

    # Удаление сервиса из реестра (Deregister Service)
    DELETE http://localhost:8761/eureka/apps/{applicationName}/{instanceId} # Когда сервис завершает свою работу, он должен удалить себя из реестра Eureka, чтобы другие сервисы не пытались с ним взаимодействовать

    # Получение списка всех активных инстансов конкретного сервиса (Fetch Active Instances)
    GET http://localhost:8761/eureka/apps/{applicationName} # Этот эндпоинт возвращает список активных инстансов (инстансов со статусом UP) для конкретного приложения

    # Получение информации о конкретном инстансе (Fetch Specific Instance)
    GET http://localhost:8761/eureka/apps/{applicationName}/{instanceId} # Этот эндпоинт позволяет получить информацию о конкретном инстансе сервиса по его ID

    # Получение списка всех инстансов по IP-адресу (Fetch Instances by IP Address)
    GET http://localhost:8761/eureka/instances/{instanceId} # Этот эндпоинт позволяет получить список всех инстансов сервисов, зарегистрированных под определенным IP-адресом
```

**instanceId** — это уникальный идентификатор для каждого экземпляра (инстанса) микросервиса, зарегистрированного в Eureka. По умолчанию, если вы не указываете instanceId, Eureka сгенерирует его автоматически на основе определённых данных вашего приложения. Для локальных и простых развёртываний, часто instanceId генерируется по шаблону: `{hostname}:{applicationName}:{port}`

Если вы хотите установить свой уникальный instanceId, вы можете это сделать в конфигурационном файле:

```properties
eureka.instance.instance-id=${spring.application.name}:${random.value} # задание  своего instanceId по шаблону: {applicationName}:{random val}
eureka.instance.instance-id=custom-instance-id # указание уникального значения идентификатора
```

### Регистрация сервиса в Eureka?

Чтобы микросервисы могли регистрироваться в Eureka Server, в них необходимо подключить соответствующую зависимость и настроить конфигурацию.

1. Добавьте зависимость **Eureka Client** в проект микросервиса:

    ```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    ```

2. Аннотируйте основной класс приложения с помощью `@EnableEurekaClient`:

    ```java
    @SpringBootApplication
    @EnableEurekaClient
    public class YourMicroserviceApplication {
        public static void main(String[] args) {
            SpringApplication.run(YourMicroserviceApplication.class, args);
        }
    }
    ```

3. Настройте конфигурацию для клиента:

    Пример `application.yml` для клиента:

    ```properties
    eureka.client.service-url.defaultZone=http://localhost:8761/eureka/  # Адрес вашего Eureka Server
    ```

Теперь этот микросервис будет автоматически регистрироваться на Eureka Server и доступен для других сервисов через механизм Service Discovery.

## Eureka Client

**Eureka Client** — это микросервис или любое другое приложение, которое регистрируется в Eureka Server для того, чтобы быть доступным другим сервисам и для взаимодействия с ними через механизм Service Discovery (обнаружение сервисов). Клиенты могут как регистрироваться в Eureka Server, так и находить другие сервисы, которые уже зарегистрированы в реестре

### Подключение Eureka Client

```xml
    <!-- Spring Boot Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Eureka Discovery Client -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>

    <!-- BOM для Spring Cloud для корректного управления версиями зависимостей -->
    <dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>2023.0.3</version> <!-- Версия Spring Cloud -->
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### Настройка application.properties для Eureka Client

```properties
    # Если указать порт 0, то приложение будет искать свободный порт автоматически и будте запущено на нем
    server.port=0 

    # Имя приложения, под которым оно будет зарегистрировано в Eureka Server
    spring.application.name=eureka-client

    # Адрес Eureka Server, к которому будет подключаться клиент
    eureka.client.service-url.defaultZone=http://localhost:8761/eureka/

    # Использовать IP-адрес вместо имени хоста
    eureka.instance.prefer-ip-address=true
```

### Использование @EnableEurekaClient

Для того чтобы ваше приложение стало Eureka Client, нужно добавить аннотацию `@EnableEurekaClient` в главный класс приложения. Это активирует функциональность Eureka Client, позволяя вашему приложению регистрироваться на Eureka Server

```java
    package com.example.eurekaclient;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

    @SpringBootApplication
    @EnableEurekaClient  // Включаем Eureka Client
    public class EurekaClientApplication {
        public static void main(String[] args) {
            SpringApplication.run(EurekaClientApplication.class, args);
        }
    }
```

## Spring Cloud Gateway

**Spring Cloud Gateway** — это проект, предназначенный для маршрутизации запросов в распределенной системе (обычно в микросервисной архитектуре). Это альтернатива таким решениям, как Netflix Zuul, и используется в качестве [API Gateway](./Microservices%20Base.md#api-gateway-шлюз-api), предоставляя единый вход для всех запросов, идущих к различным микросервисам

### Подключение зависимостей для Cloud Gateway

```xml
    <dependencies>
        <!-- Зависимость для Spring Cloud Gateway -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>

        <!-- Зависимость для WebFlux (Spring Web) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webflux</artifactId>
        </dependency>

        <!-- Эврика клиент, для подключения сервиса к общему Эврика серверу -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <!-- Spring Cloud BOM (Bill of Materials) -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>2023.0.3</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### Настройка application.properties Cloud Gateway

```properties
    spring.application.name=api-gateway

    # Порт, который обычно используют для API Gateway
    server.port=8765

    eureka.client.service-url.defaultZone=http://localhost:8761/eureka

    # Настройка указывает Spring Cloud Gateway автоматически находить зарегистрированные в Eureka (или другом сервисе) сервисы и динамически маршрутизировать запросы к ним
    spring.cloud.gateway.discovery.locator.enabled=true

    # Позволяет игнорировать регистр в названиях сервисов
    spring.cloud.gateway.discovery.locator.lower-case-service-id=true
```

### Настройка маршрутов в Cloud Gateway

**Основная структура маршрутов:**

```properties
    # Уникальный идентификатор маршрута
    spring.cloud.gateway.routes[0].id=route_1

    # Адрес назначения, куда будет направлен запрос. Это может быть URL (например, http://localhost:8081) или имя сервиса в системе обнаружения (например, lb://service2 для балансировки нагрузки)
    spring.cloud.gateway.routes[0].uri=http://localhost:8081

    #  Условия маршрутизации (предикаты) для фильтрации запросов, такие как Path, Method, Header и другие. Это условия, которые должны быть выполнены, чтобы запрос был направлен на конкретный маршрут
    spring.cloud.gateway.routes[0].predicates[0]=Path=/service1/**

    # Дополнительные фильтры, которые могут изменять запросы или ответы
    spring.cloud.gateway.routes[0].filters[0]=AddRequestHeader=Service, Service1
```

#### Основные предикаты (Predicates)

Предикаты определяют условия для маршрутизации запросов. Они помогают направлять запросы к разным микросервисам в зависимости от пути, метода, заголовков и других параметров

```properties
    # Path: Один из самых часто используемых предикатов, который позволяет маршрутизировать запросы в зависимости от пути URL. Поддерживает шаблоны, такие как /** для указания любых вложенных уровней
    # Пример: маршрутизация всех запросов, начинающихся с /service1/
    spring.cloud.gateway.routes[0].predicates[0]=Path=/service1/**

    # Method: Этот предикат ограничивает маршрут запросами с определенным HTTP-методом, например, GET, POST, DELETE и т.д.
    spring.cloud.gateway.routes[0].predicates[1]=Method=GET

    # Header: Маршрутизирует запросы в зависимости от наличия или значения заголовка. Это может быть полезно для фильтрации запросов, исходящих от определенных клиентов или устройств
    # Пример: маршрутизация запросов, которые содержат заголовок X-Request-Source со значением Mobile
    spring.cloud.gateway.routes[0].predicates[2]=Header=X-Request-Source, Mobile

    # Query: Позволяет маршрутизировать запросы в зависимости от значения параметров запроса (query parameters) в URL
    # Пример: маршрутизация запросов, которые содержат параметр clientId=12345 в URL
    spring.cloud.gateway.routes[0].predicates[3]=Query=clientId, 12345

    # Host: Ограничивает маршрут запросами, исходящими с определенного хоста. Поддерживает шаблоны для гибкой настройки
    # Пример: маршрутизация запросов, хост которых заканчивается на .mydomain.com
    spring.cloud.gateway.routes[0].predicates[4]=Host=**.mydomain.com

    # After: Маршрутизирует запросы, если они были отправлены после указанного времени. Время указывается в формате ISO-8601 с указанием временной зоны
    # Пример: маршрутизация запросов, отправленных после 1 января 2023 года в UTC
    spring.cloud.gateway.routes[0].predicates[5]=After=2023-01-01T00:00:00Z[UTC]

    # Before: Маршрутизирует запросы, отправленные до указанного времени. Также используется формат ISO-8601
    # Пример: маршрутизация запросов, отправленных до 31 декабря 2023 года в UTC
    spring.cloud.gateway.routes[0].predicates[6]=Before=2023-12-31T23:59:59Z[UTC]

    # Between: Позволяет маршрутизировать запросы, если они были отправлены в указанный временной диапазон
    # Пример: маршрутизация запросов, отправленных между 1 января и 31 декабря 2023 года в UTC
    spring.cloud.gateway.routes[0].predicates[7]=Between=2023-01-01T00:00:00Z[UTC],2023-12-31T23:59:59Z[UTC]

    # Cookie: Этот предикат позволяет маршрутизировать запросы на основе значений cookie. Полезно для управления сессиями и персонализации
    # Пример: маршрутизация запросов, содержащих cookie с именем myCookie и значением cookieValue
    spring.cloud.gateway.routes[0].predicates[8]=Cookie=myCookie, cookieValue

    # RemoteAddr: Ограничивает доступ к маршруту по IP-адресу клиента. Поддерживает IP-адреса или диапазоны CIDR. Это полезно для ограничения доступа к маршруту из определенных сетей
    # Пример: маршрутизация запросов только с IP-адресов в диапазоне 192.168.1.0/24
    spring.cloud.gateway.routes[0].predicates[9]=RemoteAddr=192.168.1.0/24

    # CloudFoundryRouteService: Специфический предикат для интеграции с Cloud Foundry Route Services. Обычно не используется вне среды Cloud Foundry
    # Пример: интеграция с Cloud Foundry Route Service
    spring.cloud.gateway.routes[0].predicates[10]=CloudFoundryRouteService
```

#### Основные фильтры в Spring Cloud Gateway

Фильтры в Spring Cloud Gateway применяются к запросам и позволяют изменять их перед отправкой на целевой сервис или модифицировать ответ перед отправкой обратно клиенту. В отличие от предикатов, которые определяют условия маршрутизации, фильтры выполняют преобразования, добавляют данные или обрабатывают запросы/ответы.

Фильтры могут добавлять или удалять заголовки, переписывать URL, добавлять параметры и реализовывать паттерны отказоустойчивости (такие как Circuit Breaker, Retry). Они позволяют гибко управлять запросами и ответами, улучшая надежность, безопасность и контролируемость API Gateway

```properties
    # AddRequestHeader: Добавляет заголовок в запрос перед его отправкой на целевой сервис
    # Пример: добавление заголовка X-Request-Source с значением Internal в запрос
    spring.cloud.gateway.routes[0].filters[0]=AddRequestHeader=X-Request-Source, Internal

    # AddRequestParameter: Добавляет параметр к запросу. Полезно, если необходимо передать дополнительный параметр на целевой сервис
    # Пример: добавление параметра clientId=12345 к запросу
    spring.cloud.gateway.routes[0].filters[1]=AddRequestParameter=clientId, 12345

    # AddResponseHeader: Добавляет заголовок к ответу перед его отправкой клиенту. Это может быть полезно для добавления метаданных к ответу (например, информации о сервере или времени обработки)
    # Пример: добавление заголовка X-Response-Time с значением 100ms к ответу
    spring.cloud.gateway.routes[0].filters[2]=AddResponseHeader=X-Response-Time, 100ms

    # RemoveRequestHeader: Удаляет указанный заголовок из запроса. Полезно для удаления ненужных или конфиденциальных данных
    # Пример: удаление заголовка X-Unwanted-Header из запроса
    spring.cloud.gateway.routes[0].filters[3]=RemoveRequestHeader=X-Unwanted-Header 
   
    # RemoveResponseHeader: Удаляет указанный заголовок из ответа, что может быть полезно для скрытия внутренних данных от клиента
    # Пример: удаление заголовка X-Internal-Info из ответа
    spring.cloud.gateway.routes[0].filters[4]=RemoveResponseHeader=X-Internal-Info

    # RewritePath: Перезаписывает часть пути URL, направляя запрос на другой путь на целевом сервисе
    # Пример: перенаправляет запросы с /service1/xxx на /xxx
    spring.cloud.gateway.routes[0].filters[5]=RewritePath=/service1/(?<segment>.*), /${segment}

    # Hystrix: Реализует паттерн Circuit Breaker (предохранитель), предотвращая каскадные ошибки. Когда целевой сервис перегружен или не отвечает, Hystrix разрывает соединение и возвращает дефолтный ответ или выполняет альтернативную логику
    # Пример: настройка Hystrix для обработки сбоев на маршруте
    spring.cloud.gateway.routes[0].filters[6].name=Hystrix
    spring.cloud.gateway.routes[0].filters[6].args.name=myHystrixCommand

    # Retry: Фильтр, который повторяет запрос при ошибке. Можно задать количество попыток, HTTP-статусы, при которых будет выполняться повтор, и другие параметры
    # Пример: 3 повторные попытки при ошибках 500 и BAD_GATEWAY
    spring.cloud.gateway.routes[0].filters[7].name=Retry
    spring.cloud.gateway.routes[0].filters[7].args.retries=3
    spring.cloud.gateway.routes[0].filters[7].args.statuses=500,BAD_GATEWAY

    # RedirectTo: Перенаправляет запрос на другой URL с указанным HTTP-статусом. Полезно для настройки постоянных или временных редиректов
    # Пример: перенаправление всех запросов на http://new-url.com с кодом 302
    spring.cloud.gateway.routes[0].filters[8].name=RedirectTo
    spring.cloud.gateway.routes[0].filters[8].args.status=302
    spring.cloud.gateway.routes[0].filters[8].args.url=http://new-url.com

    # SetPath: Устанавливает путь запроса перед его отправкой на целевой сервис, заменяя его на указанный
    # Пример: заменяет путь запроса на /new-path
    spring.cloud.gateway.routes[0].filters[9]=SetPath=/new-path

    # SetRequestHeader: Устанавливает значение заголовка запроса. Если заголовок уже существует, его значение будет заменено
    # Пример: устанавливает заголовок X-My-Header с значением NewValue
    spring.cloud.gateway.routes[0].filters[10]=SetRequestHeader=X-My-Header, NewValue

    # SetResponseHeader: Устанавливает значение заголовка ответа, заменяя его значение, если заголовок уже существует
    # Пример: устанавливает заголовок X-Response-Header с значением MyResponseValue
    spring.cloud.gateway.routes[0].filters[11]=SetResponseHeader=X-Response-Header, MyResponseValue

    # RequestRateLimiter: Реализует ограничение частоты запросов (Rate Limiting), что полезно для защиты от перегрузки. Использует Redis как хранилище для отслеживания лимитов
    # Пример: ограничивает количество запросов до 10 в секунду с максимальным выбросом до 20 запросов
    spring.cloud.gateway.routes[0].filters[12].name=RequestRateLimiter
    spring.cloud.gateway.routes[0].filters[12].args.redis-rate-limiter.replenishRate=10
    spring.cloud.gateway.routes[0].filters[12].args.redis-rate-limiter.burstCapacity=20

    # DedupeResponseHeader: Удаляет дублирующиеся заголовки из ответа. Это полезно, если дублирующиеся заголовки могут вызвать проблемы
    # Пример: удаление дублирующихся значений для заголовка X-Unique-Header
    spring.cloud.gateway.routes[0].filters[13].name=DedupeResponseHeader
    spring.cloud.gateway.routes[0].filters[13].args.name=X-Unique-Header
    spring.cloud.gateway.routes[0].filters[13].args.strategy=RETAIN_FIRST
```

## Load Balancer в API Gateway

**Load Balancer** (балансировщик нагрузки) — это компонент системы, распределяющий входящие запросы между несколькими экземплярами сервисов, что позволяет улучшить производительность, устойчивость и отказоустойчивость приложения. В микросервисной архитектуре, где отдельный сервис может иметь несколько экземпляров (например, в контейнерах Docker или Kubernetes), балансировщик нагрузки помогает равномерно распределять запросы, избегая перегрузки одного из экземпляров и снижая вероятность простоев.

**Load Balancer** необходим для обеспечения эффективной работы микросервисов, улучшения производительности, распределения нагрузки и отказоустойчивости системы. Использование балансировщика позволяет системе масштабироваться, выдерживать высокие нагрузки и оставаться доступной пользователям даже при сбоях в отдельных компонентах.

**Задачи и преимущества Load Balancer:**

1. **Распределение нагрузки**:
   Балансировщик направляет запросы на разные экземпляры одного сервиса, распределяя трафик и позволяя эффективно использовать ресурсы. Это улучшает производительность приложения, так как нагрузка равномерно распределена между всеми доступными сервисами.

2. **Повышение отказоустойчивости**:
   Если один из экземпляров сервиса выходит из строя или временно недоступен, балансировщик перенаправляет запросы к доступным экземплярам, что минимизирует вероятность отказа системы в целом.

3. **Масштабируемость**:
   Балансировщик нагрузки упрощает горизонтальное масштабирование — добавление новых экземпляров сервиса в случае роста нагрузки. Когда добавляется новый экземпляр, балансировщик начинает автоматически направлять часть трафика на него.

4. **Оптимизация времени отклика**:
   Некоторые балансировщики учитывают географическое положение пользователей, пропускную способность и время отклика сервисов, чтобы направлять запросы на наиболее быстрый и доступный экземпляр, что улучшает пользовательский опыт.

### Типы Load Balancer

1. **Клиентский балансировщик (Client-Side Load Balancer)**:
   Балансировка нагрузки осуществляется на стороне клиента или сервисного клиента. Например, библиотека **Ribbon** в Spring Cloud или встроенный балансировщик **Spring Cloud LoadBalancer**. Клиент знает о всех доступных экземплярах сервиса и сам выбирает, к какому обращаться, используя различные алгоритмы.

   Преимущество этого подхода в том, что клиентская логика легко интегрируется в код приложения, однако для этого требуется, чтобы клиент получал актуальную информацию о доступных экземплярах сервиса (обычно через систему обнаружения сервисов, такую как **Eureka**).

2. **Серверный балансировщик (Server-Side Load Balancer)**:
   Балансировка нагрузки осуществляется на выделенном сервере или облачном сервисе, расположенном между клиентами и серверами приложений. Серверный балансировщик выступает в роли посредника, перенаправляя входящие запросы на разные экземпляры сервиса.

   Примеры: **Nginx**, **HAProxy**, балансировщики нагрузки от облачных провайдеров (AWS ELB, Google Cloud Load Balancer). Этот подход позволяет гибко управлять и масштабировать балансировку, но требует выделенных серверов или облачной инфраструктуры.

3. **DNS-балансировка**:
   При этом подходе несколько IP-адресов привязываются к одному доменному имени, и DNS-сервер поочередно или по определенному алгоритму возвращает разные IP-адреса клиентам. Например, **AWS Route 53** поддерживает балансировку на уровне DNS.

4. **Программный балансировщик (Software Load Balancer)**:
   Используется в средах контейнеров, таких как **Kubernetes**, где балансировщик встроен и управляет распределением нагрузки между контейнерами внутри кластера.

### Алгоритмы балансировки нагрузки

Балансировщики нагрузки используют различные алгоритмы для выбора оптимального экземпляра сервиса:

1. **Round Robin** (Циклический алгоритм): запросы отправляются последовательно к каждому экземпляру по кругу.
2. **Least Connections** (Минимальное количество соединений): запрос отправляется на экземпляр с наименьшим количеством активных соединений.
3. **IP Hash**: балансировщик отправляет запросы с одного и того же IP-адреса клиента на один и тот же экземпляр сервиса (поддержание сессии).
4. **Weighted Round Robin** (Взвешенный циклический алгоритм): экземплярам присваиваются веса, и балансировщик отправляет больше запросов на экземпляры с более высоким весом.
5. **Random** (Случайный выбор): случайный выбор экземпляра для каждого запроса.

### Использование Load Balancer в Spring Cloud

В Spring Cloud используется **Spring Cloud LoadBalancer** (замена Ribbon с версии Spring Cloud 2020). Он позволяет реализовать клиентскую балансировку нагрузки с использованием системы обнаружения сервисов (например, **Eureka**)

В **Spring Cloud Gateway** балансировщик нагрузки используется для маршрутизации запросов к нескольким экземплярам одного и того же сервиса. По умолчанию Spring Cloud Gateway использует Spring Cloud LoadBalancer

#### Настройка балансировщика в Spring Cloud Gateway

Если у вас уже есть настройка Service Discovery, например, через Eureka, Spring Cloud LoadBalancer автоматически распределяет запросы между экземплярами сервиса, зарегистрированными в Eureka. Настройка обычно требует только указания, что Spring Cloud Gateway должен использовать балансировщик

```properties
# Включение LoadBalancer в Spring Cloud Gateway
spring.cloud.loadbalancer.enabled=true
spring.cloud.gateway.discovery.locator.enabled=true
```

## Spring Cloud Config Server

`Spring Cloud Config Server` — это компонент в экосистеме Spring Cloud, предоставляющий централизованное управление конфигурацией приложений. Он поддерживает множество свойств для гибкой настройки, включая параметры безопасности, источников данных и кэширования. Основные настройки можно разделить на несколько категорий: настройка Git-репозитория, безопасность и аутентификация, кэширование, конфигурация серверных настроек, и расширенные параметры.

### Подключение Spring Cloud Config Server

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-server</artifactId>
    </dependency>
```

### Основные настройки для Config Server

```properties
    # Стандартный порт для сервера конфигурации
    server.port=8888

    # Настройка основного репозитория Git для хранения конфигураций
    spring.cloud.config.server.git.uri=https://github.com/your-org/your-config-repo
    # Клонировать репозиторий при запуске Config Server (уменьшает задержку при первом запросе)
    spring.cloud.config.server.git.cloneOnStart=true
    # Дирректория, в которую будет клонироваться репозиторий (!!!ДАННАЯ ПАПКА БУДЕТ ОЧИЩЕНА!!!)
    spring.cloud.config.server.git.basedir=/Users/dm/Desktop/git-config
    # Путь внутри репозитория, где хранятся конфигурационные файлы (если не в корне)
    spring.cloud.config.server.git.searchPaths=config
    # Ветка по умолчанию (например, main или master)
    spring.cloud.config.server.git.default-label=main
    # Включает форсированное обновление конфигурации при каждом запросе
    spring.cloud.config.server.git.force-pull=true
    # Интервал проверки изменений в репозитории (в секундах)
    spring.cloud.config.server.git.refresh-rate=30

    # Учетные данные для доступа к приватному Git-репозиторию (если требуется)
    spring.cloud.config.server.git.username=your-git-username
    spring.cloud.config.server.git.password=your-git-password
    # Путь к приватному ключу для SSH-доступа
    spring.cloud.config.server.git.private-key=classpath:/ssh/private_key.pem
    # Проверка ключа хоста при подключении через SSH (строгая проверка, рекомендована в production)
    spring.cloud.config.server.git.strict-host-key-checking=true

    # Настройки аутентификации для безопасности Config Server
    spring.security.user.name=configuser
    spring.security.user.password=securepassword
    # Если используется базовая аутентификация для ограниченного доступа к конфигурациям
    spring.security.basic.enabled=true

    # Параметры для Vault (если используется в качестве конфигурационного источника)
    spring.cloud.config.server.vault.uri=https://vault.example.com
    spring.cloud.config.server.vault.token=my-vault-token
    spring.cloud.config.server.vault.kv-version=2 # Версия ключ-значение для Vault

    # Настройки кэширования (оптимизация производительности для часто запрашиваемых данных)
    spring.cloud.config.server.cache.enabled=true
    # Время жизни кэша (TTL) в секундах
    spring.cloud.config.server.cache.expiration=60

    # Префикс для всех API-эндпоинтов Config Server
    spring.cloud.config.server.prefix=/config

    # Настройки для получения конфигурации из нескольких источников (composite pattern)
    spring.cloud.config.server.composite[0].type=git
    spring.cloud.config.server.composite[0].uri=https://github.com/your-org/another-config-repo
    spring.cloud.config.server.composite[0].searchPaths=config

    spring.cloud.config.server.composite[1].type=vault
    spring.cloud.config.server.composite[1].uri=https://vault.example.com
    spring.cloud.config.server.composite[1].token=my-vault-token

    # Настройки параметров переопределения
    spring.cloud.config.allowOverride=true    # Разрешить клиентам переопределять значения конфигурации
    spring.cloud.config.overrideNone=false    # Если true, запрещает переопределение значений, даже если allowOverride=true

    # Основные параметры для конфигураций по умолчанию
    spring.cloud.config.name=application         # Имя приложения по умолчанию
    spring.cloud.config.profile=default          # Профиль по умолчанию
    spring.cloud.config.label=main               # Ветка/лейбл по умолчанию

    # Дополнительные настройки мониторинга и состояния сервера
    management.endpoint.health.enabled=true      # Включить эндпоинт /health для проверки состояния
    management.endpoint.info.enabled=true        # Включить эндпоинт /info для получения информации о сервере
```

### Использование аннотации @EnableConfigServer

```java
    @SpringBootApplication
    @EnableConfigServer // Подключение аннотации
    public class GitConfigServerApplication {
        public static void main(String[] args) {
            SpringApplication.run(GitConfigServerApplication.class, args);
        }
    }
```

## Spring Cloud Config Client

`Spring Cloud Config Client` — это клиентская часть системы конфигурации Spring Cloud Config, которая позволяет приложению получать централизованную конфигурацию из удаленного хранилища конфигурации, управляемого сервером `Spring Cloud Config Server`

### Подключение Spring Cloud Config Client

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-config</artifactId>
    </dependency>
```

### Настройка приложения клиента

```properties
    # Имя приложения. Очень важно, чтобы файл конфигурации назывался {name}.properties
    spring.application.name=eureka-server

    # Адрес сервера конфигураций
    spring.config.import=optional:configserver:http://localhost:8888
    # Запрашиваемый профиль
    spring.profiles.active=default
```
