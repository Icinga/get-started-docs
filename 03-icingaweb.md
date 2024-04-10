# Icinga Web 2 on Ubuntu

## Install Icinga Web 2
You can install Icinga Web 2 by using your distribution’s package manager to install the icingaweb2 package. The additional package icingacli is necessary to follow further steps in this guide.

```bash 
apt-get install icingaweb2 libapache2-mod-php icingacli
```

The additional package `libapache2-mod-php` is necessary on Ubuntu to automatically install a web server and PHP and make Icinga Web 2 work out-of-the-box.

## Install the Web Server

!!! info

    Ensure that you have a web server with PHP installed before proceeding, such as Apache or Nginx with PHP version ≥ 7.2.

An Apache configuration file to serve Icinga Web is already installed. If you want to use Nginx, you must manually create a configuration file using the following command. Save the output as a new file in the web server configuration directory:

```bash
icingacli setup config webserver nginx --document-root /usr/share/icingaweb2/public
```

## Prepare Web Setup
You can set up Icinga Web 2 quickly and easily with the Icinga Web 2 setup wizard which is available the first time you visit Icinga Web 2 in your browser. When using the web setup you are required to authenticate using a token. In order to generate a token use the icingacli:

```bash 
icingacli setup token create
```

In case you do not remember the token you can show it using the icingacli:

```bash
icingacli setup token show
```

You need to manually create a database and a database user prior to starting the web wizard. This is due to local security restrictions whereas the web wizard cannot create a database/user through a local unix domain socket.

```bash
MariaDB [mysql]> CREATE DATABASE icingaweb2;

MariaDB [mysql]> GRANT ALL ON icingaweb2.* TO icingaweb2@localhost IDENTIFIED BY 'CHANGEME';
```

!!! tip
    You may also create a separate administrative account with all privileges instead.

!!! note
    This is only required if you are using a local database as authentication type.


## Start Web Setup
Finally visit Icinga Web 2 in your browser to access the setup wizard and complete the installation: `/icingaweb2/setup`.

!!! tip
    Use the same database, user and password details created above when asked.

The setup wizard automatically detects the required packages. In case one of them is missing, e.g. a PHP module, please install the package, restart your webserver and reload the setup page.

**Open next step to [install Icinga DB Web](04-icingadbweb.md)**