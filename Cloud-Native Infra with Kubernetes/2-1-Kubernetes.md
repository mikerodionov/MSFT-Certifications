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

**Pod** - the simplest unit that you can interact with. You can create, deploy, and delete pods, and it **represents one running process on your cluster**. Pod represents one single unit of deployment, a single instance of an application in Kubernetes which is tightly coupled and shares resources.

Inside of the pod:

- Your Docker app container(s)
- Storage resources
- Unique network IP
- Options that govern how the container(s) should run

Pods are

- Ephemeral, disposable
- Never self-heal, and not restarted by the scheduler by itself
- Never create pods just by themselves
- Always use higher-level constructs to manage pods (controllers), i.e. do not use pod directly, use a controller instead

Pod lifecycle/states

- **Pending** - pod accepted by Kunernetes system, but container(s) has not been created yet
- **Running** - pod has been scheduled on a node, and all of its containers are created
- **Succeeded** - all the containers in the pod had exited with 0 exit code (= successful execution/no restart)
- **Failed** - all containers in the pod had exited and at least one container had failed and returned non 0 exit code
- **CrashLoopBackOff** - container fails to start and Kubernetes keep trying to restart the pod

### Controllers: Deployments, ReplicaSets and Services

#### Benefits of Controllers

- Application reliability - by running multiple instances of the app and preventing issues related with failure of one or more instances
- Scaling - by scaling up pods when load increases
- Load balancing - to balance load between different pods

#### Kinds of controllers

- **ReplicaSets** - ensures that a specified number of replicas for a pod are running at all times, used within deployment
- **Deployments** - provides declarative updates for pods and ReplicaSets using description of desired deployment state in a YAML file, defined to create new ReplicaSets or to replace existing ones with the new ones (most frequently created); Deployment manages a ReplicaSet, ReplicaSet manages a pod. Most deployments are package applications.
- **DaemonSets** - ensure that all nodes run a copy of a specific pod
- **Jobs** - supervisor process for pods carrying out batch jobs
- **Services** - allow the communication of one set of deployments with another

When you deploy a new deployment config a new ReplicaSet is created, but keeping old ReplicaSet to allow for rollback.

##### Deployment Controller use cases

- **Pod management** - running a ReplicaSet allows us to deploy a number of pods, and check their status as a single unit
- **Scaling a Replica set** - scales out the pods, and allows for the deployment to handle more traffic
- **Pause and Resume** - used with larger changesets; pause deployment, make changes, resume deployment; when deployment is paused only updates are paused, traffic still gets passed to existent replica sets as expected
- **Status** - deployment status is an easy way to check the health of pods, and identify issues

A Deployment provides declarative updates for Pods and ReplicaSets. You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.

##### Replication Controller

- Early implementation of deployments and ReplicaSets
- Use deployments and ReplicaSets instead

##### DaemonSets

- DaemoSets ensure that all nodes run a copy of a specific pod
- As nodes are added or removed from the cluster, a DaemonSet will add or remove the required pods
- Deleting DaemonSets will also clean up all the pods that it created
- Typical DaemonSets use case - run a single log aggregator or monitoring agent on a node

##### Jobs

- Supervisor process for pods carrying out batch jobs
- Run individual processes that run once and complete successfully
- Typically jobs run as cron jobs

##### Services

- Allow the communication of one set of pods/deployment with another
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

- Services defined against the API server: kube-proxy watches the API server for the addition and removal of services
- For each new service, kube-proxy opens a randomly chosen port on the local node
- Connection made to the chosen port are proxied to one of the corresponding back-end pods

## Kubernetes 101 - Hello World

### Mac Install

Prerequisites:
- Docker
- Hypervisor (VirtualBox)

- kubectl
- minikube

### Windows Install

Docker Desktop
kubectl
minikube

Hyper-V requirements:
- VM Monitor Extensions: Yes
- Virtualization Enabled in Firmware: Yes
- Second Level Address Translation: Yes
- DEP Available: Yes

```Bash
minikube start --vm-driver="hyperv" --hyperv-virtual-switch="minikube"
```

### Linux Install

```Bash
# Prerequisites - Docker & VirtualBox
# Install minikube - https://minikube.sigs.k8s.io/docs/start/
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
# Verify installed versions
docker version
kubectl version --client
minikube version
```

