# Quickstart

## Install Icinga 2

**Add official Icinga Package Repository**

{% include "tab-repository.md" %}

**Install Icinga 2**

The following commands must be executed with root permissions unless noted otherwise.

{% include "tab-install-icinga.md" %}

```bash
icinga2 api setup
systemctl restart icinga2
```

!!! info

    This does:

    - enable the `api` feature
    - set up certificates
    - add the API user `root` with an auto-generated password to the configuration file `/etc/icinga2/conf.d/api-users.conf`

### Set up Icinga DB

#### Install Icinga DB Redis® Package

{% include "tab-install-icingadb-redis.md" %}

#### Run Icinga DB Redis®

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

#### Enable Icinga DB Feature

```bash
icinga2 feature enable icingadb
systemctl restart icinga2
```

## Install Icinga DB

### Install Icinga DB Package

{% include "tab-install-icingadb.md" %}

### Set up MySQL Database

!!! warning 
    If you use a version of `MySQL < 5.7` or `MariaDB < 10.2`, the following server options must be set:


    ```bash
    innodb_file_format=barracuda
    innodb_file_per_table=1
    innodb_large_prefix=1
    ```

**Install MySQL**

{% include "tab-install-mysql.md" %}

Open MySQL and create a database:

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

### Configure Icinga DB

!!! info 

    Icinga DB installs its configuration file to `/etc/icingadb/config.yml`, pre-populating most of the settings for a local setup. Before running Icinga DB, adjust the Redis® and database credentials and, if necessary, the connection configuration.


**Start Icinga DB**

```bash
systemctl enable --now icingadb
```

## Icinga Web 2

**Install Icinga Web 2**

{% include "tab-install-icingaweb2.md" %}

**Install the Web Server**

An Apache configuration file to serve Icinga Web is already installed. If you want to use Nginx, you must manually create a configuration file using the following command. Save the output as a new file in the web server configuration directory:

```bash
icingacli setup config webserver nginx --document-root /usr/share/icingaweb2/public
```

**Prepare Web Setup**

You can set up Icinga Web 2 quickly and easily with the Icinga Web 2 setup wizard which is available the first time you visit Icinga Web 2 in your browser. When using the web setup you are required to authenticate using a token. In order to generate a token use the icingacli:

```bash 
icingacli setup token create
```

In case you do not remember the token you can show it using the icingacli:

```bash
icingacli setup token show
```

Create a database:

```sql
# mysql -u root -p

mysql> CREATE DATABASE icingaweb2;
mysql> CREATE USER 'icingaweb2'@'localhost' IDENTIFIED BY 'CHANGEME';
mysql> GRANT ALL PRIVILEGES ON icingaweb2.* TO 'icingaweb2'@'localhost';
```

Add the webserver user (`www-data` in case of Apache) to the `icingaweb2`-group

```bash
usermod -a -G icingaweb2 www-data   
systemctl restart apache2
```

**Start Web Setup**

Finally visit Icinga Web 2 in your browser to access the setup wizard and complete the installation: `/icingaweb2/setup`.

!!! tip
    Use the same database, user and password details created above when asked.

The setup wizard automatically detects the required packages. In case one of them is missing, e.g. a PHP module, please install the package, restart your webserver and reload the setup page.


!!! Info
    The API-user and it's password are written in the file `/etc/icinga2/conf.d/api-users.conf`

## Install Icinga DB Web

**Install the Package**

{% include "tab-install-icingadbweb.md" %}

This concludes the installation. Now proceed with the configuration.

## Install Icinga Director

**Install the Package**

{% include "tab-install-director.md" %}

### Set up MySQL Database

```sql
# mysql -u root -p 

mysql> CREATE DATABASE director CHARACTER SET 'utf8';
mysql> CREATE USER director@localhost IDENTIFIED BY 'CHANGEME';
mysql> GRANT ALL ON director.* TO director@localhost;
```

### Configure Icinga Director

Log in to your running Icinga Web setup with a privileged user and follow the steps below to configure Icinga Director:

1. Create a new resource for the Icinga Director database via the Configuration → Application → Resources menu. Please make sure that you configure utf8 as encoding.

2. Select Icinga Director directly from the main menu and you will be taken to the kickstart wizard. Follow the instructions and you are done!

## Follow-Ups after Installing Icinga

**Backup**

If you want to save your installation, ensure to include the following in your backups:

- Configuration files in `/etc/icinga2`
- Certificate files in `/var/lib/icinga2/ca` (Master CA key pair) and `/var/lib/icinga2/certs` (node certificates)
- Runtime files in `/var/lib/icinga2`

**Reference**

After you installed Icinga with all its necessary components successfully there are some more things you could do:

Refer to the...

- ... [Service Monitoring](https://icinga.com/docs/icinga-2/latest/doc/05-service-monitoring/#service-monitoring-plugins) chapter for details about how to integrate additional check plugins into your Icinga 2 setup.
- ... [API](https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#icinga2-api-setup) chapter for details about how to set up and configure the Icinga 2 API.
- ... [Monitoring Plugins Project](https://www.monitoring-plugins.org/) if you want to read more about.
- ... [Configuration Validation](https://icinga.com/docs/icinga-2/latest/doc/11-cli-commands/#config-validation) if you’re stuck with configuration errors.
- ... [Troubleshooting](https://icinga.com/docs/icinga-2/latest/doc/15-troubleshooting/#check-fork-errors) chapter if you’re running into fork errors with systemd enabled distributions.
- ... [Distributed Monitoring](https://icinga.com/docs/icinga-2/latest/doc/06-distributed-monitoring/#distributed-monitoring) chapter if you want to set up a highly available and/or distributed Icinga monitoring environment.
- ... [Icinga DB Configuration](https://icinga.com/docs/icinga-db/latest/doc/03-Configuration/) if you want to read more about.
- ... [Icinga DB Web Configuration](https://icinga.com/docs/icinga-db-web/latest/doc/03-Configuration/) if you want to read more about.