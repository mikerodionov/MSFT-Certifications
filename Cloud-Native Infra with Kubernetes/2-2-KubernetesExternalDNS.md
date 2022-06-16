# Use of ExternalDNS to manage Kubernetes resource names

- Kubernetes has internal core DNS resolution to allow name resolution/service discovery inside of the cluster
- For external resolution you will need to sync with external DNS, to enable external access via friendly names
- You can sync with any type of external DNS - on prem or from cloud providers
- ExternalDNS - Kubernetes project to sync K8s services/ingress etc. with external DNS

Service > Ingress > DNS name < user/consumer

DNS names are the last bastion for pets :) We still do not treat them as a cattle.

## ExternalDNS overview

- ExternalDNS will automatically create DNS records from Kubernetes resources
  - Services (with the annotation external-dns.alpha.kubernetes.io/hostname)
  - Ingresses (automatically)
- It requires domain name
- Domain name should be configurable via API

- Recommended TTL 5 mins
- [Look up for right tutorial](https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/)
- [ExternalDNS helm chart from Bitnami](https://artifacthub.io/packages/helm/bitnami/external-dns)
- helm upgrade --install external-dns bitnami/external-dns --namespace external-dns --create-namespace --ser provider=PROVIDE_NAME set providerapiToken = $TOKEN

## Testing ExternalDNS

- For services you need annotation - external-dns.alpha.kubernetes.io/hostname

```Bash
# Annotate nginx service (LoadBalancer type will produce 1 A record, for NodePort it will create A record for every node)
kubectl annotate service web external-dns.alpha.kubernetes.io/hostname=nginx.yourdomain.com
# Check ExternalDNS logs
kubectl logs -n external-dns -l app.kubernetes.io/name=external-dns
# It takes some time to start/process ~ 5 mins, next verify accessing domain
# TXT records are used by ExternalDNS to track ownership of the records (heritage/owner=external-dns)
```


## ExternalDNS with AWS

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