# Spring MVC

**Spring MVC** (Model-View-Controller) — это часть фреймворка **Spring**, которая предназначена для построения веб-приложений с использованием паттерна **MVC**. Spring MVC облегчает разработку гибких и легко расширяемых приложений за счёт внедрения зависимостей (DI), а также предлагает средства для организации взаимодействия между компонентами приложения.

- [Spring MVC](#spring-mvc)
  - [Что такое MVC?](#что-такое-mvc)
  - [Как Spring MVC реализует MVC](#как-spring-mvc-реализует-mvc)
  - [Конфигурация Spring MVC](#конфигурация-spring-mvc)
  - [Конфигурация web.xml](#конфигурация-webxml)
    - [Основные элементы конфигурации web.xml](#основные-элементы-конфигурации-webxml)
    - [Конфигурация servlet через applicationContext.xml](#конфигурация-servlet-через-applicationcontextxml)
  - [InternalResourceViewResolver](#internalresourceviewresolver)
    - [Основные свойства и методы](#основные-свойства-и-методы)
    - [Пример работы с контроллером](#пример-работы-с-контроллером)
    - [Особенности использования](#особенности-использования)
  - [DispatcherServlet](#dispatcherservlet)
    - [Пример жизненного цикла запроса в Spring MVC](#пример-жизненного-цикла-запроса-в-spring-mvc)
    - [Взаимодействие DispatcherServlet с InternalResourceViewResolver](#взаимодействие-dispatcherservlet-с-internalresourceviewresolver)
    - [Методы DispatcherServlet](#методы-dispatcherservlet)
    - [Возможные стратегии и взаимодействие с ними](#возможные-стратегии-и-взаимодействие-с-ними)
  - [@Controller](#controller)
    - [@RequestMapping](#requestmapping)
    - [@GetMapping, @PostMapping, @PutMapping, @DeleteMapping](#getmapping-postmapping-putmapping-deletemapping)
      - [@ResponseBody](#responsebody)
    - [Обработка параметров](#обработка-параметров)
      - [Параметры запроса (Query Parameters) — @RequestParam](#параметры-запроса-query-parameters--requestparam)
      - [Переменные пути (Path Variables) — @PathVariable](#переменные-пути-path-variables--pathvariable)
      - [@ModelAttribute](#modelattribute)
      - [Тело запроса (Request Body) — @RequestBody](#тело-запроса-request-body--requestbody)
      - [Заголовки HTTP (HTTP Headers) — @RequestHeader](#заголовки-http-http-headers--requestheader)
      - [Cookie — @CookieValue](#cookie--cookievalue)
      - [Сессия — HttpSession](#сессия--httpsession)
      - [@SessionAttribute](#sessionattribute)
      - [@RequestPart](#requestpart)
    - [Контекст запроса — HttpServletRequest и HttpServletResponse](#контекст-запроса--httpservletrequest-и-httpservletresponse)
      - [HttpServletRequest](#httpservletrequest)
      - [HttpServletResponse](#httpservletresponse)
    - [Principal](#principal)
    - [Загрузки файлов — MultipartFile](#загрузки-файлов--multipartfile)
    - [Модель и представление — Model, ModelMap, Map](#модель-и-представление--model-modelmap-map)

## Что такое MVC?

**MVC (Model-View-Controller)** — это архитектурный шаблон проектирования, который разделяет логику приложения на три компонента:

1. **Model (Модель)**:
   - Представляет бизнес-логику и данные приложения.
   - Модель отвечает за доступ к данным, манипуляцию данными и логику их представления.
   - Например, в веб-приложении модель будет взаимодействовать с базой данных для получения информации.

2. **View (Представление)**:
   - Отвечает за отображение данных пользователю.
   - Представление формирует интерфейс (обычно это HTML-страницы), который видит пользователь.
   - View получает данные от контроллера и отображает их в нужном формате.

3. **Controller (Контроллер)**:
   - Обрабатывает входящие запросы от пользователя.
   - Контроллер получает запрос, вызывает модель для обработки данных, и передает данные во View для отображения.
   - Контроллер выступает посредником между моделью и представлением.

## Как Spring MVC реализует MVC

1. **DispatcherServlet** — это основной компонент Spring MVC, который действует как центральный диспетчер всех запросов. Он принимает входящие HTTP-запросы и направляет их к соответствующему контроллеру.

2. **Controller** — это Java-класс с аннотациями, например, `@Controller` и `@RequestMapping`. В Spring MVC контроллер обрабатывает запросы, взаимодействует с моделью и возвращает представление (View).

3. **Model** — в Spring MVC это объект, который содержит данные, передаваемые из контроллера в представление. Эти данные могут быть добавлены в объект `Model` или `ModelAndView`.

4. **View** — это то, как данные отображаются пользователю. В Spring MVC представления обычно создаются с помощью шаблонов (например, JSP, Thymeleaf, или других движков шаблонов).

## Конфигурация Spring MVC

1. Создаем maven проект, используя `maven-archetype-webapp`
2. Покдлючаем необходимые библиотеки:

    ```xml
        <properties>
            <spring.version>6.1.6</spring.version>
        </properties>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    ```

3. Запуск проекта через Tomcat

    1. Скачайте Tomcat с официального сайта Apache Tomcat.
    2. Распакуйте архив в удобное место на вашем компьютере.
    3. Чтобы запустить Tomcat, перейдите в каталог bin и запустите файл:
        Для Windows: startup.bat
        Для Unix/Linux: ./startup.sh
    4. Разместите ваш WAR-файл в директории webapps, и Tomcat автоматически развернет его.

4. Дабавляем пакеты в иерархию

    По умолчанию `maven-archetype-webapp` не создает папку java со вложенными пакетами, поэтому ее стоит создать
5. Конфигурируем web.xml

## Конфигурация web.xml

Файл `web.xml` является конфигурационным файлом, который используется для настройки веб-приложений на основе Java EE (или Jakarta EE) и сервлетов. Этот файл необходим для определения компонентов веб-приложения, таких как сервлеты, фильтры, слушатели, и для задания различных параметров конфигурации. Он также известен как Deployment Descriptor (описатель развертывания).

Начиная с `Servlet API 3.0`, многие конфигурации, которые раньше были обязательны в `web.xml`, могут быть заменены аннотациями в коде (например, @WebServlet, @WebFilter, и т.д.). Однако web.xml все еще полезен для явного определения настроек приложения, особенно если используется старый стек технологий или необходимо произвести специфические настройки.

### Основные элементы конфигурации web.xml

```xml
    <web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
                             http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0"> <!-- Это корневой элемент, который содержит всю конфигурацию веб-приложения -->
    <!-- Здесь располагаются остальные элементы конфигурации -->

      <servlet> <!-- Эти элементы используются для определения сервлетов — компонентов, которые обрабатывают HTTP-запросы -->
        <servlet-name>dispatcher</servlet-name> <!-- Задает имя для данного сервлета. В этом случае имя сервлета — dispatcher. Это имя используется при маппинге сервлета на URL -->
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class> <!-- Этот элемент определяет класс сервлета, который будет обрабатывать запросы. Здесь указан класс DispatcherServlet из Spring Framework -->
        <init-param> <!-- Это элемент, который используется для передачи параметров инициализации сервлету -->
            <param-name>contextConfigLocation</param-name> <!-- Это ключевой параметр, который говорит Spring MVC, где искать файл с конфигурацией контекста Spring -->
            <param-value>/WEB-INF/applicationContext.xml</param-value> <!-- Это путь к XML-файлу, в котором описаны конфигурации Spring -->
        </init-param>
        <load-on-startup>1</load-on-startup> <!-- Этот элемент указывает серверу приложений, когда следует загружать и инициализировать этот сервлет -->
        <!-- Положительное значение (1) означает, что этот сервлет будет загружен сразу при старте, что важно для корректной работы Spring MVC, так как он должен сразу же перехватывать запросы -->
        <!-- Если указано значение меньше нуля, сервлет будет загружен только по первому запросу к нему -->
    </servlet>

    <servlet-mapping> <!-- Определяет, какие URL-запросы будут направляться на указанный сервлет -->
        <servlet-name>dispatcher</servlet-name> 
        <url-pattern>/</url-pattern> <!-- Перехватывает все запросы к корню приложения -->
    </servlet-mapping>

    </web-app>
```

### Конфигурация servlet через applicationContext.xml

```xml
     <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:mvc="http://www.springframework.org/schema/mvc"
        xmlns:context="http://www.springframework.org/schema/context"
        xsi:schemaLocation="http://www.springframework.org/schema/beans 
                            http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/mvc 
                            http://www.springframework.org/schema/mvc/spring-mvc.xsd
                            http://www.springframework.org/schema/context 
                            http://www.springframework.org/schema/context/spring-context.xsd">

        <!-- Этот элемент активирует поддержку аннотаций в Spring MVC (@Controller, @RequestMapping, @ResponseBody) -->
        <mvc:annotation-driven />

        <!-- Указываем путь к статическим ресурсам (например, CSS, JavaScript) -->
        <mvc:resources mapping="/resources/**" location="/resources/" /> <!-- Путь /resources/** — это URL-шаблон, указывающий на то, что любые запросы, начинающиеся с /resources/, должны быть направлены к статическим файлам в папке /resources/ -->

        <!-- Определение view resolver, который отвечает за отображение представлений -->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/WEB-INF/views/" />
            <property name="suffix" value=".jsp" />
        </bean>

        <!-- Сканирование пакетов для поиска аннотированных компонентов, таких как @Controller, @Service, @Repository -->
        <context:component-scan base-package="com.example.controller" />

        <!-- Другие бины и зависимости, например, DataSource, Hibernate, сервисы и репозитории -->
        <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
            <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
            <property name="url" value="jdbc:mysql://localhost:3306/mydatabase" />
            <property name="username" value="root" />
            <property name="password" value="password" />
        </bean>

        <!-- Пример bean-сервиса -->
        <bean id="myService" class="com.example.service.MyServiceImpl">
            <!-- Внедрение зависимостей -->
            <property name="myRepository" ref="myRepository" />
        </bean>

        <!-- Пример репозитория -->
        <bean id="myRepository" class="com.example.repository.MyRepositoryImpl">
            <property name="dataSource" ref="dataSource" />
        </bean>
    </beans>
```

## InternalResourceViewResolver

**`InternalResourceViewResolver`** — это один из стандартных классов Spring, используемых для настройки отображения представлений (views) в приложениях на основе **Spring MVC**. Он отвечает за разрешение логических имен представлений (views), которые возвращаются контроллерами, в фактические ресурсы, такие как JSP-страницы или другие серверные файлы, и их рендеринг.

Основная задача `InternalResourceViewResolver`:
`InternalResourceViewResolver` используется для преобразования имени представления, которое возвращается из контроллера (например, `"home"`), в фактический путь к ресурсу на сервере (например, `/WEB-INF/views/home.jsp`), который затем отображается пользователю.

Как это работает:

- Когда контроллер в Spring MVC возвращает строку с именем представления, **`InternalResourceViewResolver`** добавляет к этому имени префикс и суффикс, чтобы сформировать полный путь к файлу.
- Пример: если контроллер возвращает строку `"home"`, и у `InternalResourceViewResolver` установлен префикс `/WEB-INF/views/` и суффикс `.jsp`, то итоговый путь будет `/WEB-INF/views/home.jsp`.
- После этого он передает управление серверу для рендеринга страницы с помощью `RequestDispatcher`.

### Основные свойства и методы

```java
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    setPrefix(String prefix);  // Устанавливает путь, который будет добавлен перед именем представления.
    setSuffix(String suffix);  // Устанавливает суффикс (расширение), который будет добавлен к имени 
    
    setOrder(int order); // Задает порядок приоритетов для этого ViewResolver в случае, если в приложении определено несколько ViewResolver-ов. InternalResourceViewResolver обычно используется как последний вариант (с низким приоритетом), так как он всегда будет разрешать представления через RequestDispatcher.

    setViewClass(Class<?> viewClass); // Можно использовать для явного указания класса представления, который должен быть использован для рендеринга (например, `JstlView`, если вы используете JSTL с JSP, По умолчанию используется InternalResourceView)

    setExposeContextBeansAsAttributes(boolean expose); // Если установлено в `true`, делает доступными все Spring-бины из контекста в качестве атрибутов для представления (например, можно будет использовать в JSP).

    setExposedContextBeanNames(String... beanNames); // Позволяет указать конкретные бины, которые нужно сделать доступными в качестве атрибутов для представления.

    setExposePathVariables(boolean expose); // Если установлено в `true`, делает переменные пути (Path Variables) доступными как атрибуты в представлениях (например, параметры, захваченные через `@PathVariable` в контроллерах).

    resolveViewName(String viewName, Locale locale); // Это основной метод, который выполняет логику разрешения представления. Он принимает логическое имя представления (которое возвращает контроллер), добавляет к нему префикс и суффикс, и возвращает объект `View`.
```

### Пример работы с контроллером

Предположим, у нас есть контроллер:

```java
@Controller
public class HomeController {

    @RequestMapping("/home")
    public String homePage() {
        return "home";  // Возвращаем логическое имя представления "home"
    }
}
```

И есть конфигурация `InternalResourceViewResolver`:

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/" />
    <property name="suffix" value=".jsp" />
</bean>
```

Когда пользователь обращается к URL `/home`, контроллер возвращает строку `"home"`, а **`InternalResourceViewResolver`** преобразует это имя в путь `/WEB-INF/views/home.jsp`, который и будет отрисован сервером.

### Особенности использования

- **Поддержка JSP и других серверных ресурсов**: Основное использование `InternalResourceViewResolver` — это работа с JSP. Однако его можно использовать и с другими типами серверных файлов, такими как `.html` или `.jspx`.
  
- **Инкапсуляция логики отображения**: Контроллеры не знают точное местоположение или расширение представлений. Это делает их независимыми от реализации представления. Изменение пути или расширения файлов представлений можно сделать в одном месте, и это не потребует изменений в контроллерах.

- **Производительность**: `InternalResourceViewResolver` работает через механизм `RequestDispatcher`, что может быть менее эффективно, чем другие подходы (например, использование шаблонизаторов, таких как Thymeleaf). Однако, для небольших приложений и серверных страниц это является удобным и простым решением.

- **Работа с несколькими `ViewResolver`-ами**: В Spring MVC можно использовать несколько разных `ViewResolver`-ов, каждый из которых будет проверять, может ли он разрешить представление. Если в конфигурации присутствует, например, `ThymeleafViewResolver`, он может быть использован для разрешения определенных представлений, а `InternalResourceViewResolver` — как резервное решение для JSP.

## DispatcherServlet

**`DispatcherServlet`** — это центральный класс в **Spring MVC**, который играет роль **Front Controller** (фронт-контроллера). Он отвечает за перехват всех входящих HTTP-запросов к веб-приложению, определение подходящего обработчика (контроллера), и управление процессом обработки запросов и рендеринга ответов.

`DispatcherServlet` является реализацией паттерна **Front Controller**, который используется для централизованной обработки всех запросов в веб-приложении.

**Основные задачи `DispatcherServlet`:**

1. **Перехват запросов**: Он перехватывает все HTTP-запросы, которые направлены на определенные URL-шаблоны.
2. **Маршрутизация**: Определяет, какой контроллер должен обработать запрос, исходя из URL и HTTP-метода.
3. **Обработка запросов**: Передает запрос контроллеру, который занимается бизнес-логикой и может возвращать данные для отображения.
4. **Рендеринг ответа**: Определяет представление (view), которое должно быть отрисовано, используя механизм **ViewResolver**, и возвращает сгенерированный HTML-ответ клиенту.

### Пример жизненного цикла запроса в Spring MVC

1. **Запрос**: Клиент отправляет запрос на URL `/home`.
2. **Перехват**: `DispatcherServlet` перехватывает запрос и передает его в систему маршрутизации.
3. **Контроллер**: С помощью **HandlerMapping** находит контроллер, который отвечает за этот URL.
4. **Возврат представления**: Контроллер возвращает строку `"home"`.
5. **ViewResolver**: `InternalResourceViewResolver` преобразует имя `"home"` в путь `/WEB-INF/views/home.jsp`.
6. **Рендеринг**: JSP-страница отрисовывается и отправляется клиенту.

### Взаимодействие DispatcherServlet с InternalResourceViewResolver

1. Когда контроллер возвращает строку (например, `"home"`), **`DispatcherServlet`** передает эту строку `InternalResourceViewResolver` для разрешения.
2. **`InternalResourceViewResolver`** берет логическое имя представления и добавляет к нему префикс и суффикс (например, `/WEB-INF/views/` и `.jsp`), чтобы получить реальный путь к ресурсу (например, `/WEB-INF/views/home.jsp`).
3. **`DispatcherServlet`** передает запрос на отрисовку JSP-файла через механизм `RequestDispatcher`.

### Методы DispatcherServlet

Класс **`DispatcherServlet`** наследуется от **`HttpServlet`** и переопределяет ключевые методы для обработки запросов.

**Основные методы:**

```java
    DispatcherServlet ds = DispatcherServlet()
    ds.doDispatch(HttpServletRequest request, HttpServletResponse response);  // Это основной метод, который выполняет весь процесс обработки запроса, начиная с поиска контроллера и заканчивая рендерингом ответа. Этот метод проверяет HTTP-метод (GET, POST и т.д.) и передает запрос соответствующему обработчику

    doService(HttpServletRequest request, HttpServletResponse response); // Этот метод вызывается для обработки всех типов HTTP-запросов и использует doDispatch()для передачи запросов в MVC-компоненты

    initStrategies(ApplicationContext context); // Метод, который инициализирует все основные стратегии для обработки запросов, такие как HandlerMapping, HandlerAdapter, ViewResolver, и другие компоненты MVC. 

    onRefresh(ApplicationContext context);  // Этот метод переопределен для обновления DispatcherServlet и перезагрузки связанных компонентов Spring MVC, когда обновляется контекст. 

    processHandlerException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex); // Обрабатывает исключения, которые могут возникнуть при обработке запроса. Он пытается найти подходящий `HandlerExceptionResolver` для обработки исключений и генерации корректного ответа. 

    render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response); // Этот метод выполняет рендеринг представления (view), переданного контроллером, с использованием модели данных. 

    getDefaultStrategies(ApplicationContext context, Class<?> strategyInterface); // Возвращает стратегии по умолчанию (например, стратегии для `ViewResolver` или `HandlerAdapter`), которые `DispatcherServlet` будет использовать при обработке запросов.
```

### Возможные стратегии и взаимодействие с ними

`DispatcherServlet` полагается на набор стратегий (компонентов), которые определяются через интерфейсы и настраиваются через XML или Java-конфигурацию.

1. `HandlerMapping`: Определяет, какой контроллер должен обработать входящий запрос. Примеры: `RequestMappingHandlerMapping`.
2. `HandlerAdapter`: Выполняет контроллер, который был найден через `HandlerMapping`.
3. `HandlerExceptionResolver`: Обрабатывает исключения, которые могут возникнуть во время обработки запроса.
4. `ViewResolver`: Разрешает имя представления (строку) в реальный путь к ресурсу (например, JSP или другой шаблон).
5. `LocaleResolver`: Определяет текущую локализацию пользователя.
6. `MultipartResolver`: Обрабатывает загрузку файлов (multipart запросы).

## @Controller

Аннотация `@Controller` в Spring указывает, что класс является контроллером в контексте веб-приложения, и его задача — обрабатывать HTTP-запросы, возвращать представления или данные. Она работает в связке с аннотациями, такими как `@RequestMapping`, `@GetMapping`, `@PostMapping`, и другими, которые описывают маршруты для обработки запросов

**Основные функции аннотации @Controller:**

- `Маршрутизация запросов`: Классы, аннотированные @Controller, используются для обработки входящих HTTP-запросов (например, GET, POST, PUT, DELETE) и маршрутизации их к соответствующим методам контроллера.
- `Возвращение представлений`: Контроллеры часто возвращают логическое имя представления (view), которое будет отображено пользователю (например, JSP, Thymeleaf, HTML-файл).
- `Связывание модели и данных`: Методы контроллера могут наполнять модель данными, которые затем будут переданы в представление для отображения пользователю.

### @RequestMapping

`@RequestMapping`: Используется для задания маршрутов и указания HTTP-методов для обработки запросов

```java
    @Controller
    @RequestMapping("/products")
    public class ProductController {

        @RequestMapping(value = "/list", method = RequestMethod.GET)
        public String listProducts(Model model) {
            List<Product> products = productService.findAll();
            model.addAttribute("products", products);
            return "productList";
        }
    }
```

### @GetMapping, @PostMapping, @PutMapping, @DeleteMapping

@GetMapping, @PostMapping, @PutMapping, @DeleteMapping: Это сокращенные версии @RequestMapping для конкретных HTTP-методов (GET, POST и т.д.)

```java
    @GetMapping("/form")
    public String showForm() {
        return "form";
    }

    @PostMapping("/submit")
    public String submitForm(@ModelAttribute("user") User user) {
        userService.save(user);
        return "redirect:/success";
    }
```

#### @ResponseBody

@ResponseBody: Используется для указания, что возвращаемые данные должны быть напрямую записаны в тело HTTP-ответа (например, JSON или XML)

```java
    @GetMapping("/api/users")
    @ResponseBody
    public List<User> getUsers() {
        return userService.findAll();
    }
```

### Обработка параметров

#### Параметры запроса (Query Parameters) — @RequestParam

@RequestParam: Используется для извлечения параметров запроса (например, query-параметры или параметры формы).

```java
    @GetMapping("/greet")
    public String greetUser(@RequestParam("name") String name, Model model) { // Извлекает параметр name из строки запроса
        model.addAttribute("name", name);
        return "greeting";
    }
```

#### Переменные пути (Path Variables) — @PathVariable

@PathVariable: Используется для извлечения переменных из URL

```java
    @GetMapping("/product/{id}")
    public String getProduct(@PathVariable("id") Long productId, Model model) {
        Product product = productService.findById(productId);
        model.addAttribute("product", product);
        return "productDetail";
    }
```

#### @ModelAttribute

@ModelAttribute: Используется для связывания данных формы с объектами Java или для передачи объекта в представление

```java
    @PostMapping("/register")
    public String registerUser(@ModelAttribute("user") User user) { // Автоматически связывает данные формы с объектом User
        userService.save(user);
        return "redirect:/success";
    }

    // Также можно использовать @ModelAttribute для передачи данных в модель до вызова любого метода контроллера
    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("attribute", "value");
    }
```

#### Тело запроса (Request Body) — @RequestBody

@RequestBody используется для извлечения тела запроса (обычно JSON или XML), который отправляется в POST, PUT, DELETE и других запросах. Spring автоматически преобразует тело запроса в объект Java

```java
    @PostMapping("/users")
    public ResponseEntity<String> createUser(@RequestBody User user) { // Получает тело HTTP-запроса и автоматически десериализует его в объект User
        userService.save(user);
        return ResponseEntity.ok("User created successfully");
    }
```

#### Заголовки HTTP (HTTP Headers) — @RequestHeader

@RequestHeader позволяет получить значения заголовков HTTP из запроса

```java
    @GetMapping("/headers")
    public String showHeaders(@RequestHeader("User-Agent") String userAgent) { // Получает значение заголовка User-Agent из запроса
        System.out.println("User-Agent: " + userAgent);
        return "headers";
    }

    // Можно также указать значение по умолчанию
    @GetMapping("/headers")
    public String showHeaders(@RequestHeader(value = "User-Agent", defaultValue = "Unknown") String userAgent) {
        return "User-Agent: " + userAgent;
    }
```

#### Cookie — @CookieValue

@CookieValue используется для извлечения значений cookies

```java
    @GetMapping("/cookie")
    public String getCookie(@CookieValue(value = "sessionId", defaultValue = "12345") String sessionId) { // Извлекает значение cookie sessionId
        System.out.println("Session ID: " + sessionId);
        return "cookie";
    }
```

#### Сессия — HttpSession

Вы можете получить доступ к текущей сессии пользователя с помощью объекта HttpSession

```java
    @GetMapping("/session")
    public String getSession(HttpSession session) { // Объект для работы с данными сессии. Позволяет сохранять и извлекать атрибуты в сессии
        session.setAttribute("name", "John");
        return "sessionPage";
    }
```

#### @SessionAttribute

@SessionAttribute используется для извлечения атрибутов, сохраненных в сессии.

```java
    @GetMapping("/profile")
    public String getUserProfile(@SessionAttribute("user") User user, Model model) { // Извлекает объект User из сессии
        model.addAttribute("user", user);
        return "userProfile";
    }
```

#### @RequestPart

Используется для работы с частями многокомпонентных (multipart) запросов, например, когда нужно получить файл и другие данные одновременно

```java
    @PostMapping("/upload")
    public String uploadFile(@RequestPart("file") MultipartFile file, @RequestPart("description") String description) {
        // Обработка файла и текста
        return "success";
    }
    // @RequestPart("file"): Извлекает часть запроса, содержащую файл
    // @RequestPart("description"): Извлекает другую часть запроса (например, текст)
```

### Контекст запроса — HttpServletRequest и HttpServletResponse

#### HttpServletRequest

`HttpServletRequest` — это интерфейс, который представляет HTTP-запрос, поступивший от клиента (например, браузера) на сервер. Он используется для получения информации о запросе, таких как параметры, заголовки, данные формы, загружаемые файлы, сессии, и многое другое. В рамках веб-приложений на Java сервлеты используют этот интерфейс для извлечения данных запроса, которые сервер обрабатывает.

Класс `HttpServletRequest` расширяет интерфейс `ServletRequest`, добавляя HTTP-специфичные методы для работы с такими вещами, как метод запроса (GET, POST, PUT и т.д.), заголовки, cookies и сессии

**Основные задачи HttpServletRequest:**

- Получение данных, переданных клиентом в запросе (например, параметры, заголовки).
- Получение информации о клиенте (например, IP-адрес, браузер).
- Доступ к HTTP-метаданным (метод запроса, URL, URI и т.д.).
- Управление сессиями и cookies.
- Получение данных загружаемых файлов (в случае multipart-запросов)

**Методы:**

```java
    HttpServletRequest request;

    // Методы для получения информации о запросе

    String uri = request.getRequestURI(); // Возвращает URI (Uniform Resource Identifier) запроса
    StringBuffer url = request.getRequestURL(); // Возвращает URL (Uniform Resource Locator) запроса
    String method = request.getMethod(); // Возвращает метод HTTP-запроса (например, GET, POST, PUT)
    String queryString = request.getQueryString(); // Возвращает строку запроса (query string), которая следует после ? в URL
    String protocol = request.getProtocol(); // Возвращает протокол, используемый для запроса (например, HTTP/1.1)
    String scheme = request.getScheme(); // Возвращает схему запроса (например, http или https)
    String serverName = request.getServerName(); // Возвращает имя сервера, на который был отправлен запрос
    int port = request.getServerPort(); // Возвращает порт, на который был отправлен запрос
    String clientIp = request.getRemoteAddr(); // Возвращает IP-адрес клиента, который отправил запрос
    String clientHost = request.getRemoteHost(); // Возвращает имя хоста клиента, который отправил запрос
    String contextPath = request.getContextPath(); // Возвращает путь контекста веб-приложения
    String servletPath = request.getServletPath(); // Возвращает часть пути, которая соответствует сервлету
    String pathInfo = request.getPathInfo(); // Возвращает любую дополнительную информацию о пути, которая была предоставлена клиентом

    // Методы для получения параметров запроса

    String value = request.getParameter("name"); // Возвращает значение параметра запроса (например, ?name=value)
    String[] values = request.getParameterValues("name"); //  Возвращает массив значений параметра, если параметр передан несколько раз (например, в случае checkbox)
    Map<String, String[]> paramMap = request.getParameterMap(); // Возвращает карту всех параметров запроса, где ключи — это имена параметров, а значения — это массивы значений параметров

    // Методы для получения заголовков HTTP

    String userAgent = request.getHeader("User-Agent"); // Возвращает значение заголовка с указанным именем (например, User-Agent)
    Enumeration<String> headers = request.getHeaders("Accept"); // Возвращает объект Enumeration, содержащий все значения заголовка с указанным именем
    Enumeration<String> headerNames = request.getHeaderNames(); // Возвращает объект Enumeration, содержащий имена всех заголовков запроса

    // Методы для работы с сессиями

    HttpSession session = request.getSession(); // Возвращает текущую сессию или создает новую, если её не существует
    HttpSession session = request.getSession(false); // Возвращает текущую сессию, если она существует. Если create=false, сессия не будет создана, если её нет

    // Методы для работы с cookies
    Cookie[] cookies = request.getCookies(); // Возвращает массив объектов Cookie, представляющих все cookies, отправленные клиентом

    // Методы для работы с атрибутами запроса
    request.setAttribute("user", new User("John")); // Устанавливает атрибут в объект запроса. Атрибуты могут быть использованы для передачи данных между сервлетами
    User user = (User) request.getAttribute("user"); // Возвращает атрибут, связанный с указанным именем
    request.removeAttribute("user"); // Удаляет атрибут из запроса

    // Работа с телом запроса (для POST-запросов)

    ServletInputStream inputStream = request.getInputStream(); // Возвращает объект ServletInputStream, который можно использовать для чтения данных из тела запроса
    BufferedReader reader = request.getReader(); // Возвращает объект BufferedReader для чтения данных из тела запроса (например, текстовые данные)

    // Методы для управления аутентификацией и безопасностью

    boolean isAdmin = request.isUserInRole("admin"); // Проверяет, принадлежит ли текущий пользователь к указанной роли
    Principal principal = request.getUserPrincipal(); // Возвращает объект Principal, представляющий аутентифицированного пользователя
    String authType = request.getAuthType(); // Возвращает тип аутентификации, используемой для запроса

    // Прочие методы

    Locale locale = request.getLocale(); // Возвращает объект Locale, представляющий локаль клиента
    Enumeration<Locale> locales = request.getLocales(); // Возвращает объект Enumeration, представляющий все локали, которые поддерживает клиент
    String contentType = request.getContentType(); // Возвращает MIME-тип тела запроса, если он указан в заголовке Content-Type
    int contentLength = request.getContentLength(); // Возвращает длину тела запроса в байтах
```

#### HttpServletResponse

`HttpServletResponse` — это интерфейс, который представляет HTTP-ответ, отправляемый сервером клиенту (например, браузеру). В Java веб-приложениях этот объект используется для настройки и отправки ответа клиенту, включая такие элементы, как статус HTTP, заголовки, тело ответа, cookies и многое другое.

Вместе с `HttpServletRequest`, `HttpServletResponse` является основным интерфейсом для взаимодействия сервлетов с клиентами через HTTP

**Основные задачи HttpServletResponse:**

- Установка HTTP-статуса (например, 200 OK, 404 Not Found, 500 Internal Server Error).
- Отправка заголовков в ответе.
- Управление cookies.
- Формирование тела ответа (например, HTML, JSON, XML, или файл).
- Выполнение перенаправлений на другие ресурсы

**Методы:**

```java
    HttpServletResponse response

    // Методы для управления статусом HTTP

    response.setStatus(HttpServletResponse.SC_OK);  // Устанавливаем статус 200 OK
    response.sendError(HttpServletResponse.SC_NOT_FOUND);  // Отправляем статус 404 Not Found
    response.sendError(HttpServletResponse.SC_INTERNAL_SERVER_ERROR, "An unexpected error occurred"); // Отправляет клиенту статус ошибки с сообщением
    response.setContentType("text/html"); // Устанавливает тип содержимого (MIME-тип) ответа (например, text/html, application/json
    response.setContentLength(content.length()); // Устанавливает длину содержимого ответа
    response.setCharacterEncoding("UTF-8"); // Устанавливает кодировку символов для ответа

    // Методы для работы с заголовками HTTP

    response.setHeader("Cache-Control", "no-cache"); // Устанавливает заголовок с указанным именем и значением. Если заголовок с таким именем уже существует, он перезаписывается
    response.addHeader("Set-Cookie", "user=John"); // Добавляет заголовок с указанным именем и значением. Если заголовок с таким именем уже существует, его значения объединяются

    if (response.containsHeader("Content-Type")) { // Проверяет, был ли установлен заголовок с указанным именем
        // Заголовок Content-Type был установлен
    }

    response.setDateHeader("Last-Modified", System.currentTimeMillis()); // Устанавливает заголовок с датой (например, Last-Modified)
    response.addDateHeader("Expires", System.currentTimeMillis() + 1000 * 60 * 60); // Добавляет заголовок с датой, не перезаписывая существующие значения
    response.setIntHeader("Content-Length", 1024); // Устанавливает заголовок с числовым значением
    response.addIntHeader("Max-Age", 3600); // Добавляет заголовок с числовым значением, не перезаписывая существующие значения

    // Методы для работы с телом ответа

    PrintWriter out = response.getWriter(); // Возвращает объект PrintWriter, который используется для записи текстового контента в ответ
    out.println("<h1>Hello, World!</h1>");

    ServletOutputStream outputStream = response.getOutputStream(); // Возвращает объект ServletOutputStream, который используется для записи бинарного контента в ответ (например, изображения, файлы)
    outputStream.write(bytes);

    // Методы для работы с перенаправлениями

    response.sendRedirect("/newPage"); // Выполняет перенаправление на другой URL. Возвращает клиенту статус 302 (Found) и отправляет новый URL в заголовке Location

    // Методы для работы с cookies

    Cookie cookie = new Cookie("username", "John");
    cookie.setMaxAge(60 * 60);  // Cookie будет действителен 1 час
    response.addCookie(cookie); // Добавляет cookie в ответ. Эти cookies затем будут отправлены клиенту

    // Методы для работы с буферизацией

    response.setBufferSize(8192); // Устанавливает размер буфера для вывода данных в ответ
    int bufferSize = response.getBufferSize(); // Возвращает текущий размер буфера
    response.flushBuffer(); // Принудительно сбрасывает содержимое буфера и отправляет данные клиенту
    response.reset(); // Очищает буфер вывода и статус ответа, что позволяет повторно настроить заголовки и статус, если ответ еще не был отправлен
    response.resetBuffer(); // Очищает только содержимое буфера вывода, не сбрасывая статус и заголовки

    // Методы для работы с содержимым ответа

    response.setContentType("application/json"); // Устанавливает тип содержимого, который будет отправлен клиенту. Обычно это MIME-тип (например, text/html, application/json)
    response.setCharacterEncoding("UTF-8"); // Устанавливает кодировку символов для содержимого ответа

    // Управление кешированием

    response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate"); // Устанавливает заголовки, запрещающие кеширование
    response.setHeader("Pragma", "no-cache"); // Добавляет старый заголовок для совместимости с HTTP/1.0.
    response.setDateHeader("Expires", 0); // Устанавливает дату истечения кеша
```

### Principal

### Загрузки файлов — MultipartFile

### Модель и представление — Model, ModelMap, Map
