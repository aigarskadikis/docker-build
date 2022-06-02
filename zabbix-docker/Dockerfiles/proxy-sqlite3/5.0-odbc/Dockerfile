FROM zabbix/zabbix-proxy-sqlite3:ol-5.0.24

USER root

RUN set -eux && \
    INSTALL_PKGS="libaio \
            libnsl \
mysql-connector-odbc \
postgresql-odbc \
msodbcsql18 mssql-tools18" && \
curl -o /etc/yum.repos.d/mssql-release.repo \
https://packages.microsoft.com/config/rhel/8/prod.repo && \
rpm -ivh \
https://dev.mysql.com/get/mysql80-community-release-el8-4.noarch.rpm \
https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm && \
    ACCEPT_EULA=Y microdnf -y install \
            --disablerepo="*" \
            --enablerepo="ol8_baseos_latest" \
            --enablerepo="ol8_appstream" \
            --enablerepo="ol8_developer_EPEL" \
            --enablerepo="mysql-connectors-community" \
            --enablerepo="mysql-tools-community" \
            --enablerepo="mysql80-community" \
            --enablerepo="pgdg-common" \
            --enablerepo="pgdg14" \
            --enablerepo="packages-microsoft-com-prod" \
            --setopt=install_weak_deps=0 \
            --best \
            --nodocs \
        ${INSTALL_PKGS} && \
rpm -ivh \
https://download.oracle.com/otn_software/linux/instantclient/1915000/oracle-instantclient19.15-basic-19.15.0.0.0-1.x86_64.rpm \
https://download.oracle.com/otn_software/linux/instantclient/1915000/oracle-instantclient19.15-odbc-19.15.0.0.0-1.x86_64.rpm && \
    microdnf -y clean all && \
    rm -rf /var/cache/yum /var/lib/yum/yumdb/* /usr/lib/udev/hwdb.d/* && \
    rm -rf /var/cache/dnf /etc/udev/hwdb.bin /root/.pki

USER 1997

