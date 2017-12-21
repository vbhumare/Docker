# Docker Installation and Configuration 

Docker is an application that makes it simple and easy to run application processes in a container, which are like virtual machines, only more portable, more resource-friendly, and more dependent on the host operating system. 


# Prerequisites
To follow this tutorial, you will need the following:
  - 64-bit Ubuntu 14.04 server
  - Non-root user with sudo privileges

```sh
Note: Docker requires a 64-bit version of Ubuntu as well as a kernel version equal to or greater than 3.10.
```
# Step 1 — Installing Docker
First install linux-image-extra package, which allow docker to use the aufs storage driver.
```sh
$ sudo apt-get update
$ sudo apt-get install \
    linux-image-extra-$(uname -r) \
    linux-image-extra-virtual
```
Install Docker CE using the repository
```sh
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
Add Docker’s official GPG key:
```sh
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Update the apt package index.
```sh
$ sudo apt-get update
```

Install the latest version of Docker CE.
```sh
$ sudo apt-get install docker-ce
```
Check available Docker CE version
```sh
apt-cache madison docker-ce
 docker-ce | 17.09.1~ce-0~ubuntu | https://download.docker.com/linux/ubuntu/ trusty/stable amd64 Packages
 docker-ce | 17.09.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu/ trusty/stable amd64 Packages
```
install the specific version of Docker CE.
```sh
$ sudo apt-get install docker-ce=<VERSION>
```
Verify Docker installtion and version
```sh
$ docker --version
Docker version 17.09.1-ce, build 19e2cf6
```

If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:
```sh
$ sudo usermod -aG docker ${USER}
```
# Step 2 — Using the Docker Command
With Docker installed and working, now's the time to become familiar with the command line utility. Using docker consists of passing it a chain of options and commands followed by arguments. The syntax takes this form:
```sh
$ docker [option] [command] [arguments]
```

To view all available subcommands type

```sh
$ docker 
```
complete list of available subcommands includes:
```sh
Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

# Step 3 — Working with Docker Images

Docker containers are run from Docker images. By default, it pulls these images from Docker Hub, a Docker registry managed by Docker.

To check whether you can access and download images from Docker Hub, type:
```sh
$ docker run hello-world
```
The output should look similar to the following:
```sh
Output:
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete
Digest: sha256:445b2fe9afea8b4aa0b2f27fe49dd6ad130dfe7a8fd0832be5de99625dad47cd
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

You can search for images available on Docker Hub by using the docker command with the search subcommand. For example, to search for the centos image, type:
```sh
$ docker search <image_name>
$ docker search centos
```
You can download image from docker hub by using pull commnad.
```sh
$ docker pull image_name
$ docker pull ubuntu:14.04
```
To see the images that have been downloaded to your server, type:

```sh
$ docker images
```
The output should look similar to the following:
```sh
REPOSITORY   TAG    	 IMAGE ID      CREATED        SIZE
ubuntu       14.04  	 67759a80360c  6 days ago     221MB
hello-world  latest 	 f2a91732366c  4 weeks ago    1.85kB
```
# Step 4 — Running a Docker Container

After an image has been downloaded, you may then run a container using the downloaded image with the run subcommand

```sh
$ docker run -ubuntu:14.04
```
let's run a container using the latest image of Ubuntu. The combination of the -i and -t switches gives you interactive shell access into the container:
```sh
$ docker run -it ubuntu:14.04
root@b13f8c390f22:/# hostname
b13f8c390f22
root@b13f8c390f22:/#
```

Let's install application in container.
```sh
$ sudo apt-get update
$ sudo apt-get install nginx
```
Committing Changes in a Container to a Docker Image
```sh
docker commit -m "Installed Nginx" -a "Vilas Bhumare" b13f8c390f22 ubuntu/nginx
```
After that operation has completed, listing the Docker images now on your server should show the new image.

```sh
$ docker images
REPOSITORY     TAG     IMAGE ID            CREATED             SIZE
ubuntu/nginx   latest  acd6e9d0e944        12 seconds ago      263MB
ubuntu         14.04   67759a80360c        6 days ago          221MB
hello-world    latest  f2a91732366c        4 weeks ago         1.85kB
```

# Step 5 — Listing Docker Containers

You can list all runnig containers by using below commnad.
```sh
$ docker ps 
```
You will see output similar to the following:
```sh 
CONTAINER ID  IMAGE           COMMAND      CREATED             STATUS              PORTS  NAMES
b13f8c390f22  ubuntu:14.04    "/bin/bash"  15 minutes ago      Up 15 minutes              zealous_thompson
```
You can check all containers (active and inactive), type
```sh
$ docker ps -a
```
Use below command to stop and start container
```sh
$ docker stop <container_id>
$ docker start <container_id>
``
