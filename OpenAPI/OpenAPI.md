# OpenAPI

```plantext
    OpenAPI — это спецификация (стандарт) для описания REST API. С её помощью можно в формализованном виде задать структуру и поведение веб-сервиса. Этот стандарт позволяет разработчикам описывать все доступные эндпоинты, типы запросов, параметры, модели данных и ответы сервера
```

- [OpenAPI](#openapi)
  - [Основные цели OpenAPI](#основные-цели-openapi)
  - [Основные части спецификации OpenAPI](#основные-части-спецификации-openapi)
    - [openapi, info, tags, servers, externalDocs](#openapi-info-tags-servers-externaldocs)
    - [paths](#paths)
    - [components](#components)
      - [Пример использования компонентов](#пример-использования-компонентов)
    - [security](#security)
      - [Как работает блок security?](#как-работает-блок-security)
      - [Пример спецификации с блоком security](#пример-спецификации-с-блоком-security)

## Основные цели OpenAPI

1. Унификация: Унифицированный способ описания API делает его понятным и доступным для всех участников разработки.
2. Генерация кода: Позволяет автоматически создавать клиентские библиотеки, серверные заглушки, документацию и тесты на основе спецификации.
3. Документация: Формализованное описание API служит основой для автоматической генерации красивой документации.

## Основные части спецификации OpenAPI

### openapi, info, tags, servers, externalDocs

```yml
# Указываем версию спецификации OpenAPI
openapi: 3.0.0

# Метаданные API
info:
  title: Example API  # Название API, которое будет отображаться в документации
  description: >
    Это пример спецификации OpenAPI для демонстрации базовых секций.
    Здесь вы можете дать подробное описание вашего API.
  termsOfService: https://example.com/terms/  # Указываем ссылку на Условия использования
  contact:  # Информация для связи
    name: API Support  # Имя контактного лица или отдела
    url: https://example.com/support  # URL для связи
    email: support@example.com  # Email для обратной связи
  license:  # Лицензия, под которой распространяется API
    name: Apache 2.0  # Название лицензии
    url: https://www.apache.org/licenses/LICENSE-2.0.html  # Ссылка на текст лицензии
  version: 1.0.0  # Текущая версия API

# Серверы, на которых доступен API
servers:
  - url: https://api.example.com/{path_version}  # Базовый URL для продакшена
    description: Production server  # Описание сервера (например, для чего он используется)
  - url: https://staging-api.example.com/{path_version}  # Базовый URL для тестовой среды
    description: Staging server for testing  # Описание тестового сервера

# Используются для группировки эндпоинтов в логические категории.
tags:
  - name: Пользователи  # Имя категории
    description: Операции, связанные с пользователями
    externalDocs:
      description: Дополнительная информация о пользователях
      url: https://example.com/docs/users  # Ссылка на внешнюю документацию

  - name: Авторизация
    description: Операции, связанные с аутентификацией и авторизацией

externalDocs: # Предоставляет ссылки на внешние документы, которые могут содержать дополнительные сведения об API
  description: Полная документация API  # Глобальная ссылка на внешние документы
  url: https://example.com/docs/api
```

### paths

Описывает все доступные эндпоинты (пути) API и их методы (GET, POST, PUT, DELETE и т. д.)

```yml
    # Описание эндпоинтов API
paths:
  /users:  # Путь для работы с ресурсом "пользователи"
    get:  # HTTP-метод GET
      summary: Получить список пользователей  # Краткое описание
      description: >
        Возвращает список всех зарегистрированных пользователей.
      tags:  # Группируем эндпоинт по категориям
        - Пользователи
      responses:
        '200':  # Код успешного ответа
          description: Успешный запрос
          content:  # Тип содержимого ответа
            application/json:
              schema:
                type: array  # Массив объектов
                items:
                  $ref: '#/components/schemas/User'  # Ссылка на определение схемы User
        '500':  # Пример обработки ошибки
          description: Ошибка сервера
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /users/{id}:  # Путь с параметром {id}
    get:  # HTTP-метод GET
      summary: Получить информацию о пользователе
      description: >
        Возвращает данные о пользователе по указанному ID.
      parameters:  # Описание параметров (передан не будет, так как не определно поле requestBody)
        - name: id  # Имя параметра
          in: path  # Параметр передаётся в пути
          required: true  # Параметр обязателен
          description: Идентификатор пользователя
          schema:
            type: integer  # Тип данных параметра
            example: 123
      responses:
        '200':
          description: Успешный запрос
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':  # Пример обработки ошибки
          description: Пользователь не найден

  /users:  # Путь для создания пользователя
    post:  # HTTP-метод POST
      summary: Создать нового пользователя
      description: >
        Создаёт нового пользователя и возвращает его данные.
      requestBody:  # Тело запроса
        description: Данные нового пользователя
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UserCreate'  # Схема для данных при создании
      responses:
        '201':  # Успешный ответ на создание
          description: Пользователь создан
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Неверные данные запроса

  /users/{id}:  # Обновление пользователя
    put:  # HTTP-метод PUT
      summary: Обновить данные пользователя
      description: >
        Обновляет данные существующего пользователя.
      parameters: # Данное поле будет передано в теле запроса, так как описано поле requestBody
        - name: id
          in: path
          required: true
          description: Идентификатор пользователя
          schema:
            type: integer
      requestBody:  # Тело запроса
        description: Новые данные пользователя
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UserUpdate'
      responses:
        '200':
          description: Данные пользователя обновлены
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: Пользователь не найден
```

### components

Секция components в OpenAPI используется для определения переиспользуемых частей спецификации. Это позволяет избежать дублирования описаний, упрощает поддержку и делает pспецификацию более читаемой

```yml
components:
  schemas:  # Определяем схемы данных
    User:  # Схема объекта "User"
      type: object  # Тип данных: объект
      properties:  # Свойства объекта
        id:
          type: integer  # Тип данных свойства: число
          description: Уникальный идентификатор пользователя  # Пояснение к свойству
          example: 123  # Пример значения
        name:
          type: string  # Тип данных свойства: строка
          description: Имя пользователя
          example: Иван Иванов
        email:
          type: string
          format: email  # Указание формата (электронная почта)
          description: Электронная почта пользователя
          example: user@example.com
      required:  # Список обязательных свойств
        - id
        - name

    Error:  # Схема для описания ошибок
      type: object
      properties:
        code:
          type: integer
          description: Код ошибки
          example: 404
        message:
          type: string
          description: Описание ошибки
          example: Пользователь не найден

  parameters:  # Определяем переиспользуемые параметры
    UserIdParam:  # Параметр для идентификатора пользователя
      name: id  # Имя параметра
      in: path  # Указывается в пути URL
      required: true  # Обязательный параметр
      description: Уникальный идентификатор пользователя
      schema:
        type: integer  # Тип данных: число
        example: 123  # Пример значения

  responses:  # Определяем стандартные ответы
    NotFoundResponse:  # Ответ "Ресурс не найден"
      description: Ресурс не найден
      content:  # Формат ответа
        application/json:  # Ответ в формате JSON
          schema:
            $ref: '#/components/schemas/Error'  # Используем схему "Error" для описания структуры

  requestBodies:  # Определяем тела запросов
    UserCreateRequest:  # Тело запроса для создания пользователя
      description: Данные для создания нового пользователя
      required: true  # Тело запроса обязательно
      content:  # Формат тела запроса
        application/json:
          schema:  # Используем схему для описания структуры данных
            type: object
            properties:
              name:
                type: string  # Имя пользователя
                description: Имя нового пользователя
                example: Иван Иванов
              email:
                type: string
                format: email
                description: Электронная почта нового пользователя
                example: user@example.com
            required:
              - name
              - email  # Поля "name" и "email" обязательны в запросе

  securitySchemes:  # Определяем способы аутентификации
    ApiKeyAuth:  # Аутентификация через API-ключ
      type: apiKey  # Тип аутентификации: API-ключ
      name: X-API-KEY  # Название заголовка, в котором передаётся ключ
      in: header  # Указывается в заголовке запроса
```

#### Пример использования компонентов

```yml
paths:
  /users/{id}:  # Эндпоинт для работы с конкретным пользователем
    get:  # HTTP-метод GET
      summary: Получить информацию о пользователе
      parameters:
        - $ref: '#/components/parameters/UserIdParam'  # Ссылка на переиспользуемый параметр
      responses:
        '200':  # Успешный ответ
          description: Успешный запрос
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'  # Ссылка на схему "User"
        '404':  # Ответ "Не найдено"
          $ref: '#/components/responses/NotFoundResponse'  # Ссылка на общий ответ "404"

  /users:  # Эндпоинт для создания нового пользователя
    post:  # HTTP-метод POST
      summary: Создать нового пользователя
      requestBody:
        $ref: '#/components/requestBodies/UserCreateRequest'  # Ссылка на тело запроса
      responses:
        '201':  # Ответ "Успешное создание"
          description: Пользователь создан
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'  # Ссылка на схему "User"
```

### security

Блок security используется для описания схем аутентификации и авторизации, которые применяются к API или его отдельным эндпоинтам. Этот блок позволяет указать, какие методы безопасности требуется применять для доступа к ресурсам, и помогает автоматизировать проверки при генерации клиентских библиотек или использовании API-инструментов

#### Как работает блок security?

1. Объявление схем безопасности:
    - В секции components.securitySchemes определяются схемы аутентификации, например, через API-ключи, OAuth 2.0 или Basic Auth.
2. Применение схем безопасности:
    - В секции security на уровне всей спецификации или отдельного эндпоинта указывается, какие схемы безопасности применимы.
3. Механизм работы:
    - Клиенты обязаны предоставить соответствующую информацию (например, токены или ключи) в соответствии с описанной схемой.
    - Сервер проверяет правильность данных и предоставляет доступ или отказывает.

Поддерживаемые значения securitySchemes

**OpenAPI поддерживает следующие типы схем безопасности:**

- apiKey: Аутентификация через API-ключ.
- http: Аутентификация через HTTP (например, Basic или Bearer токены).
- oauth2: OAuth 2.0, поддерживающий потоки авторизации.
- openIdConnect: OpenID Connect.

#### Пример спецификации с блоком security

```yml
openapi: 3.0.0
info:
  title: Пример API с аутентификацией
  version: 1.0.0

components:
  securitySchemes:  # Определяем схемы безопасности
    ApiKeyAuth:  # Аутентификация через API-ключ
      type: apiKey  # Тип аутентификации
      in: header  # API-ключ передаётся в заголовке запроса
      name: X-API-KEY  # Название заголовка, в котором передаётся ключ

    BasicAuth:  # HTTP Basic аутентификация
      type: http  # Тип аутентификации
      scheme: basic  # Используется схема Basic

    BearerAuth:  # HTTP Bearer аутентификация
      type: http
      scheme: bearer  # Используется схема Bearer
      bearerFormat: JWT  # Указываем, что используется JWT-токен (необязательно)

    OAuth2Auth:  # OAuth 2.0 аутентификация
      type: oauth2
      flows:
        authorizationCode:  # Используем поток Authorization Code
          authorizationUrl: https://example.com/oauth/authorize  # URL для авторизации
          tokenUrl: https://example.com/oauth/token  # URL для получения токена
          scopes:  # Определяем доступные области (scopes)
            read:users: Чтение данных пользователей
            write:users: Запись данных пользователей

security:  # Применяем глобальные требования безопасности
  - ApiKeyAuth: []  # Требуем наличие API-ключа
  - BearerAuth: []  # Или Bearer токена

paths:
  /users:
    get:
      summary: Получить список пользователей
      security:  # Перекрываем глобальные требования безопасности
        - ApiKeyAuth: []  # Для этого эндпоинта нужен только API-ключ
      responses:
        '200':
          description: Успешный запрос
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    id:
                      type: integer
                      example: 1
                    name:
                      type: string
                      example: Иван Иванов

  /profile:
    get:
      summary: Получить профиль текущего пользователя
      security:
        - BearerAuth: []  # Требуем Bearer токен
      responses:
        '200':
          description: Успешный запрос
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: integer
                    example: 123
                  name:
                    type: string
                    example: Иван Иванов
```
