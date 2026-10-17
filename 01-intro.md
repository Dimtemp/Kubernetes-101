## local K8S
- minikube: dev and test
- kind: developer focussed

## kubectl
- for managing k8s

## Minikube
```bash
minikube start
kubectl get nodes
kubectl get pods --all-namespaces
```

## Namespaces
- logical separation and organization of resources within a cluster
```bash
kubectl get namespaces
```

default
- kube-system: Contains system services and components of Kubernetes.
- default: Default namespace for applications without explicit assignment.
- kube-public: Contains public resources that are visible to all cluster users.
- kube-node-lease: Used to manage node lease objects that are used for node health monitoring.

```bash
kubectl create namespace demo-namespace
```

## Pod
- smallest, independent unit in Kubernetes
- one (Single-Container Pod) or more containers (Multi-Container Pod)
- share resources such as network, storage, and configurations

```bash
kubectl apply -f 01-pod.yaml
kubectl get pods -n demo-namespace
kubectl config set-context --current --namespace=demo-namespace
# sets the default namespace for all subsequent commands in the current session
kubectl delete pod demo-pod
# assumes specific namespace
```
