# Director Setup Walktrough


## Configure Icinga Director

Log in to your running Icinga Web setup with a privileged user and follow the steps below to configure the Icinga Director:

Create a new resource for the Icinga Director database via the `Configuration` > `Application` > `Resources`. 


![configuration](img/director/00-configuration.png)
![create-new-ressource](img/director/01-create-new-ressource.png)

Fill the fields with the credentials of the director database user and continue. Please make sure to configure 'utf8' as encoding.

![director-config](img/director/02-director-config.png)

Select the created database resource.

![refreshed-view](img/director/03-refreshed-view.png)

Create a new schema.

![config-schema](img/director/04-create-schema.png)

Fill the fields with the hostname of your master and the credentials of your API user.

![config-schema-filled](img/director/05-config-schema.png)

If the import was successful you get to following page.

![director-view](img/director/06-director-view.png)

Now you have to open `Icinga Director` > `Activity log` and deploy the changes.

![activity-log](img/director/07-activity-log.png)

If the deployment was successful you get the green check.

![deployed-changes](img/director/08-deployed-changes.png)