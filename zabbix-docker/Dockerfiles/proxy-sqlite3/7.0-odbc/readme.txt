
# move to directory where 'Dockerfile' is located
# then run:
podman build -t aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc .

# the '-t' is title and tag for container. it's up to you to use:
# aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc
# or
# zabbix-proxy-sqlite3:7.0.13-odbc

# to list local images use
podman images

# create zabbix proxy with ODBC support
podman stop odbc-prx-7.0; podman rm odbc-prx-7.0; podman run --name odbc-prx-7.0 -t \
--net podman --ip 10.88.7.70 \
-e ZBX_HOSTNAME="odbc" \
-e ZBX_SERVER_HOST="zs.ext:10051" \
-e ZBX_ENABLEREMOTECOMMANDS=1 \
-d aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc

# test MS SQL 2019 database
podman run \
--name=mssql \
--net podman --ip 10.88.20.19 \
-e 'ACCEPT_EULA=Y' \
-e 'SA_PASSWORD=Winter2019' \
-p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-CU10-ubuntu-20.04

# oracle19 sample database
podman run -d --name oracle19c \
--net podman --ip 10.88.19.19 \
-e ORACLE_SID=test \
-e ORACLE_PWD=Password123 \
-v oracle-db:/opt/oracle/oradata \
--restart unless-stopped \
oracle/database:19.3.0-ee

