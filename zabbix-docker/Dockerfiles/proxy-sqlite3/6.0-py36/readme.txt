
# move to directory where 'Dockerfile' is located
# then run:
docker build -t custom/zabbix-proxy-sqlite3:6.0.22-py36 .

# the '-t' is title for container. it's up to you to use:
# custom/zabbix-proxy-sqlite3:6.0.22-py36
# or
# zabbix-proxy-sqlite3:6.0.22-py36

# to list local images use
docker images

# to run container with external scripts volume
export EXTERNALSCRIPTS="${HOME}/externalscripts"
mkdir -p ${EXTERNALSCRIPTS}

docker stop py36; docker rm py36; docker run --name py36 -t \
-v "${EXTERNALSCRIPTS}:/usr/lib/zabbix/externalscripts" \
-e ZBX_HOSTNAME="py36" \
-e ZBX_SERVER_HOST="10.133.253.44:16051" \
-e ZBX_ENABLEREMOTECOMMANDS=1 \
-e ZBX_CONFIGFREQUENCY=60 \
-d custom/zabbix-proxy-sqlite3:6.0.22-py36

