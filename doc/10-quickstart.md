# Quickstart

This is a simple and quick guide to try out Icinga 2 and is not intended as a production environment. It includes the following parts:

- Icinga 2
- Icinga DB
- Icinga Web
- Icinga DB Web
- Icinga Director

## Install Icinga

The following commands must be executed with root permissions unless noted otherwise.

### Add official Icinga Package Repository

{% include "include/tab-repository.md" %}

### Install Packages

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua

{% include "include/tab-install-icinga.md" %}

### Set up API
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua
```bash
icinga2 api setup
systemctl restart icinga2
```

!!! info

    This does:

    - enable the `api` feature
    - set up certificates
    - add the API user `root` with an auto-generated password to the configuration file `/etc/icinga2/conf.d/api-users.conf`

## Set up MySQL Databases

A MySQL database server is required.

!!! warning 
    If you use a version of `MySQL < 5.7` or `MariaDB < 10.2`, the following server options must be set:


    ```bash
    innodb_file_format=barracuda
    innodb_file_per_table=1
    innodb_large_prefix=1
    ```


**Create databases**

```bash
mysql -u root -p
```

DB for **Icinga DB**
```sql
mysql>
    CREATE DATABASE icingadb;
    CREATE USER 'icingadb'@'localhost' IDENTIFIED BY 'CHANGEME';
    GRANT ALL ON icingadb.* TO 'icingadb'@'localhost';
```


DB for **Icinga Web**

```sql
mysql>
    CREATE DATABASE icingaweb2;
    CREATE USER 'icingaweb2'@'localhost' IDENTIFIED BY 'CHANGEME';
    GRANT ALL PRIVILEGES ON icingaweb2.* TO 'icingaweb2'@'localhost';
```


DB for **Director** (only necessary if you want want to use the director)

```sql
mysql>
    CREATE DATABASE director CHARACTER SET 'utf8';
    CREATE USER director@localhost IDENTIFIED BY 'CHANGEME';
    GRANT ALL ON director.* TO director@localhost;
```

After creating the database, import the Icinga DB schema using the following command:

```bash
mysql -u root -p icingadb </usr/share/icingadb/schema/mysql/schema.sql
```

## Set up Icinga DB

#### Run Redis® for Icinga DB

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


### Start Icinga DB

!!! info 

    Icinga DB installs its configuration file to `/etc/icingadb/config.yml`, pre-populating most of the settings for a local setup. Before running Icinga DB, adjust the Redis® and database credentials and, if necessary, the connection configuration.

```bash
systemctl enable --now icingadb
```

## Set up Icinga Web 2

{% include "include/tab-install-icingaweb2.md" %}

### Prepare Web Setup

You can set up Icinga Web 2 quickly and easily with the Icinga Web 2 setup wizard which is available the first time you visit Icinga Web 2 in your browser. When using the web setup you are required to authenticate using a token. In order to generate a token use the icingacli:

```bash 
icingacli setup token create
```

In case you do not remember the token you can show it using the icingacli:

```bash
icingacli setup token show
```

This concludes the installation. Now proceed with the configuration.

### Start Web Setup

Finally visit Icinga Web 2 in your browser to access the setup wizard and complete the installation: `/icingaweb2/setup`.

!!! tip
    Use the same database, user and password details created above when asked.

The setup wizard automatically detects the required packages. In case one of them is missing, e.g. a PHP module, please install the package, restart your webserver and reload the setup page.


!!! Info
    The API-user and it's password are written in the file `/etc/icinga2/conf.d/api-users.conf`

### Continue with <u>**[Setup Wizard](websetup.md)**</u>
