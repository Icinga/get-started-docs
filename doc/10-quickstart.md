# Quickstart

Welcome to the Icinga Quickstart Guide! Whether you're new to system monitoring or an experienced admin, this guide will walk you through the essential steps to get Icinga up and running on your server. By the end of this guide, you'll be ready to monitor your network, servers, and applications, ensuring your infrastructure stays healthy and reliable.

Icinga consists of multiple components, each responsible for different aspects of monitoring and management. To keep things simple, we'll be installing everything on a single node in this guide. This setup will give you a complete Icinga environment in one place, perfect for getting started quickly.

### You're going to install:

- Icinga 2
- Icinga DB
- Icinga Web
- Icinga DB Web
- Icinga Director

### Requirements

A database is required to store the monitoring data collected by Icinga. For this guide, MySQL > 5.7 or MariaDB > 10.2 is required.

The commands listed below should be run with root permissions unless specified otherwise.


## Add Repository

Add the official Icinga Package Repository to your system.

{% include "doc/include/tab-repository.md" %}

## Install Packages

{% include "doc/include/tab-install-icinga.md" %}

## Set up API

The Icinga 2 API is required by other components to transmit commands, read data and deploy new Icinga configuration. Set up the API by running the API wizard:

```bash
icinga2 api setup
systemctl restart icinga2
```

The API setup wizard will guide you through the following steps:

* It enables the `api` feature
* It generates a Certification Authority (CA) and initial certificates, since Icinga always communicates through secured channels.
* It creates a new API user called `root` - with full permissions. Credentials are stored under `/etc/icinga2/conf.d/api-users.conf`

You will need the API user credentials later when setting up the Icinga web interface.

## Set up MySQL Databases

Icinga requires multiple database to store and manage different aspects of the monitoring environment.

### Create databases

Make sure you have sufficient permissions to create the databases, for example by connecting as `root`:

```bash
mysql -u root -p
```

Database for **Icinga Web**

```sql
mysql>
    CREATE DATABASE icingaweb2;
    CREATE USER 'icingaweb2'@'localhost' IDENTIFIED BY 'CHANGEME';
    GRANT ALL PRIVILEGES ON icingaweb2.* TO 'icingaweb2'@'localhost';
```

Database for **Director** (only necessary if you want want to use the director)

```sql
mysql>
    CREATE DATABASE director CHARACTER SET 'utf8';
    CREATE USER director@localhost IDENTIFIED BY 'CHANGEME';
    GRANT ALL ON director.* TO director@localhost;
```

Database for **Icinga DB**

```sql
mysql>
    CREATE DATABASE icingadb;
    CREATE USER 'icingadb'@'localhost' IDENTIFIED BY 'CHANGEME';
    GRANT ALL ON icingadb.* TO 'icingadb'@'localhost';
```

After creating the database, import the Icinga DB schema using the following command:

```bash
mysql -u root -p icingadb </usr/share/icingadb/schema/mysql/schema.sql
```

## Set up Icinga DB

Icinga DB is respnsible for storing the collected monitoring data. It uses a dedicated Redis®[\*](TRADEMARKS.md#redis) instance for caching.

### Start Redis® for Icinga DB

Start and enable Redis® for Icinga DB.

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

### Enable Icinga DB Feature

Ensure Icinga 2 sends the collected data to Icinga DB by enabling the `icingadb` feature:

```bash
icinga2 feature enable icingadb
systemctl restart icinga2
```


### Start Icinga DB

The Icinga DB configuration is stored under `/etc/icingadb/config.yml`. Make sure to adjust connection settings properly before starting Icinga DB. Start Icinga DB by executing the following command.

```bash
systemctl enable --now icingadb
```

## Set up Icinga Web

Next, we'll set up Icinga Web, the user-friendly interface for monitoring your infrastructure. Follow these steps to get the web interface up and running, so you can easily visualize and control your Icinga setup.

Depending on your operating systems, additional steps may be required for the web server:

{% include "doc/include/tab-install-icingaweb2.md" %}

### Prepare Web Setup

Icinga Web can be set up easily by using it's built in setup wizard. It will open automatically when you visit Icinga Web for the first time. By creating a setup token upfront, you ensure that you are authorized to to run the setup wizard. You will be asked for the token during the web setup.

To generate a token use the icingacli:

```bash 
icingacli setup token create
```

In case the output gets lost, you can display the token on the CLI at any time by running:

```bash
icingacli setup token show
```

### Start Web Setup

Open your browser and point it to your server's hostname, eg. `http://localhost/icingaweb2`. You will be lead to the setup wizard automatically.

!!! tip
    Use the same database name, user and password details created above when asked.

The setup wizard automatically detects and validates requirements. If anything is missing, use your package manager to install required packages and restart your web server.


!!! Info
    The API user credentials are auto-generated and stored in `/etc/icinga2/conf.d/api-users.conf`

### Continue with the **[Setup Wizard](11-websetup.md)**
