
# Hibernate

```plantext
    В данном файле собрана информация про Hibernate
```

- [Hibernate](#hibernate)
  - [Hibernate основная информация](#hibernate-основная-информация)
  - [Подключение Hibernate](#подключение-hibernate)
    - [Подключение через Maven](#подключение-через-maven)
  - [Конфигурация через hibernate.cfg.xml](#конфигурация-через-hibernatecfgxml)

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
        <version>6.6.0.Final</version>
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
