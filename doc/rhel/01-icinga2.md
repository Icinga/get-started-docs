# Install Icinga 2 on RHEL

## Add Icinga Package Repository

First add the official repositories:

!!! Info
    A paid repository subscription is required for RHEL repositories. Get more information on icinga.com/subscription

    Don’t forget to fill in the username and password section with your credentials in the local .repo file.

```bash
rpm --import https://packages.icinga.com/icinga.key
wget https://packages.icinga.com/subscription/rhel/ICINGA-release.repo -O /etc/yum.repos.d/ICINGA-release.repo
```

If you are using RHEL you need to additionally enable the codeready-builder repository before installing the EPEL rpm package.

=== "RHEL 8 or Later"
    ```bash
    ARCH=$(/bin/arch)
    OSVER=$(. /etc/os-release; echo "${VERSION_ID%%.*}")

    subscription-manager repos --enable "codeready-builder-for-rhel-${OSVER}-${ARCH}-rpms"

    dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-${OSVER}.noarch.rpm
    ```
=== "RHEL 7"
    ```bash
    subscription-manager repos --enable rhel-7-server-optional-rpms

    yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    ```

## Install Icinga 2

You can install Icinga 2 by using your distribution’s package manager to install the `icinga2` package. The following commands must be executed with root permissions unless noted otherwise.

!!! Tip
    If you have SELinux enabled, the package `icinga2-selinux` is also required.



=== "RHEL 8 or Later"
    ```bash
    dnf install icinga2
    systemctl enable icinga2
    systemctl start icinga2
    ```

=== "RHEL 7"
    ```bash
    yum install icinga2
    systemctl enable icinga2
    systemctl start icinga2
    ```

### Systemd Service

The majority of supported distributions use systemd. The Icinga 2 packages automatically install the necessary systemd unit files.

If you’re stuck with configuration errors, you can manually invoke the configuration validation.
    
```bash
icinga2 daemon -C
```

!!! tip

    If you are running into fork errors with systemd enabled distributions, please check the [troubleshooting chapter](https://icinga.com/docs/icinga-2/latest/doc/15-troubleshooting/#check-fork-errors).


## Set up Check Plugins

Without plugins Icinga 2 does not know how to check external services.

Depending on which directory your plugins are installed into you may need to update the global PluginDir constant in your Icinga 2 configuration. This constant is used by the check command definitions contained in the Icinga Template Library to determine where to find the plugin binaries.

The packages for RHEL depend on other packages which are distributed as part of the EPEL repository.


=== "RHEL 8 or Later"
    ```bash
    dnf install nagios-plugins-all
    ```

=== "RHEL 7"
    ```bash
    yum install nagios-plugins-all
    ```


## Set up Icinga 2 API

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

## Set up Icinga DB

Icinga DB is a set of components for publishing, synchronizing and visualizing monitoring data in the Icinga ecosystem, consisting of:

- Icinga 2 with its `icingadb` feature enabled
- [Icinga DB daemon](https://icinga.com/docs/icinga-db/latest/doc/01-About/)
- Icinga Web with the [Icinga DB Web](https://icinga.com/docs/icinga-db-web/latest/) module enabled

!!! info

    Setting up Icinga 2’s Icinga DB feature is only required for Icinga 2 master nodes or single-node setups.

### Set up Redis® Server

!!! info

    This guide sets up the icingadb-redis package provided by Icinga, which ships a current Redis® Server version and is preconfigured for the Icinga DB components. Using own Redis® server setups is supported as long as the version is from 6.2.

!!! tip

    Although the Redis® server can run anywhere in an Icinga environment, we recommend to install it where the corresponding Icinga 2 node is running to keep latency between the components low.

#### Install Icinga DB Redis® Package

Use your distribution’s package manager to install the icingadb-redis package as follows:


=== "RHEL 8 or Later"
    ```bash
    dnf install icingadb-redis
    ```

=== "RHEL 7"
    ```bash
    yum install icingadb-redis
    ```

#### Run Icinga DB Redis®

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


#### Enable Remote Redis Connections

By default, `icingadb-redis` only listens on `127.0.0.1`. If Icinga Web or Icinga 2 is running on another node, remote access to the Redis® server must be allowed. This requires the following directives to be set in the `/etc/icingadb-redis/icingadb-redis.conf` configuration file:

- Set `protected-mode` to `no`, i.e. `protected-mode no`
- Set `bind` to the desired binding interface or bind all interfaces, e.g. `bind 0.0.0.0`

!!! warning

    By default, Redis® has no authentication preventing others from accessing it. When opening Redis® to an external interface, make sure to set a password, set up appropriate firewall rules, or configure TLS with certificate authentication on Redis® and its consumers, i.e. Icinga 2, Icinga DB and Icinga Web.

Restart Icinga DB Redis® for these changes to take effect:

```bash
systemctl restart icingadb-redis
```


### Enable Icinga DB Feature

Icinga 2 installs the feature configuration file to `/etc/icinga2/features-available/icingadb.conf`, pre-configured for a local setup. Update this file in case Redis® is running on a different host or to set credentials.

To enable the icingadb feature use the following command:

```bash
icinga2 feature enable icingadb
```

Restart Icinga 2 for these changes to take effect:

```bash
systemctl restart icinga2
```

### Install Icinga DB Daemon

After installing Icinga 2, setting up a Redis® server and enabling the `icingadb` feature, the Icinga DB daemon that synchronizes monitoring data between the Redis® server and a database is now set up.

!!! tip

    Although the Icinga DB daemon can run anywhere in an Icinga environment, we recommend to install it where the corresponding Icinga 2 node and Redis® server is running to keep latency between the components low.

The Icinga DB daemon package is also included in the Icinga repository, and since it is already set up, you have completed the instructions here and can proceed to install the Icinga DB daemon on RHEL, which will also guide you through the setup of the database and Icinga DB Web.

**Open next step to [install Icinga DB daemon](02-icingadb.md)**

## Backup

Ensure to include the following in your backups:

- Configuration files in `/etc/icinga2`
- Certificate files in `/var/lib/icinga2/ca` (Master CA key pair) and `/var/lib/icinga2/certs` (node certificates)
- Runtime files in `/var/lib/icinga2`
