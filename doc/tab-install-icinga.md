=== "Ubuntu"

    ```bash
    apt install icinga2
    ```

=== "Debian"

    ```bash
    apt install icinga2
    ```

=== "SLES"

    ```bash
    zypper install icinga2
    ```

=== "RHEL"

    !!! Tip
        If you have SELinux enabled, the package `icinga2-selinux` is also required.



    === "RHEL 8 or Later"

        ```bash
        dnf install icinga2
        systemctl enable icinga2
        systemctl start icinga2
        ```

    === "RHEL 7"

        ```bash
        yum install icinga2
        systemctl enable icinga2
        systemctl start icinga2
        ```