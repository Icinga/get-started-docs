# Icinga DB

## Installing Icinga DB Package
Use your distribution’s package manager to install the icingadb package as follows:

```bash
apt install icingadb
```


## Setting up the Database

A MySQL (≥5.5), MariaDB (≥10.1), or PostgreSQL (≥9.6) database is required to run Icinga DB. Please follow the steps listed for your target database, which guide you through setting up the database and user and importing the schema.

### Setting up a MySQL (or MariaDB) Database

!!! warning 
    If you use a version of `MySQL < 5.7` or `MariaDB < 10.2`, the following server options must be set:


    ```bash
    innodb_file_format=barracuda
    innodb_file_per_table=1
    innodb_large_prefix=1
    ```
### Set up a MySQL database for Icinga DB:
Install MySQL 
```bash
apt install mysql-server
```

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

## Configuring Icinga DB

!!! info 

    Icinga DB installs its configuration file to `/etc/icingadb/config.yml`, pre-populating most of the settings for a local setup. Before running Icinga DB, adjust the Redis® and database credentials and, if necessary, the connection configuration.


## Running Icinga DB
The icingadb package automatically installs the necessary systemd unit files to run Icinga DB. Please run the following command to enable and start its service:

```bash
systemctl enable --now icingadb
```

## Installing Icinga DB Web
With Icinga 2, Redis®, Icinga DB and the database fully set up, it is now time to install Icinga DB Web, which connects to both Redis® and the database to display and work with the monitoring data.

**Open next step to [install Icinga Web](03-icingaweb.md)**