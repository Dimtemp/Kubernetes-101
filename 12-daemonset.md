# DaemonSets
- ensure that a specific Pod runs on each node in the cluster
- useful for applications that need to run on every node, such as monitoring agents, logging tools, etc.

```bash
kubectl apply -f 12-daemonset.yaml
```
- a DaemonSet node-exporter is created that runs the Prometheus Node Exporter on every node in the cluster
- The Node Exporter collects metrics about the state of the node and makes them available via port 9100

```bash
kubectl get pods -n kube-system -l app=node-exporter -o wide
minikube node add
kubectl get pods -n kube-system -l app=node-exporter -o wide
kubectl delete daemonset node-exporter -n kube-system
```
