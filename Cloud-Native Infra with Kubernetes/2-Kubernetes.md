# Kubernetes

## Containerization with Kubernetes

### Containerization definition

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

**Container** - a collection of software processes unified by one namespace with acces to an OS kernel that it shares with other containers and little to no access between containers

Docker (container) Instance - a runtime instance of a Docker image. Contains 3 things:
    - A Docker image
    - An execution environment
    - A standard set of instructions

If we employ OOP analogy we can say that **container is an object** and **class is an image**.

Core elements of Docker ecosystem:
    - Docker Engine
        - Comprised of the runtime and packaging tool
        - Must be installed on the hosts that run Docker
    - Docker Store
        - An online cloud service for storing and sharing Docker images
        - AKA Docker Hub

#### Container VS VM

VM includes
    - One or many applications
    - The necessary binaries and libraries
    - The entire guest OS to interact with the applications
Container includes
    - Include the application and all of its dependencies
    - Share the kernel with other containers
    - Not tied to infra - only needs Docker Engine installed on the host
    - Tun an isolated processes in user space on the host OS

#### Containers Benefits for Devs

- Applications are
  - Portable
  - Packaged in a standard way
- Deployment is
  - Easy
  - Repeatable
- Automated testing, packaging, and integrations
- Support newer microservice architectures
- Alleviate platform compatibility issues

#### Containers Benefits for DevOps

- Reliable deployments: improve speed and frequency of releases
- Consistent application lifecycle: configure once and run multiple times
- Consistent environments: no more process differences between dev and prod environments
- Simple scaling: fast deployments ease the adoption of workers and permit workload to grow and shrink for on-demand use cases

- Containers also create a common language within an enterprise for dev and ops
- The DevOps team can isolate and debug issues at the container level
- Containers allow the building of pipelines
    - Containers bring agility to your code
    - Help build a continuous integration and deployment pipeline
    - Push an IT team to develop, test, and deploy apps faster

### Kubernetes definition

Most popular open source containers orchestrator.

As adoption of containers got really large scale it created demand for orchestration solutions.

Required orchestration features:

- Provision hosts
- Instantiate containers on a host
- Restart failing containers
- Expose containers as services outside the cluster
- Scale the cluster up/down

Kubernetes (K8s)

- Definition: an open-source platform designed to automate deploying, scaling, and operating application containers
- Goal: to foster an ecosystem of components and tools that relieve the burden of running applications in public and private clouds

Borg was the predecessor to Kubernetes (created at Google to manage their internal infra).

Kubernetes is a platform to schedule and run containers on clusters of VMs which runs on bare metal, VMs, private datacenter and public cloud. Resolves cloud migrations issues.

#### Kubernetes and Docker

Kubernetes is a container platform/orchestrator

You can use Docker containers to develop and build applications, and then use Kubernetes to run these applications on your infra

Pokemon Go is the largest Kubernetes deployment on the Google container engine.

### Kubernetes features

"Kubernetes is an open source project that enables software teams of all sizes, from a small startup to a Fortune 100 company, to automate deploying, scaling, and managing applications on a group or cluster of server machines." Joe Beda
"These applications can include everything from internal-facing web applications like a content management system to marquee web propertees like Gmail to big data processing" Joe Beda

- **Multi-Host Container Scheduling**
  - Done by the kube-scheduler
  - Assigns pods to nodes at runtime
  - Checks resources, QoS, policies, and user specifications before scheduling
- **Scalability and availability**
  - Kubernetes master can be deployed in a highly available configuration
  - Multi-region deployments available
- **Scalability (v1.17)**
  - Supports 5,000 node clusters
  - 150,000 total pods
  - Max 100 pods per node
  - Pods can be horizontally scaled via API
- **Flexibility and modularization**
  - Plug-and-play architecture
  - Extend architecture when needed
  - Add-ons: network drivers, service discovery, container runtime, visualization, and command
- **Registration**
  - Nodes register themselves with master seamlessly
- **Service discovery**
  - Automatic detection of services and endpoints via DNS or environment variables
- **Persistent storage**
  - Pods can use persistent volumes to store data
  - Data retained across pod restarts and crashes
- **Application upgrades and downgrades**
  - Upgrades: rolling updates supported
  - Downgrades: rollbacks are supported
- **Maintenance**
  - Features are backward compatible
  - APIs are versioned
  - Turn off/on host during maintenance (unschedulable)
- **Logging and monitoring**
  - Application monitoring built-in
    - TCP, HTTP, or container execution health check
  - Node health check
    - Failures monitored by node controller
  - Kubernetes status
    - Add-ons: Heapster and cAdvisor
  - Logging framework
    - In place or extensible
