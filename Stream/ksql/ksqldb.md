- Executar query e receber alterações via REST

```shell
curl -X "POST" "http://localhost:8088/query" \
     -H "Content-Type: application/vnd.ksql.v1+json; charset=utf-8" \
     -d $'{
  "ksql": "SELECT * FROM possible_anomalies EMIT CHANGES;",
  "streamsProperties": {}
}'
```

```shell
curl -X "POST" "http://localhost:8088/query" \
     -H "Content-Type: application/vnd.ksql.v1+json; charset=utf-8" \
     -d $'{
  "ksql": "show streams;",
  "streamsProperties": {}
}'
```

# KSQL

- Começar do primeiro valor

```sql
SET 'auto.offset.reset' = 'earliest';
```

- Selecionar dados e emitir alterações

```sql
SELECT * FROM possible_anomalies EMIT CHANGES;
```

- Inserir valor

```sql
INSERT INTO transactions (
    email_address, card_number, rowkey, timestamp, amount
) VALUES (
    'colin@example.com',
    '373913272311617',
    'de9831c0-7cf1-4ebf-881d-0415edec0d6b',
    '2020-04-22T09:44:03',
    150.00
);
```

- Criar stream

```sql
CREATE STREAM transactions (
    tx_id VARCHAR KEY,
    email_address VARCHAR,
    card_number VARCHAR,
    timestamp VARCHAR,
    amount DECIMAL(12, 2)
) WITH (
    kafka_topic = 'transactions',
    partitions = 8,
    value_format = 'avro',
    timestamp = 'timestamp',
    timestamp_format = 'yyyy-MM-dd''T''HH:mm:ss'
);
```

- Criar tabela

```sql
CREATE TABLE possible_anomalies WITH (
    kafka_topic = 'possible_anomalies',
    VALUE_AVRO_SCHEMA_FULL_NAME = 'io.ksqldb.tutorial.PossibleAnomaly'
)   AS
    SELECT card_number AS `card_number_key`,
           as_value(card_number) AS `card_number`,
           latest_by_offset(email_address) AS `email_address`,
           count(*) AS `n_attempts`,
           sum(amount) AS `total_amount`,
           collect_list(tx_id) AS `tx_ids`,
           WINDOWSTART as `start_boundary`,
           WINDOWEND as `end_boundary`
    FROM transactions
    WINDOW TUMBLING (SIZE 30 SECONDS, RETENTION 1000 DAYS)
    GROUP BY card_number
    HAVING count(*) >= 3
    EMIT CHANGES;
```

> - For each credit card number, 30 second tumbling windows are created to group activity. A new row is inserted into the table when at least 3 transactions take place inside a given window. The window retains data for the last 1000 days based on each row's timestamp. In general, you should choose your retention carefully.
> - It is a trade-off between storing data longer and having larger state sizes. The very long retention period used in this tutorial is useful because the timestamps are fixed at the time of writing this and won't need to be adjusted often to account for retention.
> - The credit card number is selected twice. In the first instance, it becomes part of the underlying Kafka record key, because it's present in the group by clause, which is used for sharding. In the second instance, the as_value function is used to make it available in the value, too. This is generally for convenience.
> - The individual transaction IDs and amounts that make up the window are collected as lists.
> - The last transaction's email address is "carried forward" with latest_by_offset.
> - Column aliases are surrounded by backticks, which tells ksqlDB to use exactly that case. ksqlDB uppercases identity names by default.
> - The underlying Kafka topic for this table is explicitly set to possible_anomalies.
> - The Avro schema that ksqlDB generates for the value portion of its records is recorded under the namespace io.ksqldb.tutorial.PossibleAnomaly. You'll use this later in the microservice.
