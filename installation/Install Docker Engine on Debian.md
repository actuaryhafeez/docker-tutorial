## Prerequisites
## OS requirements
To install Docker Engine, you need the 64-bit version of one of these Debian or Raspbian versions:

 * Debian Bullseye 11 (stable)
 *Debian Buster 10 (oldstable)
 * Raspbian Bullseye 11 (stable)
 * Raspbian Buster 10 (oldstable)

Docker Engine is supported on x86_64 (or amd64), armhf, and arm64 architectures.

## Uninstall old versions
Older versions of Docker were called docker, docker.io, or docker-engine. If these are installed, uninstall them:

     sudo apt-get remove docker docker-engine docker.io containerd runc
It’s OK if apt-get reports that none of these packages are installed.

The contents of /var/lib/docker/, including images, containers, volumes, and networks, are preserved. The Docker Engine package is now called docker-ce.

### Installation methods
You can install Docker Engine in different ways, depending on your needs:

Most users set up Docker’s repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach, except for Raspbian.

Some users download the DEB package and install it manually and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.

In testing and development environments, some users choose to use automated convenience scripts to install Docker. This is currently the only approach for Raspbian.

Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

Raspbian users cannot use this method!

For Raspbian, installing using the repository is not yet supported. You must instead use the convenience script.

## Set up the repository
Update the apt package index and install packages to allow apt to use a repository over HTTPS:

     sudo apt-get update
     sudo apt-get install ca-certificates curl gnupg lsb-release

## Add Docker’s official GPG key:

     curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below. Learn about nightly and test channels.

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Install Docker Engine
This procedure works for Debian on x86_64 / amd64, armhf, arm64, and Raspbian.

Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:

     sudo apt-get update
     sudo apt-get install docker-ce docker-ce-cli containerd.io
Got multiple Docker repositories?

If you have multiple Docker repositories enabled, installing or updating without specifying a version in the apt-get install or apt-get update command always installs the highest possible version, which may not be appropriate for your stability needs.

Receiving a GPG error when running apt-get update?

Your default umask may not be set correctly, causing the public key file for the repo to not be detected. Run the following command and then try to update your repo again: sudo chmod a+r /usr/share/keyrings/docker-archive-keyring.gpg.

To install a specific version of Docker Engine, list the available versions in the repo, then select and install:

a. List the versions available in your repo:

     apt-cache madison docker-ce
b. Install a specific version using the version string from the second column, for example, 5:18.09.1~3-0~debian-stretch .

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
To upgrade Docker Engine, first run sudo apt-get update, then follow the installation instructions, choosing the new version you want to install.
