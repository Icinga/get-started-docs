=== "Ubuntu"

    ```bash
    usermod -a -G icingaweb2 www-data
    systemctl restart apache2
    ```

=== "Debian"

    ```bash
    usermod -a -G icingaweb2 www-data
    systemctl restart apache2
    ```

=== "SLES"

    ```bash
    usermod -a -G icingaweb2 wwwrun
    systemctl restart apache2
    ```

=== "RHEL"

    ```bash
    usermod -a -G icingaweb2 apache
    systemctl restart httpd
    ```
