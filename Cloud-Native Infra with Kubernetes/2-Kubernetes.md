# Kubernetes

## Containerization definition

Containers timeline/milestones

1979 Unix V7 added chroot
2000 FreeBSD jails
2001 Linux-VServer
2004 Oracle Solaris Zones
2005 OpenVZ
2006 Process containers
2007 cgroups in Linux Kernel (2.6.24)
2008 LXC
2011 Warden
2013 Imctfy
2013 Docker
2014 rkt
2015 OCI

Container - a collection of software processes unified by one namespace with acces to an OS kernel that it shares with other containers and little to no access between containers

Docker (container) Instance - a runtime instance of a Docker image. Contains 3 things:
    - A Docker image
    - An execution environment
    - A standard set of instructions

If we employ OOP analogy we can say that container is an object and class is an image.

Core elements of Docker ecosystem:
    - Docker Engine
        - Comprised of the runtime and packaging tool
        - Must be installed on the hosts that run Docker
    - Docker Store
        - An online cloud service for storing and sharing Docker images
        - AKA Docker Hub

### Container VS VM

VM includes
    - One or many applications
    - The necessary binaries and libraries
    - The entire guest OS to interact with the applications
Container includes
    - Include the application and all of its dependencies
    - Share the kernel with other containers
    - Not tied to infra - only needs Docker Engine installed on the host
    - Tun an isolated processes in user space on the host OS

### Containers Benefits for Devs

- Applications are
  - Portable
  - Packaged in a standard way
- Deployment is
  - Easy
  - Repeatable
- Automated testing, packaging, and integrations
- Support newer microservice architectures
- Alleviate platform compatibility issues

### Containers Benefits for DevOps

- Reliable deployments: improve speed and frequency of releases
- Consistent application lifecycle: configure once and run multiple times
- Consistent environments: no more process differences between dev and prod environments
- Simple scaling: fast deployments ease the adoption of workers and permit workload to grow and shrink for on-demand use cases

Containers also create a common language within an enterprise for dev and ops.

The DevOps team can isolate and debug issues at the container level.

Containers allow the building of pipelines
    - Containers bring agility to your code
    - Help build a continuous integration and deployment pipeline
    - Push an IT team to develop, test, and deploy apps faster

## Kubernetes definition

Most popular open source containers orchestrator.

## Kubernetes features

## Other implementations

