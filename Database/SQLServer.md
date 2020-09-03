
# Microsoft SQL Server

- Alterar coluna

```sql
ALTER TABLE THISTORIADOR ALTER COLUMN FL_VALOR FLOAT NOT NULL;
```

- Adiconar Chave Composta

```sql
ALTER table THISTORIADOR
ADD CONSTRAINT "PK_HIST" PRIMARY KEY CLUSTERED ("CH_TAG","DT_DATA","DT_HORA","CH_TIPO")
```