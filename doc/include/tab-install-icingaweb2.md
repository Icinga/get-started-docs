=== "Ubuntu"

    ```bash 
    apt install libapache2-mod-php
    ```

    The additional package `libapache2-mod-php` is necessary on Ubuntu to automatically install a web server and PHP and make Icinga Web 2 work out-of-the-box.

=== "Debian"

    On Debian there is no additional step necessary. 

=== "SLES"

    ```bash
    systemctl enable --now apache2
    a2enmod rewrite
    systemctl restart apache2
    ```

=== "RHEL"

    ```bash
    systemctl enable --now httpd
    ```