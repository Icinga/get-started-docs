# Follow-Ups after Installing Icinga

**Backup**

If you want to save your installation, ensure to include the following in your backups:

- Configuration files in `/etc/icinga2`
- Certificate files in `/var/lib/icinga2/ca` (Master CA key pair) and `/var/lib/icinga2/certs` (node certificates)
- Runtime files in `/var/lib/icinga2`

**Reference**

After you successfully installed Icinga with all its necessary components there are some more things you could do:


- [Service Monitoring](https://icinga.com/docs/icinga-2/latest/doc/05-service-monitoring/#service-monitoring-plugins) chapter for details about how to integrate additional check plugins into your Icinga 2 setup.
- [API](https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#icinga2-api-setup) chapter for details about how to set up and configure the Icinga 2 API.
- [Monitoring Plugins Project](https://www.monitoring-plugins.org/) if you want to read more about different plugins.
- [Configuration Validation](https://icinga.com/docs/icinga-2/latest/doc/11-cli-commands/#config-validation) if you’re stuck with configuration errors.
- [Troubleshooting](https://icinga.com/docs/icinga-2/latest/doc/15-troubleshooting/#check-fork-errors) chapter if you’re running into fork errors with systemd enabled distributions.
- [Distributed Monitoring](https://icinga.com/docs/icinga-2/latest/doc/06-distributed-monitoring/#distributed-monitoring) chapter if you want to set up a highly available and/or distributed Icinga monitoring environment.
- [Icinga DB Configuration](https://icinga.com/docs/icinga-db/latest/doc/03-Configuration/) if you want more informations about the configurations for Icinga DB.
- [Icinga DB Web Configuration](https://icinga.com/docs/icinga-db-web/latest/doc/03-Configuration/) if you want more informations about the configurations for Icinga DB Web.
- [Monitoring Basics](https://icinga.com/docs/icinga-2/latest/doc/03-monitoring-basics/) for an overview of all basic monitoring concepts you need to know to run Icinga 2