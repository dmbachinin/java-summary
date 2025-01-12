# MapStruct

```plantext
    MapStruct — это фреймворк для автоматического преобразования (маппинга) объектов из одного типа в другой. Он используется для написания преобразований между DTO (Data Transfer Object) и Entity-классами или любыми другими моделями
```

- [MapStruct](#mapstruct)
  - [Подключение MapStruct](#подключение-mapstruct)
    - [Подключение через Maven](#подключение-через-maven)
    - [Подключение через Gradle](#подключение-через-gradle)
  - [Принципы работы MapStruct](#принципы-работы-mapstruct)
    - [Пример реализации](#пример-реализации)
  - [Основные аннотации в MapStruct](#основные-аннотации-в-mapstruct)

## Подключение MapStruct

### Подключение через Maven

```xml
    <dependencies>
        <!-- MapStruct библиотека -->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
            <version>1.5.5.Final</version> <!-- Убедитесь, что версия актуальна -->
        </dependency>
        <!-- Аннотации MapStruct -->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-processor</artifactId>
            <version>1.5.5.Final</version>
            <scope>provided</scope> <!-- Используется только для компиляции -->
        </dependency>
    </dependencies>
```

### Подключение через Gradle

```groovy
    plugins {
        id 'java'
    }

    dependencies {
        // MapStruct библиотека
        implementation 'org.mapstruct:mapstruct:1.5.5.Final'
        annotationProcessor 'org.mapstruct:mapstruct-processor:1.5.5.Final'
    }
```

## Принципы работы MapStruct

MapStruct генерирует код на этапе компиляции, создавая класс с реализацией интерфейсов, которые вы определили как мапперы. Основной принцип:

- Вы описываете методы для преобразования объектов.
- MapStruct автоматически создает реализацию на основе структуры полей.

Пример: **если у объектов поля совпадают по именам и типам, MapStruct автоматически маппирует их**. Если поля отличаются, вы можете указать это с помощью аннотаций

### Пример реализации

```java
    @Entity
    @Getter
    @Setter
    @AllArgsConstructor
    public class UserEntity { // Объект сущности 
        private Long id;
        private String username;
        private String email;
    }

    @Getter
    @Setter
    @AllArgsConstructor
    public class UserDTO { // Объекта DTO, которые будет передаваться по сети (скорее всего будет превращаться в JSON)
        private String username;
        private String email;
    }

    import org.mapstruct.Mapper;
    import org.mapstruct.factory.Mappers;

    @Mapper // Указывает, что это интерфейс маппера
    public interface UserMapper { // Создайте интерфейс маппера
        // Ссылка на сгенерированный маппер
        UserMapper INSTANCE = Mappers.getMapper(UserMapper.class); // Singleton-объект для вызова методов маппинга

        // Маппинг метода
        UserDTO toDto(UserEntity userEntity); // Автоматически сопоставит поля с одинаковыми названиями

        UserEntity toEntity(UserDTO userDTO); // Автоматически сопоставит поля с одинаковыми названиями
    }
```

## Основные аннотации в MapStruct

```java
    @Mapper // Это основная аннотация, которая указывает, что интерфейс является маппером. Она отвечает за генерацию реализации маппера
    // Реализация маппера создается в классе с именем UserMapperImpl
    public interface UserMapper {}

    @Mapper(componentModel = "spring") // Указывает, что маппер должен быть Spring-бином
    //  Реализация будет доступна как бин Spring (например, UserMapperImpl)
    public interface UserMapper {
        
        @Mapping(source = "userName", target = "username") // Используется для указания явного маппинга между полями объектов
        @Mapping(source = "email", target = "contactEmail")
        @Mapping(target = "status", constant = "ACTIVE")
        @Mapping(target = "age", defaultValue = "18")
        UserDTO toDto(UserEntity userEntity);
        // Параметры:
        //  * source: имя поля в исходном объекте
        //  * target: имя поля в целевом объекте.
        //  * ignore: пропускает маппинг для указанного поля (true/false).
        //  * defaultValue: задает значение по умолчанию, если поле в исходном объекте null.
        //  * constant: задает постоянное значение для поля в целевом объекте


        // Параметр expression в аннотации @Mapping позволяет указать Java-выражение, которое будет выполнено для получения значения целевого поля. Это полезно, когда стандартного маппинга недостаточно и требуется вычислить значение "на лету"
        
        // Библиотеки, которые можно исопльзовать: 
        //  * Java Standard Library: Math, DateTimeFormatter, Pattern, и другие классы стандартной библиотеки
        //  * Apache Commons: StringUtils, NumberUtils, DateUtils, и т.д
        //  * Guava: методы из Strings или Lists
        //  * Java Time API (встроенная в Java 8 и новее): LocalDateTime или DateTimeFormatter

        @Mapping(target = "firstName", expression = "java(userEntity.getFullName().split(\" \")[0])") // Разделение исходного значения на кусочки
        @Mapping(target = "lastName", expression = "java(userEntity.getFullName().split(\" \")[1])")
        UserDTO toDto(UserEntity userEntity);

        @Mapping(target = "age", expression = "java(Integer.parseInt(userEntity.getAgeString()))") // expression может использоваться для вызова методов, преобразования форматов или выполнения других вычислений
        UserDTO toDto(UserEntity userEntity);

        @Mappings({ // Позволяет объединить несколько мапперов
            @Mapping(source = "userName", target = "username"),
            @Mapping(source = "email", target = "contactEmail")
        })
        UserDTO toDto(UserEntity userEntity);

        @BeanMapping(ignoreByDefault = true) // Используется для настройки поведения маппинга в целом
        @Mapping(source = "id", target = "identifier")
        UserDTO toDto(UserEntity userEntity);
        // Параметры:
        //  * ignoreByDefault: игнорирует все поля, кроме явно указанных в @Mapping
        //  * resultType: задает тип результата
        //  * qualifiedByName: указывает квалифицированный метод для кастомного маппинга

        @InheritConfiguration(name = "toDto") // Позволяет наследовать настройки маппинга из другого метода
        UserEntity toEntity(UserDTO userDTO);

        @InheritInverseConfiguration // Наследует обратный маппинг для метода
        UserEntity toEntity(UserDTO userDTO);

        // @MappingTarget Используется для обновления уже существующего объекта, а не создания нового
        void updateEntityFromDto(UserDTO dto, @MappingTarget UserEntity entity);

        @AfterMapping // Используются для выполнения действий после маппинга
        @BeforeMapping // Используются для выполнения действий до маппинга
        default void afterMapping(@MappingTarget UserDTO dto) {
            dto.setGeneratedAt(System.currentTimeMillis());
        }

        @MapperConfig // Используется для настройки общих правил маппинга. Позволяет переиспользовать настройки в нескольких мапперах
        public interface CommonMapperConfig {
            @Mapping(source = "id", target = "identifier")
            @Mapping(source = "email", target = "contactEmail")
            UserDTO toDto(UserEntity userEntity);
        }

        @Mapper(config = CommonMapperConfig.class)
        public interface UserMapper extends CommonMapperConfig {}

        @ValueMapping(source = "ACTIVE", target = "ENABLED") // Используется для маппинга Enum значений
        @ValueMapping(source = "INACTIVE", target = "DISABLED")
        @ValueMapping(source = "UNKNOWN", target = "DEFAULT")
        TargetStatus map(Status status);
    }
```
