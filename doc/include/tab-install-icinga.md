=== "Ubuntu"

    ```bash
    apt install \
        icinga2 \
        icingacli \
        icingadb \
        icingadb-redis \
        icingadb-web \
        icingaweb2 \
        icinga-director \
        monitoring-plugins 
    ```

=== "Debian"

    ```bash
    apt install \
        icinga2 \
        icingacli \
        icingadb \
        icingadb-redis \
        icingadb-web \
        icingaweb2 \
        icinga-director \
        monitoring-plugins
    ```

=== "SLES"

    ```bash
    zypper install \
        icinga2 \
        icingacli \
        icingadb \
        icingadb-redis \
        icingadb-web \
        icingaweb2 \
        icinga-director
        
    zypper install --recommends monitoring-plugins-all
    ```

=== "RHEL"

    The packages for RHEL depend on other packages which are distributed as part of the EPEL repository.

    ```bash
    dnf install \
        icinga2 \
        icingacli \
        icingadb \
        icingadb-redis \
        icingadb-web \
        icingaweb2 \
        icinga-director \
        nagios-plugins-all

    systemctl enable --now icinga2
    ```
