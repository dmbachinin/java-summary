# Многопоточность

`СТАТЬЯ НА ЭТУ ТЕМУ:` <https://habr.com/ru/articles/164487/>

В java выбран подход добавления поддержки многопоточной модели на базе последовательного языка. Вместо того чтобы пораждать внешние процессы в многозадачной операционной системе, модель создает новые задачи в пределах одного процесса, предоставленного выполняемой программой.

- [Многопоточность](#многопоточность)
  - [Thread](#thread)
    - [Создание потоков](#создание-потоков)
    - [Состояния потоков](#состояния-потоков)
    - [Методы для взаимодейтсивя с потоками](#методы-для-взаимодейтсивя-с-потоками)
    - [Приоритеты потоков](#приоритеты-потоков)
      - [Значения приоритетов потоков](#значения-приоритетов-потоков)
      - [Как работают приоритеты](#как-работают-приоритеты)
      - [Модель Producer-Consumer](#модель-producer-consumer)
      - [Основные компоненты модели Producer-Consumer](#основные-компоненты-модели-producer-consumer)
    - [Пример реализации на Java](#пример-реализации-на-java)
      - [Пример с использованием `synchronized`, `wait()` и `notify()`](#пример-с-использованием-synchronized-wait-и-notify)
    - [Объяснение примера](#объяснение-примера)
      - [Пример с использованием `BlockingQueue`](#пример-с-использованием-blockingqueue)
    - [Объяснение](#объяснение)
  - [Пул потоков](#пул-потоков)
    - [ExecutorService](#executorservice)
    - [CompletableFuture](#completablefuture)
    - [ThreadPoolExecutor](#threadpoolexecutor)
    - [Сравнительная таблица ExecutorService, CompletableFuture, ThreadPoolExecutor](#сравнительная-таблица-executorservice-completablefuture-threadpoolexecutor)
  - [Механизмы синхронизации](#механизмы-синхронизации)
    - [Мьютекс (Mutex)](#мьютекс-mutex)
      - [Пример использования мьютекса](#пример-использования-мьютекса)
    - [Монитор (Monitor)](#монитор-monitor)
      - [Пример использования монитора](#пример-использования-монитора)
    - [Семафор (Semaphore)](#семафор-semaphore)
    - [Класс `java.util.concurrent.Semaphore`](#класс-javautilconcurrentsemaphore)
      - [Основные концепции](#основные-концепции)
      - [Основные методы](#основные-методы)
      - [Примеры использования Семафора](#примеры-использования-семафора)

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

## Пул потоков

### ExecutorService

`ExecutorService` — это интерфейс в Java, представляющий пул потоков, который позволяет управлять выполнением асинхронных задач. Он предоставляет мощные и гибкие средства для создания, запуска и управления потоками, что делает его предпочтительным способом работы с многопоточностью в современных Java-приложениях.

```java

    // Виды пулов
    ExecutorService executor = Executors.newFixedThreadPool(3); // Создает пул с фиксированным числом потоков. Если все потоки заняты, новые задачи будут ожидать в очереди.

    ExecutorService executor = Executors.newCachedThreadPool(); // Создает пул потоков, который создает новые потоки по мере необходимости, но повторно использует ранее созданные потоки, когда они становятся доступны. Подходит для задач с непредсказуемой нагрузкой

    ExecutorService executor = Executors.newSingleThreadExecutor(); // Создает пул, состоящий из одного потока, который последовательно выполняет задачи. Подходит для задач, которые должны выполняться строго последовательно

    ScheduledExecutorService executor = Executors.newScheduledThreadPool(5); // Создает пул потоков, который может планировать выполнение задач через определенные интервалы времени или с заданной задержкой
    executor.schedule(() -> System.out.println("Task executed after delay"), 5, TimeUnit.SECONDS);

    Future<String> future = executor.submit(() -> {
        // Задача
        return "Task completed";
    }); // Принимает задачу (Runnable или Callable) и передает ее в пул потоков для выполнения. Этот метод возвращает объект Future, который можно использовать для получения результата выполнения задачи или для проверки ее статуса

    // Получение результата
    String result = future.get();

    executor.execute(() -> {System.out.println("Task is running");}); //  Принимает объект Runnable и запускает его выполнение. Этот метод не возвращает Future, и поэтому не предоставляет возможности отслеживания завершения задачи или получения результата

    executor.shutdown(); // Метод инициирует мягкое завершение работы пула потоков. Он позволяет завершить выполнение уже начатых задач, но не принимает новые задачи

    // Немедленная остановка пула (осторожно, может прервать задачи)
    executor.shutdownNow(); //  Этот метод пытается остановить все выполняющиеся задачи и возвращает список задач, которые не были запущены. Используется в экстренных ситуациях, когда необходимо немедленно остановить все задачи

    executor.awaitTermination(60, TimeUnit.SECONDS); // Используется для ожидания завершения всех задач после вызова shutdown(). Он блокирует выполнение до тех пор, пока все задачи не завершатся или не истечет заданное время ожидания
```

### CompletableFuture

`CompletableFuture` — это класс в Java, предоставляющий мощный API для работы с асинхронными задачами. Он позволяет выполнять задачи параллельно, связывать их в цепочки зависимостей, обрабатывать результаты и исключения. CompletableFuture предоставляет современный способ работы с многопоточностью и асинхронностью.

```java
    // Создание асинхронной задачи
    CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
        System.out.println("Task is running asynchronously");
    }); // Запускает задачу в ForkJoinPool.commonPool() или в пользовательском пуле потоков, если он указан. Используется, если задача не возвращает результат

    CompletableFuture<String> futureWithResult = CompletableFuture.supplyAsync(() -> {
        return "Result of the task";
    }); // Похож на runAsync, но возвращает результат выполнения задачи. Используется, если нужно получить результат

    // Обработка результата
    futureWithResult.thenApply(result -> {
        return result.toUpperCase();
    }); // Обрабатывает результат выполнения предыдущей задачи и возвращает новый результат

    futureWithResult.thenAccept(result -> {
        System.out.println("Processed result: " + result);
    }); // Обрабатывает результат, не возвращая значения. Используется для побочных эффектов

    // Обработка исключений
    futureWithResult.exceptionally(ex -> {
        System.out.println("Exception occurred: " + ex.getMessage());
        return "Fallback result";
    }); // Обрабатывает исключение, возвращая значение по умолчанию

    // Объединение нескольких задач
    CompletableFuture<String> combined = futureWithResult.thenCombine(
        CompletableFuture.supplyAsync(() -> "Another result"),
        (result1, result2) -> result1 + " " + result2
    ); // Объединяет два результата из разных задач в один результат

    // Выполнение нескольких задач параллельно
    CompletableFuture<Void> allTasks = CompletableFuture.allOf(
        CompletableFuture.runAsync(() -> System.out.println("Task 1")),
        CompletableFuture.runAsync(() -> System.out.println("Task 2"))
    ); // Запускает несколько задач параллельно и возвращает `CompletableFuture`, который завершится, когда завершатся все задачи

    allTasks.join(); // Блокирует выполнение текущего потока до завершения всех задач

    // Обработка первой завершившейся задачи
    CompletableFuture<Object> anyTask = CompletableFuture.anyOf(
        CompletableFuture.supplyAsync(() -> "Task 1"),
        CompletableFuture.supplyAsync(() -> "Task 2")
    ); // Завершается при завершении первой из переданных задач и возвращает результат этой задачи

    // Завершение вручную
    CompletableFuture<String> manualCompletion = new CompletableFuture<>();
    manualCompletion.complete("Manual result"); // Завершает CompletableFuture вручную с указанным результатом

    manualCompletion.completeExceptionally(new RuntimeException("Manual failure")); // Завершает CompletableFuture с исключением

    // Комбинация асинхронности и пользовательского пула потоков
    ExecutorService customExecutor = Executors.newFixedThreadPool(3);
    CompletableFuture.runAsync(() -> {
        System.out.println("Running in custom executor");
    }, customExecutor); // Указывает пользовательский пул потоков для выполнения задачи

    // Прерывание или отмена задачи
    future.cancel(true); // Отменяет задачу. Если задача уже запущена, она может быть прервана, если поддерживает прерывание

    future.isCancelled(); // Проверяет, была ли задача отменена
    future.isDone(); // Проверяет, завершена ли задача
```

### ThreadPoolExecutor

`ThreadPoolExecutor` — это мощный класс в Java, предоставляющий гибкий API для создания и настройки пользовательских пулов потоков. Он позволяет точно контролировать параметры пула, такие как количество потоков, размеры очереди и политики отклонения задач.

```java
    // Создание кастомного пула потоков
    ThreadPoolExecutor executor = new ThreadPoolExecutor(
        2, // corePoolSize: минимальное количество потоков, которые остаются активными (даже если они не заняты)
        4, // maximumPoolSize: максимальное количество потоков, которые могут быть активными одновременно
        60, // keepAliveTime: время, в течение которого неактивные потоки превышающие corePoolSize, будут уничтожены
        TimeUnit.SECONDS, // единица измерения времени keepAliveTime
        new LinkedBlockingQueue<>(10) // очередь задач, которая хранит задачи, ожидающие выполнения
    );

    executor.execute(() -> {
        System.out.println("Task executed by " + Thread.currentThread().getName());
    }); // Запускает задачу в одном из доступных потоков. Если все потоки заняты и очередь заполнена, задача обрабатывается в соответствии с политикой отклонения

    // Политики обработки задач при переполнении очереди
    executor.setRejectedExecutionHandler(new ThreadPoolExecutor.AbortPolicy()); // Политика по умолчанию. Выбрасывает RejectedExecutionException
    executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy()); // Задача выполняется в текущем потоке
    executor.setRejectedExecutionHandler(new ThreadPoolExecutor.DiscardPolicy()); // Задача отбрасывается без уведомления
    executor.setRejectedExecutionHandler(new ThreadPoolExecutor.DiscardOldestPolicy()); // Отбрасывается самая старая задача в очереди

    // Ожидание завершения задач
    executor.shutdown(); // Инициирует мягкое завершение пула потоков
    executor.awaitTermination(60, TimeUnit.SECONDS); // Ожидает завершения всех задач в течение заданного времени

    // Немедленная остановка пула
    executor.shutdownNow(); // Прерывает все выполняющиеся задачи и возвращает список задач, которые не были запущены

    // Проверка состояния пула
    executor.getPoolSize(); // Возвращает текущее количество потоков в пуле
    executor.getActiveCount(); // Возвращает количество потоков, которые выполняют задачи
    executor.getQueue().size(); // Возвращает количество задач в очереди
```

### Сравнительная таблица ExecutorService, CompletableFuture, ThreadPoolExecutor

| Особенность                | ExecutorService                | CompletableFuture            | ThreadPoolExecutor          |
|----------------------------|--------------------------------|------------------------------|-----------------------------|
| **Тип**                   | Интерфейс                     | Класс для асинхронных задач | Класс для создания пулов    |
| **Уровень управления**     | Средний                       | Высокий (упрощённая работа с потоками) | Низкий (гибкость)          |
| **Асинхронные цепочки**     | Нет                           | Да                           | Нет                         |
| **Политики управления**    | Предустановленные реализации  | Управляется через `ForkJoinPool` | Полный контроль            |
| **Примеры использования**  | Обработка задач фиксированным количеством потоков | Асинхронные и цепочные вычисления | Создание кастомного пула   |

---

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

#### Пример использования мьютекса

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

#### Пример использования монитора

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

#### Основные концепции

- **Разрешения (Permits)**: Семафор управляет набором разрешений. Потоки должны получить разрешение (acquire) перед доступом к ресурсу и освободить разрешение (release) после использования.
- **Доступность**: Если разрешения доступны, поток может получить разрешение и продолжить работу. Если разрешения недоступны, поток будет заблокирован до тех пор, пока разрешение не станет доступным.

#### Основные методы

**Конструкторы**:

```java
   Semaphore(int permits); //  Создает семафор с заданным числом разрешений.
   Semaphore(int permits, boolean fair); //  Создает семафор с заданным числом разрешений и заданной политикой честности (fairness).
```

**Методы для получения разрешений**:

```java
   void acquire() throws InterruptedException; //  Получает одно разрешение, блокируя поток, если разрешения недоступны.
   void acquire(int permits) throws InterruptedException; //  Получает заданное число разрешений.
   void acquireUninterruptibly(); //  Получает одно разрешение, блокируя поток, если разрешения недоступны, но без прерывания.
   void acquireUninterruptibly(int permits); //  Получает заданное число разрешений без возможности прерывания.
```

**Методы для освобождения разрешений**:

```java
   void release(); //  Освобождает одно разрешение.
   void release(int permits); //  Освобождает заданное число разрешений.
```

**Методы для проверки состояния**:

```java
   int availablePermits(); //  Возвращает количество доступных разрешений.
   int drainPermits(); //  Сбрасывает все доступные разрешения и возвращает их количество.
   boolean hasQueuedThreads(); //  Проверяет, есть ли потоки, ожидающие разрешений.
   int getQueueLength(); //  Возвращает количество потоков, ожидающих разрешений.
```

**Дополнительные методы**:

```java
   Collection<Thread> getQueuedThreads(); //  Возвращает коллекцию потоков, ожидающих разрешений.
   boolean tryAcquire(); //  Пытается получить одно разрешение без блокировки.
   boolean tryAcquire(int permits); //  Пытается получить заданное число разрешений без блокировки.
   boolean tryAcquire(long timeout, TimeUnit unit) throws InterruptedException; //  Пытается получить одно разрешение, ожидая заданное время перед тем, как вернуть `false`, если разрешение недоступно.
   boolean tryAcquire(int permits, long timeout, TimeUnit unit) throws InterruptedException; //  Пытается получить заданное число разрешений с ожиданием.
```

#### Примеры использования Семафора

**Ограничение доступа к ресурсу**:

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

**Честный семафор**:

В этом примере используется честный семафор, который обеспечивает, что потоки будут получать доступ к ресурсу в порядке своей очереди.

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
