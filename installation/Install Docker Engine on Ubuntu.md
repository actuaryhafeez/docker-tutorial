### Prerequisites
### OS requirements
To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:

 * Ubuntu Impish 21.10
 * Ubuntu Hirsute 21.04
 * Ubuntu Focal 20.04 (LTS)
 * Ubuntu Bionic 18.04 (LTS)
Docker Engine is supported on x86_64 (or amd64), armhf, arm64, and s390x architectures.

Ubuntu 16.04 LTS “Xenial Xerus” end-of-life

Ubuntu Linux 16.04 LTS reached the end of its five-year LTS window on April 30th 2021 and is no longer supported. Docker no longer releases packages for this distribution (including patch- and security releases). Users running Docker on Ubuntu 16.04 are recommended to update their system to a currently supported LTS version of Ubuntu.

### Uninstall old versions
Older versions of Docker were called docker, docker.io, or docker-engine. If these are installed, uninstall them:

     sudo apt-get remove docker docker-engine docker.io containerd runc
It’s OK if apt-get reports that none of these packages are installed.

The contents of /var/lib/docker/, including images, containers, volumes, and networks, are preserved. If you do not need to save your existing data, and want to start with a clean installation, refer to the uninstall Docker Engine section at the bottom of this page.

Supported storage drivers
Docker Engine on Ubuntu supports overlay2, aufs and btrfs storage drivers.

Docker Engine uses the overlay2 storage driver by default. If you need to use aufs instead, you need to configure it manually. See use the AUFS storage driver

Installation methods
You can install Docker Engine in different ways, depending on your needs:

Most users set up Docker’s repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach.

Some users download the DEB package and install it manually and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.

In testing and development environments, some users choose to use automated convenience scripts to install Docker.

## Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### Set up the repository
Update the apt package index and install packages to allow apt to use a repository over HTTPS:

     sudo apt-get update
     sudo apt-get install ca-certificates curl gnupg lsb-release
### Add Docker’s official GPG key:

     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below. Learn about nightly and test channels.

     echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
## Install Docker Engine
Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:

     sudo apt-get update
     sudo apt-get install docker-ce docker-ce-cli containerd.io
Got multiple Docker repositories?

If you have multiple Docker repositories enabled, installing or updating without specifying a version in the apt-get install or apt-get update command always installs the highest possible version, which may not be appropriate for your stability needs.

To install a specific version of Docker Engine, list the available versions in the repo, then select and install:

a. List the versions available in your repo:

     apt-cache madison docker-ce
b. Install a specific version using the version string from the second column, for example, 5:18.09.1~3-0~ubuntu-xenial.

     sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
Verify that Docker Engine is installed correctly by running the hello-world image.

     sudo docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

Docker Engine is installed and running. The docker group is created but no users are added to it. You need to use sudo to run Docker commands. Continue to Linux postinstall to allow non-privileged users to run Docker commands and for other optional configuration steps.

### Uninstall Docker Engine
Uninstall the Docker Engine, CLI, and Containerd packages:

 sudo apt-get purge docker-ce docker-ce-cli containerd.io
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

     sudo rm -rf /var/lib/docker
     sudo rm -rf /var/lib/containerd
