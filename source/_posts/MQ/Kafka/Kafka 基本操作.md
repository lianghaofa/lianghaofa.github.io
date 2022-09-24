---
title: Kafka 基本操作
date: 2022-05-19 11:40:52
summary: Kafka 基本操作
categories: MQ
tags:
- Kafka
---
## Kafka 基本操作


 

### 基本操作

```java
    public static void main(String[] args) throws Exception {
        // 1.创建 KafkaAdminClient

        Properties properties = new Properties();
        properties.put(AdminClientConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");
        KafkaAdminClient adminClient = (KafkaAdminClient) KafkaAdminClient.create(properties);

        // 创建 Topic 信息

        //  name 分区名称 numPartitions 分区数 replicationFactor 分区因子
        NewTopic topic = new NewTopic("topic01",3, (short) 3);
        List<NewTopic> list = new ArrayList<NewTopic>();
        list.add(topic);
        //  默认 异步创建
        CreateTopicsResult createTopicsResult = adminClient.createTopics(list);

        // 修改成同步创建
        createTopicsResult.all().get();

        // 查看 Topic
        ListTopicsResult topicsResult = adminClient.listTopics();
        Set<String> names = topicsResult.names().get();

        for (String name : names){
            System.out.println(name);
        }

        // 删除 Topic  默认异步删除
        DeleteTopicsResult deleteTopicsResult = adminClient.deleteTopics(Arrays.asList("topic01","topic02"));

        // 修改为同步删除
        deleteTopicsResult.all().get();

        // 查看 Topic 详细信息

        DescribeTopicsResult describeTopicsResult = adminClient.describeTopics(Arrays.asList("topic01"));
        Map<String, TopicDescription> topicDescriptionMap = describeTopicsResult.all().get();

        for (Map.Entry<String, TopicDescription> entry : topicDescriptionMap.entrySet()){
            System.out.println(entry.getKey() + " " + entry.getValue());
        }

        // 关闭 adminClient
        adminClient.close();
    }   
```



### 生产者


```java
    public static void main(String[] args) throws Exception {
        // 1. 创建 KafkaProducer
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        KafkaProducer<String, String> kafkaProducer = new KafkaProducer<String, String>(properties);

        // top01 生产信息
        for (int i = 0; i < 100; i++) {
            ProducerRecord<String, String> top = new ProducerRecord<String, String>("top01", "key" + i, "value" + i);
            // 发送消息给服务器
            kafkaProducer.send(top);
        }

        // 关闭生产者
        kafkaProducer.close();
    }
```

### 消费者

#### sub 订阅方式
```java
    public static void main(String[] args)  {
        // 1. 创建 KafkaConsumer
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");

        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.GROUP_ID_CONFIG,"g1");
        KafkaConsumer<String, String> kafkaConsumer = new KafkaConsumer<String, String>(properties);

        // 订阅相关的 Topics
        kafkaConsumer.subscribe(Pattern.compile("^topic.*"));

        // 遍历消息队列
        while (true){
            ConsumerRecords<String, String> consumerRecords = kafkaConsumer.poll(Duration.ofSeconds(1));
            // 从队列中取数据
            if (!consumerRecords.isEmpty()){
                Iterator<ConsumerRecord<String, String>> record = consumerRecords.iterator();
                while (record.hasNext()){
                    // 获取一个消费者
                    ConsumerRecord<String, String> record1 = record.next();
                    String topic = record1.topic();
                    int partition = record1.partition();
                    long offset = record1.offset();
                    String key = record1.key();
                    String value = record1.value();
                    long timestamp = record1.timestamp();
                    System.out.println(topic + " " + partition + " " + offset + " " + key + " " + value + " " + timestamp);
                }

            }
        }
    }
```

#### assign 订阅方式

