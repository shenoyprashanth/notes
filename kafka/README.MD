## Kafka Broker

- At its heart Kafka is an immutable log (append only).
- Producers and consumers publish/consume from **Topics**.
- A Topic is a logical entity such as "Sports", "Science" etc.
- Physically a Topic is a bunch of **Partitions**.
- Partitions are just **directories** on a machine.
- The goal of breaking up a topic into partition is to increase throughput.
- Also partitions are **replicated** that makes the system fault tolerant.
- Each node in the cluster acts as a **leader** for one or more partitions.
- Zookeeper maintains a list of the brokers, partition ownership etc.
- Each partition has a bunch of files. 
- The files containing the messages are called **Segments**. 
- To efficiently locate a certain message given an offset, Kafka also maintains **index** files.

## Kafka Producer
- Producers connect to the broker and publish messages to a Topic.
- Messages get an offset (montonically increasing). The offset makes sense only in the context of a partition. So in essence to read a message one needs both offset & partition.
- A publish from a producer just appends a message into a client buffer.
- After a certain time period/buffer memory consumption, the messages are batched together and sent to the broker.
- The behavior of producer can be controlled through elaborate configuration, such as no acknowledgment,  leader ack, all ack, number of retries etc.
- A partition key can be used to target a message to a specific partition.

## Kafka Consumer
- Polls the broker continously. Sends heart beat everytime it polls.
- Is part of a consumer group. CG is kafka's way of balancing pub/sub vs distribution of messages to individual processes.
- Each consumer process reads from one or more partitions.
- Dead/New consumers result in partition rebalancing.
- Consumers can also listen to specific partitions through assign(). 
- In the above case, not automatic rebalancing happens.
- Offset always represent the next record to be fetched.
- Consumers can commit the offset back into Kafka or an external system.(RDBMS)
- Consumers can read from specific partitions. Imagine a resource attached to a node and the data has to be processed on that node.

##  Kafka on Windows

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



