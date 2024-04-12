=== "Ubuntu"

    ```bash
    apt install icinga2 monitoring-plugins
    ```

=== "Debian"

    ```bash
    apt install icinga2 monitoring-plugins
    ```

=== "SLES"

    ```bash
    zypper install icinga2
    zypper install --recommends monitoring-plugins-all
    ```

=== "RHEL"

    !!! Tip
        If you have SELinux enabled, the package `icinga2-selinux` is also required.

    The packages for RHEL depend on other packages which are distributed as part of the EPEL repository.

    === "RHEL 8 or Later"

        ```bash
        dnf install icinga2
        systemctl enable icinga2
        systemctl start icinga2
        dnf install nagios-plugins-all
        ```

    === "RHEL 7"

        ```bash
        yum install icinga2
        systemctl enable icinga2
        systemctl start icinga2
        yum install nagios-plugins-all
        ```