```java
    public static void main(String[] args)  {
        // 1. 创建 KafkaConsumer
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");

        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());

        KafkaConsumer<String, String> kafkaConsumer = new KafkaConsumer<String, String>(properties);

        // 订阅相关的 Topics,手动指定消费分区，失去组管理特性
        List<TopicPartition> partitions = Arrays.asList(new TopicPartition("topic01", 0));
        // 从指定消费分区的开始位置读取
        kafkaConsumer.seekToBeginning(partitions);

        // 指定分区的 指定位置
        kafkaConsumer.seek(new TopicPartition("topic01", 0), 1);


        // 遍历消息队列
        while (true){
            ConsumerRecords<String, String> consumerRecords = kafkaConsumer.poll(Duration.ofSeconds(1));
            // 从队列中取数据
            if (!consumerRecords.isEmpty()){
                Iterator<ConsumerRecord<String, String>> record = consumerRecords.iterator();
                while (record.hasNext()){
                    // 获取一个消费者
                    ConsumerRecord<String, String> record1 = record.next();
                    String topic = record1.topic();
                    int partition = record1.partition();
                    long offset = record1.offset();
                    String key = record1.key();
                    String value = record1.value();
                    long timestamp = record1.timestamp();
                    System.out.println(topic + " " + partition + " " + offset + " " + key + " " + value + " " + timestamp);
                }

            }
        }
    }
```


### 分区

- 指定分区
```java
ProducerRecord<String, String> top = new ProducerRecord<String, String>("top01", 1,"key" + i, "value" + i);
```
- 重新定义分区规则
```java
properties.put(ProducerConfig.PARTITIONER_CLASS_CONFIG, MyPartition.class.getName());
```
```java
    public static void main(String[] args) throws Exception {
        // 1. 创建 KafkaProducer
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0: 9000,CentOS1:9001,CentOS2:9002");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // 可以重新 指定  实现了 Partition 接口的类
        // properties.put(ProducerConfig.PARTITIONER_CLASS_CONFIG, MyPartition.class.getName());



        KafkaProducer<String, String> kafkaProducer = new KafkaProducer<String, String>(properties);

        // top01 生产信息
        for (int i = 0; i < 100; i++) {

            // 存在 key ，指定分区 = "key" % 分区数，  不存在key : count % 分区数
            // 可以重新 指定  实现了 Partition 接口的类 properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
            ProducerRecord<String, String> top = new ProducerRecord<String, String>("top01", "key" + i, "value" + i);

            // 指定分区
            // ProducerRecord<String, String> top = new ProducerRecord<String, String>("top01", 1,"key" + i, "value" + i);

            // 发送消息给服务器
            kafkaProducer.send(top);
        }

        // 关闭生产者
        kafkaProducer.close();
    }
```

### 序列化 - 传输对象


对象序列化工具

```xml
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.9</version>
        </dependency>
```

```xml
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.9</version>
        </dependency>
```


MySerializer

```java

import org.apache.commons.lang3.SerializationUtils;
import org.apache.kafka.common.serialization.Serializer;

import java.io.Serializable;
import java.util.Map;

public class MySerializer implements Serializer<Object> {

    @Override
    public void configure(Map map, boolean b) {

    }

    @Override
    public byte[] serialize(String s, Object o) {

        return SerializationUtils.serialize((Serializable) o);
    }

    @Override
    public void close() {

    }
}
```

MyDeSerializer

```java

import org.apache.commons.lang3.SerializationUtils;
import org.apache.kafka.common.serialization.Deserializer;
import java.util.Map;

public class MyDeSerializer implements Deserializer<Object> {


    @Override
    public void configure(Map<String, ?> map, boolean b) {

    }

    @Override
    public Object deserialize(String s, byte[] bytes) {
        return SerializationUtils.deserialize(bytes);
    }

    @Override
    public void close() {

    }
}
```

User

```java

import java.io.Serializable;

public class User implements Serializable {

    private String name;
    private int age;

    User(String name, int age){
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```
生产者

```java
    public static void main(String[] args) throws Exception {
        // 1. 创建 KafkaProducer
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0: 9000,CentOS1:9001,CentOS2:9002");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // 传输对象
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, MySerializer.class.getName());


        KafkaProducer<String, User> kafkaProducer = new KafkaProducer<String, User>(properties);

        // top01 生产信息
        for (int i = 0; i < 100; i++) {

            ProducerRecord<String, User> top = new ProducerRecord<String, User>("top01", "key" + i, new User("name" +i, i));

            // 发送消息给服务器
            kafkaProducer.send(top);
        }

        // 关闭生产者
        kafkaProducer.close();
    }
```

消费者

