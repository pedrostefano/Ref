# Oracle Commands

## Básicos

## Query

- Filtrar por data

```sql
WHERE
COL >= to_date(:Timestamp,'YYYY-MM-DD HH24:MI:SS')
```

- Filtrar por string separada por virgula

```sql
SELECT * FROM TABLE
WHERE COLUMN IN (
select regexp_substr("A,B,C,D",'[^,]+', 1, level) from dual
connect by regexp_substr("A,B,C,D", '[^,]+', 1, level) is not null )
```

- Filtrar por string separada por virgula Oracle 9i

```sql

```

## DevOps

- Create User / Schema

```sql
create user USERNAME identified by "PASSWORD";
```

- Deletar usuário / schema

```sql
drop user USERNAME cascade;
```

- Atribuir todas as permissões

```sql
grant all privileges to USERNAME;
```

- Atribuir permissões de DBA

```sql
grant dba to USERNAME;
```

- Atribuir permissões de leitura a uma tabela para um usuário

```sql
grant SELECT on "SCHEMA"."TABLENAME" to "USERNAME" ;
```

- Atribuir permissões de leitura a todas as tabelas de um schema para um usuário.

```sql
BEGIN
    FOR t IN (SELECT * FROM user_tables)
    LOOP
        EXECUTE IMMEDIATE 'GRANT SELECT ON ' || t.table_name || ' TO USERNAME';
    END LOOP;
END;
```

- Calcular tamanho do banco

```sql
select sum(bytes)/1024/1024 size_in_mb from dba_data_files;

select owner, sum(bytes)/1024/1024 Size_MB from dba_segments
group  by owner;
```
