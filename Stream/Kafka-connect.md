
# Kafka Connect

## REST API Examples

[Confluent - API Docs](https://docs.confluent.io/current/connect/references/restapi.html)

- Listar conectores

`curl http://localhost:8083/connectors/`

- Listar Plugins de Conectores

`curl http://localhost:8083/connector-plugins/`

- Restart Connector

`curl -X POST http://localhost:8083/connectors/c-xomni-historic/restart`

- Pause Connector

`curl -X PUT http://localhost:8083/connectors/c-xomni-historic/pause`

- Resume Connector

`curl -X PUT http://localhost:8083/connectors/c-xomni-historic/resume`

- Delete Connector

`curl -X DELETE http://localhost:8083/connectors/c-xomni-historic`

- Create Connector

```shell
curl -X POST -H "Content-Type: application/json" --data '{"name": "local-file-sink", "config": {"connector.class":"FileStreamSinkConnector", "tasks.max":"1", "file":"test.sink.txt", "topics":"connect-test" }}' http://localhost:8083/connectors
```

## Connectors Examples

- Source Connector

```sql
CREATE SOURCE CONNECTOR `xomni-get-commands` WITH(
 "connector.class"='io.confluent.connect.jdbc.JdbcSourceConnector',
 "connection.url"='jdbc:oracle:thin:@//192.168.2.92:1521/xe',
 "db.timezone"='America/Sao_Paulo',
 "connection.user"='xomniapp',
 "connection.password"='SQL',
 "mode"='bulk',
 "poll.interval.ms"='600000',
 "batch.max.rows"='1000',
 "numeric.mapping"='best_fit',
 "topic.prefix"='xomni-get-commands',
 "query"='SELECT e.OBJID, e.ETIQUETA , ce.TEXTO_COMANDO FROM XOMNI.EQUIPAMENTO e JOIN XOMNI.COMANDOS_EQP ce ON e.OBJID = ce.EQUIP_ID WHERE e.OBJID IN (23081, 23144, 23212) ORDER BY e.OBJID, ce.ORDEM',
 "transforms"='createKey,extractInt',
 "transforms.createKey.type"='org.apache.kafka.connect.transforms.ValueToKey',
 "transforms.createKey.fields"='OBJID',
 "transforms.extractInt.type"='org.apache.kafka.connect.transforms.ExtractField$Key',
 "transforms.extractInt.field"='OBJID'
 );
```

- Sink Connector

```sql
CREATE SINK CONNECTOR `c-xomni-historic` WITH(
 "connector.class"='io.confluent.connect.jdbc.JdbcSinkConnector',
 "connection.url"='jdbc:sqlserver://192.168.1.141;databaseName=cimsoa',
 "connection.user"='sa',
 "connection.password"='2020#concert',
 "topics"='xomni-historic',
 "db.timezone"='America/Sao_Paulo',
 "insert.mode"='upsert',
 "batch.size"='3000',
 "table.name.format"='THISTORIADOR',
 "pk.mode"='record_value',
 "pk.fields"='CH_TAG,CH_TIPO,DT_DATA,DT_HORA',
 "auto.create"='false',
 "auto.evolve"='false',
 "max.retries"='10',
 "retry.backoff.ms"='3000'
 );
```
