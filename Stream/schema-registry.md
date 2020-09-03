# Schema Registry

[Confluent - API Docs](https://docs.confluent.io/current/schema-registry/develop/api.html)

- Listar schemas

`curl --silent -X GET http://localhost:8081/subjects/`shell

- Detalhes de um schema

`curl --silent -X GET http://localhost:8081/subjects/xomni-get-commands-value/versions/latest`shell

- Deletar um schema

`curl --silent -X DELETE http://localhost:8081/subjects/xomni-get-commands-value/`shell
