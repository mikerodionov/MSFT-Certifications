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

Kubeadm - helps you install and setup a K8s cluster.

Kubeadm allows creation of K8s clusters on machines running
- Ubuntu 16.04+
- Debian 9
- CentOS 7
- RedHat EL 7

### kops
