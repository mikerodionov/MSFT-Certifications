# Kubernetes and Containers Orchestration 101

Orchestration - making multiple machines/hosts performing together so that app as a whole works while being distributed across hosts (scaling etc.). Use musical analogy with orchestra.

Old times - monolith architecture, giant/big machies ~ "big ball of mud"

Currently - microservices architecture

K8S Cluster - Master node/Conductor which orchestrates (scaling etc.)
  - Workers (nodes)
    - Pods
      - Containers

Docker Desktop has "Enable Kubernetes" option which starts single-node cluster when starting Docker Desktop.

Kubernetes uses YAML manifests to describe pods and other entities. Those YAML files are basically name-value pairs in hierarchical order (hierarchy defined by indentation) which describe your pods or other entities.
Pod description includes image, resource specs, 