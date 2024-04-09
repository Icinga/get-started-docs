# Installing Icinga DB Web on Ubuntu

The recommended way to install Icinga DB Web is to use prebuilt packages from our official release repository. If the repository is not configured yet, please add it first before installing the package.

All packages we provide are signed with the following key.


## Adding Icinga Package Repository
Here’s how to add the official release repository:

```bash
apt update
apt -y install apt-transport-https wget gnupg

wget -O - https://packages.icinga.com/icinga.key | gpg --dearmor -o /usr/share/keyrings/icinga-archive-keyring.gpg

. /etc/os-release; if [ ! -z ${UBUNTU_CODENAME+x} ]; then DIST="${UBUNTU_CODENAME}"; else DIST="$(lsb_release -c| awk '{print $2}')"; fi; \
 echo "deb [signed-by=/usr/share/keyrings/icinga-archive-keyring.gpg] https://packages.icinga.com/ubuntu icinga-${DIST} main" > \
 /etc/apt/sources.list.d/${DIST}-icinga.list
 echo "deb-src [signed-by=/usr/share/keyrings/icinga-archive-keyring.gpg] https://packages.icinga.com/ubuntu icinga-${DIST} main" >> \
 /etc/apt/sources.list.d/${DIST}-icinga.list

apt update
```

## Installing the Package

Use your distribution’s package manager to install the icingadb-web package as follows:

```bash 
apt install icingadb-web
```

This concludes the installation. Now proceed with the configuration.


