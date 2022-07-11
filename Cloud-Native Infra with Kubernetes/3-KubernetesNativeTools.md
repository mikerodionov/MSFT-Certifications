# Kubernetes: Native Tools

## Intro

```PowerShell
# "vi" on Windows
New-Alias vi notepad
```

### Native tools description

- Kubectl - K8s client
- Minikube, kops, kubeadm - most common tools to install K8s
  - minikube - local
  - kops and kubeadm - local(bare metal)/cloud
- Dashboard, kubefed, Kompose, Helm - general management tools
  - Dashboard - visualization
  - kubefed - large cluster federation
  - Kompose - exporting from Docker
  - Helm - large application management

## Mastering kubectl

### Creating and interacting with the cluster

```Bash
# Create deployment from image
kubectl run deployment_name --image image_name
kubectl run mr --image ubuntu
# Create deployment from YAML file
kubectl create -f deployment.yaml
```

Sample deployment YAML

```Yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: SampleApp
spec:
  selector:
    matchLabels:
      app: SampleApp
  replicas: 1
  template:
    metadata:
      labels:
        app: SampleApp
    spec:
      containers:
      - name: SampleApp
        image: ubuntu
        ports:
        - containerPort: 80
```

```Bash
# List deployments
kubectl get deployments
# Create multiple deployments from file
kubectl create -f deployment1.yaml -f deployment2.yaml
# Deploy all YAMLs from folder
kubectl create -f ./folder
# Check folder contents
ls -al ./folder
# If you re-run deployment from fail using the same file/name you will get AlreadyExists error
# We can edit yaml to change name
# We can use kubectl apply which will either create OR update existing deployment
kubectl apply -f deployment.yaml
# Use of -o wide option
kubectl get deployments -o wide
# We can also create app from URL (e.g. using GitHub URL pointing to RAW YAML file)
kubectl apply -f https://raw.githubusercontent/user/repo/master/deploymen.yaml
```

### Real-world example using namespaces

```Yaml
apiVersion: v1
kind: Namespace
metadata:
  name: web
---
apiVersion: v1
kind: Namespace
metadata:
  name: auth
---
apiVersion: v1
kind: Namespace
metadata:
  name: cart
---
apiVersion: v1
kind: Namespace
metadata:
  name: social
---
apiVersion: v1
kind: Namespace
metadata:
  name: catalog
---
apiVersion: v1
kind: Namespace
metadata:
  name: quote
---
apiVersion: v1
kind: Namespace
metadata:
  name: purchasing
---
apiVersion: v1
kind: Namespace
metadata:
  name: infra
---
apiVersion: v1
kind: Pod
metadata:
  name: homepage-dev
  namespace: web
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
  namespace: web
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
  namespace: web
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
  namespace: auth
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
  namespace: auth
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
  namespace: auth
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
  namespace: cart
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
  namespace: cart
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
  namespace: cart
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
  namespace: social
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
  namespace: social
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
  namespace: social
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
  namespace: catalog
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
  namespace: catalog
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
  namespace: catalog
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
  namespace: quote
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
  namespace: quote
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
  namespace: quote
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
  namespace: purchasing
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
  namespace: purchasing
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
  namespace: purchasing
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
apiVersion: v1
kind: Pod
metadata:
  name: logger
  namespace: infra
  labels:
    env: production
    team: infrastructure
    dev-lead: tracey
    application_type: backend
    release-version: "2.0"
spec:
  containers:
  - name: logger
    image: karthequian/ruby:latest
---
apiVersion: v1
kind: Pod
metadata:
  name: monitor
  namespace: infra
  labels:
    env: production
    team: infrastructure
    dev-lead: tracey
    application_type: backend
    release-version: "2.0"
spec:
  containers:
  - name: monitor
    image: karthequian/ruby:latest
---
```

```Bash
# Deploy sample infra
kubectl apply -f sample-infra.yaml
# File above will create multiple namespaces and deploys multiple pods into them
# Get namespaces list
# default, kube-public & kube-system are K8s default namespaces
kubectl get namespaces
# View pods in specific namespace
kubectl get pods -n namespace_name
# View pods in all namespaces
kubectl get pods --all-namespaces
```

### Searching, sorting, and filtering applications

```Bash
# You break your app into namespaces based on components/teams or environment type/tier
kubectl get pods --all-namespaces
kubectl get pods -o wide --all-namespaces
kubectl get pods -o json --all-namespaces
# Sorting (ascending)
kubectl get pods --sort-by=.metadata.name --all-namespaces
# We can also traverse to JSON path
kubectl get pods -o=jsonpath="{..image}" --all-namespaces
# The same with filtering by label and from specific namespaces
kubectl get pods -o=jsonpath="{..image}" -l env=staging -n cart
```

