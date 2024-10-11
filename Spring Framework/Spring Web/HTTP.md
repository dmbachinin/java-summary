# HTTP

```plantext
    HTTP (HyperText Transfer Protocol) — это протокол прикладного уровня, который служит основой для передачи данных в интернете. Он определяет правила и стандарты, по которым веб-клиенты (например, браузеры) и серверы обмениваются информацией, обеспечивая загрузку веб-страниц, изображений, видео и других ресурсов.
```

- [HTTP](#http)
  - [Методы HTTP](#методы-http)
    - [GET](#get)
    - [POST](#post)
    - [PUT](#put)
    - [DELETE](#delete)
    - [HEAD](#head)
    - [OPTIONS](#options)
    - [PATCH](#patch)
    - [TRACE](#trace)
    - [CONNECT](#connect)
    - [Идемпотентные и неидемпотентные методы](#идемпотентные-и-неидемпотентные-методы)
    - [Безопасные методы](#безопасные-методы)
  - [HTTP Request - запрос](#http-request---запрос)
    - [Стартовая строка (Request Line)](#стартовая-строка-request-line)
    - [Заголовки (Headers) в Request](#заголовки-headers-в-request)
    - [Тело (Body) в Request](#тело-body-в-request)
      - [Основные форматы данных в теле](#основные-форматы-данных-в-теле)
      - [Пример запроса с JSON-телом](#пример-запроса-с-json-телом)
  - [HTTP Response - ответ](#http-response---ответ)
    - [Стартовая строка (Status Line)](#стартовая-строка-status-line)
    - [Заголовки (Headers) в Response](#заголовки-headers-в-response)
    - [Тело (Body) в Response](#тело-body-в-response)
    - [Коды состояния (Status Code)](#коды-состояния-status-code)
      - [1xx: Информационные ответы](#1xx-информационные-ответы)
      - [2xx: Успешные ответы](#2xx-успешные-ответы)
      - [3xx: Перенаправления](#3xx-перенаправления)
      - [4xx: Ошибки клиента](#4xx-ошибки-клиента)
      - [5xx: Ошибки сервера](#5xx-ошибки-сервера)

## Методы HTTP

HTTP-методы, также называемые **вербами**, определяют, какие действия клиент (обычно браузер или другое приложение) хочет выполнить на сервере в отношении определённого ресурса. Каждый метод имеет своё назначение и характерное поведение. Вот подробное описание всех основных методов HTTP:

### GET

Метод **GET** используется для запроса данных с сервера. Он наиболее часто используется для получения содержимого веб-страниц, изображений, файлов и других ресурсов.

**Основные характеристики**:

- Запросы **GET** только получают данные с сервера и не изменяют его состояние.
- Параметры запроса могут быть переданы через URL в виде строки запроса (query string).
- Тело запроса отсутствует.
- Ответ содержит запрашиваемые данные, обычно в виде HTML-страницы, JSON-данных или файлов.
- Безопасный метод, так как не изменяет данные на сервере.
- Идемпотентен — повторные запросы **GET** приводят к тому же результату, что и первый запрос.

### POST

Метод **POST** используется для отправки данных на сервер. Обычно применяется для передачи форм, загрузки файлов или отправки других данных, которые могут повлиять на состояние сервера.

**Основные характеристики**:

- **POST** отправляет данные в теле запроса (body).
- Обычно используется для создания новых ресурсов на сервере или выполнения действий, которые меняют данные (например, регистрация пользователя).
- Метод **POST** не идемпотентен — повторение запроса может привести к созданию дубликатов данных или другой модификации.
- Не безопасен, так как может изменять данные на сервере.

### PUT

Метод **PUT** используется для обновления существующего ресурса или создания нового ресурса с предоставленными данными.

**Основные характеристики**:

- В отличие от **POST**, метод **PUT** обычно используется для создания или полной замены ресурса.
- Если ресурс с указанным идентификатором не существует, сервер может создать его (зависит от реализации).
- **PUT** идемпотентен — повторный запрос с теми же данными приведет к такому же результату.
- Данные передаются в теле запроса.

### DELETE

Метод **DELETE** используется для удаления ресурса на сервере.

**Основные характеристики**:

- Как и **PUT**, метод **DELETE** идемпотентен — повторный запрос на удаление ресурса не изменит результат, если ресурс уже удалён.
- **DELETE** может использоваться для удаления одного ресурса или нескольких в зависимости от URL.

### HEAD

Метод **HEAD** очень похож на **GET**, за исключением того, что сервер возвращает только заголовки ответа без тела. Это полезно для получения метаданных о ресурсе, не загружая само содержимое.

**Основные характеристики**:

- Не изменяет состояние ресурса.
- Идемпотентен и безопасен.
- Используется для проверки существования ресурса, получения заголовков кэша и метаданных.

### OPTIONS

Метод **OPTIONS** используется для получения информации о том, какие методы и опции поддерживаются сервером для определённого ресурса.

**Основные характеристики**:

- Ответ включает заголовки, указывающие доступные методы (например, `Allow: GET, POST`).
- Часто используется в контексте CORS (Cross-Origin Resource Sharing), чтобы узнать, разрешает ли сервер выполнение запросов из других доменов.

### PATCH

Метод **PATCH** используется для частичного обновления ресурса. В отличие от **PUT**, который заменяет ресурс целиком, **PATCH** применяет изменения к существующему ресурсу.

**Основные характеристики**:

- Частично обновляет ресурс, а не заменяет его полностью.
- Обычно применяется для отправки небольших изменений (например, обновление одного поля).
- Не всегда идемпотентен (зависит от того, как применяются изменения).

### TRACE

Метод **TRACE** используется для диагностики, позволяя клиенту видеть точный запрос, который был получен сервером. Сервер возвращает клиенту его собственный запрос.

**Основные характеристики**:

- Обычно используется для отладки сетевых маршрутов или конфигураций.
- Может использоваться для проверки наличия изменений или искажений в запросах на пути от клиента до сервера.
- **TRACE** редко используется в современных приложениях, так как может создать уязвимости.

### CONNECT

Метод **CONNECT** используется для создания туннеля через сервер, обычно для проксирования соединений с использованием SSL/TLS. Веб-прокси серверы могут использовать **CONNECT** для установления защищённого соединения между клиентом и сервером.

**Основные характеристики**:

- Создаёт туннель для передачи данных напрямую между клиентом и конечным сервером.
- Обычно используется для установки HTTPS-соединений через прокси.

### Идемпотентные и неидемпотентные методы

- **Идемпотентные методы** — это те методы, которые при повторном выполнении возвращают тот же результат и не изменяют состояние сервера (например, **GET**, **PUT**, **DELETE**, **HEAD**).
- **Неидемпотентные методы** — это методы, которые могут изменять состояние сервера, и их повторное выполнение может привести к разным результатам (например, **POST**, **PATCH**).

### Безопасные методы

- **Безопасные методы** — это те, которые не изменяют данные на сервере. К ним относятся **GET**, **HEAD**, **OPTIONS**, **TRACE**.

## HTTP Request - запрос

HTTP Request — это сообщение, отправляемое клиентом (обычно веб-браузером) серверу, чтобы запросить ресурс или выполнить действие. Структура HTTP-запроса состоит из нескольких частей, каждая из которых имеет своё назначение

**HTTP-запрос делится на следующие основные части:**

- Стартовая строка (Request Line)
- Заголовки (Headers)
- Тело (Body)

### Стартовая строка (Request Line)

Стартовая строка содержит основную информацию о запросе, такую как метод, целевой URL и версия протокола.

**Компоненты:**

- `Метод`: указывает действие, которое клиент хочет выполнить (например, GET, POST, PUT, DELETE и т.д.).
- `Целевой ресурс (URL)`: это путь на сервере, указывающий на ресурс, который клиент хочет запросить или с которым хочет взаимодействовать. Может включать параметры запроса.
- `Версия HTTP`: версия протокола HTTP, которая используется (например, HTTP/1.1, HTTP/2).

**Пример:**

```makefile
    GET /index.html HTTP/1.1 # <Метод> <Целевой ресурс (URL)> <Версия HTTP>
```

### Заголовки (Headers) в Request

Заголовки содержат дополнительные метаданные о запросе, такие как информация о клиенте, типы данных, которые принимаются от сервера, и параметры кэширования. Заголовки помогают клиенту и серверу лучше понять контекст запроса и корректно его обработать.

**Основные категории заголовков:**

1. **Заголовки клиента (Request Headers)**: Эти заголовки передают информацию о клиенте, его возможностях и предпочтениях.
   - `Host`: указывает доменное имя сервера.
   - `User-Agent`: содержит информацию о браузере и операционной системе клиента.
   - `Accept`: указывает типы данных, которые клиент готов принять (например, text/html, application/json).
   - `Accept-Encoding`: указывает, какие типы сжатия (например, gzip, deflate) клиент может обработать.

2. **Заголовки содержимого (Entity Headers):** Эти заголовки относятся к содержимому запроса.
   - `Content-Type`: указывает тип данных, передаваемых в теле запроса (например, application/json, application/x-www-form-urlencoded).
   - `Content-Length`: указывает размер тела запроса в байтах.
   - `Content-Encoding`: указывает метод сжатия тела запроса (например, gzip).

3. **Заголовки авторизации:**
   - `Authorization`: используется для передачи данных аутентификации (например, для передачи токена доступа).
   - `Cookie`: передает куки, которые могут использоваться для аутентификации или хранения пользовательских данных.

4. **Заголовки управления соединением:**
   - `Connection`: указывает, должно ли соединение быть закрыто после выполнения запроса (Connection: close), или оно должно оставаться открытым для повторного использования (Connection: keep-alive).

**Пример:**

```makefile
    GET /index.html HTTP/1.1
    # <Название заголовка>: <Значение заголовка>
    Host: www.example.com # Указывает доменное имя сервера
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) # Содержит информацию о браузере и операционной системе клиента
    Accept: text/html # Указывает типы данных, которые клиент готов принять (например, text/html, application/json)
    Accept-Language: en-US # Указывет язык
```

- Здесь клиент запрашивает ресурс /index.html, указывая домен `www.example.com`, типы данных, которые он может принять (text/html), и другую информацию о клиенте (браузер, язык)

### Тело (Body) в Request

Тело запроса (Request Body) присутствует не во всех типах запросов. Оно обычно используется в запросах, где клиент передаёт на сервер данные (например, запросы POST, PUT или PATCH). В запросах типа GET и DELETE тело обычно отсутствует, так как они предназначены для получения данных или удаления ресурса

**Назначение тела:**

- Тело запроса содержит данные, которые клиент хочет передать серверу. Это могут быть данные формы, файлы, JSON, XML и другие форматы.
- Формат тела определяется заголовком Content-Type. Например, если в заголовке указано application/json, то тело должно содержать данные в формате JSON.

#### Основные форматы данных в теле

1. `application/x-www-form-urlencoded`: Данные формы передаются в формате «ключ=значение», как строка запроса. Обычно используется для передачи данных форм

    ```makefile
        username=user1&password=secret
    ```

2. `multipart/form-data`: Используется для отправки данных форм с возможностью передачи файлов. В этом формате данные разбиваются на части, каждая из которых имеет свои заголовки.

    ```makefile
        Content-Disposition: form-data; name="username"

        user1

        Content-Disposition: form-data; name="file"; filename="file.txt"
        Content-Type: text/plain

        <содержимое файла>
    ```

3. `application/json`: Данные передаются в формате JSON, что удобно для работы с API

    ```json
        {
            "username": "user1",
            "password": "secret"
        }
    ```

4. `text/plain`: Тело содержит обычный текст

    ```plantext
        Hello, this is a simple text message.
    ```

#### Пример запроса с JSON-телом

```makefile
    POST /api/users HTTP/1.1
    Host: api.example.com
    Content-Type: application/json
    Content-Length: 64

    {
    "username": "user1",
    "email": "user1@example.com",
    "password": "secret"
    }
```

## HTTP Response - ответ

`HTTP Response` — это ответ сервера на HTTP-запрос клиента. Ответ содержит информацию о том, как сервер обработал запрос, и, если необходимо, возвращает клиенту запрашиваемые данные. Ответ состоит из нескольких частей, каждая из которых несёт свою функцию.

**Основные части HTTP-ответа:**

- Стартовая строка (Status Line)
- Заголовки (Headers)
- Тело (Body)

### Стартовая строка (Status Line)

Стартовая строка HTTP-ответа содержит информацию о результате обработки запроса сервером

**Компоненты:**

- `Версия HTTP`: версия протокола HTTP, используемого сервером (например, HTTP/1.1 или HTTP/2).
- [`Код состояния (Status Code)`](#коды-состояния-status-code): трёхзначный числовой код, который описывает результат запроса.
- `Статусное сообщение (Reason Phrase)`: краткое текстовое описание кода состояния (например, для кода 200 сообщение будет OK).

**Пример:**

```php
    HTTP/1.1 200 OK # <Версия HTTP> <Код состояния (Status Code)> <Статусное сообщение (Reason Phrase)>
```

### Заголовки (Headers) в Response

Заголовки HTTP-ответа содержат метаданные о самом ответе, передаваемых данных, состоянии соединения и многое другое. Они помогают клиенту интерпретировать данные, управлять кэшированием и соединением, а также работать с сессиями

**Основные категории заголовков:**

1. **Заголовки общего назначения (General Headers)**: включают информацию, которая касается всего сообщения.
   - `Date`: дата и время, когда был создан ответ.
   - `Connection`: указывает, должно ли соединение быть закрыто после ответа или может быть повторно использовано (например, Connection: keep-alive).
2. **Заголовки содержимого (Entity Headers)**: касаются данных, возвращаемых в теле ответа.
   - `Content-Type`: указывает тип данных, которые сервер возвращает в ответе (например, text/html, application/json).
   - `Content-Length`: указывает длину тела ответа в байтах.
   - `Content-Encoding`: указывает метод сжатия данных (например, gzip).
   - `Last-Modified`: дата последнего изменения ресурса.
3. **Заголовки кэширования**:
   - `Cache-Control`: управляет кэшированием (например, no-cache, max-age=3600).
   - `Expires`: указывает время, после которого ресурс считается устаревшим.
4. **Заголовки перенаправления**:
   - `Location`: указывает новый URL для перенаправления в случае ответа с кодом 3xx.
5. **Заголовки безопасности**:
   - `Set-Cookie`: устанавливает куки, которые могут использоваться для аутентификации или хранения пользовательских данных.
   - `Strict-Transport-Security (HSTS)`: указывает, что браузер должен использовать только HTTPS для доступа к серверу в будущем.

**Пример:**

```yaml
    Date: Tue, 08 Oct 2024 12:00:00 GMT # Дата формирования ответа — 8 октября 2024 года
    Content-Type: text/html; charset=UTF-8 # Тип данных, возвращаемых сервером — HTML с кодировкой UTF-8
    Content-Length: 1234 # Длина тела ответа — 1234 байта
    Connection: keep-alive # Соединение будет оставаться открытым для повторного использования
```

### Тело (Body) в Response

Тело ответа содержит сам контент, который сервер возвращает клиенту. Это может быть HTML-код, изображение, JSON-данные, видеофайлы и многое другое. Тело присутствует не всегда — например, в ответах с кодом состояния 204 No Content тело отсутствует, так как сервер ничего не возвращает

Тип данных в теле определяется заголовком Content-Type (например, text/html для HTML-документов или application/json для данных в формате JSON)

**Пример ответа с HTML данными:**

```php
    HTTP/1.1 200 OK
    Date: Tue, 08 Oct 2024 12:00:00 GMT
    Content-Type: text/html; charset=UTF-8
    Content-Length: 1234
    Connection: keep-alive

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Example</title>
    </head>
    <body>
        <h1>Hello, World!</h1>
        <p>This is an example HTML response from the server.</p>
    </body>
    </html>
```

### Коды состояния (Status Code)

**HTTP коды состояния (Status Codes)** — это трёхзначные коды, которые сервер возвращает в ответ на HTTP-запрос, указывая результат его выполнения. Каждый код состояния делится на пять категорий в зависимости от первой цифры:

- **1xx**: Информационные ответы. Запрос был получен и процесс продолжается
- **2xx**: Успешные ответы. Запрос был успешно получен, понят и принят
- **3xx**: Перенаправления. Для выполнения запроса необходимо предпринять дальнейшие действия
- **4xx**: Ошибки клиента. Запрос содержит неверный синтаксис или не может быть выполнен
- **5xx**: Ошибки сервера. Серверу не удалось выполнить корректный запрос

HTTP-коды состояния дают важную информацию о результате выполнения запроса и помогают разработчикам и пользователям понять, что произошло на уровне HTTP-протокола. Они позволяют диагностировать ошибки и решать проблемы, а также помогают управлять обработкой запросов, перенаправлениями и аутентификацией.

#### 1xx: Информационные ответы

Коды состояния, начинающиеся с цифры **1**, являются информационными и сигнализируют, что запрос получен и обработка продолжается.

**Основные коды:**

- **100 Continue**  Сервер сообщает, что получил начальную часть запроса, и клиент может продолжить отправку оставшейся части (например, тела запроса). Обычно используется с запросами, содержащими тело (например, POST).
- **101 Switching Protocols**  Сервер подтверждает запрос клиента на смену протокола (например, с HTTP на WebSocket).
- **102 Processing (WebDAV)**  Сервер обрабатывает запрос, но ещё не готов отправить окончательный ответ (используется в WebDAV).
- **103 Early Hints**  Предварительный ответ, который сообщает клиенту, что сервер собирается отправить заголовки, и клиент может начать предварительную загрузку ресурсов.

#### 2xx: Успешные ответы

Эти коды состояния означают, что запрос был успешно обработан сервером.

**Основные коды:**

- **200 OK**  Запрос успешно выполнен, и сервер возвращает запрашиваемый ресурс (например, HTML-страницу, JSON-данные).
- **201 Created**  Запрос выполнен, и в результате создан новый ресурс. Этот код часто используется в ответ на запросы **POST** или **PUT**.
- **202 Accepted**  Запрос принят на обработку, но сам процесс ещё не завершён. Результат обработки будет позже.
- **203 Non-Authoritative Information**  Ответ успешен, но информация, содержащаяся в ответе, получена из стороннего источника и может отличаться от оригинальной.
- **204 No Content**  Запрос успешно выполнен, но сервер не возвращает содержимого. Этот код часто используется при запросах, которые изменяют данные (например, DELETE), но не требуют возврата данных.
- **205 Reset Content**  Запрос успешно выполнен, но клиенту следует сбросить текущее представление (например, очистить форму на веб-странице).
- **206 Partial Content**  Сервер возвращает часть ресурса. Этот код используется при возобновлении загрузок или при запросах диапазонов байтов.
- **207 Multi-Status (WebDAV)**  Сервер возвращает несколько статусов для разных операций в рамках одного запроса (используется в WebDAV).
- **208 Already Reported (WebDAV)**  Элементы, включенные в этот запрос, уже были сообщены в предыдущих ответах, и повторно они не будут возвращены (WebDAV).
- **226 IM Used**  Сервер выполнил запрос для ресурса и использовал одну или несколько манипуляций с контентом (например, использование метода Delta Encoding).

#### 3xx: Перенаправления

Коды состояния 3xx указывают, что клиенту необходимо выполнить дополнительные действия для завершения запроса, обычно — следовать новому URL.

**Основные коды:**

- **300 Multiple Choices**  Запрашиваемый ресурс имеет несколько представлений (например, разные форматы или языки), и клиент должен выбрать одно из них.
- **301 Moved Permanently**  Ресурс был навсегда перемещён на новый URL. Клиенту рекомендуется использовать этот новый URL для будущих запросов.
- **302 Found**  Ресурс временно перемещён на другой URL, но клиенту следует использовать оригинальный URL для будущих запросов. Часто используется для перенаправлений.
- **303 See Other**  Ресурс доступен по другому URL, и клиенту рекомендуется использовать GET для получения ресурса.
- **304 Not Modified**  Сервер сообщает, что содержимое ресурса не изменилось с момента последнего запроса, и клиенту следует использовать кэшированную версию.
- **305 Use Proxy**  Запрашиваемый ресурс доступен только через прокси. Этот код редко используется из-за потенциальных проблем безопасности.
- **307 Temporary Redirect**  Ресурс временно перемещён на новый URL. Клиент должен использовать оригинальный метод (например, POST) для повторного запроса по новому URL.
- **308 Permanent Redirect**  Ресурс был навсегда перемещён на новый URL, и клиент должен использовать этот новый URL для всех будущих запросов.

#### 4xx: Ошибки клиента

Коды состояния 4xx указывают, что ошибка произошла на стороне клиента (например, некорректный запрос или отсутствующий ресурс).

**Основные коды:**

- **400 Bad Request**  Сервер не может обработать запрос из-за синтаксической ошибки клиента (например, некорректные параметры запроса).
- **401 Unauthorized**  Запрос требует аутентификации. Клиент должен предоставить корректные данные для доступа к ресурсу.
- **402 Payment Required**  Код зарезервирован для будущего использования, в настоящее время не используется.
- **403 Forbidden**  Сервер понял запрос, но отказывается его выполнять. Обычно это происходит из-за недостатка прав доступа.
- **404 Not Found**  Запрашиваемый ресурс не найден. Этот код один из самых известных и часто используется.
- **405 Method Not Allowed**  Метод, указанный в запросе (например, `POST` или `GET`), не разрешён для этого ресурса.
- **406 Not Acceptable**  Запрашиваемый ресурс не может быть возвращён в одном из форматов, указанных в заголовке `Accept`.
- **407 Proxy Authentication Required**  Требуется аутентификация через прокси-сервер для доступа к запрашиваемому ресурсу.
- **408 Request Timeout**  Сервер ожидает запрос от клиента, но не получает его в течение установленного времени.
- **409 Conflict**  Запрос не может быть выполнен из-за конфликта с текущим состоянием ресурса (например, попытка обновить ресурс, который был изменён с момента последнего запроса).
- **410 Gone**  Ресурс был удалён и больше не доступен на сервере.
- **411 Length Required**  Сервер требует, чтобы клиент указал длину содержимого запроса, но в запросе этот параметр отсутствует.
- **412 Precondition Failed**  Одно из условий в заголовках запроса не выполнено (например, если запрос содержит заголовок `If-Match`, но условие не удовлетворяется).
- **413 Payload Too Large** Тело запроса слишком велико для обработки сервером.
- **414 URI Too Long**  Запрашиваемый URI слишком длинный для обработки сервером.
- **415 Unsupported Media Type**  Сервер не поддерживает формат данных, передаваемый в запросе (например, неправильный тип MIME).
- **416 Range Not Satisfiable** Клиент запросил диапазон байтов для ресурса, который не может быть выполнен (например, указанный диапазон превышает размер файла).
- **417 Expectation Failed**  Сервер не может выполнить одно из условий, указанных в заголовке `Expect` запроса клиента.
- **418 I'm a teapot**  Шутливый код, описанный в RFC 2324. Означает, что сервер — чайник, и он не может заваривать кофе.
- **421 Misdirected Request**  Запрос направлен на сервер, который не может его обработать.
- **422 Unprocessable Entity (WebDAV)**  Запрос корректен с точки зрения синтаксиса, но не может быть выполнен из-за семантических ошибок.
- **423 Locked (WebDAV)**  Ресурс заблокирован и недоступен для модификации.
- **424 Failed Dependency (WebDAV)**  Запрос не выполнен, так как зависимость другого запроса не была выполнена.
- **425 Too Early**  Сервер отказывается рисковать обработкой запроса, так как он слишком рано отправлен.
- **426 Upgrade Required**  Сервер требует от клиента обновить используемый протокол для обработки запроса.
- **428 Precondition Required**  Сервер требует, чтобы запрос содержал условия (например, заголовок `If-Match`), чтобы предотвратить конфликты.
- **429 Too Many Requests**  Клиент отправил слишком много запросов за короткий промежуток времени (ограничение по частоте запросов).
- **431 Request Header Fields Too Large**  Один или несколько заголовков запроса слишком велики для обработки сервером.
- **451 Unavailable For Legal Reasons**  Ресурс недоступен по юридическим причинам (например, удалён в связи с законом об авторских правах).

#### 5xx: Ошибки сервера

Коды состояния 5xx указывают на проблемы на стороне сервера. Эти ошибки означают, что запрос клиента был корректен, но сервер не может его обработать.

**Основные коды:**

- **500 Internal Server Error**  Внутренняя ошибка сервера. Это общий код, который указывает на неопределённую проблему с обработкой запроса.
- **501 Not Implemented**  Сервер не поддерживает функциональность, необходимую для выполнения запроса (например, сервер не поддерживает метод).
- **502 Bad Gateway**  Сервер, выступающий в роли шлюза или прокси, получил неверный ответ от другого сервера.
- **503 Service Unavailable**  Сервер временно не может обработать запрос (например, перегружен или находится на обслуживании).
- **504 Gateway Timeout**  Сервер, выступающий в роли шлюза или прокси, не получил своевременный ответ от другого сервера.
- **505 HTTP Version Not Supported**  Сервер не поддерживает версию HTTP, используемую в запросе.
- **506 Variant Also Negotiates**  Сервер обнаружил внутреннюю ошибку при согласовании вариантов ресурса.
- **507 Insufficient Storage (WebDAV)**  Сервер не может сохранить представление ресурса, так как у него недостаточно места на диске.
- **508 Loop Detected (WebDAV)**  Сервер обнаружил бесконечный цикл при обработке запроса (WebDAV).
- **510 Not Extended**  Сервер требует от клиента расширить запрос для выполнения требуемой операции.
- **511 Network Authentication Required**  Клиент должен пройти аутентификацию для доступа к сети.