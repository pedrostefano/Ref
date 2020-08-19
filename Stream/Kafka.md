# Kafka Commands

- Start the ZooKeeper service

Note: Soon, ZooKeeper will no longer be required by Apache Kafka.

```shell
bin/zookeeper-server-start.sh config/zookeeper.properties
```

- Start the Kafka broker service

```shell
bin/kafka-server-start.sh config/server.properties
```

- Create topic

```shell
bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

- Describe topic

```shell
bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```

- Run producer

```shell
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

- Run consumer

```shell
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```

- Run connector

```shell
bin/connect-standalone.sh config/connect-standalone.properties connectors/testConnector.properties
```
