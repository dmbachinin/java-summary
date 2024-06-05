Шаблоны и паттерны проектирования в программировании имеют схожее значение, но их различие кроется в контексте применения и масштабе.

СТАТЬЯ НА ТЕМУ: https://habr.com/ru/companies/vk/articles/325492/

### Шаблоны проектирования (Design Patterns)
Шаблоны проектирования — это повторно используемые решения для общих проблем, которые возникают в процессе разработки программного обеспечения. Они не являются готовым кодом, а скорее описанием решения, которое может быть адаптировано к конкретной ситуации. Шаблоны проектирования делятся на три основные категории:

 - [Порождающие (Creational)](#порождающие-creational)
 - [Структурные (Structural)](#структурные-structural)
 - [Поведенческие (Behavioral)](#поведенческие-behavioral)

## **Порождающие (Creational):** 
**Простое определение**

```Порождающие шаблоны описывают создание (instantiate) объекта или группы связанных объектов.```

**Научное определение**

```В программной инженерии порождающими называют шаблоны, которые используют механизмы создания объектов, чтобы создавать объекты подходящим для данной ситуации способом. Базовый способ создания может привести к проблемам в архитектуре или к её усложнению. Порождающие шаблоны пытаются решать эти проблемы, управляя способом создания объектов.```

**Примеры шаблонов**:
   - [Одиночка (Singleton)](#одиночка-singleton)
   - [Фабричный метод (Factory Method)](#фабричный-метод-factory-method)
   - [Абстрактная фабрика (Abstract Factory)](#абстрактная-фабрика-abstract-factory)
   - [Строитель (Builder)](#строитель-builder)
   - [Прототип (Prototype)](#прототип-prototype)

### Одиночка (Singleton)

**Аналогия** \
У страны может быть только один президент. Он должен действовать, когда того требуют обстоятельства и долг. В данном случае президент — одиночка.

**Суть** \
Шаблон «Одиночка» позволяет ограничивать создание класса единственным объектом. Это удобно, когда для координации действий в рамках системы требуется, чтобы объект был единственным в своём классе.

**Минусы**
- Данный паттерн `вносит в приложение глобальное состояние` в программу, так что изменение в одном месте может повлиять на всю отсальную программу
- `Трудности с тестированием`, из-за невозможности изменить экзмепляр Singleton
- `Нарушение принципа инверсии зависимостей (Dependency Inversion Principle)`. Использование Singleton может привести к нарушению этого принципа, так как компоненты будут напрямую зависеть от конкретной реализации, а не от абстракций.

**Примеры реализации** \
`Eager Initialization (Раняя инициализация)`. Этот подход создает экземпляр Singleton при загрузке класса. Он прост и потокобезопасен, но экземпляр создается независимо от необходимости его использования.
```java
public class Singleton {
    // Единственный экземпляр класса, созданный при загрузке класса
    private static final Singleton INSTANCE = new Singleton();

    // Приватный конструктор предотвращает создание экземпляров извне
    private Singleton() {
        // Инициализация ресурсов
    }

    // Публичный метод, который возвращает единственный экземпляр класса
    public static Singleton getInstance() {
        return INSTANCE;
    }

    // Другие методы класса
    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```
`Ленивый подход с двойной проверкой`. Данный подход обеспечивает создание экземпляра класса только при необходимости (ленивая инициализация) и использует двойную проверку для обеспечения потокобезопасности.

```java
public class Singleton {
    // Единственный экземпляр класса, объявленный как volatile для гарантии видимости изменений
    private static volatile Singleton instance;

    // Приватный конструктор предотвращает создание экземпляров извне
    private Singleton() {
        // Инициализация ресурсов
    }

    // Публичный метод, который возвращает единственный экземпляр класса
    public static Singleton getInstance() {
        if (instance == null) { // Первая проверка (без синхронизации)
            synchronized (Singleton.class) { // Синхронизация нужна, чтобы предотвратить создание более одного экземпляра в многопоточной среде.
                if (instance == null) { // Вторая проверка (с синхронизацией)
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    // Другие методы класса
    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```

`Подход с внутренним статическим классом`. Этот подход использует внутренний статический класс для создания экземпляра Singleton, что гарантирует потокобезопасность и ленивую инициализацию.

```java
public class Singleton {
    // Приватный конструктор предотвращает создание экземпляров извне
    private Singleton() {
        // Инициализация ресурсов
    }

    // Внутренний статический класс, который содержит единственный экземпляр Singleton
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    // Публичный метод, который возвращает единственный экземпляр класса
    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }

    // Другие методы класса
    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```

### Фабричный метод (Factory Method)

### Абстрактная фабрика (Abstract Factory)

### Строитель (Builder)

**Аналогия** \
Допустим, вы пришли в забегаловку, заказали бургер дня, и вам выдали его без вопросов. Это пример «Простой фабрики». Но иногда логика создания состоит из большего количества шагов. К примеру, при заказе бургера дня есть несколько вариантов хлеба, начинки, соусов, дополнительных ингредиентов. В таких ситуациях помогает шаблон «Строитель».

**Суть** \
Шаблон позволяет создавать разные свойства объекта, избегая загрязнения конструктора (constructor pollution). Это полезно, когда у объекта может быть несколько свойств. Или когда создание объекта состоит из большого количества этапов.

**Примеры реализации** \

```java 
public class User {
    private final String firstName;
    private final String lastName;
    private final int age;

    private User(UserBuilder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
    }
    // Внутренний статический класс для реализации строителя
    public static class UserBuilder {
        private String firstName;
        private String lastName;
        private int age;

        public UserBuilder setFirstName(String firstName) {
            this.firstName = firstName;
            return this;
        }

        public UserBuilder setLastName(String lastName) {
            this.lastName = lastName;
            return this;
        }

        public UserBuilder setAge(int age) {
            this.age = age;
            return this;
        }
        // Вызываем инициализацию родительского класса
        public User build() {
            return new User(this);
        }
    }

    // getters, setters, toString, etc.
}

// Создание
User user = new User.UserBuilder()
        .setFirstName("John")
        .setLastName("Doe")
        .setAge(30)
        .build();

```

### Прототип (Prototype)

## **Структурные (Structural):**

**Простое определение**

Эти шаблоны в основном посвящены компоновке объектов (object composition). То есть тому, как сущности могут друг друга использовать. Ещё одно объяснение: структурные шаблоны помогают ответить на вопрос «Как построить программный компонент?»

**Научное определение**

Структурными называют шаблоны, которые облегчают проектирование, определяя простой способ реализации взаимоотношений между сущностями.


**Примеры шаблонов**:
   - Адаптер (Adapter)
   - Мост (Bridge)
   - Компоновщик (Composite)
   - Декоратор (Decorator)
   - [Фасад (Facade)](#фасад-facade)
   - Легковес (Flyweight)
   - Заместитель (Proxy)

### Фасад (Facade)

**Аналогия**\
Как включить компьютер? Вы скажете: «Нажать кнопку включения». Это потому, что вы используете простой интерфейс, предоставляемый компьютером наружу. А внутри него происходит очень много процессов. Простой интерфейс для сложной подсистемы — это фасад.

**Суть**\
Шаблон «Фасад» предоставляет упрощённый интерфейс для сложной подсистемы. «Фасад» — это объект, предоставляющий упрощённый интерфейс для более крупного тела кода, например библиотеки классов.

**Примеры реализации** \
```java
// Классы, представляющие компоненты домашнего кинотеатра
class DVDPlayer {
    public void on() {}
    public void play(String movie) {}
    public void off() {}
}

class Amplifier {
    public void on() {}
    public void setVolume(int level) {}
    public void off() {}
}

class Projector {
    public void on() {}
    public void setInput(String input) {}
    public void off() {}
}

class Lights {
    public void dim(int level) {}
}

// Фасад для системы домашнего кинотеатра
class HomeTheaterFacade {
    private DVDPlayer dvdPlayer;
    private Amplifier amplifier;
    private Projector projector;
    private Lights lights;

    public HomeTheaterFacade(DVDPlayer dvdPlayer, Amplifier amplifier, Projector projector, Lights lights) {
        this.dvdPlayer = dvdPlayer;
        this.amplifier = amplifier;
        this.projector = projector;
        this.lights = lights;
    }

    public void watchMovie(String movie) {
        System.out.println("Get ready to watch a movie...");
        lights.dim(10);
        projector.on();
        projector.setInput("DVD");
        amplifier.on();
        amplifier.setVolume(5);
        dvdPlayer.on();
        dvdPlayer.play(movie);
    }

    public void endMovie() {
        System.out.println("Shutting movie theater down...");
        dvdPlayer.off();
        amplifier.off();
        projector.off();
        lights.dim(100);
    }
}

// Использование фасада
public class FacadePatternDemo {
    public static void main(String[] args) {
        DVDPlayer dvdPlayer = new DVDPlayer();
        Amplifier amplifier = new Amplifier();
        Projector projector = new Projector();
        Lights lights = new Lights();

        HomeTheaterFacade homeTheater = new HomeTheaterFacade(dvdPlayer, amplifier, projector, lights);
        
        homeTheater.watchMovie("Inception");
        homeTheater.endMovie();
    }
}

```

## **Поведенческие (Behavioral):**

**Простое определение**



**Научное определение**



**Примеры шаблонов**:

   - Цепочка обязанностей (Chain of Responsibility)
   - Команда (Command)
   - Интерпретатор (Interpreter)
   - Итератор (Iterator)
   - Посредник (Mediator)
   - Хранитель (Memento)
   - Наблюдатель (Observer)
   - Состояние (State)
   - Стратегия (Strategy)
   - Шаблонный метод (Template Method)
   - Посетитель (Visitor)
