FROM zabbix/zabbix-proxy-sqlite3:ol-7.0.13

USER root

RUN set -eux && \
    INSTALL_PKGS="libaio \
            libnsl \
mysql-connector-odbc \
postgresql-odbc \
msodbcsql18 mssql-tools18" && \
curl -o /etc/yum.repos.d/mssql-release.repo \
https://packages.microsoft.com/config/rhel/9/prod.repo && \
rpm -ivh \
https://dev.mysql.com/get/mysql80-community-release-el9-4.noarch.rpm \
https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm && \
    ACCEPT_EULA=Y microdnf -y install \
            --disablerepo="*" \
            --enablerepo="ol9_baseos_latest" \
            --enablerepo="ol9_appstream" \
            --enablerepo="ol9_developer_EPEL" \
            --enablerepo="mysql-connectors-community" \
            --enablerepo="mysql-tools-community" \
            --enablerepo="mysql80-community" \
            --enablerepo="pgdg-common" \
            --enablerepo="pgdg16" \
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

