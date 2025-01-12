# Spring Kafka

```plantext
    Работа с Kafka через Spring осуществляется с использованием библиотеки Spring Kafka. Она предоставляет удобный API для интеграции с Kafka, включая продюсеров, потребителей, управление топиками и транзакциями.
```

- [Spring Kafka](#spring-kafka)
  - [Добавление зависимости Maven](#добавление-зависимости-maven)
  - [Конфигурация Kafka Producer](#конфигурация-kafka-producer)
    - [KafkaTemplate в Spring Kafka](#kafkatemplate-в-spring-kafka)
      - [Основные задачи KafkaTemplate](#основные-задачи-kafkatemplate)
      - [Использование KafkaTemplate](#использование-kafkatemplate)
  - [Конфигурация Kafka Consumer](#конфигурация-kafka-consumer)
    - [Создание Listener с @KafkaListener](#создание-listener-с-kafkalistener)
    - [Аннотации @Payload и @Header](#аннотации-payload-и-header)

## Добавление зависимости Maven

```xml
    <dependency>
        <groupId>org.springframework.kafka</groupId>
        <artifactId>spring-kafka</artifactId>
        <version>3.2.1</version> 
    </dependency>
```

## Конфигурация Kafka Producer

```java
    import org.apache.kafka.clients.producer.ProducerConfig;
    import org.apache.kafka.common.serialization.StringSerializer;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.kafka.core.DefaultKafkaProducerFactory;
    import org.springframework.kafka.core.ProducerFactory;

    import java.util.HashMap;
    import java.util.Map;

    @Configuration
    public class KafkaProducerConfig {

        @Bean
        public ProducerFactory<String, String> producerFactory() {
            Map<String, Object> configProps = new HashMap<>();
            configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092"); // Адрес брокера Kafka
            configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class); // Сериализатор ключа
            configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class); // Сериализатор значения
            configProps.put(ProducerConfig.ACKS_CONFIG, "all"); // Подтверждение записи
            configProps.put(ProducerConfig.RETRIES_CONFIG, 3); // Число попыток при сбое
            configProps.put(ProducerConfig.LINGER_MS_CONFIG, 10); // Задержка перед отправкой пакета
            return new DefaultKafkaProducerFactory<>(configProps);
        }

        @Bean
        public KafkaTemplate<String, String> kafkaTemplate() { // Этот бин отвечает за создание Kafka-продюсеров
            return new KafkaTemplate<>(producerFactory());
        }
    }
```

### KafkaTemplate в Spring Kafka

`KafkaTemplate в Spring Kafka` — это центральный компонент, предоставляющий API для отправки сообщений в Apache Kafka. Это высокоуровневая абстракция, которая упрощает взаимодействие с Kafka-продюсером, включая отправку данных, управление транзакциями и обратную связь при отправке.

#### Основные задачи KafkaTemplate

1. Отправка сообщений в Kafka: KafkaTemplate позволяет отправлять сообщения в определённый топик. Вы можете указать ключ (для маршрутизации по партициям) и значение (данные сообщения).
2. Управление ключами и партициями: При отправке сообщения можно указать:
   - Ключ, чтобы сообщение всегда попадало в определённую партицию.
   - Партицию, в которую сообщение должно быть отправлено.
3. Асинхронная отправка сообщений: Отправка сообщений реализована асинхронно, что позволяет достигать высокой производительности. Вы можете получить обратную связь через Future.
4. Транзакционность: KafkaTemplate поддерживает транзакции, позволяя отправлять несколько сообщений как одну атомарную операцию.
5. Интеграция с Spring: KafkaTemplate легко интегрируется с другими компонентами Spring, обеспечивая единообразный подход к работе с Kafka.

#### Использование KafkaTemplate

```java
    KafkaTemplate<String, String> kafkaTemplate;

    kafkaTemplate.send("my-topic", "message"); // Обычная отправка сообщения
    kafkaTemplate.send("my-topic", "my-key", "message"); // Отправка сообщения с ключом
    kafkaTemplate.send("my-topic", 0, "my-key", "message"); // Сообщение будет отправлено в указанную партицию (например, 0).

    // Формирование запросов при помощи MessageBuilder — это удобный класс из Spring Framework для создания сообщений (Message)
    import org.springframework.messaging.support.MessageBuilder; 

    String payload = MyToPayload(request); // Формирование полезной нагрузки (payload)
    // Метод MyToPayload(request) преобразует входящий объект request в строку (String). Обычно это сериализация объекта в JSON или другой текстовый формат

    MessageBuilder<String> messageBuilder = MessageBuilder.withPayload(payload); // Полезная нагрузка (payload) — это основной контент сообщения, который будет отправлен в Kafka

    messageBuilder
        .setHeader(KafkaHeaders.TOPIC, "my-topic"); // Заголовки используются для передачи метаданных, которые Kafka или потребители сообщений могут использовать для маршрутизации или обработки

    Message<String> message = messageBuilder.build(); // Сборка сообщения
    ListenableFuture<SendResult<String, String>> future = kafkaTemplateCpcV4.send(message); // Отправляет сообщение в Kafka. Возвращается ListenableFuture, который позволяет отслеживать успешную отправку или обработку ошибок

    future.addCallback(result -> {
        System.out.println("Успешно отправлено: " + result.getRecordMetadata());
    }, ex -> {
        System.err.println("Ошибка отправки: " + ex.getMessage());
    }); // Позволяет указать обработчики успешного завершения операции (onSuccess) или ошибки (onFailure). Не блокирует текущий поток

    future.addCallback(MyLoggingResult(payload)); // данный код будет вызван в любом случае;

    future.get(); // Если вам нужно синхронно дождаться завершения отправки сообщения и убедиться, что оно доставлено успешно, вы можете вызвать
    //  * Если при отправке сообщения возникла ошибка, future.get() выбросит соответствующее исключение. Это позволяет вам обработать проблему (например, логировать ошибку, повторить отправку)

    // Как addCallback и get взаимодействуют?
    // addCallback и get работают независимо. Вы можете использовать только один из них в зависимости от ваших потребностей (асинхронность или синхронное ожидание результата).
```

## Конфигурация Kafka Consumer

```java
    import org.apache.kafka.clients.consumer.ConsumerConfig;
    import org.apache.kafka.common.serialization.StringDeserializer;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.kafka.core.ConsumerFactory;
    import org.springframework.kafka.core.DefaultKafkaConsumerFactory;

    import java.util.HashMap;
    import java.util.Map;

    @Configuration
    public class KafkaConsumerConfig {

        @Bean // ConsumerFactory отвечает за создание Kafka-потребителей. Настраиваем десериализацию ключей и значений, а также параметры подключения
        public ConsumerFactory<String, String> consumerFactory() {
            Map<String, Object> props = new HashMap<>();
            props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092"); // Адрес брокера Kafka
            props.put(ConsumerConfig.GROUP_ID_CONFIG, "my-group-id"); // ID группы потребителей
            props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class); // Десериализатор ключа
            props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class); // Десериализатор значения
            props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest"); // Сброс смещения
            props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false); // Ручное управление смещениями
            return new DefaultKafkaConsumerFactory<>(props);
        }

        @Bean // ListenerContainerFactory создаёт контейнеры для обработки сообщений. Его настройка позволяет задать свойства, такие как количество потоков и режим подтверждения сообщений
        public ConcurrentKafkaListenerContainerFactory<String, String> kafkaListenerContainerFactory(
                ConsumerFactory<String, String> consumerFactory) {
            ConcurrentKafkaListenerContainerFactory<String, String> factory = new ConcurrentKafkaListenerContainerFactory<>();
            factory.setConsumerFactory(consumerFactory);
            factory.setConcurrency(3); // Количество потоков для обработки сообщений
            factory.getContainerProperties().setAckMode(ContainerProperties.AckMode.MANUAL); // Ручное подтверждение
            return factory;
        }
    }
```

### Создание Listener с @KafkaListener

```java
    import org.apache.kafka.clients.consumer.ConsumerRecord;
    import org.springframework.kafka.annotation.KafkaListener;
    import org.springframework.stereotype.Service;

    @Service
    public class KafkaMessageListener {

        @KafkaListener(
                topics = "my-topic", // Название топика
                groupId = "my-group-id", // ID группы. groupId в Kafka отвечает за идентификацию группы потребителей (consumer group). Каждая группа обробатывает свои партиции. Если один из потребителей выйдет из строя, оставшиеся перераспределят партиции
                containerFactory = "kafkaListenerContainerFactory" // Название ListenerContainerFactory
        )
        public void listenConsumerRecord(ConsumerRecord<String, String> record) {
            System.out.println("Получено сообщение: " + record.value());
            System.out.println("Ключ: " + record.key());
            System.out.println("Топик: " + record.topic());
            System.out.println("Партиция: " + record.partition());
        }
    }
```

### Аннотации @Payload и @Header

Аннотации `@Payload` и `@Header` в Spring Kafka используются для извлечения данных из сообщений, которые прослушиваются с помощью @KafkaListener. Эти аннотации позволяют получить полезную нагрузку (payload) и заголовки (headers) сообщений в методах-слушателях.

Аннотация `@Payload` используется для извлечения полезной нагрузки сообщения (основного содержимого сообщения, отправленного в Kafka)

Аннотация `@Header` используется для извлечения заголовков сообщений Kafka. Заголовки могут содержать метаданные, такие как ключи, временные метки, идентификаторы запросов и другую информацию

```java
    @KafkaListener(topics = "my-topic", groupId = "my-group-id") // containerFactory - Если не указать, то Spring будет искать бин по умолчанию
    public void listen(
        @Payload String message,
        @Header(KafkaHeaders.RECEIVED_TOPIC) String topic,
        @Header(KafkaHeaders.RECEIVED_PARTITION_ID) int partition,
        @Header(KafkaHeaders.OFFSET) long offset
    ) {
        ObjectMapper mapper = new ObjectMapper();
        // Настройка mapper-a
        MyClasss response = JsonUtils.readSafely(mapper, message.getBytes(StandardCharsets.UTF_8), MyClasss.class);
        System.out.println("Сообщение: " + response);
        System.out.println("Топик: " + topic);
        System.out.println("Партиция: " + partition);
        System.out.println("Смещение: " + offset);
    }
```
