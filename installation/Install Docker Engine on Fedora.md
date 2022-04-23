## Prerequisites
## OS requirements
To install Docker Engine, you need the 64-bit version of one of these Fedora versions:

 * Fedora 34
 * Fedora 35

## Uninstall old versions
Older versions of Docker were called docker or docker-engine. If these are installed, uninstall them, along with associated dependencies.

     sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
It’s OK if dnf reports that none of these packages are installed.

The contents of /var/lib/docker/, including images, containers, volumes, and networks, are preserved. The Docker Engine package is now called docker-ce.

## Installation methods
You can install Docker Engine in different ways, depending on your needs:

Most users set up Docker’s repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach.

Some users download the RPM package and install it manually and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.

In testing and development environments, some users choose to use automated convenience scripts to install Docker.

## Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### Set up the repository
Install the dnf-plugins-core package (which provides the commands to manage your DNF repositories) and set up the stable repository.

     sudo dnf -y install dnf-plugins-core
     sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
Optional: Enable the nightly or test repositories.

These repositories are included in the docker.repo file above but are disabled by default. You can enable them alongside the stable repository. The following command enables the nightly repository.

     sudo dnf config-manager --set-enabled docker-ce-nightly
To enable the test channel, run the following command:

     sudo dnf config-manager --set-enabled docker-ce-test
You can disable the nightly or test repository by running the dnf config-manager command with the --set-disabled flag. To re-enable it, use the --set-enabled flag. The following command disables the nightly repository.

    sudo dnf config-manager --set-disabled docker-ce-nightly
Learn about nightly and test channels.

### Install Docker Engine
Install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:

     sudo dnf install docker-ce docker-ce-cli containerd.io
If prompted to accept the GPG key, verify that the fingerprint matches 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

Got multiple Docker repositories?

If you have multiple Docker repositories enabled, installing or updating without specifying a version in the dnf install or dnf update command always installs the highest possible version, which may not be appropriate for your stability needs.

This command installs Docker, but it doesn’t start Docker. It also creates a docker group, however, it doesn’t add any users to the group by default.

To install a specific version of Docker Engine, list the available versions in the repo, then select and install:

a. List and sort the versions available in your repo. This example sorts results by version number, highest to lowest, and is truncated:

     dnf list docker-ce  --showduplicates | sort -r
The list returned depends on which repositories are enabled, and is specific to your version of Fedora (indicated by the .fc28 suffix in this example).

b. Install a specific version by its fully qualified package name, which is the package name (docker-ce) plus the version string (2nd column) up to the first hyphen, separated by a hyphen (-), for example, docker-ce-3:18.09.1.

     sudo dnf -y install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
This command installs Docker, but it doesn’t start Docker. It also creates a docker group, however, it doesn’t add any users to the group by default.

### Start Docker.

     sudo systemctl start docker
Verify that Docker Engine is installed correctly by running the hello-world image.

     sudo docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

This installs and runs Docker Engine. Use sudo to run Docker commands. Continue to Linux postinstall to allow non-privileged users to run Docker commands and for other optional configuration steps.

### Upgrade Docker Engine
To upgrade Docker Engine, follow the installation instructions, choosing the new version you want to install.

Install from a package
If you cannot use Docker’s repository to install Docker, you can download the .rpm file for your release and install it manually. You need to download a new file each time you want to upgrade Docker Engine.

Go to https://download.docker.com/linux/fedora/ and choose your version of Fedora. Then browse to x86_64/stable/Packages/ and download the .rpm file for the Docker version you want to install.

Note

To install a nightly or test (pre-release) package, change the word stable in the above URL to nightly or test. Learn about nightly and test channels.

Install Docker Engine, changing the path below to the path where you downloaded the Docker package.

 sudo dnf -y install /path/to/package.rpm
Docker is installed but not started. The docker group is created, but no users are added to the group.

### Start Docker.

     sudo systemctl start docker
Verify that Docker Engine is installed correctly by running the hello-world image.

     sudo docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

This installs and runs Docker Engine. Use sudo to run Docker commands. Continue to Post-installation steps for Linux to allow non-privileged users to run Docker commands and for other optional configuration steps.

Upgrade Docker Engine
To upgrade Docker Engine, download the newer package file and repeat the installation procedure, using dnf -y upgrade instead of dnf -y install, and point to the new file.

Install using the convenience script
Docker provides a convenience script at get.docker.com to install Docker into development environments quickly and non-interactively. The convenience script is not recommended for production environments, but can be used as an example to create a provisioning script that is tailored to your needs. Also refer to the install using the repository steps to learn about installation steps to install using the package repository. The source code for the script is open source, and can be found in the docker-install repository on GitHub.

Always examine scripts downloaded from the internet before running them locally. Before installing, make yourself familiar with potential risks and limitations of the convenience script:

The script requires root or sudo privileges to run.
The script attempts to detect your Linux distribution and version and configure your package management system for you, and does not allow you to customize most installation parameters.
The script installs dependencies and recommendations without asking for confirmation. This may install a large number of packages, depending on the current configuration of your host machine.
By default, the script installs the latest stable release of Docker, containerd, and runc. When using this script to provision a machine, this may result in unexpected major version upgrades of Docker. Always test (major) upgrades in a test environment before deploying to your production systems.
The script is not designed to upgrade an existing Docker installation. When using the script to update an existing installation, dependencies may not be updated to the expected version, causing outdated versions to be used.
Tip: preview script steps before running

You can run the script with the DRY_RUN=1 option to learn what steps the script will execute during installation:

 curl -fsSL https://get.docker.com -o get-docker.sh
 DRY_RUN=1 sh ./get-docker.sh
This example downloads the script from get.docker.com and runs it to install the latest stable release of Docker on Linux:


### Uninstall Docker Engine
Uninstall the Docker Engine, CLI, and Containerd packages:

     sudo dnf remove docker-ce docker-ce-cli containerd.io
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

     sudo rm -rf /var/lib/docker
 sudo rm -rf /var/lib/containerd
