
# Коллекции

- [Коллекции](#коллекции)
  - [Collection](#collection)
    - [Основные методы Collection](#основные-методы-collection)
  - [List](#list)
    - [Основные методы List](#основные-методы-list)
    - [Реализации List](#реализации-list)
    - [1. ArrayList](#1-arraylist)
      - [Описание ArrayList](#описание-arraylist)
      - [Особенности ArrayList](#особенности-arraylist)
      - [Методы ArrayList](#методы-arraylist)
      - [Применение ArrayList](#применение-arraylist)
    - [2. LinkedList (List)](#2-linkedlist-list)
      - [Описание LinkedList (List)](#описание-linkedlist-list)
      - [Особенности LinkedList (List)](#особенности-linkedlist-list)
      - [Методы LinkedLis (List)](#методы-linkedlis-list)
      - [Применение LinkedList (List)](#применение-linkedlist-list)
    - [3. Vector](#3-vector)
      - [Описание Vector](#описание-vector)
      - [Особенности Vector](#особенности-vector)
      - [Методы](#методы)
      - [Применение](#применение)
    - [4. CopyOnWriteArrayList](#4-copyonwritearraylist)
      - [Описание](#описание)
      - [Особенности](#особенности)
      - [Методы CopyOnWriteArrayList](#методы-copyonwritearraylist)
      - [Применение CopyOnWriteArrayList](#применение-copyonwritearraylist)
  - [Queue](#queue)
    - [Основные методы Queue](#основные-методы-queue)
    - [Реализации Queue](#реализации-queue)
    - [1. LinkedList (Queue)](#1-linkedlist-queue)
      - [Описание LinkedList (Queue)](#описание-linkedlist-queue)
      - [Особенности LinkedList (Queue)](#особенности-linkedlist-queue)
      - [Методы LinkedList (Queue)](#методы-linkedlist-queue)
      - [Применение LinkedList (Queue)](#применение-linkedlist-queue)
    - [2. PriorityQueue](#2-priorityqueue)
      - [Описание PriorityQueue](#описание-priorityqueue)
      - [Особенности PriorityQueue](#особенности-priorityqueue)
      - [Методы PriorityQueue](#методы-priorityqueue)
      - [Применение PriorityQueue](#применение-priorityqueue)
      - [Примеры PriorityQueue](#примеры-priorityqueue)
    - [3. ArrayDeque](#3-arraydeque)
      - [Описание ArrayDeque](#описание-arraydeque)
      - [Особенности ArrayDeque](#особенности-arraydeque)
      - [Методы ArrayDeque](#методы-arraydeque)
      - [Применение ArrayDeque](#применение-arraydeque)
    - [4. ConcurrentLinkedQueue](#4-concurrentlinkedqueue)
      - [Описание ConcurrentLinkedQueue](#описание-concurrentlinkedqueue)
      - [Особенности ConcurrentLinkedQueue](#особенности-concurrentlinkedqueue)
      - [Методы ConcurrentLinkedQueue](#методы-concurrentlinkedqueue)
      - [Применение ConcurrentLinkedQueue](#применение-concurrentlinkedqueue)
    - [5. LinkedBlockingQueue](#5-linkedblockingqueue)
      - [Описание LinkedBlockingQueue](#описание-linkedblockingqueue)
      - [Особенности LinkedBlockingQueue](#особенности-linkedblockingqueue)
      - [Методы LinkedBlockingQueue](#методы-linkedblockingqueue)
      - [Применение LinkedBlockingQueue](#применение-linkedblockingqueue)
    - [6. ArrayBlockingQueue](#6-arrayblockingqueue)
      - [Описание ArrayBlockingQueue](#описание-arrayblockingqueue)
      - [Особенности ArrayBlockingQueue](#особенности-arrayblockingqueue)
      - [Методы ArrayBlockingQueue](#методы-arrayblockingqueue)
      - [Применение ArrayBlockingQueue](#применение-arrayblockingqueue)
    - [7. PriorityBlockingQueue](#7-priorityblockingqueue)
      - [Описание PriorityBlockingQueue](#описание-priorityblockingqueue)
      - [Особенности PriorityBlockingQueue](#особенности-priorityblockingqueue)
      - [Методы PriorityBlockingQueue](#методы-priorityblockingqueue)
      - [Применение PriorityBlockingQueue](#применение-priorityblockingqueue)
    - [8. SynchronousQueue](#8-synchronousqueue)
      - [Описание SynchronousQueue](#описание-synchronousqueue)
      - [Особенности SynchronousQueue](#особенности-synchronousqueue)
      - [Методы SynchronousQueue](#методы-synchronousqueue)
      - [Применение SynchronousQueue](#применение-synchronousqueue)
  - [Deque](#deque)
    - [Основные методы Deque](#основные-методы-deque)
    - [Реализации Deque](#реализации-deque)
    - [1. ArrayDeque](#1-arraydeque)
      - [Описание ArrayDeque (Deque)](#описание-arraydeque-deque)
      - [Особенности ArrayDeque (Deque)](#особенности-arraydeque-deque)
      - [Методы ArrayDeque (Deque)](#методы-arraydeque-deque)
      - [Применение ArrayDeque (Deque)](#применение-arraydeque-deque)
    - [2. LinkedList (Deque)](#2-linkedlist-deque)
      - [Описание LinkedList (Deque)](#описание-linkedlist-deque)
      - [Особенности LinkedList (Deque)](#особенности-linkedlist-deque)
      - [Методы LinkedList (Deque)](#методы-linkedlist-deque)
      - [Применение LinkedList (Deque)](#применение-linkedlist-deque)
    - [3. ConcurrentLinkedDeque](#3-concurrentlinkeddeque)
      - [Описание ConcurrentLinkedDeque](#описание-concurrentlinkeddeque)
      - [Особенности ConcurrentLinkedDeque](#особенности-concurrentlinkeddeque)
      - [Методы ConcurrentLinkedDeque](#методы-concurrentlinkeddeque)
      - [Применение ConcurrentLinkedDeque](#применение-concurrentlinkeddeque)
    - [4. LinkedBlockingDeque](#4-linkedblockingdeque)
      - [Описание LinkedBlockingDeque](#описание-linkedblockingdeque)
      - [Особенности LinkedBlockingDeque](#особенности-linkedblockingdeque)
      - [Методы LinkedBlockingDeque](#методы-linkedblockingdeque)
      - [Применение LinkedBlockingDeque](#применение-linkedblockingdeque)
  - [Set](#set)
    - [Основные методы Set](#основные-методы-set)
    - [Реализации Set](#реализации-set)
    - [1. HashSet](#1-hashset)
      - [Описание HashSet](#описание-hashset)
      - [Особенности HashSet](#особенности-hashset)
      - [Применение HashSet](#применение-hashset)
    - [2. LinkedHashSet](#2-linkedhashset)
      - [Описание LinkedHashSet](#описание-linkedhashset)
      - [Особенности LinkedHashSet](#особенности-linkedhashset)
      - [Применение LinkedHashSet](#применение-linkedhashset)
    - [3. TreeSet](#3-treeset)
      - [Описание TreeSet](#описание-treeset)
      - [Особенности TreeSet](#особенности-treeset)
      - [Применение TreeSet](#применение-treeset)
    - [4. ConcurrentSkipListSet](#4-concurrentskiplistset)
      - [Описание ConcurrentSkipListSet](#описание-concurrentskiplistset)
      - [Особенности ConcurrentSkipListSet](#особенности-concurrentskiplistset)
      - [Применение ConcurrentSkipListSet](#применение-concurrentskiplistset)
    - [5. CopyOnWriteArraySet](#5-copyonwritearrayset)
      - [Описание CopyOnWriteArraySet](#описание-copyonwritearrayset)
      - [Особенности CopyOnWriteArraySet](#особенности-copyonwritearrayset)
      - [Применение CopyOnWriteArraySet](#применение-copyonwritearrayset)
  - [Map](#map)
    - [Основные методы Map](#основные-методы-map)
    - [Реализации Map](#реализации-map)
    - [1. HashMap](#1-hashmap)
      - [Описание HashMap](#описание-hashmap)
      - [Особенности HashMap](#особенности-hashmap)
      - [Методы HashMap](#методы-hashmap)
      - [Применение HashMap](#применение-hashmap)
    - [2. LinkedHashMap](#2-linkedhashmap)
      - [Описание LinkedHashMap](#описание-linkedhashmap)
      - [Особенности LinkedHashMap](#особенности-linkedhashmap)
      - [Методы LinkedHashMap](#методы-linkedhashmap)
      - [Применение LinkedHashMap](#применение-linkedhashmap)
    - [3. TreeMap](#3-treemap)
      - [Описание TreeMap](#описание-treemap)
      - [Особенности TreeMap](#особенности-treemap)
      - [Методы TreeMap](#методы-treemap)
      - [Применение TreeMap](#применение-treemap)
    - [4. ConcurrentHashMap](#4-concurrenthashmap)
      - [Описание ConcurrentHashMap](#описание-concurrenthashmap)
      - [Особенности ConcurrentHashMap](#особенности-concurrenthashmap)
      - [Методы ConcurrentHashMap](#методы-concurrenthashmap)
      - [Применение ConcurrentHashMap](#применение-concurrenthashmap)
  - [Optional](#optional)
    - [Основные методы Optional](#основные-методы-optional)

**Что такое коллекции?** \
В Java коллекции представляют собой наборы объектов, которые могут храниться, управляться и манипулироваться в виде единой структуры данных. Коллекции предоставляют более гибкие и мощные механизмы для работы с группами объектов по сравнению с массивами.

**Зачем нужны коллекции?** \
Коллекции нужны для упрощения работы с группами объектов. Они предоставляют методы для добавления, удаления, поиска и итерации по элементам, что делает управление данными проще и эффективнее. Также они позволяют использовать различные алгоритмы и структуры данных, такие как списки, множества, очереди и карты.

Основные интерфейсы коллекций
В Java существует несколько ключевых интерфейсов для работы с коллекциями:

1. `Collection`: Базовый интерфейс для всех коллекций.
2. `List`: Упорядоченная коллекция, допускающая дубликаты. Примеры: ArrayList, LinkedList.
3. `Set`: Коллекция, не допускающая дубликатов. Примеры: HashSet, TreeSet.
4. `Queue`: Коллекция, представляющая очередь. Примеры: LinkedList, PriorityQueue.
5. `Map`: Коллекция, состоящая из пар ключ-значение. Примеры: HashMap, TreeMap.

## Collection

Collection -  базовый интерфейс для других коллекций

`ДОКУМЕНТАЦИЯ`: [Collection](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Collection.html)

### Основные методы Collection

```java
    Collection<Type> collection = ... ; // пример объявления коллекции

    // Осноные методы Collection:
    collection.size(); // возвращает размер коллекции
    collection.isEmpty(); // проверяет пустой ли список
    collection.contains(Object o); // проверка принадлежности коллекции
    collection.add([Object]); // добавление элемента к коллекции (возвращает boolean, который указывает изменилась коллекция или нет)
    collection.remove(Object o); // удаление элемента (возвращает boolean, который указывает изменилась коллекция или нет)
    collection.clear(); // очистить коллекцию

    // Collection реализует интерфейс Iterable, который имеет следующие методы:
    void forEach(Consumer<? super T> action) // Метод forEach принимает в качестве аргумента объект, реализующий функциональный интерфейс Consumer. Этот интерфейс содержит единственный метод accept, который выполняется для каждого элемента коллекции.
    collection.forEach(element -> System.out.println(element)); // Используем forEach с лямбда-выражением
    collection.forEach(System.out::println); // Используем forEach со ссылкой на метод (тоже реализация интерфейса Consumer)
    // * forEach не может менять значения, она работает с копиями данных. Однако она может менять изменяемые объекты (например, экземпляры классов):
    collection.forEach(person -> person.age += 1); // Изменение значения поля через forEach

    Iterator<Type> it = collection.iterator(); // преобразует коллекцию в итерируемы объект
    // * Итератор нужно использовать для изменения элементов коллекции
    it.hasNext(); // проверка на существание следующего элемента
    it.next(); // получение следующего элемента
    it.remove(); // удаляет текущий элемент из коллекции

    Spliterator<Type> sit = collection.spliterator(); // Этот метод возвращает объект типа Spliterator, который можно использовать для разделения элементов коллекции для параллельной обработки.
    sit.tryAdvance(Consumer<? super T> action); // Если есть еще элементы для обработки, выполняет действие action на следующем элементе и возвращает true.
    sit.trySplit(); // Попытка разделить текущий Spliterator на два. Возвращает новый Spliterator, который охватывает часть элементов, или null, если разделение невозможно.
    sit.estimateSize(); // Оценивает количество оставшихся элементов для обработки. Если количество элементов не известно, возвращает Long.MAX_VALUE.
    sit.characteristics(); // Возвращает битовую маску характеристик этого Spliterator. Возможные характеристики:
        // * ORDERED: элементы имеют определенный порядок.
        // * DISTINCT: элементы уникальны.
        // * SORTED: элементы отсортированы.
        // * SIZED: известно точное количество элементов.
        // * NONNULL: элементы не содержат null.
        // * IMMUTABLE: элементы неизменяемы.
        // * CONCURRENT: поддерживается параллельная модификация.
        // * SUBSIZED: все разделенные Spliterator имеют известный размер.

```

## List

Интерфейс `List` в Java является частью коллекционного фреймворка и представляет собой упорядоченную коллекцию, которая допускает дубликаты. Элементы в списке упорядочены в определенной последовательности, и доступ к ним осуществляется с использованием индексов.

`ДОКУМЕНТАЦИЯ`: [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)

Интерфейс `List` расширяет интерфейс [Collection](#collection)

### Основные методы List

```java
    import java.util.List; // импорт интерфейса List

    List<Type> list = new ArrayList<>();

    list.get([idx]); // получение элемента по индексу
    list.set([idx], value); // изменить элемент по индексу
    list.add([idx], value); // добавить элемент в указанное место
    list.remove([idx]); // удалить элемент по индексу
    list.indexOf(Object o); // поиск элемента по его содержанию (возвращает первый индекс)
    list.lastIndexOf(Object o); // поиск элемента по его содержанию (возвращает последний индекс)
    list.sublist([fromIndex],[toIndex]); // возвращает часть списка по его индексам
    * Возвращает новый элемент List<E>, !однако списки остаются связанными!
    * Позволяет реализовывать новый функционал:
    list.sublist(1, 2).clear(); // очистить часть списка
    list.sublist(1, 2).indexOf("foo"); // найти индекс элемента по значению в указанном отрезке
    list.equals(list1); // сравнение списков по содержимому
    Object[] objectArr = list.toArray(); // перевод списка в массив объектов
    Type[] typeArr = list.toArray(new Type[list.size()]); // перевод списка в массив объектов определенного типа

    // Полезные функции

    import java.util.Collections; // импорт класса Collections;

    Collections.shuffle(list); // перемешать список
    Collections.sort(list); // сортирует указанный список
    Collections.unmodifiableSet(originalSet); // возвращает обертку для данного класса, которая запрещает вносить изменения в получившийся объект
    // * Может быть применени и к другим коллекциям, например:
    unmodifiableList() // к List
    unmodifiableMap() // к Map
```

### Реализации List

Интерфейс `List` в Java имеет несколько реализаций, каждая из которых имеет свои особенности, преимущества и ограничения. Основные реализации включают `ArrayList`, `LinkedList`, `Vector` и `CopyOnWriteArrayList`. Рассмотрим каждую из них подробно.

### 1. ArrayList

#### Описание ArrayList

`ArrayList` — это реализация `List`, основанная на массиве. Она предоставляет динамически изменяемый массив, который автоматически расширяется по мере добавления элементов.

#### Особенности ArrayList

- **Скорость доступа**: Быстрый доступ к элементам по индексу (O(1)).
- **Вставка и удаление**: Медленные операции вставки и удаления, особенно в середине списка (O(n)), так как элементы сдвигаются.
- **Ресурсоемкость**: Требует больше памяти, чем фактически необходимо, из-за внутреннего массива, который часто выделяется с запасом.

#### Методы ArrayList

`ArrayList` не добавляет новых методов к интерфейсу `List`, но предоставляет высокоэффективную реализацию методов `get`, `set`, `add`, и `remove`.

#### Применение ArrayList

- Подходит для случаев, когда основная операция — это доступ к элементам по индексу.
- Идеален для чтения данных и редкого обновления.

### 2. LinkedList (List)

#### Описание LinkedList (List)

`LinkedList` — это реализация `List`, основанная на двусвязном списке. Каждый элемент списка содержит ссылку на предыдущий и следующий элементы.

#### Особенности LinkedList (List)

- **Скорость доступа**: Медленный доступ к элементам по индексу (O(n)).
- **Вставка и удаление**: Быстрые операции вставки и удаления (O(1)), если известно местоположение узла.
- **Ресурсоемкость**: Использует больше памяти на каждый элемент из-за хранения ссылок.

#### Методы LinkedLis (List)

`LinkedList` добавляет методы из интерфейса `Deque`, так как реализует 2 интерфейса:

``` java
    void addFirst(E e); // Вставляет элемент в начало списка.
    void addLast(E e); // Вставляет элемент в конец списка.
    E getFirst(); // Возвращает первый элемент списка.
    E getLast(); // Возвращает последний элемент списка.
    E removeFirst(); // Удаляет и возвращает первый элемент списка.
    E removeLast(); // Удаляет и возвращает последний элемент списка.
```

#### Применение LinkedList (List)

- Подходит для случаев, когда часто выполняются операции вставки и удаления.
- Идеален для реализации очередей и стеков.

### 3. Vector

#### Описание Vector

`Vector` — это синхронизированная реализация `List`, основанная на массиве. Это устаревшая версия динамического массива, введенная в ранних версиях Java.

#### Особенности Vector

- **Скорость доступа**: Быстрый доступ к элементам по индексу (O(1)).
- **Вставка и удаление**: Медленные операции вставки и удаления (O(n)).
- **Синхронизация**: Все методы синхронизированы, что делает его безопасным для многопоточного использования, но добавляет оверхед.

#### Методы

`Vector` добавляет несколько методов:

``` java
    void addElement(E obj); // Добавляет элемент в конец вектора.
    int capacity(); // Возвращает текущую емкость вектора.
    void ensureCapacity(int minCapacity); // Увеличивает емкость вектора, если она меньше указанной.
```

#### Применение

- Подходит для многопоточных сред, где требуется синхронизированный доступ к элементам.
- В современных приложениях рекомендуется использовать `ArrayList` с ручной синхронизацией.

### 4. CopyOnWriteArrayList

#### Описание

`CopyOnWriteArrayList` — это реализация `List`, предназначенная для многопоточного доступа. Каждый раз, когда в список вносятся изменения, создается новая копия массива.

#### Особенности

- **Скорость доступа**: Быстрый доступ к элементам по индексу (O(1)).
- **Вставка и удаление**: Медленные операции вставки и удаления (O(n)), так как создается копия массива.
- **Синхронизация**: Обеспечивает безопасный доступ для чтения без необходимости внешней синхронизации.

#### Методы CopyOnWriteArrayList

`CopyOnWriteArrayList` не добавляет новых методов, но изменяет поведение существующих для обеспечения безопасного многопоточного доступа.

#### Применение CopyOnWriteArrayList

- Подходит для случаев, когда операции чтения значительно преобладают над операциями записи.
- Идеален для реализации неизменяемых коллекций в многопоточных средах.

```java
    List<Type> list = new ArrayList<>(Arrays.asList(array)); // объявление списка на базе массива
    * Arrays.asList(array) - позволяет перевести какой то массив в List
    ** Можно передавать в любой экземпляр класса Collection
    * Также можно написать начения через запятую: ... new ArrayList<>(1,2,3,4,5);

    List<Type> list = new LinkedList<>(); - объявление списка, основанного на двусвязном списке
    * Позволяет эффективно добавлять и удалять элементы в начале или конце списка
    * Доступ по индексу не такой эффективный
```

## Queue

Интерфейс `Queue` в Java представляет собой коллекцию, предназначенную для хранения элементов, которые обрабатываются в определенном порядке. Обычно очереди следуют принципу FIFO (First-In-First-Out)

Интерфейс Queue расширяет интерфейс [Collection](#collection) и добавляет методы для вставки, извлечения и просмотра элементов. Все эти методы могут быть разделены на два основных набора: методы, которые бросают исключения при неудаче, и методы, которые возвращают специальное значение (или null) при неудаче.

`ДОКУМЕНТАЦИЯ`: [Queue](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Queue.html)

### Основные методы Queue

```java
    import java.util.LinkedList;
    import java.util.Queue;
    Queue<Type> queue = new LinkedList<>();

    queue.add(element); - добавляет элемент в хвост очереди (если не удается добавить элемент из-за ограничения по размеру, например, то будет выдано исключение)
    queue.offer(element); - добавляет элемент в хвост очереди (если не удается добавить элемент из-за ограничения по размеру, например, то будет возвращен false)

    queue.remove(); - извлекают первый элемент из головы очереди (если извлечь не получается, то кидает ошибку)
    queue.poll(); - извлекают первый элемент из головы очереди (если извлечь не получается, то возвращает null)

    queue.element(); - позволяет получить элемент из головы очереди, но не удалаять его (если извлечь не получается, то кидает ошибку)
    queue.peek();- позволяет получить элемент из головы очереди, но не удалаять его (если извлечь не получается, то возвращает null)
```

### Реализации Queue

Интерфейс `Queue` в Java имеет несколько реализаций, каждая из которых предназначена для различных сценариев использования и имеет свои особенности. Основные реализации включают `LinkedList`, `PriorityQueue`, `ArrayDeque`, а также многопоточные реализации из пакета `java.util.concurrent`. Рассмотрим каждую из них подробно.

### 1. LinkedList (Queue)

#### Описание LinkedList (Queue)

`LinkedList` реализует интерфейсы `List`, `Deque`, и `Queue`. Эта реализация основана на двусвязном списке.

#### Особенности LinkedList (Queue)

- **Доступ и изменение**: Быстрое добавление и удаление элементов с начала и конца списка.
- **Память**: Использует больше памяти на элемент по сравнению с массивами из-за хранения ссылок.

#### Методы LinkedList (Queue)

`LinkedList` добавляет методы из интерфейса `Deque`, такие как `addFirst`, `addLast`, `removeFirst`, `removeLast`, `getFirst`, `getLast`.

#### Применение LinkedList (Queue)

- Подходит для реализации очередей FIFO и двусторонних очередей.
- Идеален для частых операций добавления и удаления элементов с начала и конца списка.

### 2. PriorityQueue

#### Описание PriorityQueue

`PriorityQueue` реализует приоритетную очередь на основе бинарной кучи. Элементы очереди извлекаются в порядке их приоритета.

#### Особенности PriorityQueue

- **Приоритет**: Элементы с наивысшим приоритетом извлекаются первыми.
- **Сортировка**: Элементы упорядочиваются в соответствии с их естественным порядком или компаратором, заданным при создании очереди.

#### Методы PriorityQueue

`PriorityQueue` не добавляет новых методов к интерфейсу `Queue`, но изменяет поведение методов `add`, `offer`, `poll`, и `remove` в соответствии с приоритетом элементов.

#### Применение PriorityQueue

- Подходит для задач, где элементы должны обрабатываться в порядке приоритета.
- Идеален для алгоритмов, таких как алгоритм Дейкстры для нахождения кратчайшего пути.

#### Примеры PriorityQueue

**Пример 1: Использование с естественным порядком**:

```java
    import java.util.PriorityQueue;
    PriorityQueue<Integer> pq = new PriorityQueue<>();

    pq.add(10); pq.add(20); pq.add(15);

    // Извлечение элементов в порядке приоритета
    System.out.println(pq.poll()); // 10
    System.out.println(pq.poll()); // 15
    System.out.println(pq.poll()); // 20
```

**Пример 2: Использование с компаратором**:

```java
    import java.util.PriorityQueue;
    Comparator<String> lengthComparator = (s1, s2) -> Integer.compare(s1.length(), s2.length());
    PriorityQueue<String> pq = new PriorityQueue<>(lengthComparator);

    pq.add("apple"); pq.add("banana"); pq.add("cherry");

    // Извлечение элементов в порядке длины строк
    System.out.println(pq.poll()); // apple
    System.out.println(pq.poll()); // cherry
    System.out.println(pq.poll()); // banana
```

**Пример 3: Реализация задачи с приоритетами**:

Предположим, у нас есть класс Task с полем priority, и мы хотим управлять задачами в очереди с приоритетом.

```java
    import java.util.PriorityQueue;

    class Task implements Comparable<Task> {
        private String name;
        private int priority;

        public Task(String name, int priority) {
            this.name = name;
            this.priority = priority;
        }

        @Override
        public int compareTo(Task other) {
            return Integer.compare(this.priority, other.priority);
        }
    }
    // ...
    PriorityQueue<Task> taskQueue = new PriorityQueue<>();

    taskQueue.add(new Task("Write code", 2));
    taskQueue.add(new Task("Debug code", 1));
    taskQueue.add(new Task("Test code", 3));

    while (!taskQueue.isEmpty()) {
        System.out.println("Processing task: " + taskQueue.poll());
    }
```

### 3. ArrayDeque

#### Описание ArrayDeque

`ArrayDeque` реализует интерфейсы `Deque` и `Queue` и основан на массиве, который автоматически увеличивается по мере необходимости.

#### Особенности ArrayDeque

- **Производительность**: Быстрая амортизированная вставка и удаление элементов с начала и конца.
- **Память**: Меньше накладных расходов по сравнению с `LinkedList`.

#### Методы ArrayDeque

`ArrayDeque` добавляет методы из интерфейса `Deque`, такие как `addFirst`, `addLast`, `removeFirst`, `removeLast`, `getFirst`, `getLast`.

#### Применение ArrayDeque

- Подходит для реализации стеков и очередей FIFO.
- Идеален для многопоточных сред благодаря отсутствию синхронизации.

### 4. ConcurrentLinkedQueue

#### Описание ConcurrentLinkedQueue

`ConcurrentLinkedQueue` — это многопоточная неблокирующая очередь, основанная на алгоритме Майкла-Скотта (Michael-Scott).

#### Особенности ConcurrentLinkedQueue

- **Безопасность для многопоточности**: Реализация не требует внешней синхронизации.
- **Производительность**: Высокая производительность в многопоточных средах.

#### Методы ConcurrentLinkedQueue

`ConcurrentLinkedQueue` не добавляет новых методов к интерфейсу `Queue`, но все методы безопасны для использования несколькими потоками.

#### Применение ConcurrentLinkedQueue

- Подходит для многопоточных приложений, где требуется неблокирующий доступ к очереди.
- Идеален для реализации пула задач.

### 5. LinkedBlockingQueue

#### Описание LinkedBlockingQueue

`LinkedBlockingQueue` — это блокирующая очередь, основанная на связном списке. Очередь поддерживает лимит на максимальный размер.

#### Особенности LinkedBlockingQueue

- **Блокировка**: Поддержка блокирующих операций `put` и `take`.
- **Память**: Использует больше памяти на элемент по сравнению с массивами.

#### Методы LinkedBlockingQueue

`LinkedBlockingQueue` добавляет методы:

```java
boolean offer(E e, long timeout, TimeUnit unit) // Вставляет элемент, ожидая свободного места в течение указанного времени.
E poll(long timeout, TimeUnit unit) // Извлекает и удаляет элемент, ожидая появления элемента в течение указанного времени.
void put(E e) // Вставляет элемент, ожидая свободного места.
E take() // Извлекает и удаляет элемент, ожидая появления элемента.
```

#### Применение LinkedBlockingQueue

- Подходит для многопоточных приложений, где требуется блокирующий доступ к очереди.
- Идеален для реализации производственных потоков и потребителей.

### 6. ArrayBlockingQueue

#### Описание ArrayBlockingQueue

`ArrayBlockingQueue` — это блокирующая очередь, основанная на массиве фиксированного размера.

#### Особенности ArrayBlockingQueue

- **Фиксированный размер**: Размер очереди задается при создании и не может изменяться.
- **Блокировка**: Поддержка блокирующих операций `put` и `take`.

#### Методы ArrayBlockingQueue

Методы аналогичны `LinkedBlockingQueue`.

#### Применение ArrayBlockingQueue

- Подходит для многопоточных приложений, где требуется блокирующий доступ к очереди с фиксированным размером.
- Идеален для задач, требующих фиксированного количества элементов в очереди.

### 7. PriorityBlockingQueue

#### Описание PriorityBlockingQueue

`PriorityBlockingQueue` — это блокирующая версия `PriorityQueue`, основанная на бинарной куче.

#### Особенности PriorityBlockingQueue

- **Приоритет**: Элементы обрабатываются в порядке приоритета.
- **Блокировка**: Поддержка блокирующих операций.

#### Методы PriorityBlockingQueue

Методы аналогичны `PriorityQueue`, но добавляются блокирующие операции.

#### Применение PriorityBlockingQueue

- Подходит для многопоточных приложений, где элементы должны обрабатываться в порядке приоритета.
- Идеален для задач, требующих приоритетного выполнения.

### 8. SynchronousQueue

#### Описание SynchronousQueue

`SynchronousQueue` — это блокирующая очередь, в которой каждая операция вставки должна ждать соответствующую операцию удаления и наоборот.

#### Особенности SynchronousQueue

- **Без буфера**: Элементы передаются напрямую между потоками.
- **Блокировка**: Поддержка блокирующих операций.

#### Методы SynchronousQueue

Методы аналогичны другим блокирующим очередям, но каждая операция вставки блокируется до тех пор, пока не будет выполнена соответствующая операция удаления.

#### Применение SynchronousQueue

- Подходит для многопоточных приложений, где требуется непосредственная передача данных между потоками.
- Идеален для задач, требующих синхронной передачи данных.

## Deque

Интерфейс `Deque` (Double-Ended Queue) в Java представляет собой двустороннюю очередь, которая поддерживает вставку и удаление элементов с обеих сторон. Этот интерфейс расширяет интерфейс [Queue](#queue) и предоставляет дополнительные методы для работы с элементами на обоих концах очереди.

`ДОКУМЕНТАЦИЯ`: [Deque](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Deque.html)

### Основные методы Deque

```java
    Deque<Type> deque = new ArrayDeque<>(); // способ создания дека на базе массива
    Deque<Type> deque1 = new LinkedList<>(); // способ создания дека на базе двусвязанного списка

    // Методы для работы с первым элементом

    void deque.addFirst(E e) // Вставляет элемент в начало очереди. Бросает исключение `IllegalStateException`, если очередь ограничена по размеру и переполнена.
    boolean deque.offerFirst(E e) // Вставляет элемент в начало очереди. Возвращает `true`, если вставка успешна, иначе возвращает `false`.
    E deque.removeFirst() // Удаляет и возвращает первый элемент из очереди. Бросает исключение `NoSuchElementException`, если очередь пуста.
    E deque.pollFirst() // Удаляет и возвращает первый элемент из очереди. Возвращает `null`, если очередь пуста.
    E deque.getFirst() // Возвращает первый элемент из очереди без удаления. Бросает исключение `NoSuchElementException`, если очередь пуста.
    E deque.peekFirst() // Возвращает первый элемент из очереди без удаления. Возвращает `null`, если очередь пуста.

    // Методы для работы с последним элементом

    void deque.addLast(E e) // Вставляет элемент в конец очереди. Бросает исключение `IllegalStateException`, если очередь ограничена по размеру и переполнена.
    boolean deque.offerLast(E e) // Вставляет элемент в конец очереди. Возвращает `true`, если вставка успешна, иначе возвращает `false`.
    E deque.removeLast() // Удаляет и возвращает последний элемент из очереди. Бросает исключение `NoSuchElementException`, если очередь пуста.
    E deque.pollLast() // Удаляет и возвращает последний элемент из очереди. Возвращает `null`, если очередь пуста.
    E deque.getLast() // Возвращает последний элемент из очереди без удаления. Бросает исключение `NoSuchElementException`, если очередь пуста.
    E deque.peekLast() // Возвращает последний элемент из очереди без удаления. Возвращает `null`, если очередь пуста.

    // Методы для работы со стэком

    void deque.push(E e) // Вставляет элемент в начало очереди (эквивалентно `addFirst`).
    E deque.pop() // Удаляет и возвращает первый элемент из очереди (эквивалентно `removeFirst`).

    // Методы для удаления элементов

    boolean deque.removeFirstOccurrence(Object o) // Удаляет первое вхождение указанного элемента из очереди.
    boolean deque.removeLastOccurrence(Object o) // Удаляет последнее вхождение указанного элемента из очереди.
```

### Реализации Deque

Интерфейс `Deque` в Java имеет несколько реализаций, каждая из которых предназначена для различных сценариев использования и имеет свои особенности. Основные реализации включают `ArrayDeque`, `LinkedList`, и многопоточные реализации из пакета `java.util.concurrent`, такие как `ConcurrentLinkedDeque` и `LinkedBlockingDeque`. Рассмотрим каждую из них подробно.

### 1. ArrayDeque

#### Описание ArrayDeque (Deque)

`ArrayDeque` реализует интерфейсы `Deque` и `Queue` и основан на массиве, который автоматически увеличивается по мере необходимости.

#### Особенности ArrayDeque (Deque)

- **Производительность**: Быстрая амортизированная вставка и удаление элементов с начала и конца.
- **Память**: Меньше накладных расходов по сравнению с `LinkedList`, так как не требуется дополнительная память для хранения ссылок на элементы.
- **Нет синхронизации**: Не является потокобезопасной.

#### Методы ArrayDeque (Deque)

`ArrayDeque` не добавляет новых методов к интерфейсу `Deque`, но реализует все методы `Deque` эффективно благодаря внутреннему использованию массива.

#### Применение ArrayDeque (Deque)

- Подходит для реализации стеков и очередей FIFO.
- Идеален для однопоточных сред, где требуется быстрая вставка и удаление элементов.

### 2. LinkedList (Deque)

#### Описание LinkedList (Deque)

`LinkedList` реализует интерфейсы `List`, `Deque`, и `Queue`. Эта реализация основана на двусвязном списке.

#### Особенности LinkedList (Deque)

- **Производительность**: Быстрая вставка и удаление элементов с начала и конца.
- **Память**: Использует больше памяти на элемент по сравнению с массивами из-за хранения ссылок.
- **Гибкость**: Может быть использован как список, очередь или стек.

#### Методы LinkedList (Deque)

`LinkedList` добавляет методы из интерфейса `Deque`, такие как `addFirst`, `addLast`, `removeFirst`, `removeLast`, `getFirst`, `getLast`.

#### Применение LinkedList (Deque)

- Подходит для задач, требующих частых операций добавления и удаления элементов с начала и конца.
- Идеален для реализации двусторонних очередей и стеков.

### 3. ConcurrentLinkedDeque

#### Описание ConcurrentLinkedDeque

`ConcurrentLinkedDeque` — это неблокирующая многопоточная реализация интерфейса `Deque` на основе алгоритма Майкла-Скотта.

#### Особенности ConcurrentLinkedDeque

- **Производительность**: Высокая производительность в многопоточных средах.
- **Безопасность для многопоточности**: Реализация не требует внешней синхронизации.
- **Память**: Использует больше памяти на элемент по сравнению с массивами из-за хранения ссылок.

#### Методы ConcurrentLinkedDeque

`ConcurrentLinkedDeque` не добавляет новых методов к интерфейсу `Deque`, но все методы безопасны для использования несколькими потоками.

#### Применение ConcurrentLinkedDeque

- Подходит для многопоточных приложений, где требуется неблокирующий доступ к двусторонней очереди.
- Идеален для задач, требующих высокопроизводительного многопоточного доступа.

### 4. LinkedBlockingDeque

#### Описание LinkedBlockingDeque

`LinkedBlockingDeque` — это блокирующая двусторонняя очередь, основанная на двусвязном списке. Очередь поддерживает лимит на максимальный размер.

#### Особенности LinkedBlockingDeque

- **Блокировка**: Поддержка блокирующих операций `put` и `take`.
- **Память**: Использует больше памяти на элемент по сравнению с массивами.
- **Производительность**: Потокобезопасна благодаря блокировкам.

#### Методы LinkedBlockingDeque

`LinkedBlockingDeque` добавляет методы:

```java
boolean offerFirst(E e, long timeout, TimeUnit unit) //  Вставляет элемент в начало очереди, ожидая свободного места в течение указанного времени.
boolean offerLast(E e, long timeout, TimeUnit unit) //  Вставляет элемент в конец очереди, ожидая свободного места в течение указанного времени.
E pollFirst(long timeout, TimeUnit unit) //  Извлекает и удаляет первый элемент из очереди, ожидая появления элемента в течение указанного времени.
E pollLast(long timeout, TimeUnit unit) //  Извлекает и удаляет последний элемент из очереди, ожидая появления элемента в течение указанного времени.
void putFirst(E e) //  Вставляет элемент в начало очереди, ожидая свободного места.
void putLast(E e) //  Вставляет элемент в конец очереди, ожидая свободного места.
E takeFirst() //  Извлекает и удаляет первый элемент из очереди, ожидая появления элемента.
E takeLast() //  Извлекает и удаляет последний элемент из очереди, ожидая появления элемента.
```

#### Применение LinkedBlockingDeque

- Подходит для многопоточных приложений, где требуется блокирующий доступ к двусторонней очереди.
- Идеален для задач, требующих синхронной передачи данных между потоками.

## Set

Интерфейс `Set` в Java представляет собой коллекцию, которая не допускает дублирующихся элементов. Этот интерфейс расширяет интерфейс [Collection](#collection) и определяет поведение коллекций, в которых каждое значение может встречаться только один раз.

Порядок элементов не гарантирован.

`ДОКУМЕНТАЦИЯ`: [Set](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)

### Основные методы Set

Так как `Set` расширяет интерфейс `Collection`, он наследует все методы `Collection`. Однако, некоторые методы из `Collection` имеют специфическое поведение для `Set`.

```java
    import java.util.Set;

    Set<Type> set = new HashSet<>(); // способ создания множества при помощи хеш таблиц
    // ** Для данной реализации очень важно согласование equals() и hashCode() (a.equals(b) => a.hashCode() == b.hashCode())
    // * При переборе элементов они будут возвращаться в случаном порядке (по значению hashCode элементов)
    Set<Type> set = new LinkedHashSet<>(); // способ создания множества
    // * При переборе элементов они будут возвращаться в порядке добавления во множество

    // Методы для добавления элементов

    boolean set.add(E e) //  Добавляет элемент в набор, если он еще не присутствует. Возвращает `true`, если элемент был добавлен (то есть он не был в наборе ранее), и `false`, если элемент уже был в наборе.

    // Методы для удаления элементов

    boolean set.remove(Object o) //  Удаляет элемент из набора, если он присутствует. Возвращает `true`, если элемент был удален, и `false`, если элемент не был найден в наборе.
    void set.clear() //  Удаляет все элементы из набора.

    // Методы для проверки наличия элементов

    boolean set.contains(Object o) //  Проверяет, содержится ли указанный элемент в наборе. Возвращает `true`, если элемент присутствует в наборе, и `false` в противном случае.

    // Методы для размерности и пустоты

    int set.size() //  Возвращает количество элементов в наборе.
    boolean set.isEmpty() //  Проверяет, пуст ли набор. Возвращает `true`, если набор пуст, и `false` в противном случае.

    // Методы для итерации

    Iterator<E> set.iterator() //  Возвращает итератор, который позволяет обходить элементы набора.

    // Методы для операций с другими коллекциями

    boolean set.addAll(Collection<? extends E> c) //  Добавляет все элементы из указанной коллекции в набор. Возвращает `true`, если набор был изменен в результате этой операции (то есть, если хотя бы один элемент из коллекции не был ранее в наборе).
    boolean set.containsAll(Collection<?> c) //  Проверяет, содержатся ли все элементы указанной коллекции в наборе.
    boolean set.removeAll(Collection<?> c) //  Удаляет из набора все его элементы, которые содержатся в указанной коллекции. Возвращает `true`, если набор был изменен в результате этой операции.
    boolean set.retainAll(Collection<?> c) //  Удаляет из набора все элементы, которые не содержатся в указанной коллекции. Возвращает `true`, если набор был изменен в результате этой операции.

    SortedSet<Type> sortedSet = new TreeSet<>(); // создание отсартированного множества при помощи бинарного дерева поиска

    sortedSet.subSet(fromElement, toElement); // возвращает элементы множетсва, которые принадлежат данному промежутку
    sortedSet.headSet(toElement); // возвращает элементы множетсва, которые меньше данного элемента
    sortedSet.tailSet(toElement); // возвращает элементы множетсва, которые больше данного элемента
    // * Все методы возвращают не копию множества, а его чать (т.е. они связаны)
    sortedSet.first(); // возвращает первый элемент множетсва
    sortedSet.last(); // возвращает последний элемент множетсва
```

### Реализации Set

Интерфейс `Set` в Java имеет несколько ключевых реализаций, каждая из которых имеет свои особенности и подходит для различных сценариев использования. Основные реализации включают `HashSet`, `LinkedHashSet`, `TreeSet`, и многопоточные реализации из пакета `java.util.concurrent`, такие как `ConcurrentSkipListSet` и `CopyOnWriteArraySet`. Давайте рассмотрим каждую из них подробно.

### 1. HashSet

#### Описание HashSet

`HashSet` — это самая простая и широко используемая реализация интерфейса `Set`, основанная на хэш-таблице.

#### Особенности HashSet

- **Производительность**: Быстрое добавление, удаление и проверка наличия элемента (в среднем O(1)).
- **Порядок**: Не гарантирует порядок элементов.
- **Дополнительные методы**: Не добавляет дополнительных методов.

#### Применение HashSet

- Подходит для случаев, когда требуется быстрая производительность и нет необходимости в упорядоченности элементов.
- Идеален для хранения уникальных элементов без необходимости в определенном порядке.

### 2. LinkedHashSet

#### Описание LinkedHashSet

`LinkedHashSet` расширяет `HashSet` и использует связный список для поддержания порядка вставки элементов.

#### Особенности LinkedHashSet

- **Производительность**: Быстрое добавление, удаление и проверка наличия элемента (в среднем O(1)), немного медленнее, чем `HashSet`, из-за поддержания порядка вставки.
- **Порядок**: Гарантирует сохранение порядка вставки элементов.
- **Дополнительные методы**: Не добавляет дополнительных методов.

#### Применение LinkedHashSet

- Подходит для случаев, когда требуется сохранение порядка вставки элементов.
- Идеален для приложений, где важен порядок элементов наряду с уникальностью.

### 3. TreeSet

#### Описание TreeSet

`TreeSet` реализует интерфейсы `NavigableSet` и `SortedSet` и основан на красно-черном дереве.

#### Особенности TreeSet

- **Производительность**: Добавление, удаление и проверка наличия элемента имеют логарифмическое время выполнения (O(log n)).
- **Порядок**: Гарантирует упорядоченность элементов по их естественному порядку или по предоставленному компаратору.
- **Дополнительные методы**: Добавляет методы из `NavigableSet` и `SortedSet`.

#### Применение TreeSet

- Подходит для случаев, когда требуется упорядоченность элементов и логарифмическое время выполнения операций.
- Идеален для задач, требующих диапазонных запросов и сортированных наборов данных.

### 4. ConcurrentSkipListSet

#### Описание ConcurrentSkipListSet

`ConcurrentSkipListSet` — это потокобезопасная реализация `NavigableSet`, основанная на алгоритме skip list (пропускной список).

#### Особенности ConcurrentSkipListSet

- **Производительность**: Добавление, удаление и проверка наличия элемента имеют логарифмическое время выполнения (O(log n)).
- **Порядок**: Гарантирует упорядоченность элементов по их естественному порядку или по предоставленному компаратору.
- **Потокобезопасность**: Все методы безопасны для многопоточного использования.
- **Дополнительные методы**: Добавляет методы из `NavigableSet` и `SortedSet`.

#### Применение ConcurrentSkipListSet

- Подходит для многопоточных приложений, где требуется упорядоченность элементов.
- Идеален для задач, требующих потокобезопасного доступа к сортированным наборам данных.

### 5. CopyOnWriteArraySet

#### Описание CopyOnWriteArraySet

`CopyOnWriteArraySet` — это потокобезопасная реализация `Set`, основанная на механизме копирования при записи.

#### Особенности CopyOnWriteArraySet

- **Производительность**: Быстрое чтение элементов (O(1)), медленные операции записи (O(n)), так как при каждой модификации создается копия внутреннего массива.
- **Порядок**: Гарантирует сохранение порядка вставки элементов.
- **Потокобезопасность**: Все методы безопасны для многопоточного использования.
- **Дополнительные методы**: Не добавляет дополнительных методов.

#### Применение CopyOnWriteArraySet

- Подходит для многопоточных приложений, где операции чтения значительно преобладают над операциями записи.
- Идеален для неизменяемых наборов данных и сценариев, где частые изменения не требуются.

## Map

Интерфейс `Map` в Java представляет собой коллекцию, которая связывает ключи с соответствующими значениями. Карта не может содержать дублирующихся ключей, и каждый ключ может быть связан с не более чем одним значением.

- Позволяет индексировать элементы другими объектами

`ДОКУМЕНТАЦИЯ`: [Map](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Map.html)

### Основные методы Map

```java
    import java.util.Map;

    Map<TypeKey, TypeValue> hashMap = new HashMap<>();
    Map<TypeKey, TypeValue> linkedMap = new LinkedHashMap<>();
    Map<TypeKey, TypeValue> treeMap = new TreeMap<>();
    * Отличия данных вариаций аналогичны отличиям подобных в методе Set
        
    // Методы для работы с элементами

    V map.put(K key, V value); // Вставляет указанное значение, связанное с указанным ключом, в карту. Если карта ранее содержала значение для этого ключа, старое значение заменяется.
    V map.get(Object key); // Возвращает значение, связанное с указанным ключом, или `null`, если карта не содержит такого ключа.
    V map.remove(Object key); // Удаляет отображение для указанного ключа, если оно присутствует.
    boolean map.containsKey(Object key); // Возвращает `true`, если карта содержит отображение для указанного ключа.
    boolean map.containsValue(Object value); // Возвращает `true`, если карта содержит одно или несколько значений, которые равны указанному значению.
    int map.size(); // Возвращает количество отображений ключей в этой карте.
    boolean map.isEmpty(); // Возвращает `true`, если карта не содержит отображений ключей.
    void map.clear(); // Удаляет все отображения из этой карты.

    // Методы для получения представлений карты

    Set<K> map.keySet(); // Возвращает множество ключей, содержащихся в этой карте.
    Collection<V> map.values(); // Возвращает коллекцию значений, содержащихся в этой карте.
    Set<Map.Entry<K, V>> map.entrySet(); // Возвращает множество отображений ключ-значение, содержащихся в этой карте.
        // * Возвращает объект Map.Entry<A,B>, который имеет 2 метода: getKey() и getValue()
    
    // Дополнительные методы

    V map.putIfAbsent(K key, V value); //  (Java 8): Вставляет значение, связанное с указанным ключом, если ключ отсутствует в карте или связан с `null`.
    boolean map.remove(Object key, Object value); //  (Java 8): Удаляет отображение для ключа только в том случае, если оно связано с указанным значением.
    boolean map.replace(K key, V oldValue, V newValue); //  (Java 8):Заменяет значение, связанное с указанным ключом, только в том случае, если оно в настоящее время связано с указанным значением.
    V map.replace(K key, V value); //  (Java 8): Заменяет значение, связанное с указанным ключом, только в том случае, если ключ уже связан с некоторым значением.
    void map.forEach(BiConsumer<? super K, ? super V> action); //  (Java 8): Выполняет указанное действие для каждой пары ключ-значение в карте.
    void map.replaceAll(BiFunction<? super K, ? super V, ? extends V> function); //  (Java 8): Заменяет каждое значение в карте результатом применения указанной функции к его текущему значению.
```

### Реализации Map

Интерфейс `Map` в Java имеет несколько ключевых реализаций, каждая из которых обладает своими особенностями и подходит для различных сценариев использования. Основные реализации включают `HashMap`, `LinkedHashMap`, `TreeMap`, и многопоточные реализации из пакета `java.util.concurrent`, такие как `ConcurrentHashMap`. Рассмотрим каждую из них подробно.

### 1. HashMap

#### Описание HashMap

`HashMap` — это самая распространенная и широко используемая реализация интерфейса `Map`. Она основана на хэш-таблице.

#### Особенности HashMap

- **Производительность**: Быстрое добавление, удаление и поиск элементов (в среднем O(1)).
- **Порядок**: Не гарантирует порядок элементов.
- **Память**: Эффективное использование памяти, но в худшем случае (много коллизий) производительность может ухудшиться до O(n).

#### Методы HashMap

`HashMap` не добавляет новых методов к интерфейсу `Map`, но предоставляет эффективную реализацию всех стандартных методов.

#### Применение HashMap

- Подходит для случаев, когда требуется быстрая производительность и нет необходимости в упорядоченности элементов.
- Идеален для хранения уникальных ключей с быстрым доступом к значениям.

### 2. LinkedHashMap

#### Описание LinkedHashMap

`LinkedHashMap` расширяет `HashMap` и использует двусвязный список для поддержания порядка вставки элементов.

#### Особенности LinkedHashMap

- **Производительность**: Быстрое добавление, удаление и поиск элементов (в среднем O(1)), немного медленнее, чем `HashMap`, из-за поддержания порядка вставки.
- **Порядок**: Гарантирует сохранение порядка вставки элементов или порядка доступа (если включен режим доступа).
- **Память**: Использует больше памяти на элемент по сравнению с `HashMap` из-за хранения ссылок.

#### Методы LinkedHashMap

- **`LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder)`**: Конструктор позволяет задать начальную емкость, коэффициент загрузки и порядок (по вставке или по доступу).

#### Применение LinkedHashMap

- Подходит для случаев, когда требуется сохранение порядка вставки элементов.
- Идеален для приложений, где важен порядок элементов наряду с уникальностью.

### 3. TreeMap

#### Описание TreeMap

`TreeMap` реализует интерфейс `NavigableMap` и основан на красно-черном дереве.

#### Особенности TreeMap

- **Производительность**: Добавление, удаление и поиск элементов имеют логарифмическое время выполнения (O(log n)).
- **Порядок**: Гарантирует упорядоченность элементов по их естественному порядку или по предоставленному компаратору.
- **Память**: Использует больше памяти на элемент по сравнению с `HashMap` из-за хранения ссылок и дополнительных данных для поддержания структуры дерева.

#### Методы TreeMap

- **Методы `NavigableMap`**: Добавляет методы для работы с подмножествами, такие как `subMap`, `headMap`, `tailMap`, а также методы для получения первого и последнего элементов.

#### Применение TreeMap

- Подходит для случаев, когда требуется упорядоченность элементов и логарифмическое время выполнения операций.
- Идеален для задач, требующих диапазонных запросов и сортированных наборов данных.

### 4. ConcurrentHashMap

#### Описание ConcurrentHashMap

`ConcurrentHashMap` — это потокобезопасная реализация `Map`, основанная на сегментации.

#### Особенности ConcurrentHashMap

- **Производительность**: Высокая производительность в многопоточных средах благодаря сегментации, которая позволяет нескольким потокам одновременно модифицировать разные сегменты карты.
- **Порядок**: Не гарантирует порядок элементов.
- **Память**: Использует больше памяти на элемент по сравнению с `HashMap` из-за хранения сегментов.

#### Методы ConcurrentHashMap

- **Методы `ConcurrentMap`**: Добавляет методы для безопасного многопоточного доступа, такие как `putIfAbsent`, `remove` и `replace`.

#### Применение ConcurrentHashMap

- Подходит для многопоточных приложений, где требуется высокопроизводительный доступ к карте.
- Идеален для задач, требующих потокобезопасного доступа к данным.

## Optional

`Optional` в Java — это контейнер, который может содержать значение или быть пустым. Этот класс был введен в Java 8 как часть пакета java.util и предназначен для решения проблемы, связанной с использованием null, что часто приводит к ошибкам типа NullPointerException.

`Optional` позволяет разработчикам явно указывать, что значение может быть отсутствующим, что делает код более читаемым и безопасным. Вместо того чтобы проверять на null, мы используем методы Optional, которые делают работу с потенциально отсутствующими значениями более понятной и безопасной

### Основные методы Optional

```java
    import java.util.Optional;

    // Создание
    Optional<String> emptyOptional = Optional.empty(); // Пустой Optional
    Optional<String> optional = Optional.of("Hello"); // Optional с ненулевым значением
    Optional<String> optional = Optional.ofNullable(maybeNullValue); // Optional с потенциально нулевым значением

    // Методы
    boolean isPresent(); // Проверяет, содержит ли Optional значение
    void ifPresent(Consumer<? super T> action); // Выполняет действие, если значение присутствует
    T get(); // Возвращает значение, если оно присутствует. Бросает NoSuchElementException, если значение отсутствует
    T orElse(T other); // Возвращает значение, если оно присутствует, иначе возвращает указанное значение по умолчанию
    T orElseGet(Supplier<? extends T> other); // Возвращает значение, если оно присутствует, иначе вызывает и возвращает значение из переданного Supplier
    String value = optional.orElseGet(() -> "Default Value from Supplier");
    T orElseThrow(Supplier<? extends X> exceptionSupplier); // Возвращает значение, если оно присутствует, иначе бросает исключение, создаваемое переданным Supplier
    String value = optional.orElseThrow(() -> new RuntimeException("Value not present"));
    Optional<U> map(Function<? super T, ? extends U> mapper); // Применяет функцию к содержимому Optional, если значение присутствует, и возвращает Optional с результатом
    Optional<Integer> length = optional.map(String::length);
    Optional<U> flatMap(Function<? super T, Optional<U>> mapper); // Применяет функцию, которая возвращает Optional, и "разворачивает" его, если значение присутствует
    Optional<Integer> length = optional.flatMap(s -> Optional.of(s.length()));
    Optional<T> ilter(Predicate<? super T> predicate); // Возвращает Optional с содержимым, если оно удовлетворяет условию, иначе возвращает пустой Optional
    Optional<String> filtered = optional.filter(s -> s.length() > 3);
    Optional<T> or(Optional<? extends T> other); // Возвращает этот Optional, если значение присутствует, иначе возвращает указанный Optional
    Optional<String> result = optional.or(Optional.of("Default Value"));
    Stream<T> stream(); // Возвращает поток, состоящий из единственного значения, если оно присутствует, иначе возвращает пустой поток
```