```java
    public static void main(String[] args)  {
        // 1. 创建 KafkaConsumer
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");

        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, MyDeSerializer.class.getName());
        properties.put(ConsumerConfig.GROUP_ID_CONFIG,"g1");
        KafkaConsumer<String, User> kafkaConsumer = new KafkaConsumer<String, User>(properties);

        // 订阅相关的 Topics
        kafkaConsumer.subscribe(Arrays.asList("top01"));

        // 遍历消息队列
        while (true){
            ConsumerRecords<String, User> consumerRecords = kafkaConsumer.poll(Duration.ofSeconds(1));
            // 从队列中取数据
            if (!consumerRecords.isEmpty()){
                Iterator<ConsumerRecord<String, User>> record = consumerRecords.iterator();
                while (record.hasNext()){
                    // 获取一个消费者
                    ConsumerRecord<String, User> record1 = record.next();
                    String topic = record1.topic();
                    int partition = record1.partition();
                    long offset = record1.offset();
                    String key = record1.key();
                    User user = record1.value();
                    long timestamp = record1.timestamp();
                    System.out.println(topic + " " + partition + " " + offset + " " + key + " " + user + " " + timestamp);
                }

            }
        }
    }
```

### 拦截器

自定义拦截器

```java
import org.apache.kafka.clients.producer.ProducerInterceptor;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.clients.producer.RecordMetadata;
import java.util.Map;

public class MyProducerInterceptor implements ProducerInterceptor {

    @Override
    public ProducerRecord onSend(ProducerRecord producerRecord) {

        // 生产者发送的信息， 可以在此处对消息进行处理

        producerRecord.headers();
        producerRecord.partition();
        producerRecord.value(); // .....

        return null;
    }

    @Override
    public void onAcknowledgement(RecordMetadata recordMetadata, Exception e) {

        // 发送成功后的回调

    }

    @Override
    public void close() {

    }

    @Override
    public void configure(Map<String, ?> map) {

    }
}
```


```java
    public static void main(String[] args) throws Exception {
        // 1. 创建 KafkaProducer
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // 配置拦截器
        properties.put(ProducerConfig.INTERCEPTOR_CLASSES_CONFIG, MyProducerInterceptor.class.getName());
        

        KafkaProducer<String, String> kafkaProducer = new KafkaProducer<String, String>(properties);

        // top01 生产信息
        for (int i = 0; i < 100; i++) {
            ProducerRecord<String, String> top = new ProducerRecord<String, String>("top01", "key" + i, "value" + i);
            // 发送消息给服务器
            kafkaProducer.send(top);
        }

        // 关闭生产者
        kafkaProducer.close();
    }
```

### offset

Kafka 保存消费者的消费记录

新来的消费者该从哪里开始消费。

- auto.offset.reset = latest    消费者订阅时的位置
- auto.offset.reset = earliest  从分区的最早位置
- auto.offset.reset = none      没有该消费者的记录，抛异常。

默认配置 latest

Kafka 消费者默认定期提交偏移量

- enable.auto.commit = true
- auto.commit.interval.ms = 5000

如果改为手动提交，提交的 offset 为当前 offset + 1

```java
    public static void main(String[] args)  {
        // 1. 创建 KafkaConsumer
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");

        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.GROUP_ID_CONFIG,"g1");

        properties.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        properties.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);

        KafkaConsumer<String, String> kafkaConsumer = new KafkaConsumer<String, String>(properties);

        // 订阅相关的 Topics
        kafkaConsumer.subscribe(Arrays.asList("topic01"));

        // 遍历消息队列
        while (true){
            ConsumerRecords<String, String> consumerRecords = kafkaConsumer.poll(Duration.ofSeconds(1));
            // 从队列中取数据
            if (!consumerRecords.isEmpty()){
                Iterator<ConsumerRecord<String, String>> record = consumerRecords.iterator();

                HashMap<TopicPartition, OffsetAndMetadata> offsetsMap = new HashMap<>();
                while (record.hasNext()){
                    // 获取一个消费者
                    ConsumerRecord<String, String> record1 = record.next();
                    String topic = record1.topic();
                    int partition = record1.partition();
                    long offset = record1.offset();
                    String key = record1.key();
                    String value = record1.value();
                    long timestamp = record1.timestamp();
                    // 记录消费者分区的偏移量,  当前偏移量 + 1
                    offsetsMap.put(new TopicPartition(topic, partition), new OffsetAndMetadata(offset + 1));

                    kafkaConsumer.commitAsync(offsetsMap, new OffsetCommitCallback() {
                        @Override
                        public void onComplete(Map<TopicPartition, OffsetAndMetadata> map, Exception e) {
                            // 回调信息
                        }
                    });
                    System.out.println(topic + " " + partition + " " + offset + " " + key + " " + value + " " + timestamp);
                }

            }
        }
    }
```