- **Secrets Management**
  - Sensitive data is a first-class citizen
  - Mounted as data volumes or environment variables
  - Specific to namespaces
- **Community**
  - Governed by CNC
  - Good docs
  - [KubeCon](https://events.linuxfoundation.org/)
  - [Meetups](https://www.meetup.com/topics/kubernetes/)
  - [Slack](https://kubernetes.slack.com), further divided into SIGs

### Other implementations

Major players in container orchestration

- Kubernetes
- Docker Swarm
- Rancher
- Mesos
- Nomad

Cloud-specific options

- Amazon EC2 Container Service
- Google Anthos

#### Mesos

- Written in C++, APIs in Java, C++, Python
- Oldest, most stable tool
- Distributed kernel
- Marathon framework to schedule and execute tasks
- More complicated architecture than Swarm
- Typical Mesos users
  - Large enterprises
  - Projects that require lots of compute (e.g. big data) or task-oriented workloads
  - Driven by devs
  - Needs an ops team to manage

#### Rancher

- Full stack container management platform
- Install and manage Kubernetes clusters
- Early Docker ecosystem player
- Great UI and API to interact with clusters
- Enterprise support
- Typical Rancher users
  - Small teams to enterprises
  - Support organizations and teams out of the box
  - Nice UI & APIs
  - Easy to manage infra effectively

A lot of enterprises joined CNCF, which backs Kubernetes.

#### When to go Serverless

- Consider serverless tech when
  - Starting to build your infra from scratch
  - Running your infra in the cloud

## Kubernetes Terminology

### Kubernetes Cluster Architecture

A lot of boxes :)

#### Master node

- Overall management of K8s cluster, includes
  - The API Server - frontend for K8s control plane
  - Scheduler - watches created pods without nodes assigned yet and assign them to run on specific node
  - Controller Manager - runs controllers - background threads that run tasks in a cluster
    - Nodes Controller - worker states
    - Replication Controller - maintains correct number of pods
    - End-point controller - joins services and pods together
    - Service account and token controllers - handle access management
  - etcd - simple distributed key-value store - K8s cluster data store (job scheduling info, Pod details, stage information, etc.)

#### kubectl

- CLI client to interact with master node/API server
- cubeconfig file contains server and authentication infor to access the API server

#### Worker nodes

- kubelet - agents that communicates back to master node/API server
- Docker - runs container on the node, alternate container platforms can be used
  - Pods - pod is a smallest deployment unit in Kubernetes
    - containers
- kube-proxy - network proxy & LB for the service on a single worker node, performs network routing and connection forwarding

### Nodes and pods - basic building blocks

#### Kubernetes Cluster

- Nodes
  - kubelet
  - Docker
- Master
- Node Processes
  - kubelet
  - Docker

#### Node

Node - serves as a worker machine in a K8s cluster. One important thing to note is that the node can be a physical computer or a VM.

Node Requirements

- A kubelet running
- Container tooling/runtime like Docker
- A kube-proxy process running
- Supervisord - to restart components

For production use cases it is typically recommended to have at least 3 node cluster.

#### Minikube

Lightweight Kubernetes implementation that creates a VM on your local machine and deploys a simple cluster containing only one node. Good for learning/testing.

#### Pods

**Pod** - the simplest unit that you can interact with. You can create, deploy, and delete pods, and it **represents one running process on your cluster**.

Inside of the pod:

- Your Docker app container(s)
- Storage resources
- Unique network IP
- Options that govern how the container(s) should run

Pod represents one single unit of deployment, a single instance of an application in Kubernetes which is tightly coupled and shares resources.

Pods are

- Ephemeral, disposable
- Never self-heal, and not restarted by the scheduler by itself
- Never create pods just by themselves
- Always use higher-level constructs to manage pods (controllers), i.e. do not use pod directly, use a controller instead

Pod lifecycle/states

- Pending - pod accepted by Kunernetes system, but container has not been created yet
- Running - pod has been scheduled on a node, and all of its containers are created
- Succeeded - all the containers in the pod had exited with 0 exit code (= successful execution/no restart)
- Failed - all containers in the pod had exited and at least one container had failed and returned non 0 exit code
- CrashLoopBackOff - container fails to start and Kubernetes keep trying to restart the pod

### Controllers: Deployments, ReplicaSets and Services

#### Benefits of Controllers

- Application reliability - by running multiple instances of the app
- Scaling
- Load balancing

#### Kinds of controllers

- ReplicaSets - ensures that a specified number of replicas for a pod are running at all times, used within deployment
- Deployments - provides declarative updates for pods and ReplicaSets using description of desired deployment state in a YAML file, defined to create new ReplicaSets or replace existing ones with new (most frequently created)
- DaemonSets - ensure that all nodes run a copy of a specific pod
- Jobs - supervisor process for pods carrying out batch jobs
- Services - allow the communication of one set of deployments with another

Deployment manages a ReplicaSet, ReplicaSet manages a pod.
When you deploy a new deployment config a new ReplicaSet is created, but keeping old ReplicaSet to allow for rollback.

#### Deployment Controller use cases

- Pod management - running a ReplicaSet allows us to deploy a number of pods, and check their status as a single unit
- Scaling a Replica set - scales out the pods, and allows for the deployment to handle more traffic
- Pause and Resume - used with larger changesets; pause deployment, make changes, resume deployment
- Status - deployment status is an easy way to check the health of pods, and identify issues

#### Replication Controller

- Early implementation of deployments and ReplicaSets
- Use deployments and ReplicaSets instead

#### DaemonSets

- DaemoSets ensure that all nodes run a copy of a specific pod
- As nodes are added or removed from the cluster, a DaemonSet will add or remove the required pods
- Deleting DaemonSets will also clean up all the pods that it created
- Typical DaemonSets use case - run a single log aggregator or monitoring agent on a node

#### Jobs

- Supervisor process for pods carrying out batch jobs
- Run individual processes that run once and complete successfully

#### Services

- Allow the communication of one set of deployments with another
- Use a service to get pods in 2 deployments to talk to each other (best practice) - ensures the same IP which deployment will be using to connect to another deployment - way better than connecting over changing back end pod IP
  - Frontend Pod > Backend Service > Backend Pod(s)
- Once again: service provides unchangin IP

#### Kinds of services

- Internal - IP is only reachable within the cluster (cluster IP)
- External - endpoint available through node IP:port (NodePort)
- Load balancer - exposes application to the internet with a load balancer (available with a cloud provider)

### Labels, selectors, and namespaces

Used to annotate and organize applications to simplify life of Kubernetes operators.

#### Labels

Labels are key/value pairs that are attached to objects like pods, services, and deployments. Lavels are for users of Kubernetes to identify attributes for objects.
Labels can be added at deployment time or added/changed at any time.
Label keys are unique per object.

Labels examples:

"release": "stable", "release" : "canary"
"environment" : "dev", "environment" : "qa", "environment" : "production"
"tier" : "frontend", "tier" : "backend", "tier" : "cache"

- Think about how your environment looks like, and create a labeling hierarchy for it.

#### Labels and Selectors

- Labels used with selectors are more powerful feature
- Label selectors allow you to identify a set of objects

- Selector types
  - Equality-based
    - = Two labells pr values of labels should be equal
    - != = Two labells pr values of labels should not be equal
  - Set-based
    - IN - A value shoud be inside of a set of defined values
    - NOTIN - A value shoud not be inside of a set of defined values
    - EXISTS - Determines whether a label exists or not
- Labels and label selectors are typically used iwth kubectl

#### Namespaces

- Allow to have multiple virtual clusters bakced by the same physical cluster
- Great for large enterprises
- Allows teams to access resources, with accountability
- Great way to divide cluster resources between users
- Provides scope for names - names must be unique in the namespace
- Default namespace created when you launch Kubernetes
- Objects places in default namespaces if other not specified
- Newer applications install their resources in a different namespace (do not use default one)

### Kubelet and kube-proxy

#### Kubelet

- The Kubelet is the Kubernetes node agent that runs on each node
- Communicates with an API server to see if pods have been assigned to nodes
- Executes pod containers via a container engine
- Mounts and runs pod volumes and secrets
- Executes health checks to identify pod/node status

#### Kubelet and Podspec

- Podspec - YAML file that describes a pod
- The kubelet takes a set of podspecs that are provided by the kube-api server and ensures that the containers described in  those podspecs are running and healthy
- Kubelet only manages containers that were created by the API server - not any container running on the node
- Kubelet can be managed without an API server via HTTP endpoint or a file

#### kube-proxy - the network proxy

- Process that runs on all worker nodes
- Reflects services as defined on each node, and can do simple network stream or round-robin forwarding across a set of backends
- Service cluster IPs and ports are currently found through Docker --link copmpatible environment variables specifying ports opened by the service proxy

#### 3 modes of kube-proxy

- User space mode - the most common mode
- Iptables mode
- Ipvs mode (alpha feature)

These modes are important because
-