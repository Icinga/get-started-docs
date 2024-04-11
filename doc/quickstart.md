# Install Icinga 2

### Add Icinga Package Repository

First add the official repositories:

{% include "tab-repository.md" %}

### Install Icinga 2

You can use your distribution’s package manager to install the `icinga2` package. The following commands must be executed with root permissions unless noted otherwise.

{% include "tab-install-icinga.md" %}

#### Systemd Service

The majority of supported distributions use systemd. The Icinga 2 packages automatically install the necessary systemd unit files.

If you’re stuck with configuration errors, you can manually invoke the configuration validation.
    
```bash
icinga2 daemon -C
```

!!! tip

    If you are running into fork errors with systemd enabled distributions, please check the [troubleshooting chapter](https://icinga.com/docs/icinga-2/latest/doc/15-troubleshooting/#check-fork-errors).

### Set up Check Plugins

Without plugins Icinga 2 does not know how to check external services.

Depending on which directory your plugins are installed into you may need to update the global PluginDir constant in your Icinga 2 configuration. This constant is used by the check command definitions contained in the Icinga Template Library to determine where to find the plugin binaries.

{% include "tab-setup-check-plugins.md" %}

### Set up Icinga 2 API

!!! info

    Almost every Icinga 2 setup requires the Icinga 2 API as Icinga Web connects to it, Icinga DB requires it, and it enables cluster communication functionality for highly available and distributed setups.

See the API chapter for details, or follow the steps below to set up the API quickly:

Run the following command to:
- enable the `api` feature
- set up certificates, and
- add the API user `root` with an auto-generated password to the configuration file `/etc/icinga2/conf.d/api-users.conf`

```bash
icinga2 api setup
```

Restart Icinga 2 for these changes to take effect.

```bash
systemctl restart icinga2
```

### Set up Icinga DB

Icinga DB is a set of components for publishing, synchronizing and visualizing monitoring data in the Icinga ecosystem, consisting of:

- Icinga 2 with its `icingadb` feature enabled
- [Icinga DB daemon](https://icinga.com/docs/icinga-db/latest/doc/01-About/)
- Icinga Web with the [Icinga DB Web](https://icinga.com/docs/icinga-db-web/latest/) module enabled

!!! info

    Setting up Icinga 2’s Icinga DB feature is only required for Icinga 2 master nodes or single-node setups.

#### Set up Redis® Server

!!! info

    This guide sets up the icingadb-redis package provided by Icinga, which ships a current Redis® Server version and is preconfigured for the Icinga DB components. Using own Redis® server setups is supported as long as the version is from 6.2.

!!! tip

    Although the Redis® server can run anywhere in an Icinga environment, we recommend to install it where the corresponding Icinga 2 node is running to keep latency between the components low.

##### Install Icinga DB Redis® Package

Use your distribution’s package manager to install the icingadb-redis package as follows:

{% include "tab-install-icingadb-redis.md" %}

##### Run Icinga DB Redis®

The `icingadb-redis` package automatically installs the necessary systemd unit files to run Icinga DB Redis®. Please run the following command to enable and start its service:

```bash
systemctl enable --now icingadb-redis
```

!!! Warning

    If  the following error occurs: 
    *"Failed to enable unit: Refusing to operate on alias name or linked unit file: icingadb-redis.service"*
    Check the service's status with: 
    
    ```bash
    systemctl status icingadb-redis
    ```

    If it's already running, everything is okay.



##### Enable Remote Redis Connections

By default, `icingadb-redis` only listens on `127.0.0.1`. If Icinga Web or Icinga 2 is running on another node, remote access to the Redis® server must be allowed. This requires the following directives to be set in the `/etc/icingadb-redis/icingadb-redis.conf` configuration file:

- Set `protected-mode` to `no`, i.e. `protected-mode no`
- Set `bind` to the desired binding interface or bind all interfaces, e.g. `bind 0.0.0.0`

!!! warning

    By default, Redis® has no authentication preventing others from accessing it. When opening Redis® to an external interface, make sure to set a password, set up appropriate firewall rules, or configure TLS with certificate authentication on Redis® and its consumers, i.e. Icinga 2, Icinga DB and Icinga Web.

Restart Icinga DB Redis® for these changes to take effect:

```bash
systemctl restart icingadb-redis
```

#### Enable Icinga DB Feature

Icinga 2 installs the feature configuration file to `/etc/icinga2/features-available/icingadb.conf`, pre-configured for a local setup. Update this file in case Redis® is running on a different host or to set credentials.

To enable the icingadb feature use the following command:

```bash
icinga2 feature enable icingadb
```

Restart Icinga 2 for these changes to take effect:

```bash
systemctl restart icinga2
```

#### Install Icinga DB Daemon

After installing Icinga 2, setting up a Redis® server and enabling the `icingadb` feature, the Icinga DB daemon that synchronizes monitoring data between the Redis® server and a database is now set up.

!!! tip

    Although the Icinga DB daemon can run anywhere in an Icinga environment, we recommend to install it where the corresponding Icinga 2 node and Redis® server is running to keep latency between the components low.

The Icinga DB daemon package is also included in the Icinga repository, and since it is already set up, you have completed the instructions here and can proceed to install the Icinga DB daemon, which will also guide you through the setup of the database and Icinga DB Web.

### Backup

Ensure to include the following in your backups:

- Configuration files in `/etc/icinga2`
- Certificate files in `/var/lib/icinga2/ca` (Master CA key pair) and `/var/lib/icinga2/certs` (node certificates)
- Runtime files in `/var/lib/icinga2`


## Install Icinga DB

### Installing Icinga DB Package
Use your distribution’s package manager to install the icingadb package as follows:

{% include "tab-install-icingadb.md" %}

### Setting up the Database

A MySQL (≥5.5), MariaDB (≥10.1), or PostgreSQL (≥9.6) database is required to run Icinga DB. Please follow the steps listed for your target database, which guide you through setting up the database and user and importing the schema.

#### Setting up a MySQL (or MariaDB) Database

!!! warning 
    If you use a version of `MySQL < 5.7` or `MariaDB < 10.2`, the following server options must be set:


    ```bash
    innodb_file_format=barracuda
    innodb_file_per_table=1
    innodb_large_prefix=1
    ```
#### Set up a MySQL database for Icinga DB

Install MySQL 

{% include "tab-install-mysql.md" %}

Open MySQL and create a database

```sql
# mysql -u root -p

mysql> CREATE DATABASE icingadb;
mysql> CREATE USER 'icingadb'@'localhost' IDENTIFIED BY 'CHANGEME';
mysql> GRANT ALL ON icingadb.* TO 'icingadb'@'localhost';
```

After creating the database, import the Icinga DB schema using the following command:


```bash
mysql -u root -p icingadb </usr/share/icingadb/schema/mysql/schema.sql
```

### Configuring Icinga DB

!!! info 

    Icinga DB installs its configuration file to `/etc/icingadb/config.yml`, pre-populating most of the settings for a local setup. Before running Icinga DB, adjust the Redis® and database credentials and, if necessary, the connection configuration.


### Running Icinga DB
The icingadb package automatically installs the necessary systemd unit files to run Icinga DB. Please run the following command to enable and start its service:

```bash
systemctl enable --now icingadb
```

### Installing Icinga DB Web
With Icinga 2, Redis®, Icinga DB and the database fully set up, it is now time to install Icinga DB Web, which connects to both Redis® and the database to display and work with the monitoring data.

## Icinga Web 2 on Ubuntu

### Install Icinga Web 2
You can install Icinga Web 2 by using your distribution’s package manager to install the icingaweb2 package. The additional package icingacli is necessary to follow further steps in this guide.

{% include "tab-install-icingaweb2.md" %}

### Install the Web Server

!!! info

    Ensure that you have a web server with PHP installed before proceeding, such as Apache or Nginx with PHP version ≥ 7.2.

An Apache configuration file to serve Icinga Web is already installed. If you want to use Nginx, you must manually create a configuration file using the following command. Save the output as a new file in the web server configuration directory:

```bash
icingacli setup config webserver nginx --document-root /usr/share/icingaweb2/public
```

### Prepare Web Setup
You can set up Icinga Web 2 quickly and easily with the Icinga Web 2 setup wizard which is available the first time you visit Icinga Web 2 in your browser. When using the web setup you are required to authenticate using a token. In order to generate a token use the icingacli:

```bash 
icingacli setup token create
```

In case you do not remember the token you can show it using the icingacli:

```bash
icingacli setup token show
```

You need to manually create a database and a database user prior to starting the web wizard. This is due to local security restrictions whereas the web wizard cannot create a database/user through a local unix domain socket.

```sql
# mysql -u root -p

mysql> CREATE DATABASE icingaweb2;
mysql> CREATE USER 'icingaweb2'@'localhost' IDENTIFIED BY 'CHANGEME';
mysql> GRANT ALL PRIVILEGES ON icingaweb2.* TO 'icingaweb2'@'localhost';
```

!!! note
    This is only required if you are using a local database as authentication type.

Add the webserver user (`www-data` in case of Apache) to the `icingaweb2`-group

```bash
usermod -a -G icingaweb2 www-data
```

Restart the webserver

```bash
systemctl restart apache2
```

### Start Web Setup
Finally visit Icinga Web 2 in your browser to access the setup wizard and complete the installation: `/icingaweb2/setup`.

!!! tip
    Use the same database, user and password details created above when asked.

The setup wizard automatically detects the required packages. In case one of them is missing, e.g. a PHP module, please install the package, restart your webserver and reload the setup page.


!!! Info
    The API-user and it's password are written in the file `/etc/icinga2/conf.d/api-users.conf`

## Installing Icinga DB Web on Ubuntu

### Installing the Package

Use your distribution’s package manager to install the icingadb-web package as follows:

{% include "tab-install-icingadbweb.md" %}

This concludes the installation. Now proceed with the configuration.

## Installing Icinga Director on Debian

### Installing the Package

Use your distribution’s package manager to install the icinga-director package as follows:

{% include "tab-install-director.md" %}

### Setting up MySQL or MariaDB Database

!!! Warn
    Warning Make sure to replace `CHANGEME` with a secure password.

```sql
# mysql -u root -p 

mysql> CREATE DATABASE director CHARACTER SET 'utf8';
mysql> CREATE USER director@localhost IDENTIFIED BY 'CHANGEME';
mysql> GRANT ALL ON director.* TO director@localhost;
```

### Configuring Icinga Director

Log in to your running Icinga Web setup with a privileged user and follow the steps below to configure Icinga Director:

1. Create a new resource for the Icinga Director database via the Configuration → Application → Resources menu. Please make sure that you configure utf8 as encoding.

2. Select Icinga Director directly from the main menu and you will be taken to the kickstart wizard. Follow the instructions and you are done!

## Follow-Ups after Installing Icinga

After you installed Icinga with all its necessary components successfully there are some more things you could do:

- Refer to the [service monitoring](https://icinga.com/docs/icinga-2/latest/doc/05-service-monitoring/#service-monitoring-plugins) chapter for details about how to integrate additional check plugins into your Icinga 2 setup.