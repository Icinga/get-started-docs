# Installing Icinga Director on Debian
## Installing the Package

Use your distribution’s package manager to install the icinga-director package as follows:

```bash
apt install icinga-director
```

## Setting up MySQL or MariaDB Database

!!! Warn
    Warning Make sure to replace `CHANGEME` with a secure password.

```sql
# mysql -u root -p 

mysql> CREATE DATABASE director CHARACTER SET 'utf8';
mysql> CREATE USER director@localhost IDENTIFIED BY 'CHANGEME';
mysql> GRANT ALL ON director.* TO director@localhost;
```

## Configuring Icinga Director
Log in to your running Icinga Web setup with a privileged user and follow the steps below to configure Icinga Director:

1. Create a new resource for the Icinga Director database via the Configuration → Application → Resources menu. Please make sure that you configure utf8 as encoding.

2. Select Icinga Director directly from the main menu and you will be taken to the kickstart wizard. Follow the instructions and you are done!

**Open next step to [show follow-ups](../06-followups.md)**