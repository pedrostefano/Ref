# Kafka Commands

- Start the ZooKeeper service

Note: Soon, ZooKeeper will no longer be required by Apache Kafka.

```
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

- Start the Kafka broker service

```
$ bin/kafka-server-start.sh config/server.properties
```

- Create topic

```
$ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

- Describe topic

```
$ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```

- Run producer

```
$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

- Run consumer

```
$ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```
