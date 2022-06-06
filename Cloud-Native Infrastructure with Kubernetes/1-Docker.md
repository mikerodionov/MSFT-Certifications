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
- when it finishes, the result will be in your local registry

#### Producing the next image with each step

- each line takes the image from the previous line and makes another image; **each line of a Docker file makes a new, independent image based on the previous line's image**
- the previous image is unchanged it just used as a starting point to make a new one
- it does not edit the state from the previous line
- you don't want large files to span lines or your image will be huge

[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

#### Caching with each step

- watch the build output for "using cache" - docker build skips lines if there were no changes since previous run
- Docker skips lines that have not changed since the last build
- if your first line is "downlad the latest file", it may not always run
- caching saves a lot of time
- the parts that change the most belong at the end of the Dockerfile; **include the most changed parts of code at the end of the Dockerfile so the parts befroe them do not need to be redone every time you change this part**

#### Docker files are not shell scripts

- Dockerfiles look like shell scripts - design desigion to make it look more familiar
- Dockerfiles are **not** shell scripts
- Processes you start on one line will not be running on the next line - each line = container starts, gets shutdown and saved to an image (= all process in it are terminated)
- Environment variables do persist across lines (as those got saved inside of image)
    - If you use the ENV command - each line is its own call to docker run

### Building Dockerfiles

#### The most basic Dockerfile

Put content below into file named Dockerfile:

```Bash
FROM busybox
RUN echo "building simple docker image"
CMD echo "Hello Container"
```

Build container image from this Dockerfile:

```Bash
# -t = one of the foreground modes - allocate pseudo-tty, TTY is what the most command line executables expect
docker build -t image_name .
# Once image created you can see it or run a container using it
docker images image_name
docker run --rm image_name
```

#### Installing a program with Docker Build

Dockerfile:

```Bash
FROM debian:sid
RUN apt-get -y update
RUN apt-get -y install nano 
CMD ["bin/nano", "/tmp/notes"]
```

Installs nano and opens it on container run

```Bash
docker build -t example/name .
# Run and remove afterwards (rm), command which will run are defined in image
docker run --rn -ti example/name
```

#### Adding a file through Docker Build, based on previously created image (example above)

```Bash
FROM example/name
ADD notes.txt /notes.txt
CMD ["bin/nano", "/notes.txt"]
```

```Bash
# Create notes.txt with content before running docker build
# Build container
docker build -t example/name2 .
# Run and remove
docker run -ti --rm example/name2
```

### Dockerfile syntax

#### FROM statement

- Which image to download and start from
- Must be the first command in a Dockerfile
    FROM java:8

#### MAINTAINER statement

- Defines the author of the Dockerfile
    MAINTAINERFirstname Lastname <email@domain.com>

#### RUN statement

- Runs the command line, waits for it to finish, and saves the result
    RUN unzip install.zip /opt/install
    RUN echo hello world

#### ADD statement

- Adds local files
    ADD run.sh /run.sh
- Adds the contents of tar archives to directory in a container
    ADD project.tar.gz /install/
- Download from URL
    ADD https://site.com/downloads/project/rpm /project/

#### ENV statement

- Sets environment variables
- Both during the build and when running the result (saved in resulting image)
    ENV DB_HOST=db.domain.com
    ENV DB_PORT=5432

#### ENTRYPOINT and CMD statements

- ENTRYPOINT specifies the start of the command to run (will be run by default, and whatever you type after *docker run image_name* it will be treated as an argument to it)
- CMD specifies the whole command to run (and if you type something after *docker run image_name* will override this/will be run instead as a command)
- If you have both ENTRYPOINT and CMD, they are combined together
- If your container acts like a CLI program, you can use ENTRYPOINT
- If you are unsure, you can use CMD

#### Shell form VS Exec form

- ENTRYPOINT RUN and CMD can use either form
- Shell form looks like normal command input in shell
    *nano notes.txt*
- Exec form looks as follows (pay attention to coma) - runs command directly not via shell call first (a bit more efficient)
    *["bin/nano", "/notes.txt"]*

#### EXPOSE statement

- Maps a port into container
    EXPOSE 8080

#### VOLUME statement

- Defines shared or ephemeral volumes - depending on number of arguments provided
    - With 2 arguments we map host path to container path
        VOLUME ["/host/path/" "/container/path/"]
    - With 1 argument it creates volume that can be inherited by containers
        VOLUME ["/shared/data"]
- Avoid defining shared folders in Dockerfiles (as it creates dependency on Docker host containing folder contents)

#### WORKDIR statement

- Sets the directory the container starts in (for the remainder of a Docker file and for the resulting container)
    WORKDIR /install/

#### USER statement

- Sets the user the container will run as (useful when shared directory is used which can be source of fixed name)
    USER john
    USER 1000

Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more details

### Multi-project Docker files

- Completeness (self-contained container) VS minimalism (small container)
- Multi-stage builds feature

```Bash
FROM ubuntu:22.04
RUN apt-get update
RUN apt-get -y install curl
RUN curl https://google.com | wc -c > google-size
ENTRYPOINT echo goolge has size; cat google-size
```

In the image above we calculate number of words on Google homepage

```Bash
# Build an image
docker build -t google-size .
# Run an image
docker run t google-size
# Check imge size - it will be 100+ MB
docker images
```

We can split image in parts, re-run docker build and see an image size of around 5 MB

```Bash
FROM ubuntu:22.04 as builder
RUN apt-get update
RUN apt-get -y install curl
RUN curl https://google.com | wc -c > google-size

FROM alpine
COPY --from=builder /google-size /google-size
ENTRYPOINT echo goolge has size; cat google-size
```

### Avoid golden images problems

- Include installers in your project (built-in your dependencies into base image)
- Have a canonical build that builds everything completely from scratch
- Tag your builds with the git hash of the code that built it (do not make changes outside of code control)
- Use small base images, such as Alpine
- Build images you share publicly from Dockerfiles, always
- Do not leave passwords in layers, delete files in the same step

## Under the hood

### Docker the program

#### Kernel

- Kernel runs directly on top of hardware and interacts with it (responds to messages from the hardware)
- Starts and schedules programs
- Controls and organizes storage
- Passes messages between programs
- Allocates resources - memory, CPU, network etc.
- Create container by Docker configuring the kernel

#### What Docker does

- Program written in Go - a statically typed, compiled programming language designed at Google
- Manages some kernel features and uses them to provide containers abstraction
    - Uses "cgroups" to contain processes
    - Uses "namespaces" to contain networks
    - Uses "copy-on-write" filesystems to build images
- Features mentioned above were used for years before Docker - Docker just made it easy and popular to use

#### What Docker really does :)

- Makes scripting distributed systems relatively easy

#### Docker control socket

- Docker is two programs - a client and a server
- The server receives commands over a socket (either over a network socker or through a file socket when both components are on the same machine)
- The client can even run inside Docker itself

- Running Docker Locally
    Docker client program <-> Socket <-> Docker server program {Container 1, Container 2}
- Running the client inside Docker
    Docker server program <-> Socket <-> Docker container {Docker client program}

```Bash
ls /var/run/docker.sock
/var/run/docker.sock
# Run Docker client inside of container and give it access to the Docker control socket on the host
docker run -ti --rm -v /var/run/docker.sock:/var/run/docker.sock docker sh
# Inside of container's Docker shell
docker info
# Start another container from a client within a container - this is not Docker-in-Docker, it is Docker client within a container controlling a server/host that is outside of this container
docker run -ti --rm ubuntu bash
# Check Ubuntu version inside of container
cat /etc/lsb-release
```

### Networking and namespaces

#### Networking core layers/parts

- Ethernet layer - moves frames on a wire or Wi-Fi
- IP layer - moves packets on a local network/between networks
- Routing - forwards packets between networks
- Ports - address particular programs on a computer

#### Bridging

- Docker uses bridges to create virtual networks in your computer
- Bridges are software switches
- They control ethernet layer
- To look at Docker bridges we need a system with brctrl (Bridge Control)
- To turn off network isolation between container and host network use --net=host switch
    *docker run --net=host options image-name command*

```Bash
# To look at Docker bridges we need a system with brctrl (Bridge Control)
# --net-host - gives containers full access to the host networking stack
docker run -ti --net=host ubuntu:22.04 bash
apt-get update && apt-get install bridge-utils
# View system's bridges
brctl show
# Add new network/bridge
docker network create new-net1
```

#### Routing

- Creates firewall rules (iptables) that controls packets movement between networks
- NAT (Network Address Translation)
    - Change the source address on the way out
    - Change the destination address on the way back in
    - sudo iptables -n -L -t nat

```Bash
# Run container with full control over host net and host
docker run -ti --rm --net=host --privileged=true ubuntu bash
# Inside of the container
apt-get update && apt-get install -y iptables
iptables-legacy -n -L -t nat
# If we then create new container with port mapping we can see new nat rules added rerunning the command above
docker run -ti --rm -p 8080:8080 ubuntu bash
iptables-legacy -n -L -t nat
# Exposing ports in Docker = port forwarding at the networking layer 
```
#### Namespaces

- Namespaces allow processes to be attached to private network segments
- These private networks are bridged into a shared network with the rest of the containers
- Containers have virtual NICs
- Containers have their own copy of a full Linux networking stack

### Processes and cgroups

#### Primer on Linux Processes

- Process come from other processes, i.e. have parent-child relationships
- When a child process exists, it returns an exit code to its parent
- Process zero is a special process called init, this is the process which starts the rest
- In Docker, your container starts with an init process and disappears when this process exits

```Bash
# Start new container
docker run -ti --rm --name hello-container ubuntu bash
# Use docker inspect to inspect state of container init process
docker inspect --format '{{.State.Pid}}' hello-container
## Start privileged container
docker run -ti --privileged=true --pid=host ubuntu bash
```

#### Resource limiting

- Scheduling CPU time
- Memory allocation limits
- Inherited limitations and quotas
- You can't exceed the limits by starting more processes

### Storage

#### Unix storage in brief

Unix storage system layers
- Actual stroage device
- Logical storage devices
- Filesystems
- FUSE filesystems and network filesystems (programs emulating filesystems)

#### Docker COWs

- COW - Copy on write
- Base image + data on top of it presented as the same although data is just layered on top of base image


## Orchestration - Building systems with  Docker
