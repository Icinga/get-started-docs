=== "Ubuntu"

    ```bash 
    apt install icingaweb2 libapache2-mod-php icingacli
    ```

    The additional package `libapache2-mod-php` is necessary on Ubuntu to automatically install a web server and PHP and make Icinga Web 2 work out-of-the-box.

=== "Debian"

    ```bash 
    apt install icingaweb2 icingacli
    ```

=== "SLES"

    ```bash
    zypper install icingaweb2 icingacli
    ```

=== "RHEL"

    === "RHEL 8 or Later"

        ```bash
        dnf install icingaweb2 icingacli
        ```

    === "RHEL 7"

        ```bash
        yum install icingaweb2 icingacli
        ```
        