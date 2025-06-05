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
 --env ZBX_HOSTNAME="odbc" \
 --env ZBX_SERVER_HOST="zs.ext:10051" \
 --env ZBX_ENABLEREMOTECOMMANDS=1 \
--detach aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc
```

## Smaple DB flavors

### Oracle MySQL 8.0
```
mkdir -p ${HOME}/mysql80/datadir ${HOME}/mysql80/conf.d
podman run --name mysql80 \
--net podman --ip 10.88.80.80 \
--volume ${HOME}/mysql80/datadir:/var/lib/mysql \
--volume ${HOME}/mysql80/conf.d:/etc/mysql/conf.d \
 --env MYSQL_RANDOM_ROOT_PASSWORD='yes' \
--detach mysql:8.0.42-oraclelinux9 \
--character-set-server='utf8mb4' \
--collation-server='utf8mb4_unicode_ci'
```

Read root password:
```
podman logs -f mysql80 | grep "GENERATED ROOT PASSWORD"
```



### MS SQL 2019 database
```
podman run --detach \
--name=mssql \
--net podman --ip 10.88.20.19 \
--env 'ACCEPT_EULA=Y' \
--env 'SA_PASSWORD=Winter2019' \
-p 1433:1433 \
mcr.microsoft.com/mssql/server:2019-CU10-ubuntu-20.04
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
podman run --detach \
--name oracle19c \
--net podman --ip 10.88.19.19 \
--env ORACLE_SID=test \
--env ORACLE_PWD=Password123 \
--volume oracle-db:/opt/oracle/oradata \
--restart unless-stopped \
oracle/database:19.3.0-ee
```

Macros:
```
  {$ORACLE.DRIVER} = /usr/lib/oracle/19.15/client64/lib/libsqora.so.19.1 
    {$ORACLE.HOST} = 10.88.19.19
    {$ORACLE.PORT} = 1521
 {$ORACLE.SERVICE} = test
    {$ORACLE.USER} = system
{$ORACLE.PASSWORD} = Password123
```


Item
```
db.odbc.get[get_db,,"Driver={$ORACLE.DRIVER};DBQ=//{$ORACLE.HOST}:{$ORACLE.PORT}/{$ORACLE.SERVICE};"]
```


