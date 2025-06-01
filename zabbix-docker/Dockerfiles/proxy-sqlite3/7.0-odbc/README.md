# HowTo create Zabbix 7.0 proxy container with ODBC (PostgreSQL, MySQL, MS SQL, Oracle) support

## Build

Move to directory where 'Dockerfile' is located. Then run
```
podman build -t aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc .
```

'-t' is title and tag for container. it's up to you to use:
```
aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc
```
or
```
zabbix-proxy-sqlite3:7.0.13-odbc
```

## List local images use
```
podman images
```

## Launch

Create zabbix proxy with ODBC support
```
podman stop odbc-prx-7.0; podman rm odbc-prx-7.0; podman run --name odbc-prx-7.0 -t \
--net podman --ip 10.88.7.70 \
-e ZBX_HOSTNAME="odbc" \
-e ZBX_SERVER_HOST="zs.ext:10051" \
-e ZBX_ENABLEREMOTECOMMANDS=1 \
-d aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc
```

## Smaple DB flavors

### MS SQL 2019 database
```
podman run \
--name=mssql \
--net podman --ip 10.88.20.19 \
-e 'ACCEPT_EULA=Y' \
-e 'SA_PASSWORD=Winter2019' \
-p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-CU10-ubuntu-20.04
```

fetch sample item
```
db.odbc.get[first,,"Driver=/opt/microsoft/msodbcsql18/lib64/libmsodbcsql-18.5.so.1.1;Server=10.88.20.19;Database=master;UID=sa;PWD=Winter2019;TrustServerCertificate=Yes;"]
```

SQL
```sql
SELECT object_name,counter_name,instance_name,cntr_value FROM sys.dm_os_performance_counters
```

### Oracle19 sample database
```
podman run -d --name oracle19c \
--net podman --ip 10.88.19.19 \
-e ORACLE_SID=test \
-e ORACLE_PWD=Password123 \
-v oracle-db:/opt/oracle/oradata \
--restart unless-stopped \
oracle/database:19.3.0-ee
```
