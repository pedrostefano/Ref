# Postgresql Commands

## Simples




## DevOPS

- Import CSV with Tab delimiter and Header

```sql
COPY floco."EVENTOS_OPC"
FROM 'C:\Concert\repository\eventos.txt' DELIMITER E'\t' CSV HEADER;
```

- Import CSV with ';' delimiter without Header to a temporary table and update other table from schema

```sql
create temp table TMP_X(nom text, nf text, nt text);
COPY TMP_X FROM 'C:\Concert\repository\CHAVES.csv' DELIMITER ';' ;

update floco."T_SWITCHES" 
set "NAME" = X."nom"
from TMP_X X
where floco."T_SWITCHES"."NODE_FROM" = X.nf and floco."T_SWITCHES"."NODE_TO" = X.nt;

drop table TMP_X;
```

## POSTGIS

- Geo field of a Multilinesegment to json and extract coordinates field

```sql
ST_AsGeoJSON(geom)::json->'coordinates' as COORD
```

- Geo field of a Point to X and Y

```sql
ST_X(UNTRD.geom) COORD_X,
ST_Y(UNTRD.geom) COORD_Y
```

- Find intersections from GEO

```sql
select *
from
(select SUB."COD_ID", SUB.geom from "BDGD2018"."SUB" SUB  where SUB."COD_ID" = '1727135' ) a,
(select SSDMT."COD_ID", SSDMT.geom from "BDGD2018"."SSDMT" SSDMT ) b
where ST_Intersects(a.geom, b.geom);
```

- Adjacent Feeders

```sql
select distinct s2."CTMT"
from "BDGD2018"."SSDMT" s1
left join "BDGD2018"."SSDMT" s2 on (s2."CTMT" <> 'NLAE733' and ST_touches(s1.geom, s2.geom))
left join "BDGD2018"."UNSEMT" s3 on  (s3."CTMT" <> 'NLAE733' and ST_touches(s1.geom, s3.geom))
where
s1."CTMT" = 'NLAE733'
```
