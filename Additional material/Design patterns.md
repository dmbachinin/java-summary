# Шаблоны проектирования (Design Patterns)

```plantext
    Паттерны проектирования — это повторяющиеся решения общих задач при разработке программного обеспечения. Они помогают сделать код более структурированным, гибким и удобным для сопровождения.
```

- [Шаблоны проектирования (Design Patterns)](#шаблоны-проектирования-design-patterns)
  - [Порождающие паттерны](#порождающие-паттерны)
    - [Одиночка (Singleton)](#одиночка-singleton)
    - [Фабричный метод (Factory Method)](#фабричный-метод-factory-method)
    - [Абстрактная фабрика (Abstract Factory)](#абстрактная-фабрика-abstract-factory)
    - [Строитель (Builder)](#строитель-builder)
    - [Прототип (Prototype)](#прототип-prototype)
    - [Структурные паттерны проектирования](#структурные-паттерны-проектирования)
  - [Структурные паттерны](#структурные-паттерны)
    - [Адаптер (Adapter)](#адаптер-adapter)
    - [Мост (Bridge)](#мост-bridge)
    - [Компоновщик (Composite)](#компоновщик-composite)
    - [Декоратор (Decorator)](#декоратор-decorator)
    - [Фасад (Facade)](#фасад-facade)
    - [Легковес (Flyweight)](#легковес-flyweight)
    - [Заместитель (Proxy)](#заместитель-proxy)
  - [Поведенческие паттерны](#поведенческие-паттерны)
    - [Цепочка обязанностей (Chain of Responsibility)](#цепочка-обязанностей-chain-of-responsibility)
    - [Команда (Command)](#команда-command)
    - [Итератор (Iterator)](#итератор-iterator)
    - [Наблюдатель (Observer)](#наблюдатель-observer)
    - [Посредник (Mediator)](#посредник-mediator)
    - [Снимок (Memento)](#снимок-memento)
    - [Состояние (State)](#состояние-state)
    - [Стратегия (Strategy)](#стратегия-strategy)
    - [Шаблонный метод (Template Method)](#шаблонный-метод-template-method)
    - [Посетитель (Visitor)](#посетитель-visitor)

## Порождающие паттерны

**Простое определение**:

```Порождающие шаблоны описывают создание (instantiate) объекта или группы связанных объектов.```

**Научное определение**:

```В программной инженерии порождающими называют шаблоны, которые используют механизмы создания объектов, чтобы создавать объекты подходящим для данной ситуации способом. Базовый способ создания может привести к проблемам в архитектуре или к её усложнению. Порождающие шаблоны пытаются решать эти проблемы, управляя способом создания объектов.```

### Одиночка (Singleton)

**Аналогия:**

У страны может быть только один президент. Он должен действовать, когда того требуют обстоятельства и долг. В данном случае президент — одиночка.

**Суть:**

Шаблон «Одиночка» позволяет ограничивать создание класса единственным объектом. Это удобно, когда для координации действий в рамках системы требуется, чтобы объект был единственным в своём классе.

**Минусы:**

- Данный паттерн `вносит в приложение глобальное состояние` в программу, так что изменение в одном месте может повлиять на всю отсальную программу
- `Трудности с тестированием`, из-за невозможности изменить экзмепляр Singleton
- `Нарушение принципа инверсии зависимостей (Dependency Inversion Principle)`. Использование Singleton может привести к нарушению этого принципа, так как компоненты будут напрямую зависеть от конкретной реализации, а не от абстракций.

**Примеры реализации:**

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

**Фабричный метод** — это порождающий паттерн проектирования, который определяет интерфейс для создания объектов, но оставляет выбор того, какой объект создавать, на усмотрение подклассов.

Этот паттерн делегирует создание объекта на уровень подклассов, позволяя им решать, какой именно объект должен быть создан. То есть, основной класс определяет общий интерфейс для создания объекта, а подклассы решают, какой конкретный тип объекта нужно создавать.

**Проблема, которую решает Фабричный метод:**

Паттерн Фабричный метод решает проблему зависимости от конкретных классов в коде. Когда в коде необходимо создать объект какого-то класса, это ведет к жёсткой связке с этим классом. Если нужно будет заменить один класс другим, придется изменять код в нескольких местах. Это нарушает принцип открытости/закрытости (Open/Closed Principle)

**Как работает Фабричный метод:**

1. Определяется базовый класс или интерфейс для объектов, которые нужно создать. Этот класс будет использоваться клиентами, не зная о его конкретной реализации.
2. Создается абстрактный класс или интерфейс "Создатель" (Creator), который объявляет метод фабрики — абстрактный метод для создания объектов. Этот метод должен возвращать объекты базового типа.
3. Подклассы реализуют конкретные версии фабричного метода, создавая конкретные объекты

**Пример на Java:**

```java
    // 1. Абстрактный продукт (базовый класс)
    abstract class Product {
        public abstract void use();
    }

    // Конкретный продукт А
    class ConcreteProductA extends Product {
        public void use() {
            System.out.println("Using product A");
        }
    }

    // Конкретный продукт B
    class ConcreteProductB extends Product {
        public void use() {
            System.out.println("Using product B");
        }
    }
    
    // Абстрактный создатель, который определяет метод factoryMethod для создания объекта
    abstract class Creator {
        public abstract Product factoryMethod();
    }

    // Конкретные создатели
    class ConcreteCreatorA extends Creator {
        public Product factoryMethod() {
            return new ConcreteProductA();
        }
    }

    // Конкретные создатели
    class ConcreteCreatorB extends Creator {
        public Product factoryMethod() {
            return new ConcreteProductB();
        }
    }
```

### Абстрактная фабрика (Abstract Factory)

**Абстрактная фабрика** — это порождающий паттерн проектирования, который предоставляет интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов, не указывая их конкретные классы. Паттерн позволяет абстрагироваться от конкретных классов создаваемых объектов, вместо этого предоставляя интерфейс для работы с продуктами

**Пример реальной задачи:**

Представьте себе кроссплатформенное приложение с графическим интерфейсом, в котором для каждой операционной системы (Windows, macOS, Linux) требуется своё представление пользовательского интерфейса. Например, кнопки, окна, чекбоксы должны выглядеть и вести себя по-разному в зависимости от платформы, но при этом вам нужно гарантировать, что интерфейс для всех этих элементов будет единым. Абстрактная фабрика поможет организовать создание таких элементов, абстрагируя от платформы

**Как работает Абстрактная фабрика:**

1. Абстрактные продукты: Определяются интерфейсы для каждого типа объектов, которые могут быть созданы (например, Button, Checkbox).
2. Конкретные продукты: Для каждого семейства создаются классы, которые реализуют интерфейсы продуктов (например, WindowsButton, MacButton).
3. Абстрактная фабрика: Определяется интерфейс фабрики, который описывает методы для создания всех видов продуктов (например, GUIFactory с методами createButton() и createCheckbox()).
4. Конкретные фабрики: Создаются классы для каждой платформы или семейства, которые реализуют методы создания соответствующих объектов (например, WindowsFactory, MacFactory).

**Пример на Java:**

```java
    // Абстрактная фабрика
    interface GUIFactory {
        Button createButton();
        Checkbox createCheckbox();
    }

    // Конкретные фабрики
    class WinFactory implements GUIFactory {
        public Button createButton() {
            return new WinButton();
        }
        
        public Checkbox createCheckbox() {
            return new WinCheckbox();
        }
    }

    class MacFactory implements GUIFactory {
        public Button createButton() {
            return new MacButton();
        }
        
        public Checkbox createCheckbox() {
            return new MacCheckbox();
        }
    }
```

### Строитель (Builder)

**Аналогия:**

Допустим, вы пришли в забегаловку, заказали бургер дня, и вам выдали его без вопросов. Это пример «Простой фабрики». Но иногда логика создания состоит из большего количества шагов. К примеру, при заказе бургера дня есть несколько вариантов хлеба, начинки, соусов, дополнительных ингредиентов. В таких ситуациях помогает шаблон «Строитель».

**Суть:**

Шаблон позволяет создавать разные свойства объекта, избегая загрязнения конструктора (constructor pollution). Это полезно, когда у объекта может быть несколько свойств. Или когда создание объекта состоит из большого количества этапов.

**Примеры реализации:**

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

**Прототип** — это порождающий паттерн проектирования, который позволяет создавать новые объекты, копируя существующие. Этот паттерн используется, когда процесс создания объекта является сложным или ресурсоёмким, и проще или быстрее создать новый объект, клонировав уже существующий, чем создавать его с нуля

**Как работает Прототип:**

1. Определение интерфейса клонирования: Класс объекта должен реализовывать интерфейс клонирования или содержать метод clone(), который будет возвращать копию объекта.
2. Создание копий объектов: Вместо создания новых объектов через конструктор, клиент вызывает метод clone() на существующем объекте. Этот метод возвращает копию объекта.
3. Глубокое или поверхностное копирование: Паттерн Прототип может использовать как глубокое копирование (все вложенные объекты копируются), так и поверхностное копирование (копируются только ссылки на вложенные объекты).

### Структурные паттерны проектирования

**Структурные паттерны** проектирования касаются того, как классы и объекты комбинируются для создания более крупных и гибких структур. Они помогают упрощать и организовывать систему, делая её более модульной, расширяемой и понятной. Ниже описаны все основные структурные паттерны с примерами из реальной жизни и кодом на Java.

## Структурные паттерны

Структурные паттерны проектирования касаются того, как классы и объекты комбинируются для создания более крупных и гибких структур. Они помогают упрощать и организовывать систему, делая её более модульной, расширяемой и понятной. Ниже описаны все основные структурные паттерны с примерами из реальной жизни и кодом на Java

### Адаптер (Adapter)

**Назначение:**

Паттерн **Адаптер** используется для того, чтобы объекты с несовместимыми интерфейсами могли работать вместе. Он преобразует интерфейс одного класса в интерфейс, ожидаемый другим классом. Это позволяет двум несовместимым объектам взаимодействовать без изменения их исходного кода.

**Как работает:**

Адаптер действует как посредник, который оборачивает один объект и преобразует его интерфейс в тот, который ожидается клиентом.

**Пример из жизни:**

Розетка и вилка — адаптер можно сравнить с переходником для электроприборов. Например, у вас есть вилка европейского стандарта, а розетка — американского. Вы не можете подключить вилку напрямую, но адаптер (переходник) позволит это сделать, преобразуя контакт.

**Пример на Java:**

```java
// Старый интерфейс
class OldCharger {
    public void chargeViaOldPort() {
        System.out.println("Charging via old port...");
    }
}

// Новый интерфейс
interface NewCharger {
    void chargeViaNewPort();
}

// Адаптер для старого интерфейса
class ChargerAdapter implements NewCharger {
    private OldCharger oldCharger;

    public ChargerAdapter(OldCharger oldCharger) {
        this.oldCharger = oldCharger;
    }

    @Override
    public void chargeViaNewPort() {
        oldCharger.chargeViaOldPort(); // Используем старый интерфейс через адаптер
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        OldCharger oldCharger = new OldCharger();
        NewCharger charger = new ChargerAdapter(oldCharger);
        charger.chargeViaNewPort(); // Зарядка через адаптер
    }
}
```

### Мост (Bridge)

**Назначение:**

Паттерн **Мост** разделяет абстракцию и реализацию, позволяя изменять их независимо друг от друга. Он полезен, когда существует множество возможных вариаций реализации абстракции, и их нужно изменять независимо.

**Как работает:**

Мост отделяет интерфейс (абстракцию) от его реализации, что позволяет менять обе части независимо друг от друга. Абстракция использует объект реализации через интерфейс, а не напрямую.

**Пример из жизни:**

Пульт дистанционного управления для телевизора можно считать абстракцией, а конкретный телевизор — реализацией. Вы можете использовать один и тот же пульт для управления разными телевизорами.

**Пример на Java:**

```java
// Реализация интерфейса
interface Device {
    void turnOn();
    void turnOff();
}

// Конкретные реализации
class TV implements Device {
    @Override
    public void turnOn() {
        System.out.println("TV is turned on");
    }

    @Override
    public void turnOff() {
        System.out.println("TV is turned off");
    }
}

class Radio implements Device {
    @Override
    public void turnOn() {
        System.out.println("Radio is turned on");
    }

    @Override
    public void turnOff() {
        System.out.println("Radio is turned off");
    }
}

// Абстракция
abstract class RemoteControl {
    protected Device device;

    public RemoteControl(Device device) {
        this.device = device;
    }

    public abstract void togglePower();
}

// Расширение абстракции
class BasicRemote extends RemoteControl {

    public BasicRemote(Device device) {
        super(device);
    }

    @Override
    public void togglePower() {
        System.out.println("Toggling power...");
        device.turnOn();
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        Device tv = new TV();
        RemoteControl remote = new BasicRemote(tv);
        remote.togglePower(); // Управляем телевизором

        Device radio = new Radio();
        remote = new BasicRemote(radio);
        remote.togglePower(); // Теперь управляем радио
    }
}
```

### Компоновщик (Composite)

**Назначение:**

Паттерн **Компоновщик** позволяет группировать объекты в древовидную структуру, чтобы можно было работать с группами объектов так же, как с одиночными объектами. Этот паттерн применяется для реализации иерархий "часть-целое".

**Как работает:**

Компоновщик создаёт единый интерфейс для объектов и их контейнеров. И "листовые" объекты, и контейнеры объектов реализуют этот интерфейс, что позволяет одинаково работать с отдельными элементами и их группами.

**Пример из жизни:**

Организация и сотрудники — у вас может быть иерархия, где компания — это целое, а сотрудники — это части. Структура компании может включать подкомпании, отделы и сотрудников.

**Пример на Java:**

```java
// Общий интерфейс компонента
interface Employee {
    void showDetails();
}

// "Листовой" компонент
class Developer implements Employee {
    private String name;
    private String position;

    public Developer(String name, String position) {
        this.name = name;
        this.position = position;
    }

    @Override
    public void showDetails() {
        System.out.println(name + " is a " + position);
    }
}

// "Компоновщик" для хранения других объектов
class Manager implements Employee {
    private String name;
    private String department;
    private List<Employee> employees = new ArrayList<>();

    public Manager(String name, String department) {
        this.name = name;
        this.department = department;
    }

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    @Override
    public void showDetails() {
        System.out.println(name + " is a manager of " + department);
        for (Employee employee : employees) {
            employee.showDetails();
        }
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        Developer dev1 = new Developer("John", "Frontend Developer");
        Developer dev2 = new Developer("Jane", "Backend Developer");

        Manager manager = new Manager("Paul", "IT Department");
        manager.addEmployee(dev1);
        manager.addEmployee(dev2);

        manager.showDetails(); // Отображаем всю иерархию
    }
}
```

### Декоратор (Decorator)

**Назначение:**

Паттерн **Декоратор** динамически добавляет новые обязанности объекту. Декораторы обеспечивают гибкость в расширении функциональности классов без изменения их исходного кода.

**Как работает:**

Декоратор оборачивает объект и добавляет к его поведению дополнительные возможности. Ключевым моментом является то, что декоратор реализует тот же интерфейс, что и объект, который он оборачивает.

**Пример из жизни:**

Дополнительные услуги в кафе — базовый кофе можно "декорировать" добавлением молока, сахара или сиропа, не изменяя сам кофе, а просто добавляя новые ингредиенты поверх.

**Пример на Java:**

```java
// Общий интерфейс компонента
interface Coffee {
    String getDescription();
    double getCost();
}

// Базовый компонент
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}

// Декоратор
class MilkDecorator implements Coffee {
    private Coffee coffee;

    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", with milk";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 1.5;
    }
}

// Другой декоратор
class SugarDecorator implements Coffee {
    private Coffee coffee;

    public SugarDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", with sugar";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.5;
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " costs " + coffee.getCost());

        coffee = new MilkDecorator(coffee);
        System.out.println(coffee.getDescription() + " costs " + coffee.getCost());

        coffee = new SugarDecorator(coffee);
        System.out.println(coffee.getDescription() + " costs " + coffee.getCost());
    }
}
```

### Фасад (Facade)

**Назначение:**

Паттерн **Фасад** предоставляет упрощённый интерфейс для работы с сложной подсистемой. Он скрывает сложность системы за простым интерфейсом и используется для того, чтобы клиенты взаимодействовали с системой через этот интерфейс, а не напрямую с компонентами подсистемы.

**Как работает:**

Фасад создаёт единый объект, который упрощает взаимодействие с более сложной системой или набором классов. Он скрывает детали реализации и предоставляет упрощённый интерфейс для клиента.

**Пример из жизни:**

Диспетчер в турагентстве — когда вы хотите поехать в отпуск, вы не связываетесь с каждой авиакомпанией, отелем и туроператором напрямую, а просто обращаетесь к агенту, который всё организует за вас.

**Пример на Java:**

```java
// Подсистемы
class HotelBooking {
    public void bookHotel() {
        System.out.println("Hotel booked");
    }
}

class FlightBooking {
    public void bookFlight() {
        System.out.println("Flight booked");
    }
}

class CarRental {
    public void rentCar() {
        System.out.println("Car rented");
    }
}

// Фасад
class TravelFacade {
    private HotelBooking hotelBooking;
    private FlightBooking flightBooking;
    private CarRental carRental;

    public TravelFacade() {
        this.hotelBooking = new HotelBooking();
        this.flightBooking = new FlightBooking();
        this.carRental = new CarRental();
    }

    public void bookCompleteTrip() {
        hotelBooking.bookHotel();
        flightBooking.bookFlight();
        carRental.rentCar();
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        TravelFacade facade = new TravelFacade();
        facade.bookCompleteTrip(); // Клиенту не нужно знать детали подсистем
    }
}
```

### Легковес (Flyweight)

**Назначение:**

Паттерн **Легковес** используется для оптимизации работы с большим количеством мелких объектов, используя разделение общего состояния объектов. Он помогает экономить память, минимизируя количество объектов.

**Как работает:**

Легковес делит объект на две части: внутреннее состояние, которое может быть разделено между несколькими объектами, и внешнее состояние, которое уникально для каждого объекта. В результате несколько объектов могут использовать одно и то же внутреннее состояние, что позволяет сократить количество создаваемых объектов.

**Пример из жизни:**

Шахматные фигуры — фигуры одного типа (например, пешки) могут делить одно и то же внутреннее состояние (цвет и тип), а их положение на доске будет внешним состоянием.

**Пример на Java:**

```java
import java.util.HashMap;
import java.util.Map;

// Легковес (внутреннее состояние)
class ChessPiece {
    private final String color; // Внутреннее состояние (разделяемое)

    public ChessPiece(String color) {
        this.color = color;
    }

    public String getColor() {
        return color;
    }

    public void draw(String name, int x, int y) {
        System.out.println(name + " (" + color + ") placed at " + x + ", " + y);
    }
}

// Контекст (внешнее состояние)
class ChessPieceContext {
    private final String name; // Название фигуры, например, "Белая пешка"
    private final int x; // Позиция по оси x
    private final int y; // Позиция по оси y
    private final ChessPiece chessPiece; // Легковес

    public ChessPieceContext(String name, ChessPiece chessPiece, int x, int y) {
        this.name = name;
        this.chessPiece = chessPiece;
        this.x = x;
        this.y = y;
    }

    public void place() {
        chessPiece.draw(name, x, y); // Используем легковес для отрисовки фигуры
    }
}

// Фабрика легковесов
class ChessPieceFactory {
    private static final Map<String, ChessPiece> pieces = new HashMap<>();

    public static ChessPiece getPiece(String color) {
        if (!pieces.containsKey(color)) {
            pieces.put(color, new ChessPiece(color));
        }
        return pieces.get(color);
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        ChessPiece whitePiece = ChessPieceFactory.getPiece("White");
        ChessPiece blackPiece = ChessPieceFactory.getPiece("Black");

        // Создаем белую пешку на позиции (1,1)
        ChessPieceContext whitePawn1 = new ChessPieceContext("White Pawn", whitePiece, 1, 1);
        whitePawn1.place();

        // Создаем черную пешку на позиции (1,2)
        ChessPieceContext blackPawn = new ChessPieceContext("Black Pawn", blackPiece, 1, 2);
        blackPawn.place();

        // Повторно создаем белую пешку на новой позиции (2,1)
        ChessPieceContext whitePawn2 = new ChessPieceContext("White Pawn", whitePiece, 2, 1);
        whitePawn2.place();
    }
}
```

### Заместитель (Proxy)

**Назначение:**

Паттерн **Заместитель** предоставляет суррогат или "заместителя" для другого объекта, чтобы контролировать доступ к нему или добавлять дополнительную логику. Это позволяет управлять доступом к объекту, не изменяя его.

В наиболее общей форме «Заместитель» — это класс, функционирующий как интерфейс к чему-либо. Это оболочка или объект-агент, вызываемый клиентом для получения доступа к другому, «настоящему» объекту. «Заместитель» может просто переадресовывать запросы настоящему объекту, а может предоставлять дополнительную логику: кеширование данных при интенсивном выполнении операций или потреблении ресурсов настоящим объектом; проверка предварительных условий (preconditions) до вызова выполнения операций настоящим объектом.

**Как работает:**

Заместитель выступает как обёртка над реальным объектом и перенаправляет вызовы к нему. Это позволяет добавить дополнительные функции, такие как контроль доступа, кеширование, логирование и т.д.

**Пример из жизни:**

Охранник перед зданием — охранник (заместитель) решает, пускать ли вас в здание или нет, основываясь на ваших данных. Он предоставляет доступ к зданию только при выполнении определённых условий.

**Пример на Java:**

```java
// Интерфейс реального объекта
interface Service {
    void request();
}

// Реальный объект
class RealService implements Service {
    @Override
    public void request() {
        System.out.println("Executing real request...");
    }
}

// Заместитель
class ProxyService implements Service {
    private Service realService;

    // Внедрение зависимости через конструктор
    public ProxyService(Service realService) {
        this.realService = realService;
    }

    @Override
    public void request() {
        System.out.println("Proxy: Checking access...");
        realService.request();
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        // Создаем реальный объект
        Service realService = new RealService();

        // Передаем реальный объект в прокси
        Service proxyService = new ProxyService(realService);

        // Вызов через заместителя
        proxyService.request();
    }
}
```

## Поведенческие паттерны

**Поведенческие паттерны** проектирования описывают способы взаимодействия между объектами и классами. Они обеспечивают гибкость в организации передачи сообщений и задач между объектами, позволяя им работать вместе более эффективно и упрощая структуру коммуникаций в сложных системах.

Поведенческие паттерны помогают структурировать взаимодействие между объектами в системе, делая их более гибкими и удобными для изменения и расширения. Эти паттерны решают проблемы передачи данных, управления состояниями и взаимодействия между объектами, что делает код более масштабируемым и поддерживаемым.

### Цепочка обязанностей (Chain of Responsibility)

**Назначение:**

Паттерн **Цепочка обязанностей** позволяет передавать запрос последовательно по цепочке обработчиков. Каждый обработчик решает, обрабатывать ли запрос или передать его следующему по цепочке.

**Как работает:**

Запрос проходит через цепочку объектов, где каждый объект решает, может ли он обработать запрос, и если не может, передаёт его дальше по цепочке.

**Пример из жизни:**

Служба поддержки — когда вы обращаетесь в службу поддержки, ваш запрос может пройти через несколько уровней специалистов, начиная с самых низких уровней поддержки и доходя до более высоких, если начальные уровни не смогли решить проблему.

**Пример на Java:**

```java
// Определение абстрактного обработчика в поддержке
abstract class SupportHandler {
    // Ссылка на следующего по цепочке побработчика
    protected SupportHandler nextHandler;

    public void setNextHandler(SupportHandler nextHandler) {
        this.nextHandler = nextHandler;
    }
    // Метод обработки
    public abstract void handleRequest(String request);
}
// Конеретные реализации
class BasicSupportHandler extends SupportHandler {
    @Override
    public void handleRequest(String request) {
        if (request.equals("basic")) { // если запрос может выполнить данный обрабтчик, то он его выполняет
            System.out.println("Basic support handled the request.");
        } else if (nextHandler != null) { // если нет, то переадет его дальше по цепочке
            nextHandler.handleRequest(request);
        }
    }
}

class AdvancedSupportHandler extends SupportHandler {
    @Override
    public void handleRequest(String request) {
        if (request.equals("advanced")) {
            System.out.println("Advanced support handled the request.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        SupportHandler basicSupport = new BasicSupportHandler();
        SupportHandler advancedSupport = new AdvancedSupportHandler();

        basicSupport.setNextHandler(advancedSupport);

        basicSupport.handleRequest("basic");
        basicSupport.handleRequest("advanced");
    }
}
```

### Команда (Command)

**Назначение:**

Паттерн **Команда** превращает запросы в объекты, что позволяет откладывать выполнение команды, ставить её в очередь, логировать или отменять её.

**Как работает:**

Запрос упаковывается в виде объекта-команды, который содержит всю необходимую информацию для выполнения действия. Этот объект можно передавать и сохранять, а выполнение запроса можно откладывать или отменять.

**Пример из жизни:**

Пульт дистанционного управления — каждая кнопка на пульте действует как команда: вы нажимаете кнопку, и команда передаётся телевизору, который выполняет соответствующее действие (например, переключает канал).

**Пример на Java:**

```java
// Интерфейс команды
interface Command {
    void execute();
}

// Реализация команды включения света
class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }
}

// Реализация команды выключения света
class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }
}

// Получатель
class Light {
    public void turnOn() {
        System.out.println("The light is on");
    }

    public void turnOff() {
        System.out.println("The light is off");
    }
}

// Отправитель
class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        Light light = new Light();

        Command lightOn = new LightOnCommand(light);
        Command lightOff = new LightOffCommand(light);

        RemoteControl remote = new RemoteControl();
        remote.setCommand(lightOn);
        remote.pressButton();

        remote.setCommand(lightOff);
        remote.pressButton();
    }
}
```

### Итератор (Iterator)

**Назначение:**

Паттерн **Итератор** предоставляет способ поочерёдного доступа ко всем элементам коллекции без раскрытия её внутренней структуры.

**Как работает:**

Итератор обходит коллекцию по одному элементу за раз, предоставляя унифицированный интерфейс для работы с различными типами коллекций.

**Пример из жизни:**

Книжная полка — можно рассматривать книжную полку как коллекцию книг. Итератор — это процесс поочерёдного выбора каждой книги с полки для чтения.

**Пример на Java:**

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }
}

class BookShelf implements Iterable<Book> {
    private List<Book> books = new ArrayList<>();

    public void addBook(Book book) {
        books.add(book);
    }

    @Override
    public Iterator<Book> iterator() {
        return books.iterator();
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        BookShelf shelf = new BookShelf();
        shelf.addBook(new Book("The Catcher in the Rye"));
        shelf.addBook(new Book("To Kill a Mockingbird"));

        for (Book book : shelf) {
            System.out.println(book.getTitle());
        }
    }
}
```

### Наблюдатель (Observer)

**Назначение:**

Паттерн **Наблюдатель** определяет зависимость «один ко многим», где один объект рассылает уведомления нескольким другим, которые подписаны на его изменения. Это позволяет объектам реагировать на события другого объекта без жёсткой связи между ними.

**Как работает:**

Есть "издатель" (субъект), который поддерживает список "наблюдателей". Когда состояние издателя меняется, он уведомляет всех наблюдателей.

**Пример из жизни:**

Подписка на новости — когда происходит важное событие, новостной сайт отправляет уведомления своим подписчикам. Каждый подписчик получает уведомление независимо от других.

**Пример на Java:**

```java
import java.util.ArrayList;
import java.util.List;

// Интерфейс наблюдателя
interface Observer {
    void update(String message);
}

// Интерфейс издателя (субъект)
interface Subject {
    void subscribe(Observer observer);
    void unsubscribe(Observer observer);
    void notifyObservers();
}

// Конкретный издатель
class NewsAgency implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String latestNews;

    public void setNews(String news) {
        this.latestNews = news;
        notifyObservers();
    }

    @Override
    public void subscribe(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void unsubscribe(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(latestNews);
        }
    }
}

// Конкретный наблюдатель
class NewsSubscriber implements Observer {
    private String name;

    public NewsSubscriber(String name) {
        this.name = name;
    }

    @Override
    public void update(String message) {
        System.out.println(name + " received news: " + message);
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        NewsAgency agency = new NewsAgency();
        
        NewsSubscriber sub1 = new NewsSubscriber("Alice");
        NewsSubscriber sub2 = new NewsSubscriber("Bob");
        
        agency.subscribe(sub1);
        agency.subscribe(sub2);
        
        agency.setNews("Breaking News: New Java Version Released!");

        agency.unsubscribe(sub2);
        
        agency.setNews("Update: Java Version Features Detailed.");
    }
}
```

### Посредник (Mediator)

**Назначение:**

Паттерн **Посредник** определяет объект, который инкапсулирует взаимодействие между набором объектов, снижая их прямые зависимости друг от друга. Это позволяет объектам общаться через посредника, а не напрямую.

**Как работает:**

Посредник управляет взаимодействием между объектами, чтобы они не взаимодействовали напрямую, что помогает снизить связанность между объектами.

**Пример из жизни:**

Диспетчер аэропорта — диспетчер (посредник) координирует взлёт и посадку самолётов, обеспечивая, что каждый самолёт взаимодействует с ним, а не напрямую с другими самолётами.

**Пример на Java:**

```java
import java.util.ArrayList;
import java.util.List;

// Интерфейс посредника
interface ChatMediator {
    void sendMessage(String message, User user);
    void addUser(User user);
}

// Конкретный посредник
class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();

    @Override
    public void addUser(User user) {
        users.add(user);
    }

    @Override
    public void sendMessage(String message, User user) {
        for (User u : users) {
            if (u != user) {
                u.receive(message);
            }
        }
    }
}

// Абстрактный класс пользователя
abstract class User {
    protected ChatMediator mediator;
    protected String name;

    public User(ChatMediator mediator, String name) {
        this.mediator = mediator;
        this.name = name;
    }

    public abstract void send(String message);
    public abstract void receive(String message);
}

// Конкретный пользователь
class ChatUser extends User {

    public ChatUser(ChatMediator mediator, String name) {
        super(mediator, name);
    }

    @Override
    public void send(String message) {
        System.out.println(this.name + " sends: " + message);
        mediator.sendMessage(message, this);
    }

    @Override
    public void receive(String message) {
        System.out.println(this.name + " receives: " + message);
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        ChatMediator mediator = new ChatRoom();
        
        User user1 = new ChatUser(mediator, "Alice");
        User user2 = new ChatUser(mediator, "Bob");
        User user3 = new ChatUser(mediator, "Charlie");
        
        mediator.addUser(user1);
        mediator.addUser(user2);
        mediator.addUser(user3);
        
        user1.send("Hello, everyone!");
    }
}
```

---

### Снимок (Memento)

**Назначение:**

Паттерн **Снимок** позволяет сохранять и восстанавливать состояние объекта без нарушения инкапсуляции. Это полезно для создания функций отмены действий.

**Как работает:**

Снимок хранит внутреннее состояние объекта. Объект может создавать снимок своего состояния и позже восстановить его из этого снимка.

**Пример из жизни:**

Функция "Отмена" (Undo) — текстовый редактор сохраняет состояние документа в определённые моменты. Когда вы нажимаете "Отмена", он восстанавливает последнее сохранённое состояние.

**Пример на Java:**

```java
// Снимок состояния
class TextEditorMemento {
    private final String content;

    public TextEditorMemento(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }
}

// Оригинатор
class TextEditor {
    private String content;

    public void setContent(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }

    public TextEditorMemento save() {
        return new TextEditorMemento(content);
    }

    public void restore(TextEditorMemento memento) {
        content = memento.getContent();
    }
}

// Хранитель снимков
class Caretaker {
    private TextEditorMemento memento;

    public void save(TextEditor editor) {
        memento = editor.save();
    }

    public void undo(TextEditor editor) {
        editor.restore(memento);
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        TextEditor editor = new TextEditor();
        Caretaker caretaker = new Caretaker();

        editor.setContent("Version 1");
        System.out.println("Content: " + editor.getContent());

        caretaker.save(editor); // Сохраняем состояние

        editor.setContent("Version 2");
        System.out.println("Content: " + editor.getContent());

        caretaker.undo(editor); // Восстанавливаем предыдущее состояние
        System.out.println("Content after undo: " + editor.getContent());
    }
}
```

### Состояние (State)

**Назначение:**

Паттерн **Состояние** позволяет объекту изменять своё поведение в зависимости от его состояния. При этом создаётся впечатление, что объект меняет свой класс.

**Как работает:**

Объект имеет множество состояний, и его поведение изменяется в зависимости от того, в каком состоянии он находится. Каждое состояние реализуется как отдельный класс, а объект делегирует выполнение действий своему текущему состоянию.

**Пример из жизни:**

Автомат с напитками — автомат может находиться в разных состояниях: ожидание монеты, выбор напитка, выдача напитка и т.д. Поведение автомата меняется в зависимости от его состояния.

**Пример на Java:**

```java
// Интерфейс состояния
interface State {
    void insertCoin(VendingMachine machine);
    void pressButton(VendingMachine machine);
}

// Конкретное состояние: ожидание монеты
class WaitingForCoinState implements State {
    @Override
    public void insertCoin(VendingMachine machine) {
        System.out.println("Coin inserted.");
        machine.setState(new WaitingForSelectionState());
    }

    @Override
    public void pressButton(VendingMachine machine) {
        System.out.println("Please insert a coin first.");
    }
}

// Конкретное состояние: ожидание выбора
class WaitingForSelectionState implements State {
    @Override
    public void insertCoin(VendingMachine machine) {
        System.out.println("Coin already inserted.");
    }

    @Override
    public void pressButton(VendingMachine machine) {
        System.out.println("Product selected. Dispensing...");
        machine.setState(new WaitingForCoinState());
    }
}

// Контекст
class VendingMachine {
    private State state;

    public VendingMachine() {
        this.state = new WaitingForCoinState();
    }

    public void setState(State state) {
        this.state = state;
    }

    public void insertCoin() {
        state.insertCoin(this);
    }

    public void pressButton() {
        state.pressButton(this);
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        VendingMachine machine = new VendingMachine();
        
        machine.pressButton();  // Ожидание монеты
        machine.insertCoin();   // Вставляем монету
        machine.pressButton();  // Выбор продукта
    }
}
```

---

### Стратегия (Strategy)

**Назначение:**

Паттерн **Стратегия** определяет семейство алгоритмов, инкапсулирует каждый из них и делает их взаимозаменяемыми. Это позволяет изменять алгоритмы независимо от клиентов, которые их используют.

**Как работает:**

Объект использует алгоритм, который передаётся ему в виде стратегии (объект, реализующий интерфейс стратегии). Алгоритм можно менять на лету, просто подменив объект стратегии.

**Пример из жизни:**

Выбор способа оплаты — в интернет-магазине можно выбирать разные способы оплаты (карта, PayPal, наложенный платёж). Все они работают по-разному, но процесс оплаты в целом один и тот же.

**Пример на Java:**

```java
// Интерфейс стратегии
interface PaymentStrategy {
    void pay(int amount);
}

// Конкретная стратегия: оплата картой
class CreditCardPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card.");
    }
}

// Конкретная стратегия: оплата PayPal
class PayPalPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal.");
    }
}

// Контекст
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        cart.setPaymentStrategy(new CreditCardPayment());
        cart.checkout(100);  // Оплата картой

        cart.setPaymentStrategy(new PayPalPayment());
        cart.checkout(200);  // Оплата через PayPal
    }
}
```

### Шаблонный метод (Template Method)

**Назначение:**

Паттерн **Шаблонный метод** определяет скелет алгоритма, позволяя подклассам переопределять отдельные этапы алгоритма без изменения его структуры.

**Как работает:**

Базовый класс предоставляет шаблон метода, который определяет шаги алгоритма. Подклассы могут переопределять эти шаги, не изменяя общий алгоритм.

**Пример из жизни:**

Процесс приготовления кофе — у вас есть общий алгоритм: вскипятить воду, добавить кофе, добавить молоко или сахар (если нужно). Разные виды кофе могут отличаться по шагам, но основной процесс остаётся неизменным.

**Пример на Java:**

```java
abstract class CoffeeTemplate {
    // Шаблонный метод
    public final void prepareCoffee() {
        boilWater();
        brewCoffee();
        pourInCup();
        addExtras();
    }

    private void boilWater() {
        System.out.println("Boiling water");
    }

    private void pourInCup() {
        System.out.println("Pouring coffee into the cup");
    }

    protected abstract void brewCoffee();
    protected abstract void addExtras();
}

// Конкретный класс
class BlackCoffee extends CoffeeTemplate {
    @Override
    protected void brewCoffee() {
        System.out.println("Brewing black coffee");
    }

    @Override
    protected void addExtras() {
        System.out.println("No extras added");
    }
}

// Другой конкретный класс
class Latte extends CoffeeTemplate {
    @Override
    protected void brewCoffee() {
        System.out.println("Brewing espresso");
    }

    @Override
    protected void addExtras() {
        System.out.println("Adding milk and sugar");
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        CoffeeTemplate blackCoffee = new BlackCoffee();
        blackCoffee.prepareCoffee();

        CoffeeTemplate latte = new Latte();
        latte.prepareCoffee();
    }
}
```

### Посетитель (Visitor)

**Назначение:**

Паттерн **Посетитель** позволяет добавлять новые операции для классов, не изменяя их. Он отделяет алгоритмы от структур данных, по которым эти алгоритмы должны работать.

**Шаблон «Посетитель»** — это способ отделения алгоритма от структуры объекта, в которой он оперирует. Результат отделения — возможность добавлять новые операции в существующие структуры объектов без их модифицирования. Это один из способов соблюдения принципа открытости/закрытости (open/closed principle)

**Как работает:**

Посетитель инкапсулирует операцию и передаётся в объекты структуры данных, чтобы выполнить операцию. Каждый элемент структуры данных знает, как передать себя посетителю для выполнения операции.

**Пример из жизни:**

Налоговый инспектор (посетитель) посещает различные объекты бизнеса (элементы) для расчёта налогов. Каждый бизнес знает, как предоставить данные для инспектора, но инспектор решает, как обрабатывать эти данные.

**Пример на Java:**

```java
// Интерфейс посетителя
interface Visitor {
    void visit(Book book);
    void visit(CD cd);
}

// Интерфейс для элементов
interface Item {
    void accept(Visitor visitor);
}

// Конкретный элемент: книга
class Book implements Item {
    private String title;
    private double price;

    public Book(String title, double price) {
        this.title = title;
        this.price = price;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// Конкретный элемент: CD
class CD implements Item {
    private String albumName;
    private double price;

    public CD(String albumName, double price) {
        this.albumName = albumName;
        this.price = price;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// Конкретный посетитель: корзина покупок
class ShoppingCartVisitor implements Visitor {
    @Override
    public void visit(Book book) {
        System.out.println("Book: " + book.getTitle() + ", Price: " + book.getPrice());
    }

    @Override
    public void visit(CD cd) {
        System.out.println("CD: " + cd.getAlbumName() + ", Price: " + cd.getPrice());
    }
}

// Клиентский код
public class Main {
    public static void main(String[] args) {
        List<Item> items = new ArrayList<>();
        items.add(new Book("Design Patterns", 50.0));
        items.add(new CD("Best of 90s", 15.0));

        Visitor visitor = new ShoppingCartVisitor();

        for (Item item : items) {
            item.accept(visitor);
        }
    }
}
```