### Acks & Retries

Acks

Kafka生产者在发送完一个消息之后，要求Broker在规定的时间内Ack应答，如果没有在规定时间内应答，生产者会尝试n次重发。

- acks = 1 Leader会将Record写到本地日志中，不会等待Follower的完全确认的情况做出响应。 记录有可能丢失，当本地日志是缓存，没有刷新到磁盘。
  
- acks = 0 生产者不会等待服务器的任何确认。该记录添加到套接字缓冲区就视为已发送。不能保证服务器收到记录。

- acks = all Leader将等待全部Follower的确认，只要有一个Follower能保存记录，记录就不会丢失。

Retries

如果生产者在规定的时间内，没有得到Kafka Leader的Ack应答，Kafka可以开启 Retries 机制

request.timeout.ms = 30000

retries = 2147483647


重发，Kafka 如何检测是否重复数据 ？ 幂等性   



### 幂等性

HTTP/1.1 中幂等性的定义：一次和多次请求某一资源对于资源本身应该具有同样的结果(网络超时等问题除外)。- 任意多次执行与一次执行的影响相同。


对于Kafka生产者而言，保证生产者发送的消息，不会丢失。而且不会重复。实现幂等的关键 - 如何区分请求是否重复

- 唯一标识

- 唯一标识的请求是否已被处理


生产者唯一标识 PID

生产者消息序列号 SeqID

![Kafka - 幂等性](/medias/MQ/1652932655.png)

enable.idempotence = false , 默认关闭

使用幂等性，必须要求开启 retries=true, acks=all


### 生产者事务 

生产向某个 topic 的 3个分区同时写消息，必须要3条都写成功。有一个失败，必须要回滚。问题是分区0，分区2已经写进了文件。此时可以通过消费者进行隔离，确认是否要丢弃。

- isolation.level = read_uncommitted 默认， 不开启事务

- isolation.level = read_committed 开启事务
 

开启事务后，需要指定 transactional.id 

**需要注意的是：生产者指定了 read_uncommitted 与 read_committed，消费者仍然读取 read_uncommitted 与 read_committed 的数据**
例如，生产者在开启事务的条件下，提交了一部分消息，被中断了。这部分消息默认为read_uncommitted，消费者在read_uncommitted条件下能读取，在read_committed条件下不能读取。


生产者

```java
    public static void main(String[] args) {
        KafkaProducer<String, String> kafkaProducer = buildKafkaProducer();
        // 初始化事务
        kafkaProducer.initTransactions();

        try {
            kafkaProducer.beginTransaction();
            for (int i = 0; i < 20; i++) {
                ProducerRecord<String, String> record = new ProducerRecord<>("topic01", 1, "key" + i, "value" + i);
                kafkaProducer.send(record);
            }
            kafkaProducer.flush();
            // 提交事务
            kafkaProducer.commitTransaction();
        }catch (Exception e){
            System.out.println(e);
            // 结束事务
            kafkaProducer.abortTransaction();
        }finally {
            kafkaProducer.close();
        }

    }


    public static KafkaProducer<String, String> buildKafkaProducer(){
        // 1. 创建 KafkaProducer
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // 必须配置 事务ID。必须唯一
        properties.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, UUID.randomUUID());
        // 配置 Kafka 批处理大小
        properties.put(ProducerConfig.BATCH_SIZE_CONFIG, 1024);
        // 配置等待时间， 如果 BATCH 中的数据不足 1024 大小，等待 5 ms
        properties.put(ProducerConfig.LINGER_MS_CONFIG, 5);
        // 配置 Kafka 重试机制和幂等性
        properties.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
        properties.put(ProducerConfig.ACKS_CONFIG,"all");
        properties.put(ProducerConfig.REQUEST_TIMEOUT_MS_CONFIG, 10000);
        
        return new KafkaProducer<String, String>(properties);
    }

```

消费者

