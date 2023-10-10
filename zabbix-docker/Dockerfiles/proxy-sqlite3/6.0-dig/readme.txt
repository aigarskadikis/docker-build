
# move to directory where 'Dockerfile' is located
# then run:
docker build -t custom/zabbix-proxy-sqlite3:6.0.4-dig .

# the '-t' is title for container. it's up to you to use:
# custom/zabbix-proxy-sqlite3:6.0.4-dig
# or
# zabbix-proxy-sqlite3:6.0.4-dig

# to list local images use
docker images

# to run container with external scripts volume
export EXTERNALSCRIPTS="${HOME}/externalscripts"
mkdir -p ${EXTERNALSCRIPTS}

docker stop proxyB; docker rm proxyB; docker run --name proxyB -t \
-v "${EXTERNALSCRIPTS}:/usr/lib/zabbix/externalscripts" \
-e ZBX_HOSTNAME="proxyB" \
-e ZBX_SERVER_HOST="10.133.253.44:16051" \
-e ZBX_ENABLEREMOTECOMMANDS=1 \
-e ZBX_CONFIGFREQUENCY=60 \
-p 26051:10051 \
-d custom/zabbix-proxy-sqlite3:6.0.4-dig

