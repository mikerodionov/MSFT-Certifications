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
sudo groupadd docker
sudo usermod -aG docker $USER
# Restart terminal
```
```Bash
# If you get error below
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
# Run this
sudo chmod 666 /var/run/docker.sock
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

- **docker run**
    - starts a container by giving it a name and starting main process to run it
    - containers stop when their process stops
    - containers have names, if name is not provided it will be auto generated
- **docker attach**
    - detached containers
    - interactive containers
    - detach with CRTL+P, CTRL+Q
    - docker ps
    - docker attach container_name
- **docker exec**
    - starts another process in an existing container
    - useful for debugging and administration
    - can't add ports, volumes, etc.

```Bash
# --rm - run something in a container but not keep the container afterwards
# -ti - terminal interactive
# Command below will start a container and exit after command execution is done
docker run --rm -ti ubuntu sleep 5
# Running multiple commands after start
docker run -ti ubuntu bash -c "sleep 3; echo all done"
# Detached container - start container and let it go, -d = detached (start and leave it running in background)
docker run -d -ti ubuntu bash
# get name with docker ps, then connect using docker attach
docker attach container_name
# CTRL+P, CTRL+Q to detach from container (exit) but keep it running
# Run bash in existing container
docker exec -ti container_name bash
```

### Manage containers

- docker logs
    - keep the output of containers
    - view with *docker logs container_name*
    - don't let the output get too large
- stopoing and removing containers
    - docker kill container_name - stops container
    - docker rm container_name - deletes container
- resource constraints
    - memory limits - docker run --memory
    - CPU limits - docker run --cpu-shares or docker run --cpu-quota
- recommendations
    - don't let your containers fethch dependencies when they start - make containers to include dependencies
    - don't leave important things in unnamed stopped containers


```Bash
# Run a terminal to look at password file
docker run --name example -d ubuntu bash -c "cat /etc/passwd"
# If you pass in command with typo you can see an error using docker logs
docker logs container_name
# Stop container
docker kill container_name
# Check last exited container
docker ps -l
# Delete container
docker rm container_name
# Delete all containers
docker rm -f $(docker ps -a -q)
# Limit container memory
docker run --memory maximum_allowed_memory image_name command
# Limit container CPU usage relative to other containers
docker run --cpu-shares
# Limit container CPU usage hard limit
docker run --cpu-quota
```

### Exposing ports

- Programs in containers are isolated from the interned by default
- You can group your containers into "private" networks
- You explicitly choose who can connect to whom

- Expose ports to allow incoming connections
- Private networks to connect between containers

- Exposing a specific port
    - Explicitly specifies the port inside the container and outside
    - Exposes as many ports as you want
    - Requires coordination between containers
    - Makes it easy to find the exposed ports
- Exposing ports dynamically
    - the port inside the container is fixed
    - leave off external port definition and docker will assign unused port
    - this allows many containers running programs with fixed ports
    - this is often used with a service discovery program/orchestration (e.g. Kubernetes)
- Exposing UDP ports
    - docker run -p host-port:container-port/protocol (tcp/udp)
    - docker run -p 1234:1234/udp

```Bash
# Server receiving data from one server and sending it to another, publish 2 ports inside/outside
# -p EXT_PORT:INT_PORT, i.e. HOST_PORT:CONTAINER_PORT
docker run --rm -ti -p 45678:45678 -p 45679:45679 --name echo-server ubuntu bash
# Omit EXT_PORT to have it dynamically assigned for you
docker run --rm -ti -p 45678 -p 45679 --name echo-server ubuntu bash
# View assigned ports
docker port container_name
# Inside of container
apt update
apt install netcat
netcat -lp 45678 | netcat -lp 45679
# Then in another terminal from host run command
nc localhost 45678
# Open one more terminal and run command
nc localshost 45679
# From terminal where you run nc localhost 45678 type in something and hit Enter, it will appear in another terminal where nc localshost 45679 is running
# To connect from container to host on a Mac you can use special name
nc host.docker.internal 45678
# On Win/Ubuntu use IP or DNS of host machine
# Example with dynamic external port and UDP
docker run --rm -ti -p 45678/udp --name echo-server ubuntu bash
# Inside of the container
netcat -ulp 45678
# In another Terminal window get dynamic port and run netcat over UDP to send data into container app
docker port container_name
netcat -u localhost 49153
```

### Containers Networking - Connecting between containers

