# Web Setup Walktrough
### Steps of Web Setup

**1. Welcome**

Here is the output of the command `icingacli setup token show` requiered.

![Welcome](img/web/00-welcome-to-webconfiguration.png)

**2. Modules**

If you already installed the Director you can choose it, no other changes are needed.

![Modules](img/web/01-choose-modules.png)

**3. Requirements**

There should be all avaible, apart from 'PHP Module: Imagick', but that's expected.

![Requirements](img/web/02-requirements.png)

**4. Configuration**


**4.1 Authentication**

Choose the Authentication Type : Database

![Authentication-Type](img/web/03-authentication-type-database.png)

**4.1 Database Resource**

At this point is the 'icingaweb2' Database requiered:

- Resource Name: icingaweb_db
- Database Type: MySQL
- Host: localhost
- Database Name: icingaweb2
- Username: icingaweb2
- Password: CHANGEME

!!! Info
    The Validation checks just the syntax.

![Database-Ressource-Icinga-Web](img/web/04-icinga-web-database.png)

**4.2 Authentication Backend**

- Backend Name: icingaweb2

![Authentication-Backend](img/web/05-authentication-backend.png)

**4.3 Aministration**

Create a administrative account, this is also your login for icingaweb, f.e. :

- Username: icingaadmin
- Passowrd: icingaweb
- Repeat Passowrd: icingaweb

![Create-Admin](img/web/06-create-admin-account.png)

**4.4 Application Configuration**

The defaults don't need to be change.

![Application-Configuration](img/web/07-application-configuration.png)

**4.5 Check Configurations**

![Overview](img/web/08-configurration-overview.png)

**5. Configuration of Icinga DB Web**

![Icinga-DB-Web-Configuration](img/web/09-icinga-db-web-configuration.png)

**5.1 Icinga DB Resource**

Here is the Icnga DB Database requiered:

- Database Type: MySQL 
- Host: localhost
- Database Name: icingadb
- Username: icingadb
- Password: CHANGEME

![Icinga-DB-Database](img/web/10-icinga-db-database.png
)
**5.2 Icinga DB Redis**

Set following field of the Primary Icinga Master is
- Redis Host: localhost

![Redis-Configuration](img/web/11-redis-configuration.png)
![Redis-Validation](img/web/12-redis-validation.png)


**5.3 Icinga 2 API**

The credentials of the API-user are in the file `/etc/icinga2/conf.d/api-users.conf`

- Host: localhost
- Port: 5665
- API Username: root
- API Password: [password in the file]

![Icinga-API-Configuration](img/web/13-icinga-api-configuration.png)

**5.4 Check Configurations**

![Overview](img/web/14-configuration-overview-2.png)

![Success-View](img/web/15-success-view.png)
![Login](img/web/16-login-page.png)
![Login-Filled](img/web/17-admin-login.png)