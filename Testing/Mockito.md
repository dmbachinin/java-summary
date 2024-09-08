# Mockito

`Mockito` — это популярная библиотека для модульного тестирования в Java, которая позволяет создавать mock-объекты для тестирования поведения других объектов в изоляции. Она широко используется для упрощения тестирования кода, особенно когда есть зависимости, которые трудно или невозможно протестировать напрямую.

- [Mockito](#mockito)
  - [Основные возможности Mockito](#основные-возможности-mockito)
    - [Подключение с использованием Maven](#подключение-с-использованием-maven)
    - [Основные аннотации Mockito](#основные-аннотации-mockito)
    - [Основные методы Mockito](#основные-методы-mockito)
    - [Пример использования](#пример-использования)

## Основные возможности Mockito

1. **Создание mock-объектов:** позволяет создать имитацию (mock) объекта, который можно настроить для возврата определенных значений или выполнения определенных действий.
2. **Захват аргументов:** можно захватывать аргументы методов для дальнейшей проверки.
3. **Верификация поведения:** проверяет, что методы были вызваны с определенными аргументами, определенное количество раз и в определенной последовательности.
4. **Стаббинг:** настройка методов mock-объектов для возврата определенных значений или выбрасывания исключений.

### Подключение с использованием Maven

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>4.0.0</version>
    <scope>test</scope>
</dependency>
```

### Основные аннотации Mockito

```java
    @Mock // Создает mock-объект, который используется для имитации поведения реального объекта. Mock-объекты позволяют задать, какие значения методы должны возвращать или какие действия выполнять, без необходимости использования реальной реализации
    private UserRepository mockRepository;
    //  * Изолирует тестируемый код от его зависимостей. Полезно для тестирования в изоляции, особенно когда реальные зависимости требуют сложной настройки или доступа к внешним ресурсам

    @Spy // Создает spy-объект, который позволяет следить за реальным объектом и частично замещать его методы. Spy-объекты вызывают реальные методы объекта, если не настроено иное поведение
    private UserMapper userMapper = new UserMapper();
    //  * Полезно для тестирования частичного поведения объекта, где часть методов должна быть замещена, а часть — использовать реальную логику

    @InjectMocks // Создает экземпляр класса и автоматически внедряет в него mock-объекты. Позволяет автоматизировать процесс внедрения зависимостей в тестируемый объект
    private UserService userService; // Данный объект будет тестироваться (Не нужно вызывать конструктор)
    //  * Упрощает настройку тестируемого объекта, автоматически внедряя все необходимые зависимости (mock-объекты)
    //  * Предполагает, что Mockito будет автоматически внедрять mock-объекты в конструктор или сеттеры тестируемого класса. Однако для этого ваш тестируемый класс должен иметь конструктор, принимающий mock-объекты. Если конструктор отсутствует или не соответствует ожиданиям Mockito, тогда mock-объекты не будут внедрены корректно

    @Captor // Создает аргумент-каптор, который используется для захвата аргументов, передаваемых в методы mock-объектов. Капторы позволяют захватывать и проверять значения аргументов, передаваемых в методы
    private ArgumentCaptor<User> userCaptor;
    //  * Полезно для проверки значений аргументов, которые передавались в методы mock-объектов, особенно когда нужно убедиться, что метод был вызван с правильными параметрами
```

### Основные методы Mockito

```java
    UserRepository mockRepository = mock(UserRepository.class); // Создает mock-объект заданного класса или интерфейса. Полезно для создания тестовых объектов, которые имитируют поведение реальных объектов без необходимости их реальной реализации
    List<String> spyList = spy(new ArrayList<>()); // Создает spy-объект реального объекта, позволяющий следить за вызовами методов и частично замещать их поведение.  Полезно для тестирования реальных объектов, когда необходимо проверять вызовы их методов или частично замещать их поведение
    when(mockRepository.findById(1)).thenReturn(new User()); //  Настраивает поведение метода mock-объекта. Используется для указания возвращаемого значения или выбрасываемого исключения
    when(mockRepository.findById(anyInt())).thenThrow(new RuntimeException()); // Также можно настроить выбрасывание исключения
    doReturn(), doThrow(), doAnswer(), doNothing(), doCallRealMethod() // Альтернативные методы для настройки поведения mock-объектов. Используются в случаях, когда метод, который необходимо настроить, является final, private или protected
    doReturn(new User()).when(mockRepository).findById(1);
    doThrow(new RuntimeException()).when(mockRepository).delete(any(User.class));

    verify(mockRepository).findById(1); // Проверяет, что метод был вызван один раз с аргументом 1
    verify(mockRepository, times(2)).findById(1); // Проверяет, что метод был вызван дважды
    verify(mockSomeService, times(0)).someMethod(); // // Проверяем, что метод не был вызван
    verify(mockRepository, never()).delete(any(User.class)); // Проверяет, что метод никогда не был вызван
    //  * Вызываются после выполнения основного метода

    @Captor
    ArgumentCaptor<User> userCaptor; // Захватывает аргументы, переданные в методы mock-объектов, для дальнейшей проверки
    verify(mockRepository).save(userCaptor.capture());
    User capturedUser = userCaptor.getValue();

    reset(mockRepository); // Сбрасывает настройки mock-объектов, позволяя заново настроить их методы
    //  * Полезно для повторного использования mock-объектов с новой настройкой в рамках одного теста или набора тестов
```

### Пример использования

```java
    import static org.mockito.Mockito.*;
    import static org.junit.jupiter.api.Assertions.*;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;
    import org.mockito.*;

    public class UserServiceTest {

        @Mock
        private UserRepository mockRepository;

        @Spy
        private UserMapper userMapper = new UserMapper();

        @InjectMocks
        private UserService userService;

        @Captor
        private ArgumentCaptor<User> userCaptor;

        @BeforeEach
        public void setUp() {
            MockitoAnnotations.openMocks(this); // Инициализация аннотаций Mockito
        }

        @Test
        public void testGetUserById() {
            // Настройка mock-объекта
            User user = new User();
            user.setId(1);
            user.setName("John");
            when(mockRepository.findById(1)).thenReturn(user);

            User result = userService.getUserById(1); // Вызов метода тестируемого объекта

            // Проверка результата и вызова метода mock-объекта
            assertNotNull(result);
            assertEquals("John", result.getName());
            verify(mockRepository).findById(1);
        }

        @Test
        public void testSaveUser() {
            User user = new User();
            user.setId(1);
            user.setName("John");

            userService.saveUser(user);

            verify(mockRepository).save(userCaptor.capture());
            User capturedUser = userCaptor.getValue();

            assertEquals("John", capturedUser.getName());
        }

        @Test
        public void testUpdateUser() {
            User user = new User();
            user.setId(1);
            user.setName("John");
            doReturn(user).when(userMapper).map(any(UserDto.class));

            UserDto userDto = new UserDto();
            userDto.setId(1);
            userDto.setName("John Updated");

            userService.updateUser(userDto);

            verify(userMapper).map(userDto);
            verify(mockRepository).save(userCaptor.capture());
            User capturedUser = userCaptor.getValue();

            assertEquals("John Updated", capturedUser.getName());
        }

        @Test
        public void testResetMock() {
            User user = new User();
            user.setId(1);
            user.setName("John");
            when(mockRepository.findById(1)).thenReturn(user);

            userService.getUserById(1);
            verify(mockRepository).findById(1);

            reset(mockRepository);

            when(mockRepository.findById(2)).thenReturn(new User());
            userService.getUserById(2);
            verify(mockRepository).findById(2);
        }
    }
```
