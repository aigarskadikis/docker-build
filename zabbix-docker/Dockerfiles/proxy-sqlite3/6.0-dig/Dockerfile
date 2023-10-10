FROM zabbix/zabbix-proxy-sqlite3:ol-6.0.4

USER root

RUN set -eux && \
    INSTALL_PKGS="bind-utils" && \
    ACCEPT_EULA=Y microdnf -y install \
            --disablerepo="*" \
            --enablerepo="ol8_baseos_latest" \
            --enablerepo="ol8_appstream" \
            --enablerepo="ol8_developer_EPEL" \
            --setopt=install_weak_deps=0 \
            --best \
            --nodocs \
        ${INSTALL_PKGS} && \
    microdnf -y clean all && \
    rm -rf /var/cache/yum /var/lib/yum/yumdb/* /usr/lib/udev/hwdb.d/* && \
    rm -rf /var/cache/dnf /etc/udev/hwdb.bin /root/.pki

USER 1997

