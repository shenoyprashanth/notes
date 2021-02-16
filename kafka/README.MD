# Getting started with Kafka on Windows

### Start Zookeeper

```console
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```

### Start Kafka

```console
bin\windows\kafka-server-start.bat config\server.properties
```

### Create topic

```console
bin\windows\kafka-topics.bat --create --topic mytopic --bootstrap-server localhost:9092
```

### Produce messages

```console
 bin\windows\kafka-console-producer.bat --topic mytopic --bootstrap-server localhost:9092
 ```

 ### Consume messages
 
 ```console
 bin\windows\kafka-console-consumer.bat --topic mytopic --from-beginning --bootstrap-server localhost:9092 --consumer-property group.id=group1
 ```
 
```console
 bin\windows\kafka-console-consumer.bat --topic mytopic --from-beginning --bootstrap-server localhost:9092 --consumer-property group.id=group2
 ```

 ### DumpLogSegments

 ```console
 bin\windows\kafka-run-class.bat kafka.tools.DumpLogSegments --deep-iteration --print-data-log --files D:\data\kafka\mytopic-0\00000000000000000000.log
```

 ### Producer

 -  **acks** parameter indicates whether the producer waits for an acknowledgment from the broker, only from the leader or does not care.
 
 -  **linger** parameter tells the producer to wait for the specified time, in anticipation of the more messages. 
 
 - Messages are batched. And this is governed by a batch size. 
 
 - **retry** configures number of retries in case the producer fails. 
