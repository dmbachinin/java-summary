# Aspect Oriented Programming

```plantext
    В данном файле собрана информация об аспектно-ориентированном программированим(AOP) в Spring Framework
```

- [Aspect Oriented Programming](#aspect-oriented-programming)
  - [Aspect Oriented Programming Основная информация](#aspect-oriented-programming-основная-информация)
  - [AOP в Spring](#aop-в-spring)
    - [@EnableAspectJAutoProxy](#enableaspectjautoproxy)
    - [@Aspect](#aspect)
      - [Подключение через Maven](#подключение-через-maven)
    - [@Before](#before)
    - [@AfterReturning](#afterreturning)
    - [@AfterThrowing](#afterthrowing)
    - [@After](#after)
    - [@Around](#around)
    - [Способы написания Pointcut](#способы-написания-pointcut)
      - [Использование Pointcut классов](#использование-pointcut-классов)
    - [Указание порядка выполнения](#указание-порядка-выполнения)
      - [Использование Ordered Interface](#использование-ordered-interface)
      - [Использование @Order Аннотации](#использование-order-аннотации)
    - [Класс JoinPoint](#класс-joinpoint)
      - [Методы JoinPoint](#методы-joinpoint)

## Aspect Oriented Programming Основная информация

`Aspect Oriented Programming`(AOP) - парадигма программирования, основанная на идее разделения основного и служебного функционала. Служебный функционал записывается в Aspect-классы.

В основе Aspect заключена сквозная логика (cross-cutting logic). По сути это любой служебный функционал, который не относится к бизнес логике.

**Аспекты**: Аспект представляет собой модуль, инкапсулирующий поведение, которое влияет на несколько классов. Аспекты могут быть реализованы в виде обычных классов с аннотациями или в виде классов, реализующих специальные интерфейсы.

**Совет (Advice)**: Это действие (метод внутри аспект класса), которое выполняется аспектом в определённой точке выполнения программы. В Spring есть различные типы советов:

- [`Before`](#before): Совет выполняется перед методом.
- [`After returning`](#afterreturning): Совет выполняется после успешного выполнения метода.
- [`After throwing`](#afterthrowing): Совет выполняется, если метод завершается исключением.
- [`After (finally)`](#after): Совет выполняется после выполнения метода, независимо от исхода.
- [`Around`](#around): Совет, который окружает метод, и может контролировать выполнение.

[**Точка соединения (Join point)**](#класс-joinpoint): Точка в приложении, где можно вставить аспект, такая как вызов метода или обработка исключения.

**Срез (Pointcut)**: Выражение, которое выбирает определённые точки соединения. Срезы определяют, где и когда должен выполняться совет (Advice). Точка соединения определяется с помощью AspectJ expression language.

**Введение (Introduction)**: Добавление новых методов или полей к существующим классам.

**Целевой объект (Target object)**: Объект, к которому применяется аспект. В Spring AOP целевые объекты всегда являются прокси-объектами.

## AOP в Spring

### @EnableAspectJAutoProxy

`@EnableAspectJAutoProxy` - аннотация, которая позволяет использовать Spring AOP Proxy в приложении

**Пример подключения:**

```java
    @Configuration
    @ComponentScan("ru.spring.training.aop")
    @EnableAspectJAutoProxy // Включает поддержку Spring AOP Proxy
    public class Config {

    }
```

### @Aspect

`@Aspect` - аннотация говорит о том, что это не простой класс, а Aspect, т.е. он выполянет сквозную функциональность.

#### Подключение через Maven

```xml
    <!-- Spring AOP зависимость -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>5.3.37</version>
    </dependency>

    <!-- AspectJ Weaver, необходим для выполнения аспектов в рантайме -->
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.22</version>
    </dependency>
```

### @Before

Аннотация `@Before`в Spring AOP используется для указания совета (advice), который должен выполняться **до вызова метода целевого объекта**. Она является частью механизма AspectJ, интегрированного в Spring, и позволяет внедрять дополнительное поведение в программу, не изменяя сам код целевых методов.

`@Before` принимает один параметр — [точку соединения (pointcut expression)](#способы-написания-pointcut), которая определяет, при каких условиях совет должен быть выполнен.

**Пример:**

```java
    @Before("execution(* com.example.service.*.*(..))")
    public void doBeforeTask(JoinPoint joinPoint) {
        // Логика, выполняемая перед каждым методом в пакете com.example.service
        System.out.println("Before executing method: " + joinPoint.getSignature().getName());
    }
```

### @AfterReturning

Аннотация `@AfterReturning` в Spring AOP используется для определения совета (advice), который должен выполняться после того, как метод целевого объекта успешно завершил выполнение и вернул результат, но до присвоения результата этого метода какой-либо переменной. Поэтому с помощью `@AfterReturning` возможно изменять возвращаемый результат метода.

Этот тип совета часто используется для обработки возвращаемых значений, выполнения постобработки или логирования после успешного завершения метода.

**Основные особенности @AfterReturning:**

- **Доступ к возвращаемому значению**: `@AfterReturning` позволяет извлечь возвращаемое значение метода для его дальнейшей обработки или проверки.
- **Условное выполнение**: Совет выполняется только в том случае, если метод успешно завершился, т.е. не было выброшено исключений.
- **Изменение результата**: Позволяет изменить результат, перехваченный работой `@AfterReturning`, чтобы функция вернула уже изменненный результат

**Пример:**

```java
    @Aspect
    public class LoggingAspect {

        @AfterReturning(
        pointcut = "execution(* com.example.service.UserService.findUserById(..))",
        returning = "result" // Возвращаемое значение передаётся в параметр result, который затем используется внутри совета для логирования.
        )
        public void logResult(JoinPoint joinPoint, User result) {
            System.out.println("Method " + joinPoint.getSignature().getName() + " returned " + result);
        }
    }
```

### @AfterThrowing

Аннотация `@AfterThrowing` в Spring AOP предназначена для определения совета (advice), который должен выполняться после того, как метод целевого объекта завершается с выбросом исключения, не влияет на протекание программы при выбрасывании исключений.

Это позволяет обрабатывать исключения или выполнять определённые действия в результате ошибок, возникших в процессе выполнения метода.

**Пример:**

```java
@Aspect
    public class ErrorHandlingAspect {

        @AfterThrowing(
        pointcut = "execution(* com.example.service.UserService.updateUser(..))",
        throwing = "ex" // Исключение передаётся в параметр ex, который затем используется внутри совета для логирования.
        )
        public void logException(JoinPoint joinPoint, Exception ex) {
            System.out.println("An exception " + ex + " has been thrown in " + joinPoint.getSignature().getName());
        }
    }
```

### @After

Аннотация `@After` в Spring AOP представляет собой совет (advice), который выполняется после того, как метод целевого объекта завершился, независимо от того, завершился ли он успешно или с исключением.

Этот тип совета является аналогом блока finally в Java и используется для выполнения кода очистки или завершающих операций, которые должны выполняться в любом случае.

**Пример:**

```java
    @Aspect
    public class ResourceManagementAspect {

        @After("execution(* com.example.service.FileService.openFile(..))")
        public void finalizeFileOperation(JoinPoint joinPoint) {
            // Логирование завершения работы с файлом
            System.out.println("Finished operation on file: " + joinPoint.getSignature().getName());
            
            // Предположим, что в методе могли использоваться ресурсы, которые теперь нужно закрыть
            // (на самом деле код для закрытия ресурсов должен быть в блоке finally в самом методе)
        }
    }
```

### @Around

Аннотация `@Around` в Spring AOP представляет собой один из самых мощных типов советов (advices), позволяя вам полностью управлять выполнением целевого метода.

Это означает, что с помощью `@Around` можно не только выполнять код до и после метода, но и решать, будет ли вообще выполнен сам метод, какие аргументы передавать, какой возвращать результат или нужно ли генерировать исключение.

**Возможности `@Around`**:

- Произвести какие-либо действия до работы target метода;
- Произвести какие-либо действия после работы target метода;
- Получить результат работы target метода/изменить его;
- Предпринять действия, если из target метода выбрасывается исключение;

**Синтаксис аннотации @Around:**

```java
    @Around("execution(* com.example.service.*.*(..))")
    public Object doAroundTask(ProceedingJoinPoint pjp) throws Throwable {
        // Действия до выполнения метода
        System.out.println("Before method: " + pjp.getSignature().getName());
        Object result = null;
        try {
            result = pjp.proceed(); // Выполнение целевого метода
            // Действия после выполнения метода
            System.out.println("After method: " + pjp.getSignature().getName());
        } catch (Throwable e) {
            // Обработка исключения
            System.out.println("Exception in method: " + pjp.getSignature().getName());
            throw e;
        }
        return result; // Возвращаем результат
    }
```

**Пример:**

```java
    @Aspect
    public class PerformanceAspect {
        // В этом примере перед вызовом метода запускается таймер, и после выполнения метода измеряется и выводится время его выполнения. Это может быть полезно для анализа производительности различных частей системы.
        @Around("execution(* com.example.service.*.*(..))")
        public Object measureMethodExecutionTime(ProceedingJoinPoint pjp) throws Throwable {
            long start = System.currentTimeMillis(); // Запуск таймера
            Object result = pjp.proceed(); // Выполнение
            long executionTime = System.currentTimeMillis() - start; // Время работы программы
            System.out.println(pjp.getSignature().getName() + " executed in " + executionTime + "ms");
            return result;
        }
    }
```

### Способы написания Pointcut

`Pointcut` - выражение, описывающее где должен быть применён Advice.

Шаблон для написания `Pointcut`:
execution(область видимости? **Тип возаращаемого значения** Класс, в котором указан метод? **Имя метода(Парамтеры)** исключениен, которое выбрасывает метод?)

- Элементы с ? - необязательные
- **Жирные элементы** - обязательные

wildcard(*) - в шаблоне `Pointcut` означает, что на его месте может быть что угодно

**Комбинирование `Pointcut`** происходит при помощи логических операторов `&&`, `||`, `!`

**Пример написания:**

```java
    @Component
    @Aspect
    public class ClassAspect {
        @Before("execution(* com.example.service.MyService.*(..))") //  Совет будет применен ко всем методам в классе MyService
        public void m1() {...}

        @Before("@annotation(com.example.annotations.MyAnnotation)") // Совет будет применяться ко всем методам, которые аннотированы @MyAnnotation
        public void m2() {...}

        @Before("execution(public String com.example.*.*(..))") // Совет будет применен ко всем методам, которые возвращают String и находятся в пакете com.example
        public void m3() {...}

        @Before("execution(* *.*(com.example.domain.MyClass))") // Совет будет применяться к любым методам, которые принимают в качестве параметра объект MyClass
        public void m4() {...}

        // Комбинирование условий

        @Before("execution(* com.example.service.*.*(String, ..)) && @annotation(org.springframework.transaction.annotation.Transactional)") // Это выражение применяет совет к методам, которые принимают String как первый аргумент и аннотированы @Transactional
        public void m5() {...}

        @Before("execution(public void getBook())") // Совет будет применяться ко всем методам public void getBook(), которые будут найдены
        public void m6() {...}

        // Применение одного совета к ряду методов
        @Before("execution(* com.example.*.get*(..))") // Применит совет ко всем методам, которые начинаются на get во всех классах пакета com.example
        public void beforeGetter(JoinPoint joinPoint) {
            System.out.println("Before method: " + joinPoint.getSignature().getName());
        }

        // Объявление pointcut для использования во множесте Advice-ов

        // Определение pointcut с именем "allServiceMethods"
        @Pointcut("execution(* com.example.service.*.*(..))")
        public void allServiceMethods() {/* Код в данном месте не будет выполняться */} 

        @Before("allServiceMethods()") // Использование pointcut, который был объявлен выше
        public void m7(JoinPoint joinPoint) {}
    }
```

#### Использование Pointcut классов

```java
    // Объявляем класс с Pointcut
    public class MyPointcuts {
        @Pointcut("* get*()")
        public void allGetMethods() {}
    }

    @Before("MyPointcuts.allGetMethods()") // Для использования класса с Pointcut необходимо написать полное имя метода с указанием класса
    public void m7(JoinPoint joinPoint) {}

```

### Указание порядка выполнения

В Spring AOP, когда несколько советов (advices) применяются к одному и тому же методу, может возникнуть необходимость контролировать порядок их выполнения. Это особенно важно, если логика одного совета зависит от результата выполнения другого. Spring предоставляет несколько способов управления порядком выполнения советов.

#### Использование Ordered Interface

Один из способов контролировать порядок выполнения советов — реализация интерфейса `org.springframework.core.Ordered.` Этот интерфейс позволяет указать порядок выполнения аспектов, где меньшее число указывает на более высокий приоритет (т.е., совет с меньшим значением order будет выполнен раньше). Обычно используют числа кратные 10, чтобы было удобно в случае необходимость добавить промежуточный аспект между двумя другими

**Пример:**

```java
@Aspect
public class SecurityAspect implements Ordered {
    @Override
    public int getOrder() {
        return 10; // Выполнять первым (можно использовать отрицательные значени)
    }

    @Before("MyPointcuts.allGetMethods()")
    public void checkSecurity(JoinPoint joinPoint) {
        // Проверка безопасности
    }
}

@Aspect
public class TransactionAspect implements Ordered {
    @Override
    public int getOrder() {
        return 20; // Выполнять вторым (можно использовать отрицательные значени)
    }

    @Before("MyPointcuts.allGetMethods()")
    public void startTransaction(JoinPoint joinPoint) {
        // Начало транзакции
    }
}
```

#### Использование @Order Аннотации

```java
@Aspect
@Order(10) // Выполнять первым (можно использовать отрицательные значени)
public class SecurityAspect {
    @Before("MyPointcuts.allGetMethods()")
    public void checkSecurity(JoinPoint joinPoint) {
        // Проверка безопасности
    }
}

@Aspect
@Order(20) // Выполнять вторым (можно использовать отрицательные значени)
public class TransactionAspect {
    @Before("MyPointcuts.allGetMethods()")
    public void startTransaction(JoinPoint joinPoint) {
        // Начало транзакции
    }
}
```

### Класс JoinPoint

`JoinPoint` представляет собой точку в программе, где активируется аспект (например, перед выполнением метода или после него). Это ключевой элемент в определении того, где и когда советы (advices) должны применяться в рамках аспектно-ориентированного программирования

`JoinPoint` предоставляет подробную информацию о текущем месте выполнения программы, где применяется аспект. Эта информация может быть использована аспектом для выполнения различных действий, таких как логирование, проверки безопасности, управление транзакциями и другие

**JoinPoint прописывается в параметрах advices:**

#### Методы JoinPoint

```java
    Object[] args = joinPoint.getArgs(); // Возвращает массив объектов, содержащих аргументы, с которыми был вызван метод
    Object proxy = joinPoint.getThis(); // Возвращает прокси-объект, для которого был вызван соответствующий метод
    Object target = joinPoint.getTarget(); // Возвращает целевой объект, на котором выполняется метод (отличается от прокси, если используется проксирование)

    Signature signature = joinPoint.getSignature(); // Возвращает сигнатуру метода, который был вызван.
    String name = signature.getName(); //  Возвращает имя элемента (метода, конструктора и т.д.), к которому применяется аспект
    Class<?> declaringType = signature.getDeclaringType(); // Возвращает объект Class, который представляет тип, объявляющий данный элемент
    String declaringTypeName = signature.getDeclaringTypeName(); // Возвращает имя класса, который объявляет элемент
    int modifiers = signature.getModifiers(); // Возвращает модификаторы элемента в виде целого числа, которые могут быть интерпретированы с помощью класса java.lang.reflect.Modifier
    String shortString = signature.toShortString(); // Возвращает краткое строковое представление сигнатуры
    String longString = signature.toLongString(); // Возвращает полное строковое представление сигнатуры, включая типы параметров и возвращаемый тип

    MethodSignature methodSignature = (MethodSignature) joinPoint.getSignature(); // Приведение сигнатуры к MethodSignature (Предоставляет дополнительные методы для доступа к информации)
    // Получение информации о методе
    String name = methodSignature.getName(); // Возвращает имя метода
    Class<?>[] parameterTypes = methodSignature.getParameterTypes(); // Возвращает массив типов параметров метода
    Class<?> returnType = methodSignature.getReturnType(); // Возвращает тип возвращаемого значения метода
    String[] parameterNames = methodSignature.getParameterNames(); // Возвращает имена параметров метода
    Class<?>[] exceptionTypes = methodSignature.getExceptionTypes(); // Возвращает массив типов исключений, которые метод объявляет, что может бросить
```