### Other options

Ways to run Kubernetes

- Minikube
- Docker Desktop
- Kubernetes in Docker (kind)
- Managed Kubernetes service in a cloud, e.g. Amazon Elastic Kubernetes Service (EKS)

Minikube

- Original tooling to run Kubernetes locally
- One of the largest subcommunities in cloud native ecosystem
- Supports the latest version of Kubernetes quickly
- UX is targeted to brand-new users of Kubernetes
- Abstracts complexities while getting started
- Powerfull addons feature - some tooling bundled in

Docker Desktop

- Easy creation of a Kubernetes cluster with the Docker Desktop interface
- Pleasant UI
- Disadavantage - can take longer to see the latest version compared to Minikube

kind aka Kubernetes in Docker

- Kubernetes in Docker
- Popular tooling
- Run Kubernetes clusters in Docker containers as nodes
- Typically used for testing or Kubernetes development
- Not suited for beginners

Managed Kubernetes Services

- Run Kubernetes clusters without worrying about running the control plane
- Available on all major cloud providers

### Hello World app

[A simple helloworld application for Kubernetes that has a service and deployment.](https://github.com/karthequian/kubernetesHelloworld)

```yaml
apiVersion: apps/v1
#apiVersion: apps/v1beta1 # done for K8S version > 1.16
kind: Deployment
metadata:
  name: hello-k8s-deployment
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1 # deployment runs 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld
        image: karthequian/helloworld:latest
        ports:
        - containerPort: 80 #Endpoint is at port 80 in the container
---
apiVersion: v1
kind: Service
metadata:
  name: hello-k8s-service
spec:
  type: NodePort #Exposes the service as a node port
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: helloworld
```

```Bash
minikube start
kubectl get nodes
# Get all resources
kubectl get all
# Deploy helloworld Deployment/ReplicaSet/pod
kubectl create -f helloworld.yaml
kubectl get all
# Expose deployment as a service
kubectl expose deployment hello-k8s-deployment --type NodePort
kubectl get all
minikube service --all
# command below should open up browser tab but didn't work for me on Ubuntu
minikube service service_name
```

### Hello World App parts

#### Deployment

```Bash
# List all running K8s resources in the cluster - deployments, replicasets, pods, services
kubectl get all
# Look at the deployments
kubectl get deployment
# Inspect YAML of running deployment
kubectl get deployment/hello-k8s-deployment -o yaml
```

Running deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2022-06-11T19:44:30Z"
  generation: 1
  name: hello-k8s-deployment
  namespace: default
  resourceVersion: "726"
  uid: 789b705c-807a-4666-a068-8195c3dab6da
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: helloworld
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: helloworld
    spec:
      containers:
      - image: karthequian/helloworld:latest
        imagePullPolicy: Always
        name: helloworld
        ports:
        - containerPort: 80
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2022-06-11T19:44:42Z"
    lastUpdateTime: "2022-06-11T19:44:42Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2022-06-11T19:44:31Z"
    lastUpdateTime: "2022-06-11T19:44:42Z"
    message: ReplicaSet "hello-k8s-deployment-d7c6dd56" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
```

#### Service

```Bash
kubectl get service
kubectl get service/hello-k8s-service
kubectl get service/hello-k8s-service -o yaml
# Get running service YAML and save it into clipboard
kubectl get service/hello-k8s-service -o yaml | xclip -sel clip
```

Running service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2022-06-11T19:44:31Z"
  name: hello-k8s-service
  namespace: default
  resourceVersion: "694"
  uid: 6a4e223f-6103-41ae-b050-fdeac1b15cad
spec:
  clusterIP: 10.100.180.184
  clusterIPs:
  - 10.100.180.184
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 32057
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: helloworld
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}
```

#### Creating services & deployments

Sample service YAML - kubectl create -f service.yaml

```yaml
# Helloworld service for nodeports
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: helloworld
```

Sample deployment YAML - kubectl create -f deployment.yaml

```yaml
# Helloworld application- just the deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-deployment
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1 # tells deployment to run 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld
        image: karthequian/helloworld:latest
        ports:
        - containerPort: 80
```

Separate YAML files above can be combined into 1 YAML file with --- seprator.

## Making an app production ready

### Add, change, and delete labels

#### Adding labels at deployment time

```bash
# Create pod with labels
kubectl create -f pod-with-labels.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: helloworld
  labels:
    env: production
    author: karthequian
    application_type: ui
    release-version: "1.0"
spec:
  containers:
  - name: helloworld
    image: karthequian/helloworld:latest
```

```Bash
# View pods
kubectl get pods
# View pods labels
kubectl get pods --show-labels
```

#### Adding/deleting labels at runtime

```Bash
# Add/overwrite label
kubectl label pod/helloworld app=superapp --overwrite
kubectl get pods --show-labels
# Delete label - jist lablename- (minus)
kubectl label pod/helloworld app-
```

### Working with labels

Sample infra with labels

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: homepage-dev
  labels:
    env: development
    dev-lead: karthik
    team: web
    application_type: ui
    release-version: "12.0"
spec:
  containers:
  - name: helloworld
    image: karthequian/helloworld:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: homepage-staging
  labels:
    env: staging
    team: web
    dev-lead: karthik
    application_type: ui
    release-version: "12.0"
spec:
  containers:
  - name: helloworld
    image: karthequian/helloworld:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: homepage-prod
  labels:
    env: production
    team: web
    dev-lead: karthik
    application_type: ui
    release-version: "12.0"
spec:
  containers:
  - name: helloworld
    image: karthequian/helloworld:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: login-dev
  labels:
    env: development
    team: auth
    dev-lead: jim
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: login
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: login-staging
  labels:
    env: staging
    team: auth
    dev-lead: jim
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: login
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: login-prod
  labels:
    env: production
    team: auth
    dev-lead: jim
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: login
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: cart-dev
  labels:
    env: development
    team: ecommerce
    dev-lead: carisa
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: cart
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: cart-staging
  labels:
    env: staging
    team: ecommerce
    dev-lead: carisa
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: cart
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: cart-prod
  labels:
    env: production
    team: ecommerce
    dev-lead: carisa
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: cart
    image: karthequian/ruby:latest
---

apiVersion: v1
kind: Pod
metadata:
  name: social-dev
  labels:
    env: development
    team: marketing
    dev-lead: carisa
    application_type: api
    release-version: "2.0"
spec:
  containers:
  - name: social
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: social-staging
  labels:
    env: staging
    team: marketing
    dev-lead: marketing
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: social
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: social-prod
  labels:
    env: production
    team: marketing
    dev-lead: marketing
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: social
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: catalog-dev
  labels:
    env: development
    team: ecommerce
    dev-lead: daniel
    application_type: api
    release-version: "4.0"
spec:
  containers:
  - name: catalog
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: catalog-staging
  labels:
    env: staging
    team: ecommerce
    dev-lead: daniel
    application_type: api
    release-version: "4.0"
spec:
  containers:
  - name: catalog
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: catalog-prod
  labels:
    env: production
    team: ecommerce
    dev-lead: daniel
    application_type: api
    release-version: "4.0"
spec:
  containers:
  - name: catalog
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: quote-dev
  labels:
    env: development
    team: ecommerce
    dev-lead: amy
    application_type: api
    release-version: "2.0"
spec:
  containers:
  - name: quote
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: quote-staging
  labels:
    env: staging
    team: ecommerce
    dev-lead: amy
    application_type: api
    release-version: "2.0"
spec:
  containers:
  - name: quote
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: quote-prod
  labels:
    env: production
    team: ecommerce
    dev-lead: amy
    application_type: api
    release-version: "1.0"
spec:
  containers:
  - name: quote
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: ordering-dev
  labels:
    env: development
    team: purchasing
    dev-lead: chen
    application_type: backend
    release-version: "2.0"
spec:
  containers:
  - name: ordering
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: ordering-staging
  labels:
    env: staging
    team: purchasing
    dev-lead: chen
    application_type: backend
    release-version: "2.0"
spec:
  containers:
  - name: ordering
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: ordering-prod
  labels:
    env: production
    team: purchasing
    dev-lead: chen
    application_type: backend
    release-version: "2.0"
spec:
  containers:
  - name: ordering
    image: karthequian/ruby:latest
---
```

```bash
# Create infra with labels
kubectl create -f sample-infra-with-labels.yml
# View pods
kubectl get pods
# View labels
kubectl get pods --show-labels
# Search by label
kubectl get pods --selector env=production
kubectl get pods --selector env=production --show-labels
kubectl get pods --selector dev-lead=john
# Search by multiple labels
kubectl get pods --selector env=production, dev-lead=john
# NOT operator can also be used
kubectl get pods --selector dev-lead!=john
# IN operator
kubectl get pods -l 'release-version in (1.0,2.0)'
kubectl get pods -l 'release-version in (1.0,2.0)' --show-labels
# NOTIN
kubectl get pods -l 'release-version notin (1.0,2.0)'
kubectl get pods -l 'release-version notin (1.0,2.0)' --show-labels
# Delete pods by label
kubectl delete  pods -l dev-lead=john
kubectl get pods --show-labels
```

Labels can be used with any Kubernetes entity - pods, deployments, replicasets, etc.

### Application health checks

App health check YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-deployment-with-probe
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1 # tells deployment to run 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld
        image: karthequian/helloworld:latest
        ports:
        - containerPort: 80
        readinessProbe:
          # length of time to wait for a pod to initialize
          # after pod startup, before applying health checking
          initialDelaySeconds: 5
          # Amount of time to wait before timing out
          timeoutSeconds: 1
          # Probe for http
          httpGet:
            # Path to probe
            path: /
            # Port to probe
            port: 80
        livenessProbe:
          # length of time to wait for a pod to initialize
          # after pod startup, before applying health checking
          initialDelaySeconds: 5
          # Amount of time to wait before timing out
          timeoutSeconds: 1
          # Probe for http
          httpGet:
            # Path to probe
            path: /
            # Port to probe
            port: 80
```

```bash
# Adding app health checks
kubectl create -f helloworld-with-probes.yaml
kubectl get deployments
kubectl get replicaset
kubectl get pods
```

Example with failing readiness probe (via using wrong port)

```Yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-deployment-with-bad-readiness-probe
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1 # tells deployment to run 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld
        image: karthequian/helloworld:latest
        ports:
        - containerPort: 80
        readinessProbe:
          # length of time to wait for a pod to initialize
          # after pod startup, before applying health checking
          initialDelaySeconds: 5
          # Amount of time to wait before timing out
          timeoutSeconds: 1
          # Probe for http
          httpGet:
            # Path to probe
            path: /
            # Port to probe
            port: 90
```

```Bash
kubectl create -f helloworld-with-bad-readiness-probe.yaml
# Check deployment
kubectl get deployments
kubectl get replicaset
kubectl get pods
# Describe specific pod to see failing probe which prevents it from entering into ready state
kubectl describe po helloworld-deployment-with-bad-readiness-probe-6cb6cd4bf4-wftzm
```

Liveness probes example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-deployment-with-bad-liveness-probe
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1 # tells deployment to run 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld
        image: karthequian/helloworld:latest
        ports:
        - containerPort: 80
        livenessProbe:
          # length of time to wait for a pod to initialize
          # after pod startup, before applying health checking
          initialDelaySeconds: 5
          # How often (in seconds) to perform the probe.
          periodSeconds: 5
          # Amount of time to wait before timing out
          timeoutSeconds: 1
          # Kubernetes will try failureThreshold times before giving up and restarting the Pod
          failureThreshold: 2
          # Probe for http
          httpGet:
            # Path to probe
            path: /
            # Port to probe
            port: 90
```

```Bash
kubectl create -f helloworld-with-bad-liveness-probe.yaml
# Check deployment
kubectl get deployments
kubectl get replicaset
# You will see that now readiness probe is passing but pod keeps restarting as liveness probe fails, so eventually it will enter into CrashLoopBackOff status
kubectl get pods
# Describe specific pod to see failing liveness probe
kubectl describe po helloworld-deployment-with-bad-readiness-probe-6cb6cd4bf4-wftzm
```

### Handling application upgrades

Upgrading deployment from one version to another

### Basic troubleshooting techniques

## Kubernetes 201

### More complex example