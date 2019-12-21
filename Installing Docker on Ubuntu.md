# Installing Docker on Ubuntu

This article explains how you can install docker on various Linux distributions ( e.g  *Arch*, *Ubuntu*, *Cent OS*).

In this article, we are gonna install **Docker Engine - Community** for Ubuntu, I'm using a `64-bit` ***Ubuntu 18.04*** machine. The supported architectures are : `x86_64` (or `amd64`), `armhf`, `arm64`, `s390x` (IBM Z), and `ppc64le` (IBM Power) architectures. Also, the supported Ubuntu versions are : `16.04 LTS`, `18.04 LTS`, `18.10`, `19.04`.

### Getting rid of Old Versions

If you got any older version of Docker such as `docker`, `docker.io `, or `docker-engine` installed, uninstall them :

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

Don't worry all your docker stuff including images, containers, volumes , networks etc will not be deleted and can be still found under `/var/lib/docker`

### Preparing `apt` for Installation

Follow these steps to prepare `apt` package manager for the installation.

- Update package index

  ```bash
  $ sudo apt-get update
  ```

- Install essentials HTTPS packages for apt

  ```bash
  $ sudo apt-get install apt-transport-https gnupg-agent software-properties-common 
  ca-certificates curl
  ```

- Add the official GPG key for Docker

  ```bash
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  ```

  Wait for a second, you will get a `OK` response  on success. To verify that you now have the key with the fingerprint `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint.

  ```bash
  $ sudo apt-key fingerprint 0EBFCD88
  pub   rsa4096 2017-02-22 [SCEA]
        9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
  uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
  sub   rsa4096 2017-02-22 [S]
  ```

- Now to add docker apt repository

  ```bash
  $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  ```
  You will see the apt repositories being fetched as an output to this command. To verify look for something like `https://download.docker.com/linux/ubuntu bionic InRelease`
  
  > **Note**: The `lsb_release -cs` sub-command here returns the name of    your Ubuntu distribution, such as `bionic` in my case. You might also   need to change the `[arch=<architecture>] ` if you are using `armhf`,   `arm64`, `s390x` or `ppc64le` with the respective architecture name.

### Installing `Docker Engine` - Community Edition

Now, follow the steps below to install docker on your machine.

- Update package index

  ```bash
  $ sudo apt-get update
  ```

- Install the latest `Docker Engine - Community` and `containerd` or you can install a specific version 

  ( see next step for that ).

  ```bash
  $ sudo apt-get install docker-ce docker-ce-cli containerd.io
  ```

- You can find the list of available versions from `apt-cache` as below

  ```bash
  $ apt-cache madison docker-ce
   docker-ce | 5:19.03.3~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
   docker-ce | 5:19.03.2~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
   docker-ce | 5:19.03.1~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
   docker-ce | 5:19.03.0~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
  
  ```

  To install a specific version use the command below : 

  ```bash
  $ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
  ```

- To verify that Docker Engine - Community is installed correctly by running the `hello-world` image

  ```bash
  $ sudo docker run hello-world
  ```

### Some fixes to Run Docker commands it without `sudo` (optional)

The following steps sets up a docker user group adding users to which will allow them to run docker commands without `sudo` privileges required.

- Add the `docker` group if it doesn't already exist

  ```bash
  $ sudo groupadd docker
  ```

- Adding current user to docker group

  ```bash
  $ sudo gpasswd -a $USER docker
  ```

  You can change the `$USER` to match the preferred username.

- You need to restart the system in order to be these changes effect.