```java
    public static void main(String[] args) {
        // 1. 创建 KafkaConsumer
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");

        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, MyDeSerializer.class.getName());
        properties.put(ConsumerConfig.GROUP_ID_CONFIG,"g1");

        // 设置消费者的消费事务的隔离级别  read_committed
        properties.put(ConsumerConfig.ISOLATION_LEVEL_CONFIG,"read_committed");

        KafkaConsumer<String, User> kafkaConsumer = new KafkaConsumer<String, User>(properties);

        // 订阅相关的 Topics
        kafkaConsumer.subscribe(Arrays.asList("top01"));

        // 遍历消息队列
        while (true){
            ConsumerRecords<String, User> consumerRecords = kafkaConsumer.poll(Duration.ofSeconds(1));
            // 从队列中取数据
            if (!consumerRecords.isEmpty()){
                Iterator<ConsumerRecord<String, User>> record = consumerRecords.iterator();
                while (record.hasNext()){
                    // 获取一个消费者
                    ConsumerRecord<String, User> record1 = record.next();
                    String topic = record1.topic();
                    int partition = record1.partition();
                    long offset = record1.offset();
                    String key = record1.key();
                    User user = record1.value();
                    long timestamp = record1.timestamp();
                    System.out.println(topic + " " + partition + " " + offset + " " + key + " " + user + " " + timestamp);
                }

            }
        }
    }

```
### 生产者 & 消费者 事务

```java
    public static void main(String[] args) {
        KafkaProducer<String, String> kafkaProducer = buildKafkaProducer();
        KafkaConsumer<String, String> kafkaConsumer = buildKafkaConsumer("g1");
        // 初始化事务
        kafkaProducer.initTransactions();

        // 消费者订阅事务
        kafkaConsumer.subscribe(Arrays.asList("topic01"));
        while (true){
            ConsumerRecords<String, String> consumerRecords = kafkaConsumer.poll(Duration.ofSeconds(1));
            // 从队列中取数据
            if (!consumerRecords.isEmpty()){
                Map<TopicPartition, OffsetAndMetadata> offsetsMap = new HashMap<>();
                Iterator<ConsumerRecord<String, String>> records = consumerRecords.iterator();
                // 开启事务
                kafkaProducer.beginTransaction();
                try {
                    // 获取数据
                    while (records.hasNext()){
                        // 获取一个消费
                        ConsumerRecord<String, String> record = records.next();
                        String topic = record.topic();
                        int partition = record.partition();
                        long offset = record.offset();
                        // 记录消费者分区的偏移量,  当前偏移量 + 1
                        offsetsMap.put(new TopicPartition(topic, partition), new OffsetAndMetadata(offset + 1));
                        ProducerRecord<String, String> producerRecord = new ProducerRecord<>("topic02", record.key(), record.value() + "topic01 done");
                        kafkaProducer.send(producerRecord);
                    }
                    // 提交事务
                    // 提交消费者的偏移量
                    kafkaProducer.sendOffsetsToTransaction(offsetsMap, "g1");
                    kafkaProducer.commitTransaction();
                }catch (Exception e){
                    e.printStackTrace();
                    // 异常，终止事务
                    kafkaProducer.abortTransaction();
                }
            }
        }
    }


    public static KafkaProducer<String, String> buildKafkaProducer(){
        // 1. 创建 KafkaProducer
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // 必须配置 事务ID。必须唯一
        properties.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, UUID.randomUUID());
        // 配置 Kafka 批处理大小
        properties.put(ProducerConfig.BATCH_SIZE_CONFIG, 1024);
        // 配置等待时间， 如果 BATCH 中的数据不足 1024 大小，等待 5 ms
        properties.put(ProducerConfig.LINGER_MS_CONFIG, 5);
        // 配置 Kafka 重试机制和幂等性
        properties.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
        properties.put(ProducerConfig.ACKS_CONFIG,"all");
        properties.put(ProducerConfig.REQUEST_TIMEOUT_MS_CONFIG, 10000);

        return new KafkaProducer<String, String>(properties);
    }

    public static KafkaConsumer<String, String> buildKafkaConsumer(String groupId){

        // 1. 创建 KafkaConsumer
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,"CentOS0:9000,CentOS1:9001,CentOS2:9002");

        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);

        // 设置消费者的消费事务的隔离级别  read_committed
        properties.put(ConsumerConfig.ISOLATION_LEVEL_CONFIG,"read_committed");

        // 必须关闭消费者端的 offset自动提交
        properties.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);

        return new KafkaConsumer<String, String>(properties);
    }

```



