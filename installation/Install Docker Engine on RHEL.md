### Prerequisites

### OS requirements
To install Docker Engine, you need a maintained version of RHEL 7 or 8 on s390x (IBM Z). Archived versions aren’t supported or tested.

The overlay2 storage driver is recommended.

### Uninstall old versions
Older versions of Docker were called docker or docker-engine. If these are installed, uninstall them, along with associated dependencies. Also uninstall Podman and the associated dependencies if installed already.

     sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine \
                  podman \
                  runc
It’s OK if yum reports that none of these packages are installed.

The contents of /var/lib/docker/, including images, containers, volumes, and networks, are preserved. The Docker Engine package is now called docker-ce.

Installation methods
You can install Docker Engine in different ways, depending on your needs:

Most users set up Docker’s repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach.

Some users download the RPM package and install it manually and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.

In testing and development environments, some users choose to use automated convenience scripts to install Docker.

## Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### Set up the repository
Install the yum-utils package (which provides the yum-config-manager utility) and set up the stable repository.

     sudo yum install -y yum-utils
     sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
Optional: Enable the nightly or test repositories.

These repositories are included in the docker.repo file above but are disabled by default. You can enable them alongside the stable repository. The following command enables the nightly repository.

     sudo yum-config-manager --enable docker-ce-nightly
To enable the test channel, run the following command:

     sudo yum-config-manager --enable docker-ce-test
You can disable the nightly or test repository by running the yum-config-manager command with the --disable flag. To re-enable it, use the --enable flag. The following command disables the nightly repository.

     sudo yum-config-manager --disable docker-ce-nightly
Learn about nightly and test channels.

### Install Docker Engine
Install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:

     sudo yum install docker-ce docker-ce-cli containerd.io
If prompted to accept the GPG key, verify that the fingerprint matches 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

Got multiple Docker repositories?

If you have multiple Docker repositories enabled, installing or updating without specifying a version in the yum install or yum update command always installs the highest possible version, which may not be appropriate for your stability needs.

This command installs Docker, but it doesn’t start Docker. It also creates a docker group, however, it doesn’t add any users to the group by default.

To install a specific version of Docker Engine, list the available versions in the repo, then select and install:

a. List and sort the versions available in your repo. This example sorts results by version number, highest to lowest, and is truncated:

     yum list docker-ce --showduplicates | sort -r
The list returned depends on which repositories are enabled, and is specific to your version of RHEL (indicated by the .el8 suffix in this example).

b. Install a specific version by its fully qualified package name, which is the package name (docker-ce) plus the version string (2nd column) starting at the first colon (:), up to the first hyphen, separated by a hyphen (-). For example, docker-ce-20.10.7.

 sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
This command installs Docker, but it doesn’t start Docker. It also creates a docker group, however, it doesn’t add any users to the group by default.

### Start Docker.

     sudo systemctl start docker
Verify that Docker Engine is installed correctly by running the hello-world image.

     sudo docker run hello-world

### Start Docker.

     sudo systemctl start docker
Verify that Docker Engine is installed correctly by running the hello-world image.

     sudo docker run hello-world


### Uninstall Docker Engine
Uninstall the Docker Engine, CLI, and Containerd packages:

 sudo yum remove docker-ce docker-ce-cli containerd.io
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

     sudo rm -rf /var/lib/docker
     sudo rm -rf /var/lib/containerd
