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