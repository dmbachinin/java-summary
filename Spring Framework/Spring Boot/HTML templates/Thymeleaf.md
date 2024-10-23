# Thymeleaf

```plantext
    Thymeleaf — современный серверный шаблонизатор для Java-приложений, который идеально интегрируется со Spring Boot. Он позволяет создавать динамические веб-страницы с чистым и понятным синтаксисом, поддерживая как HTML, так и другие форматы.
```

- [Thymeleaf](#thymeleaf)
  - [Добавление зависимостей в Spring Boot](#добавление-зависимостей-в-spring-boot)
    - [Добавление зависимости для Maven](#добавление-зависимости-для-maven)
  - [Настройка application.properties](#настройка-applicationproperties)
  - [Основные атрибуты Thymeleaf](#основные-атрибуты-thymeleaf)
  - [Формы и привязка данных](#формы-и-привязка-данных)
  - [Фрагменты Thymeleaf (Reusable Templates)](#фрагменты-thymeleaf-reusable-templates)
    - [Создание фрагмента](#создание-фрагмента)
    - [Вставка фрагмента в другие шаблоны](#вставка-фрагмента-в-другие-шаблоны)
    - [Передача параметров во фрагменты](#передача-параметров-во-фрагменты)

## Добавление зависимостей в Spring Boot

### Добавление зависимости для Maven

```xml
    <!-- Spring Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Thymeleaf -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
```

## Настройка application.properties

```properties
    # Префикс и суффикс для шаблонов Thymeleaf
    spring.thymeleaf.prefix=classpath:/templates/
    spring.thymeleaf.suffix=.html

    # Кэширование шаблонов (для разработки можно отключить)
    spring.thymeleaf.cache=false

    # Режим HTML
    spring.thymeleaf.mode=HTML5
```

## Основные атрибуты Thymeleaf

**Перед началом работы необходимо покдлючить библиотеку:**

```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Форма пользователя</title>
    </head>
    <body>
        <!-- ... -->
    </body>
```

**Основные атрибуты:**

```html
    <!-- th:text - Этот атрибут заменяет текст внутри элемента на значение из модели. Он используется для вывода текста в HTML-коде -->
    <h1 th:text="${message}">Текст по умолчанию</h1>
    
    <!-- th:if и th:unless - Эти атрибуты используются для условного отображения элементов на странице -->
    <!-- th:if — отображает элемент, если условие истинно (истина) -->
    <!-- th:unless — противоположен th:if: элемент отображается, если условие ложно (ложь) -->
    <p th:if="${user != null}">Вы вошли как <span th:text="${user.name}">Имя</span>.</p>
    <p th:unless="${user != null}">Вы не вошли в систему.</p>

    <!-- Вы можете писать сложные логические выражения внутри th:if и th:unless, используя стандартные операторы Java -->
    <p th:if="${user != null && user.role == 'ADMIN'}">Привет, Администратор!</p>
    <p th:if="${user != null && user.role == 'USER'}">Привет, Пользователь!</p>
    <p th:if="${user == null || user.role != 'ADMIN'}">Доступ ограничен.</p>
    
    <div th:unless="${user != null}"> <!-- В этом примере блок <div>...</div> будет отображаться, только если условие user != null выполнено -->
        <h1>Пожалуйста, войдите в систему</h1>
        <p>Чтобы получить доступ к сайту, вам необходимо авторизоваться.</p>
    </div>

    <!-- th:block Thymeleaf предоставляет специальный тег th:block, который не отображается в HTML, но позволяет вам группировать элементы для управления условиями -->
    <th:block th:if="${user != null}">
        <h1>Добро пожаловать, <span th:text="${user.name}">Пользователь</span>!</h1>
        <p>Ваш email: <span th:text="${user.email}">email@example.com</span></p>
    </th:block>

    <!-- th:each Этот атрибут позволяет итерировать по коллекциям объектов и отображать их. Это полезно для отображения списков -->
    <ul>
        <li th:each="user : ${users}">
            <span th:text="${user.name}">Имя</span> - <span th:text="${user.email}">Email</span>
        </li>
    </ul>

    <!-- th:href Динамическое создание ссылки -->
    <a th:href="@{/profile/{id}(id=${user.id})}">Профиль пользователя</a>

    <!-- th:src: динамическое создание пути для изображений -->
    <img th:src="@{/images/{imageName}(imageName=${image.name})}" alt="Изображение" />

    <!-- th:action: используется в формах для указания динамических путей при отправке данных -->
    <form th:action="@{/submit}" method="post">
        <!-- поля формы -->
    </form>
```

## Формы и привязка данных

**Форма для ввода данных пользователя:**

```java
    @GetMapping("/form")
    public String showForm(Model model) {
        model.addAttribute("userForm", new User());
        return "form";
    }

    @PostMapping("/submit")
    public String submitForm(@ModelAttribute("userForm") User user, Model model) {
        model.addAttribute("message", "Привет, " + user.getName() + "!");
        return "result";
    }
```

**Шаблон form.html:**

```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Форма пользователя</title>
    </head>
    <body>
        <h1>Заполните форму</h1>

        <form th:action="@{/submit}" th:object="${userForm}" method="post">
            <!-- th:object: указывает, к какому объекту привязана форма (в данном случае это userForm) -->
            <label for="name">Имя:</label>
            <input type="text" id="name" th:field="*{name}" /><br/>
            <!-- th:field: привязывает поля формы к конкретным свойствам объекта. Например, поле ввода с th:field="*{name}" привязано к свойству name объекта userForm -->

            <label for="email">Email:</label>
            <input type="email" id="email" th:field="*{email}" /><br/>

            <button type="submit">Отправить</button>
        </form>
    </body>
    </html>
```

## Фрагменты Thymeleaf (Reusable Templates)

Thymeleaf поддерживает фрагменты, что позволяет переиспользовать части кода (например, шапку или подвал) на нескольких страницах

### Создание фрагмента

```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <body>
        <div th:fragment="header">
            <header>
                <h1>Моя компания</h1>
                <nav>
                    <ul>
                        <li><a href="/">Главная</a></li>
                        <li><a href="/about">О нас</a></li>
                        <li><a href="/contact">Контакты</a></li>
                    </ul>
                </nav>
            </header>
        </div>
    </body>
    </html>

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <body>
        <div th:fragment="footer">
            <footer>
                <p>&copy; 2024 Моя компания</p>
            </footer>
        </div>
    </body>
    </html>
```

### Вставка фрагмента в другие шаблоны

Теперь мы можем включить этот фрагмент в другие страницы с помощью атрибута th:replace или th:insert

```html
    <!-- th:replace полностью заменяет текущий элемент содержимым фрагмента -->
    <div th:replace="~{fragments :: header}"></div> <!-- th:replace="fragments :: header" — указывает на фрагмент header, который находится в файле fragments.html. Thymeleaf заменит этот <div> содержимым фрагмента header -->

    <!-- th:include вставляет содержимое фрагмента внутрь текущего элемента, не удаляя его -->
    <div th:insert="~{fragments :: header}"></div> <!-- Этот код вставит содержимое фрагмента header внутрь <div>, оставляя сам элемент <div> -->
```

### Передача параметров во фрагменты

Фрагменты Thymeleaf могут принимать параметры, что делает их еще более гибкими. Чтобы передать параметры, вы можете использовать th:with

**Пример фрагмента с параметром:**

```html
    <!-- Файл fragments/header.html -->
    <div th:fragment="headerWithTitle(title)">
        <header>
            <h1 th:text="${title}">Моя компания</h1>
            <nav>
                <ul>
                    <li><a href="/">Главная</a></li>
                    <li><a href="/about">О нас</a></li>
                    <li><a href="/contact">Контакты</a></li>
                </ul>
            </nav>
        </header>
    </div>
```

**Вставка фрагмента с параметром:**

```html
    <div th:replace="~{fragments/header :: headerWithTitle('Добро пожаловать!')}"></div> <!-- Путь к файлам фрагментов должен быть относительным относительно папки templates, которая является базовой для Thymeleaf -->
```