[Kubernetes JSONPath Support](https://kubernetes.io/docs/reference/kubectl/jsonpath/)

[Kubernetes output options](https://kubernetes.io/docs/reference/kubectl/#output-options)
-o custom-columns=<spec>
-o custom-columns-file=<filename>
-o json
-o jsonpath=<template>
-o jsonpath-file=<filename>
-o name
-o wide
-o yaml

### Deleting strategies for applications

```Bash
# Delete deployment with its pods
kubectl delete deployment deployment_name
# Delete all pods in specific namespace
kubectl delete pods -n namespace_name --all
# You can see pods will be in Terminating state immediately after issuing command above and then disappear
kubectl get pods -n namespace_name
# You can use -w flag after listing/getting the requested object to watch for changes
kubectl get pods -n namespace_name -w
# To force immediate delete without waiting for pods to stop/kill pods without waiting
kubectl delete pods -n namespace_name --grace-period=0
# Delete by label
kubectl get pods -l env=staging -n namespace_name
kubectl delete pods -l env=staging -n namespace_name
# Delete all instances of various object types in a specific namespace
kubectl -n my-namespace delete po,svc --all
```

## Running Kubernetes

### Minikube

Great tool for runnnig locally single node cluster for testing.

Minikube requirements
- Container platform (Docker)
- Mac/Linux/Windows Pro
- Mac/Linux - get a VM driver like VirtualBox or VMware Fusion
- Windows - xhyve driver is required to run Docker

Minikube advanced features
- Add-Ons - additional components that can be enabled
- Kubernetes version selection - pass the version you want to run when you start minikube via **--kubernetes-version** flag (minikube officially supports the latest Kubernetes release +6 previous minor versions, older version may not work and require --force flag)
- Can be used with private repo
    - use **--insecure-registry** flag with the minikube start command to enable insecure Docker registries
    - enable the **registry-creds add-on** to communicate with Google Container Registry (GCR), Amazon EC2 Container Registry (ECR) and private Docker registries

Minikube is useful when you want to quickly test upgrades to the latest version before moving one to more complex tests as it is gets updated fast/regularly.

### kubeadm

Kubeadm - helps you install and setup a K8s cluster. Native tool since v1.4

Kubeadm allows creation of K8s clusters on machines running:

- Ubuntu 16.04+
- Debian 9
- CentOS 7
- RedHat EL 7

#### Cluster setup 0 - Prerequisites

- Docker
- kubeadm
- kubelet
- kubectl

#### Cluster setup 1 - all nodes

- Kubeadm tool has to be installed on all hosts that will be part of a cluster.

#### Cluster setup 2 - on a master node(s)

- Run **kubeadm init** on the master node
  - Runs checks to verify that selected node can run Kubrnetes
  - Downloads and installs cluster database and sets up control plane components
  - Provides a join token that will join any worker node to the kubernetes cluster
  - Join token should be treated as a sensitive information (allows any machine to join the cluster)

#### Cluster setup 3 - create pod network

- Once master is provisioned we also need a **pod network** so that pods can communicate
- Kubeadm only supports networks that are a part of the [**ContainerNetwork Interface (CNI) spec**](https://www.cni.dev/docs/spec/) and also doesn't support kubenet
- Run the **kubectl apply** command to add most of the network plugins

#### Cluster setup 4 - join worker nodes

- Run the **kubeadm join** command on the worker nodes using the join token provided at the end of the kubeadm init provisioning (step 2)

Kubeadm enables simple Kubernetes install.

#### Drawbacks of kubeadm

- Lack of support for multi-master nodes in old versions (before 1.8)
- Some add-ons are still in alpha phase

### kops

kops (Kubernetes Operations) - allows to create, destroy, upgrade, and maintain production-grade, HA Kubernetes clusters from the command line. Allows creation of production grade K8s cluster for AWS (and also for GCE & VMware vSphere)

#### kops Requirements:

- kops
- kubectl
- AWS credehtials
- AWS CLI tools

#### AWS side requirements:

- AWS IAM account
- Full access to S3, EC2, Route53, and IAM
- DNS record for the Kubernetes etcd service

#### Cluster set up

Create variables

- export
  - NAME=clustername.k8s.local
- export
  -KOPS_STATE_STORE=s3://prefix-example-com-state-store

Build cluster

```Bash
kops create cluster \ --zones us-west-2a \ ${NAME}
```

kops create cluster command

- Results in a configuration file for what your cluster is going to look like
- Verify configuration by runnig kops edit cluster followed by the name of the cluster

kops update command

- Builds the cluster per spec

```Bash
kops update cluster <cluster name>
```

- Use kubectl get nodes to monitor when nodes are ready
- Once deployment completes, run **kops validate cluster command** to verify that all works as expected

#### Deploy to AWS

- kops is a command-line tool that you can use to run create/delete/get/operations
- apt for Kubernetes admins as it is similar to kubectl/easy to understand
- allows HA clusters provisioning

## Other Useful Tools

### Kubernetes Dashboard

- General web based UI to visualize cluster and resources
- 3 main purposes
  - Manage running applications
  - Troubleshot any issues with applications
  - Manage the entire Kubernetes cluster
- Comes installed as a minikube add-on
- If you running K8s on a real cluster, you'll need to install Dasboard as an application
  - https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
- Access via browser
  - Set up kubectl proxy, which sets up an HTTP proxy to the Kubernetes API server
  - Access via httos://localhost:8001/ui
    - Cluster page
    - Kubernetes overview
- Dashboard login page introduced in K8s 1.7 (supports all K8s authentication mechanisms)
  - Useful feature to enable in large enterprises

### Federation and kubefed

- Cluster federation key features
  - Sync resources across clusters - most of basic constructs such as deployments, services, have federated versions of themselves enabling them reside in multiple clusters in sync
  - Cross-cluster discovery - provides ability to auto configure DNS servers and NLBs with backends from all clusters; single DNS record connected to back ends from multiple clusters
- Benefits of federation
  - Create HA clusters in different regions (single-cloud scenario)
  - Avoid vendor lock-in by migrating applications across clusters (hybrid-cloud scenario)
  - Low latency creates a better UX
  - Smaller clusters = easier management for fault isolation
  - Avoids scalability limits on pods and nodes

- kubefed - tool to configure federation
- kubefed workflow
  1. Set up clusters in different regions
  2. Configure DNS required for each of these clusters
  3. Initialize the kubefed tool to set up your **federation control plane**
  4. Add clusters to your federation - witch conrol plane initialized you can add your existing clusters to kubefed as federated clusters by passing the cluster config

### Kompose

#### Kompose definition

- Kompose tool converts Docker Compose files into Kubernetes objects like deployments and services
- Provides an export path for people comfortable with Docker Compose to create Kubernetes objects using the Kubernetes-style YAML

#### History/Why?

- At early stages each vendor used its own standards to describe container objects
  - Docker - Compose
  - Kubernetes - Replication Controllers
  - Rancher - Cattle
  - Stack Engine - applications and deployments
- With time community settled on 2 standards/frameworks
  - Docker Compose
  - Kubernetes Objects
- Kompose - path from Docker Tooling to Kubernetes
- Kompose is a translator, review translation to make sure the outcome is as expected

#### Kompose Installation Process

1. Download the binary
2. Place the binary in your path
3. Convert your compose files (use compose convert to read Docker Compose and create Kubernetes objects in the same directory)

- Kompose can get you up and running with Kubernetes quickly if you have Docker Compose files describing your architecture
- Caveat: If you're not sure what you're doing, or are unfamiliar with the Kubernetes objects and resources, you might accept the wrong kind of object because Kompose misinterpreted it
- Not all Docker Compose keys are transposed to Kubernetes

### Helm

Helm - tool that streamlines the installation and management of Kubernetes applications; it is like a package manager for Kubernetes.

Helm manages charts - packages of preconfigured K8s resources

#### Helm Architecture

Local Machine - Helm (client)
Server - Tiller (server component which runs in K8s cluster as deployment in its own namespace and manages releases and installation of Helm charts)

#### Helm Chart Content

Helm chart contains 3 things

1. Chart - Chart YAML - contains high level metadata about the chart, MUST contain name and version
2. Templates - One or more templates in the template folder - template contains Kubernetes resources and components that make up an application, can contain parameters that can be passed in from values.yaml (JINJA syntax)
3. Values - values.yaml file 

Chart YAML example:

```YAML
name: nginx
description: A basic NGINX HTTP server
version: 0.1.0
keywords:
  - http
  - nginx
  - www
  - web
home: https://github.com/kubernetes/helm
sources:
  - https://hub.docker.com/_/nginx/
maintainers:
  - name: technosophos
  - email: mbutcher@deis.com
```

#### Running helm install

- Sends Helm Chart to Tiller
- Tiller compiles template and values into an application
- Versions the application
- Deploys to a K8s cluster

- You can interact with deployed application using kubectl commands
- You can look at the application holistically using helm commands

#### Helm  Client Tool

- Allows you to install and manage applications in your cluster
- Makes it easier to configure and manage larger Kubernetes applications

#### Helm benefits

- Templating - Helm introduces templating into your applications
- Versioning - Helm applications are versioned and can be rolled back to an earlier version
- Application packaging - Helm makes getting started with a lot of K8s projects easy (kubernetes/charts project on GitHub)