- Docker host has its own network
- Containers use virtual network
    - you can connect from one container to another over vnet > host net > vnet (not efficient)
    - or you can connect between containers directly by placing them into the same vnet
- Default Docker networks
    - bridge - used by default by newly created containers 
    - host - no network isolation isolation
    - none - no netwokring
- We can create new networks & assign them to containers
    - containers within the same network have connectivity between them

```Bash
# List docker networks, default networks are bridge, host, none
docker network ls
# Create new network
docker network create network_name
# Assign network to a newly created container, put 2 containers on the same network
# docker run --rm -ti --net network_name --name container_name ubuntu bash
# docker run --rm -ti --net network_name --name container_name2 ubuntu bash
docker network create net1
docker run --rm -ti --net net1 --name srv1 ubuntu bash
docker run --rm -ti --net net1 --name srv2 ubuntu bash
# We can assign additional network(s) to container
docker network connect network_name container_name
```

### Legacy linking

- links all ports, but only one way (from all ports on one machine to all ports on the other, but only in one direction)
- secrets on the linked machine (e.g. env variables with passwords) are shared too and only the one way (i.e. from linked machine towards any machine it is linked too)
- depends on startup order
- restarting containers sometimes break the links

- not recommended to use, except for some scenarios

```Bash
# Create container with env var with secret
docker run --rm -ti -e SECRET=secretpassword --name test-container ubuntu bash
# Creare another one and link it to the first
docker run --rm -ti --link test-container -e SECRET2=secretpassword2 --name test-container2 ubuntu bash
# Now you can test with netcat that connection from linked server (test-container2) to test-container works but not the other way round
# on test-container
netcat -lp 1234
# on test-container2 run command below and try sending messages - it will work
netcat test-container 1234
# try the other way rount - it won't work, note that you can use any ports but links are uni-directional
# you will also see that ENV VAR was copied from the linked server, you linked test-container to test-container2
# test-container2 will see SECRET env var as TEST_CONTAINER_ENV_VAR_SECRET, you can verify that with command below
env | grep SECRET
```

### Images

- Listing images
    - list downloaded images - docker images
- Tagging images
    - tagging give images names
    - *docker commit* tags images for you
    - name structure:
        - registry.example.com:port/organization/image-name:version-tag
        - organization/image-name is often enough
- Getting images
    - docker pull
    - run automatically by docker run
    - useful for offline work
    - opposite: docker push
- Cleaning up
    - images can accumulate quickly
        docker rmi image-name:tag
        docker rmi image-id

```Bash
# List downloaded images
docker images
# commit tags images
docker commit CONTAINER_ID CONTAINER_TAG
# Removing image
docker rmi image-name:tag
docker rmi image-id
```

### Volumes

- Virtual disks to store and share data to share data between containers and/or containers and host
- Two main varieties
    - **Persistent** - data stays after container termination
    - **Ephemeral** - exists as long as used by container
- Volumes are not part of images

- Sharing data with the host
    - "Shared folders" with the host
    - Sharing a "single file" into a container
    - File must exist before you start the container

```Bash
# Mount host folder as /shared-folder in the container
docker run -ti -v ~/DockerLab:/shared-folder ubuntu bash
# You can share specific file with the container the same way (make sure it exists before container start)
```

- Sharing data between containers
    - volumes-from
    - shared discs that exist only as long as they are being used

```Bash
# Creating folder/volume not shared with host
docker run -ti -v /shared-data ubuntu bash
# Next you can start another container and connect this volume indicating folder/volume hosting container name
docker run -ti --volumes-from container_name ubuntu bash
# Data persist even if you exist first host container untill there is at least one container using the folder
```

### Docker registries

- Registries manage and distribute images
- Docker (the company) offers these for free
- You can run your own registry

- Finding images
    - docker search command
    - search on hub.docker.com

- Best practices
    - do not push images with sensitive information/passwords
    - clean up your images regularly
    - make sure to publish to registry images you may need in the future/those which you really need to keep
    - be conscious of how much you are trusting the containers you fetch - trust but verify


```Bash
# Search image by name
docker search ubuntu
# To connect to Docker Hub & push images
docker login
docker pull image_name:tag
docker tag image_name:tag registry/new_image_name:new_tag
docker push registry/new_image_name:new_tag
```

## Building Docker Images

### Dockerfiles

- a small "program" to create an image
- you run this program with
    ```Bash
    # -t = tag, make sure to put . in the end = current dir/location of your dockerfile
    docker build -t name .
    ```
