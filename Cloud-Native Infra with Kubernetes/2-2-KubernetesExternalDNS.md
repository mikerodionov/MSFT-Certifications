# Use of ExternalDNS to manage Kubernetes resource names

- Kubernetes has internal core DNS resolution to allow name resolution/service discovery inside of the cluster
- For external resolution you will need to sync with external DNS, to enable external access via friendly names
- You can sync with any type of external DNS - on prem or from cloud providers
- ExternalDNS - Kubernetes project to sync K8s services/ingress etc. with external DNS

Service > Ingress > DNS name < user/consumer

DNS names are the last bastion for pets :) We still do not treat them as a cattle.

```Bash
# AWS Setup w/o helm chart
# Get namespaces
kubectl get ns
# Get LBs
kubectl get po -n kube-system
# Check Setting up ExternalDNS for Services on AWS tutorial on GitHub
# Requires setting IAM permissions on DNS side, we can go granular and grant permissions only to specific pod
# Check service accounts
kubectl get sa
# Create service account and link policy, policy has to be created beforehand
eksctl create iamserviceaccount --name external-dns --namespace default --cluster demo --attach-policy-arn "arn:aws:iam::323238900174:policy/AllowExternalDNSUpdates" --approve
# Check created service account
kubectl get sa external-dns -o yaml L
```