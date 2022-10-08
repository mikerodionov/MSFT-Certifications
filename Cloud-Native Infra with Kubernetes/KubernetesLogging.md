# Kubernetes Logging

- Every container generates logs and outputs them into STD OUT and STD ERR streams
- Kubernetes cluster has cluster-level logging (in addition to container logs it produces logs for containers crash, pods evcition, node death events)
- Cluster level logging life cycle is different that life cycle of logging of elements within the cluster
- Docker redirects ist 2 log streams to a K8s logging driver which writes logs to a file in JSON format