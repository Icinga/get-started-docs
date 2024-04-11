=== "Ubuntu"

    ```bash
    apt install monitoring-plugins
    ```

=== "Debian"

    ```bash
    apt install monitoring-plugins
    ```

=== "SLES"

    ```bash
    zypper install --recommends monitoring-plugins-all
    ```

=== "RHEL"

    The packages for RHEL depend on other packages which are distributed as part of the EPEL repository.


    === "RHEL 8 or Later"

        ```bash
        dnf install nagios-plugins-all
        ```

    === "RHEL 7"

        ```bash
        yum install nagios-plugins-all
        ```