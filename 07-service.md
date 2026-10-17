# Services

Well-defined network access to Pods

types
- ClusterIP: Standard service that is only accessible within the cluster.
- NodePort: Opens a port on each node, making the Service accessible externally.
- LoadBalancer: Uses an external load balancer to distribute traffic.

```bash
kubectl apply -f 07-service.yaml
```

- demo-service is created that refers to Pods with the label app: demo (from the previously created Deployment)
- forwards port 80 to target port 80
- The Service type ClusterIP enables the Service to be reached only within the cluster

```bash
kubectl get svc demo-service
kubectl port-forward svc/demo-service 8080:80
```
http://localhost:8080

nondeclarative:
```bash
kubectl expose deployment demo-deployment --type=LoadBalancer --name=demo-service --port=80 --target-port=80
```