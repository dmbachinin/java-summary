# СОДЕРЖАНИЕ:

## [Многопоточность](#многопоточность)
- [Thread](#thread)
    - [Создание потоков](#создание-потоков)
    - [Состояния потоков](#состояния-потоков)
    - [Методы для взаимодейтсивя с потоками](#методы-для-взаимодейтсивя-с-потоками)
    - [Приоритеты потоков](#приоритеты-потоков)
        - [Значения приоритетов потоков](#значения-приоритетов-потоков)
        - [Как работают приоритеты](#как-работают-приоритеты)
        - [Модель Producer-Consumer](#модель-producer-consumer)
            - [Пример реализации на Java](#пример-реализации-на-java)
- [Механизмы синхронизации](#механизмы-синхронизации)
    - [Мьютекс (Mutex)](#мьютекс-mutex)
    - [Монитор (Monitor)](#монитор-monitor)
    - [Семафор (Semaphore)](#семафор-semaphore)
        - [java.util.concurrent.Semaphore](#класс-javautilconcurrentsemaphore)
            - [Основные концепции](#основные-концепции)
            - [Основные методы](#основные-методы)
            - [Примеры использования](#примеры-использования-семафора)


# Многопоточность

`СТАТЬЯ НА ЭТУ ТЕМУ:` https://habr.com/ru/articles/164487/

В java выбран подход добавления поддержки многопоточной модели на базе последовательного языка. Вместо того чтобы пораждать внешние процессы в многозадачной операционной системе, модель создает новые задачи в пределах одного процесса, предоставленного выполняемой программой.

## Thread

Класс `Thread` в Java является основным способом работы с потоками (threads) в многопоточном программировании.

### Создание потоков 

Для создания потоков необходимо либо создать унаследоваться от класса Thread, либо передать в конструтор класса Thread объект, который реализует интерфес Runnable:

```java
class MyThread extends Thread { // Наследование от Thread
    @Override
    public void run() { 
        System.out.println("MyThread is running");
    }
}

class MyRunnable implements Runnable { // Реализация Runnable
    @Override
    public void run() {
        System.out.println("MyRunnable is running");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread thread_2 = new MyThread();
        Thread thread_1 = new Thread(new MyRunnable());
        thread_1.start();
        thread_2.start();
    }
}
```

### Состояния потоков
```java
    var thread = new Thread();
    thread.getState(); // Получение текущего состояния потока
    // Возможные состояния:
    NEW // Поток было создан но еще не запускался
    RUNNABLE // Поток в данный момент запущен
    BLOCKED // Поток заблокирован другим потоком (возникает если вызвать wait())
    WAITING // Поток находится в состоянии ожидания (возникает если вызвать otherThread.join())
    TIMED_WAITING // Поток находится во временом ожидании (возникает если вызвать otherThread.join(1000) с укозанием времени ожидания)
    TERMINATED // Поток закончил свою работу (после выполенения НЕЛЬЗЯ повторно вызвать start(), нужно создавать новый объект)
```

### Методы для взаимодейтсивя с потоками

```java
Thread thread = new MyThread();
thread.start(); // Запуск потока на выполнение
thread.sleep(long millis); // Останавливает выполнение потока
thread.join(); // Приостанавливает выполнение ТЕКУЩЕГО потока, пока поток не закончит работу

wait(); // Приостанавливает поток. Используется для управления синхронизацией потоков. Заставляет текущий поток ожидать (приостанавливается его выполнение) до тех пор, пока другой поток не вызовет метод notify() или notifyAll() на этом же объекте (объект которые используют сразу неселько потоков).
notify(); // Пробуждает один из потоков, который находится в wait
notifyAll(); // Пробуждает все потоки, которые ожидают на мониторинге объекта

thread.interrupt(); // Прерывает поток, если он заблокирован вызовом методов sleep, join или wait
thread.isAlive(); // Проверяет, выполняется ли поток в данный момент. Возвращает true, если поток запущен и ещё не завершился
thread.getId(); // Возвращает идентификатор потока (уникален и присваивается при создании)
thread.getName(); // Возвращает имя потока
thread.setName(String name); // Устонавливает имя для потока
thread.getPriority(); // Возвращает приоритет потока. Значение приоритета может быть от Thread.MIN_PRIORITY (1) до Thread.MAX_PRIORITY (10).
thread.setPriority(int priority); // Устанавливает приоритет потока. Может принимать значения от Thread.MIN_PRIORITY до Thread.MAX_PRIORITY.
thread.isInterrupted(); // Проверяет, был ли поток прерван. Возвращает true, если поток был прерван.

synchronized - ключевое слово для методов, которое указывает на то, что данный метод может выполняться только одним потоком.

```
### Приоритеты потоков

Приоритеты потоков в Java используются для предоставления планировщику потоков подсказок о важности потоков относительно друг друга. Однако **важно отметить, что приоритеты потоков в Java не гарантируют строгого управления порядком выполнения потоков**, поскольку их точное поведение зависит от реализации планировщика потоков в операционной системе.

#### Значения приоритетов потоков

В Java приоритеты потоков задаются целыми числами в диапазоне от 1 до 10. Эти значения определяются тремя константами в классе `Thread`:

- `Thread.MIN_PRIORITY` (1): Минимальный приоритет.
- `Thread.NORM_PRIORITY` (5): Нормальный (по умолчанию) приоритет.
- `Thread.MAX_PRIORITY` (10): Максимальный приоритет.

#### Как работают приоритеты

1. **Подсказки для планировщика**: Приоритеты потоков служат подсказками для планировщика потоков операционной системы. Поток с более высоким приоритетом имеет больше шансов получить процессорное время, чем поток с более низким приоритетом.
   
2. **Влияние реализации**: Реальное влияние приоритетов зависит от реализации планировщика потоков в конкретной операционной системе. Некоторые операционные системы могут игнорировать приоритеты, другие могут предоставлять более точное управление.

3. **Не гарантирует строгий порядок**: Приоритеты не обеспечивают строгого порядка выполнения. Даже поток с более высоким приоритетом может быть приостановлен или блокирован, позволяя потокам с более низким приоритетом выполнять задачи.

#### Модель Producer-Consumer

Модель `Producer-Consumer (производитель-потребитель)` — это классическая проблема синхронизации, которая часто используется для иллюстрации принципов многопоточности и взаимодействия между потоками. Она описывает сценарий, в котором производители (producers) создают данные и помещают их в буфер, а потребители (consumers) извлекают эти данные из буфера и обрабатывают их. Буфер может иметь ограниченный размер, и важно обеспечить корректную синхронизацию, чтобы избежать проблем, таких как переполнение буфера или попытки потребителя извлечь данные из пустого буфера.

Модель Producer-Consumer — это мощный паттерн для управления многопоточностью, обеспечивающий безопасное взаимодействие между потоками через общий буфер.

Модель Producer-Consumer (производитель-потребитель) — это классическая проблема синхронизации, которая часто используется для иллюстрации принципов многопоточности и взаимодействия между потоками. Она описывает сценарий, в котором производители (producers) создают данные и помещают их в буфер, а потребители (consumers) извлекают эти данные из буфера и обрабатывают их. Буфер может иметь ограниченный размер, и важно обеспечить корректную синхронизацию, чтобы избежать проблем, таких как переполнение буфера или попытки потребителя извлечь данные из пустого буфера.

#### Основные компоненты модели Producer-Consumer

1. **Производитель (Producer)**:
   - Поток, который производит данные и помещает их в буфер.
   - Производитель должен ждать, если буфер заполнен, до тех пор, пока потребитель не освободит место.

2. **Потребитель (Consumer)**:
   - Поток, который извлекает данные из буфера и обрабатывает их.
   - Потребитель должен ждать, если буфер пуст, до тех пор, пока производитель не добавит новые данные.

3. **Буфер (Buffer)**:
   - Общий ресурс, который используется для хранения данных, произведенных производителями и потребляемых потребителями.
   - Буфер обычно реализуется в виде очереди с фиксированным размером.

### Пример реализации на Java

Для реализации модели Producer-Consumer в Java можно использовать разные механизмы синхронизации, такие как `wait()`, `notify()`, и `synchronized`, а также более высокоуровневые конструкции из пакета `java.util.concurrent`, такие как `BlockingQueue`.

#### Пример с использованием `synchronized`, `wait()` и `notify()`

```java
import java.util.LinkedList;
import java.util.Queue;

class Buffer {
    private final Queue<Integer> queue = new LinkedList<>();
    private final int capacity;

    public Buffer(int capacity) {
        this.capacity = capacity;
    }

    public synchronized void produce(int value) throws InterruptedException {
        while (queue.size() == capacity) {
            wait(); // Ожидание, если буфер заполнен
        }
        queue.add(value);
        System.out.println("Produced " + value);
        notifyAll(); // Уведомление потребителей о том, что появился новый элемент
    }

    public synchronized int consume() throws InterruptedException {
        while (queue.isEmpty()) {
            wait(); // Ожидание, если буфер пуст
        }
        int value = queue.poll();
        System.out.println("Consumed " + value);
        notifyAll(); // Уведомление производителей о том, что освободилось место
        return value;
    }
}

class Producer implements Runnable {
    private final Buffer buffer;

    public Producer(Buffer buffer) {
        this.buffer = buffer;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                buffer.produce(i);
                Thread.sleep(100); // Имитация времени производства
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

class Consumer implements Runnable {
    private final Buffer buffer;

    public Consumer(Buffer buffer) {
        this.buffer = buffer;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                buffer.consume();
                Thread.sleep(150); // Имитация времени потребления
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

public class ProducerConsumerExample {
    public static void main(String[] args) {
        Buffer buffer = new Buffer(5);
        Thread producerThread = new Thread(new Producer(buffer));
        Thread consumerThread = new Thread(new Consumer(buffer));

        producerThread.start();
        consumerThread.start();
    }
}
```

### Объяснение примера

1. **Buffer**:
   - Класс `Buffer` содержит очередь с фиксированной емкостью.
   - Метод `produce` добавляет элементы в очередь и уведомляет потребителей, если появляются новые элементы.
   - Метод `consume` извлекает элементы из очереди и уведомляет производителей, если освобождается место.

2. **Producer**:
   - Класс `Producer` реализует интерфейс `Runnable` и в методе `run` производит данные, добавляя их в буфер.

3. **Consumer**:
   - Класс `Consumer` реализует интерфейс `Runnable` и в методе `run` потребляет данные, извлекая их из буфера.

4. **ProducerConsumerExample**:
   - Основной класс создает экземпляры `Buffer`, `Producer` и `Consumer`, а затем запускает их в отдельных потоках.

#### Пример с использованием `BlockingQueue`

Для упрощения управления многопоточностью можно использовать `BlockingQueue` из пакета `java.util.concurrent`.

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

class Producer implements Runnable {
    private final BlockingQueue<Integer> queue;

    public Producer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                queue.put(i);
                System.out.println("Produced " + i);
                Thread.sleep(100); // Имитация времени производства
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

class Consumer implements Runnable {
    private final BlockingQueue<Integer> queue;

    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                int value = queue.take();
                System.out.println("Consumed " + value);
                Thread.sleep(150); // Имитация времени потребления
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

public class ProducerConsumerExample {
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);
        Thread producerThread = new Thread(new Producer(queue));
        Thread consumerThread = new Thread(new Consumer(queue));

        producerThread.start();
        consumerThread.start();
    }
}
```

### Объяснение

1. **BlockingQueue**:
   - `BlockingQueue` автоматически управляет блокировкой потоков, если очередь заполнена (при добавлении) или пуста (при извлечении), что упрощает реализацию и делает её более безопасной.

2. **Производитель и потребитель**:
   - Производитель и потребитель взаимодействуют через общую очередь `BlockingQueue`, используя методы `put` и `take`, которые автоматически блокируют поток при необходимости.

## Механизмы синхронизации

В Java, мьютексы, мониторы и семафоры используются для управления потоками и синхронизации доступа к общим ресурсам. Вот краткое объяснение каждого из них:

`Мьютекс` обеспечивает эксклюзивный доступ к ресурсу одному потоку.\
`Монитор` включает в себя мьютекс и дополнительные функции для управления многопоточностью.\
`Семафор` управляет доступом к ресурсу через счетчик разрешений, позволяя ограничивать количество потоков, которые могут одновременно использовать ресурс.

### Мьютекс (Mutex)

Мьютекс — это специальный объект для синхронизации потоков. Он «прикреплен» к каждому объекту в Java.
Неважно, пользуешься ли ты стандартными классами или создал собственные классы, скажем, Cat и Dog: `у всех объектов всех классов есть мьютекс`.
У мьютекса есть несколько важных особенностей.
1. `Во-первых, возможны только два состояния — «свободен» и «занят»`.\
Это упрощает понимание принципа работы: можно провести параллели с булевыми переменными true/false или двоичной системой счисления 1/0.

2. `Во-вторых, состояниями нельзя управлять напрямую`.\
В Java нет механизмов, которые позволили бы явно взять объект, получить его мьютекс и присвоить ему нужный статус.

`Мьютекс (сокращение от "mutual exclusion" - взаимное исключение)` - это механизм синхронизации, который обеспечивает доступ к ресурсу только одному потоку в один момент времени. В Java роль мьютекса выполняет ключевое слово `synchronized`.

#### Пример использования мьютекса:
```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```
В этом примере методы `increment` и `getCount` синхронизированы, что гарантирует, что только один поток может выполнять эти методы одновременно.

### Монитор (Monitor)
Монитор - это более высокоуровневый механизм синхронизации, который включает в себя мьютекс и дополнительные функции для управления многопоточностью, такие как условные переменные для ожидания и оповещения потоков. В Java каждый объект может быть монитором, так как каждый объект имеет встроенные методы для синхронизации (`wait()`, `notify()`, `notifyAll()`).

#### Пример использования монитора:
```java
public class SharedResource {
    private int value;

    public synchronized void produce(int newValue) {
        value = newValue;
        notify(); // уведомить ждущий поток
    }

    public synchronized int consume() throws InterruptedException {
        wait(); // ожидание уведомления от другого потока
        return value;
    }
}
```
Здесь методы `produce` и `consume` синхронизированы, и используется `wait` для ожидания и `notify` для уведомления потоков.

### Семафор (Semaphore)
Семафор - это механизм синхронизации, который управляет доступом к ресурсу через счетчик. Счетчик семафора показывает количество разрешений, доступных для доступа к ресурсу. Семафоры полезны, когда нужно ограничить доступ к ресурсам определенным числом потоков.

В Java семафоры реализованы в классе `java.util.concurrent.Semaphore`.

### Класс `java.util.concurrent.Semaphore`

Класс `Semaphore` из пакета `java.util.concurrent` предоставляет механизм синхронизации, который управляет доступом к ресурсам через счетчик разрешений. Семафор позволяет ограничить количество потоков, которые могут одновременно использовать заданный ресурс, что может быть полезно для контроля доступа к ресурсам, которые имеют ограниченные возможности или необходимо ограничить.

#### Основные концепции:

- **Разрешения (Permits)**: Семафор управляет набором разрешений. Потоки должны получить разрешение (acquire) перед доступом к ресурсу и освободить разрешение (release) после использования.
- **Доступность**: Если разрешения доступны, поток может получить разрешение и продолжить работу. Если разрешения недоступны, поток будет заблокирован до тех пор, пока разрешение не станет доступным.

#### Основные методы:

1. **Конструкторы:**
```java
   Semaphore(int permits); //  Создает семафор с заданным числом разрешений.
   Semaphore(int permits, boolean fair); //  Создает семафор с заданным числом разрешений и заданной политикой честности (fairness).
```
2. **Методы для получения разрешений:**
```java
   void acquire() throws InterruptedException; //  Получает одно разрешение, блокируя поток, если разрешения недоступны.
   void acquire(int permits) throws InterruptedException; //  Получает заданное число разрешений.
   void acquireUninterruptibly(); //  Получает одно разрешение, блокируя поток, если разрешения недоступны, но без прерывания.
   void acquireUninterruptibly(int permits); //  Получает заданное число разрешений без возможности прерывания.
```
3. **Методы для освобождения разрешений:**
```java
   void release(); //  Освобождает одно разрешение.
   void release(int permits); //  Освобождает заданное число разрешений.
```
4. **Методы для проверки состояния:**
```java
   int availablePermits(); //  Возвращает количество доступных разрешений.
   int drainPermits(); //  Сбрасывает все доступные разрешения и возвращает их количество.
   boolean hasQueuedThreads(); //  Проверяет, есть ли потоки, ожидающие разрешений.
   int getQueueLength(); //  Возвращает количество потоков, ожидающих разрешений.
```
5. **Дополнительные методы:**
```java
   Collection<Thread> getQueuedThreads(); //  Возвращает коллекцию потоков, ожидающих разрешений.
   boolean tryAcquire(); //  Пытается получить одно разрешение без блокировки.
   boolean tryAcquire(int permits); //  Пытается получить заданное число разрешений без блокировки.
   boolean tryAcquire(long timeout, TimeUnit unit) throws InterruptedException; //  Пытается получить одно разрешение, ожидая заданное время перед тем, как вернуть `false`, если разрешение недоступно.
   boolean tryAcquire(int permits, long timeout, TimeUnit unit) throws InterruptedException; //  Пытается получить заданное число разрешений с ожиданием.
```

#### Примеры использования Семафора:

1. **Ограничение доступа к ресурсу:**
```java
import java.util.concurrent.Semaphore;

public class ResourceAccess {
    private final Semaphore semaphore = new Semaphore(3); // Разрешаем доступ трем потокам одновременно

    public void accessResource() {
        try {
            semaphore.acquire();
            System.out.println("Resource accessed by " + Thread.currentThread().getName());
            // Код для доступа к ресурсу
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            semaphore.release();
        }
    }

    public static void main(String[] args) {
        ResourceAccess resource = new ResourceAccess();
        Runnable task = resource::accessResource;

        for (int i = 0; i < 10; i++) {
            new Thread(task).start();
        }
    }
}
```

2. **Честный семафор:**
```java
import java.util.concurrent.Semaphore;

public class FairSemaphoreExample {
    private final Semaphore semaphore = new Semaphore(1, true); // Честный семафор

    public void accessResource() {
        try {
            semaphore.acquire();
            System.out.println("Resource accessed by " + Thread.currentThread().getName());
            // Код для доступа к ресурсу
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            semaphore.release();
        }
    }

    public static void main(String[] args) {
        FairSemaphoreExample resource = new FairSemaphoreExample();
        Runnable task = resource::accessResource;

        for (int i = 0; i < 10; i++) {
            new Thread(task).start();
        }
    }
}
```
В этом примере используется честный семафор, который обеспечивает, что потоки будут получать доступ к ресурсу в порядке своей очереди.