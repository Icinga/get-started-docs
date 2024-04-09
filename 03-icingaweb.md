# Icinga Web 2 on Ubuntu

!!! info
    ## Installation Requirements 
    - Icinga 2 and Icinga DB to monitor your infrastructure
    - A web server, e.g. Apache or Nginx
    - PHP version ≥ 7.2

    ### Optional Requirements¶
    - The pdfexport module (≥0.10) is required for the export to PDF
    - LDAP PHP library when using Active Directory or LDAP for authentication

## Add Icinga Package Repository 

You need to add the Icinga repository to your package management configuration for installing Icinga Web 2. If you’ve already configured your OS to use the Icinga repository for installing Icinga 2, you may skip this step.

### Ubuntu Repository

```bash 
apt-get update
apt-get -y install apt-transport-https wget gnupg

wget -O - https://packages.icinga.com/icinga.key | apt-key add -

. /etc/os-release; if [ ! -z ${UBUNTU_CODENAME+x} ]; then DIST="${UBUNTU_CODENAME}"; else DIST="$(lsb_release -c| awk '{print $2}')"; fi; \
 echo "deb https://packages.icinga.com/ubuntu icinga-${DIST} main" > \
 /etc/apt/sources.list.d/${DIST}-icinga.list
 echo "deb-src https://packages.icinga.com/ubuntu icinga-${DIST} main" >> \
 /etc/apt/sources.list.d/${DIST}-icinga.list

apt-get update
```

## Install Icinga Web 2
You can install Icinga Web 2 by using your distribution’s package manager to install the icingaweb2 package. The additional package icingacli is necessary to follow further steps in this guide.

```bash 
apt-get install icingaweb2 libapache2-mod-php icingacli
```

The additional package `libapache2-mod-php` is necessary on Ubuntu to automatically install a web server and PHP and make Icinga Web 2 work out-of-the-box.

## Install the Web Server
Ensure that you have a web server with PHP installed before proceeding, such as Apache or Nginx with PHP version ≥ 7.2. Depending on your operating system, you may need to install and configure the web server separately. An Apache configuration file to serve Icinga Web is already installed. If you want to use Nginx, you must manually create a configuration file using the following command. Save the output as a new file in the web server configuration directory:

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






