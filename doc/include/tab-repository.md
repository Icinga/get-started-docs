=== "Ubuntu"

    ```bash
    apt update
    apt -y install apt-transport-https wget

    wget -O icinga-archive-keyring.deb "https://packages.icinga.com/icinga-archive-keyring_latest+ubuntu$(
     . /etc/os-release; echo "$VERSION_ID"
    ).deb"

    apt install ./icinga-archive-keyring.deb

    . /etc/os-release; if [ ! -z ${UBUNTU_CODENAME+x} ]; then DIST="${UBUNTU_CODENAME}"; else DIST="$(lsb_release -c| awk '{print $2}')"; fi; \
    echo "deb [signed-by=/usr/share/keyrings/icinga-archive-keyring.gpg] https://packages.icinga.com/ubuntu icinga-${DIST} main" > \
    /etc/apt/sources.list.d/${DIST}-icinga.list
    echo "deb-src [signed-by=/usr/share/keyrings/icinga-archive-keyring.gpg] https://packages.icinga.com/ubuntu icinga-${DIST} main" >> \
    /etc/apt/sources.list.d/${DIST}-icinga.list

    apt update
    ```

=== "Debian"

    ```bash
    apt update
    apt -y install apt-transport-https wget

    wget -O icinga-archive-keyring.deb "https://packages.icinga.com/icinga-archive-keyring_latest+debian$(
     . /etc/os-release; echo "$VERSION_ID"
    ).deb"

    apt install ./icinga-archive-keyring.deb

    DIST=$(awk -F"[)(]+" '/VERSION=/ {print $2}' /etc/os-release); \
    echo "deb [signed-by=/usr/share/keyrings/icinga-archive-keyring.gpg] https://packages.icinga.com/debian icinga-${DIST} main" > \
    /etc/apt/sources.list.d/${DIST}-icinga.list
    echo "deb-src [signed-by=/usr/share/keyrings/icinga-archive-keyring.gpg] https://packages.icinga.com/debian icinga-${DIST} main" >> \
    /etc/apt/sources.list.d/${DIST}-icinga.list

    apt update
    ```

    **Debian Backports Repository**

    This repository is required for Debian Stretch since Icinga v2.11.

    Debian Stretch:

    ```bash
    DIST=$(awk -F"[)(]+" '/VERSION=/ {print $2}' /etc/os-release); \
    echo "deb https://deb.debian.org/debian ${DIST}-backports main" > \
    /etc/apt/sources.list.d/${DIST}-backports.list

    apt update
    ```

=== "SLES"

    !!! Info
        A paid repository subscription is required for SLES repositories. Get more information on [icinga.com/subscription](https://icinga.com/subscription)

        Don't forget to fill in the username and password section with your credentials in the local .repo file.

    First add the official repositories:

    ```bash
    wget https://packages.icinga.com/subscription/sles/ICINGA-release.repo -O /etc/zypp/repos.d/ICINGA-release.repo
    zypper ref
    ```

    You need to additionally add the PackageHub repository to fulfill dependencies:


    ```bash
    source /etc/os-release

    SUSEConnect -p PackageHub/$VERSION_ID/x86_64
    SUSEConnect -p sle-module-web-scripting/$VERSION_ID/x86_64
    ```

=== "RHEL"

    !!! Info
        A paid repository subscription is required for RHEL repositories. Get more information on [icinga.com/subscription](https://icinga.com/subscription)

        Don't forget to fill in the username and password section with your credentials in the local .repo file.

    ```bash
    rpm --import https://packages.icinga.com/icinga.key
    curl https://packages.icinga.com/subscription/rhel/ICINGA-release.repo -o /etc/yum.repos.d/ICINGA-release.repo
    ```

    If you are using RHEL you need to additionally enable the codeready-builder repository before installing the EPEL rpm package.

    ```bash
    ARCH=$(/bin/arch)
    OSVER=$(. /etc/os-release; echo "${VERSION_ID%%.*}")

    subscription-manager repos --enable "codeready-builder-for-rhel-${OSVER}-${ARCH}-rpms"

    dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-${OSVER}.noarch.rpm
    ```
