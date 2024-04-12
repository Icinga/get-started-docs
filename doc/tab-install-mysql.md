=== "Ubuntu"

    ```bash
    apt install mysql-server
    ```

=== "Debian"

    ```bash
    apt install mariadb-server
    ```

=== "SLES"

    ```bash
    zypper install mysql-server
    systemctl enable mysql --now
    ```

=== "RHEL"

    === "RHEL 8 or Later"

        ```bash
        dnf install mysql-server
        systemctl start mysqld.service
        systemctl enable mysqld.service
        ```

    === "RHEL 7"

        ```bash
        yum install mysql-server
        systemctl start mysqld.service
        systemctl enable mysqld.service
        ```
        