# Oracle Commands



## Query



- Filtrar por string separada por virgula

```sql

```



## DevOps

- Create User / Schema

```sql
create user USERNAME identified by "PASSWORD"
```

- Atribuir todas as permissões 

```sql
grant all privileges to USERNAME
```

- Atribuir permissões de DBA

```sql
grant dba to USERNAME
```

- Deletar usuário / schema

```sql
drop user USERNAME cascade
```

- Calcular tamanho do banco

```sql
select sum(bytes)/1024/1024 size_in_mb from dba_data_files;

select owner, sum(bytes)/1024/1024 Size_MB from dba_segments
group  by owner;
```