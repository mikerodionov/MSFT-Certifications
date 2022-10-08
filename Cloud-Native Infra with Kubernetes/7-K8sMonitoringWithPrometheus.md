# Kubernetes: Monitoring with Prometheus

## Enable Prometheus Monitoring

### Logging VS monitoring

Logging

- Data about the state or an event of an application or service
- Often long form providing detail about the state change

Metering

- Data about the rate of change
- Time-series based

```Bash
# Looking at the logs in Unix/Linux
ls /var/log/syslog
/var/log/syslog
cat /var/log/syslog
# To see resourcr usage by process updated every second
top
```

### Monitoring Kubernetes

Most frequently used tool - Prometheus, but there are other tools.

1. cAdvisor - originally developed by Google, container monitoring solution exposing individual container metrics (memory, CPU, network, etc.); only collects containers data

2. Heapster - a tool to collect cAdvisor data from kubelet to central storage, takes into account existence of other Kubernetes entities other than containers, although it uses cAdvisor data; Heapster does not have storage component.

3. Prometheus - a generic pull-based time-series capture, storage and alerting service with Kubernetes integration; able to cature any time-series data and store it; allows monitro application level

### Enabling Prometheus Monitoring

02-promoperator.yml

Role binding - ClusterRoleBinding ServiceAccount - prometheus-operator
apiGroup: [""]

Prometheus manifest - 03-prometheusalerts.yml

```Bash
ls -l
# Prometheus manifests
# Prometheus operator - Kubernetes controller class, which runs Prometheus nodes
# it can be run stand alone outside of K8s env or within it with simple manifest, but these deployment options limit system's ability to dynamically behave and interact with K8s environment
# Prometheus alrer service (alert manager main, alert service, alertmanager-secret)
more 02-promoperator.yaml
# RBAC
more 01-promrbac.yml
kubectl create -f 01-promrbac.yml
kubectl create -f 02-promoperator.yml
kubectl get pods
kubectl logs podname
kubectl create -f 03-prometheusalerts.yml
```
