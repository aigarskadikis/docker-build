
# move to directory where 'Dockerfile' is located
# then run:
podman build -t aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc .

# the '-t' is title and tag for container. it's up to you to use:
# aigarskadikis/zabbix-proxy-sqlite3:7.0.13-odbc
# or
# zabbix-proxy-sqlite3:7.0.13-odbc

# to list local images use
podman images

# to run container with external scripts volume
export EXTERNALSCRIPTS="${HOME}/externalscripts"
mkdir -p ${EXTERNALSCRIPTS}

podman stop odbc-prx-7.0; podman rm odbc-prx-7.0; podman run --name odbc-prx-7.0 -t \
-v "${EXTERNALSCRIPTS}:/usr/lib/zabbix/externalscripts" \
-e ZBX_HOSTNAME="odbc-prx-7.0" \
-e ZBX_SERVER_HOST="10.133.253.44:27051" \
-e ZBX_ENABLEREMOTECOMMANDS=1 \
-d aigarskadikis/zabbix-proxy-sqlite3:6.0.22-py36

