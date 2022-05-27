# Docker

## What is docker

- Splits your base OS into **isolated** containers that run your code
- Containers are **portable**, i.e. designed to get code to and from your computer
- Docker **builds** containers
- Docker **registry** is used to share container images

- Unlike with VMs containers do not include OS

- Container
    - a self-contained sealed unit of software, which contains everything required to run a service
    - contains everything required to run the code
        - Code
        - Configs
        - Processes
        - Networking
        - Dependencues
        - "Just enough of OS"

Docker creates a copy of Linux OS services (networking, storage, interprocess communication) within Linux kernel for each container.

Docker includes
- Docker client program - Docker
- Docker server program that manages a Linux system
- Docker program that builds containers from code

```Bash
# Run a Docker Container that introduces docker and exits
docker run hello-world
# Get help for specific commmand
docker COMMAND --help
# Get Docker info
docker info
```

- **boot2docker** - legacy/deprecated CLI tool, remove and use Docker Desktop
- **Docker Desktop**
- Docker ID - account you will use to push/pull private images

- Docker on Windows requires Hyper-V enabled to create containers
- [Installing Docker on Ubuntu](https://docs.docker.com/desktop/linux/install/ubuntu/)

```Bash
# Add Docker org signing key as a trusted key on your system
curl -fsDL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# Post installation steps
# Manage docker as a non root user
sudo grroupadd docker
sudo usermod -aG docker $USER
# Restart terminal
```

## Using Docker

## Docker flow - Images to containers

### Dockerfiles

- Docker Images - just enough OS to run app, fixed/configured starting point

```Bash
docker ps -a
# List images
docker images
# Returns REPOSITORY, TAG, IMAGE ID, CREATED, SIZE
```

- Docker container - running instance of an image

```Bash
# -ti = terminal interactive, to have full terminal in the image to run full-fledged shell with auto-completion etc.
# run container with latest version of Ubuntu docker -arguments IMAGE:TAG COMMAND
# TAG is optional, latest is used by default
docker run -ti ubuntu:latest bash
# check Ubuntu version
cat /etc/lsb-release
# to exit - exit or CTRL+D
exit
```

- Look at running images

```Bash
# format string can be defined to produce vertical output --format $FORMAT
docker ps
```

### Building Dockerfiles

- Stopped container, files/changes preserved

```Bash
# list containers including stopped ones
docker ps -a
# look at most recently exited container
docker ps -l
```

- You can save stopped container and create new image out of it using docker commit command

```Bash
# Stop container, use docker ps -l to get its ID
docker commit CONTAINER_ID
# Command above outputs image ID (very long number)
# Give name to image using tag command
docker tag IMAGE_ID TAG
# You can then use docker run IMAGE_TAG to run the image
# Alternattively you can assign new image name on commit
docker commit IMAGE_MAME NEW_IMAGE_NAME
```

### Run processes in container