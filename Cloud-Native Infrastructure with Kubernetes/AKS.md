# AKS

## Kubernetes on Azure

### Deployment models

- AKS
  - **Master node(s)** - AKS control plane, Azute handles everything here - we do not see/manage master nodes or care about control plane scaling, we only provision worker node pools
    - API Server
    - Scheduler
    - etcd
    - Controller manager
  - Worker node(s)
    - kubelet
    - service proxy
- API/CLI
  - kubectl
  - ajax
  - etc.

### Create Azure Container Registry

```Bash
# Create private ACR
az group create --mame rg-name --location eastus
az acr create --resource-group rg-name --acr-name --sku Basic
az acr login --name --acr name
docker images
```

### Push a container to the registry

### Verify container registry image
