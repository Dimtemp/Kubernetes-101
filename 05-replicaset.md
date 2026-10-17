# ReplicaSets
- automatically managed
- ensure the desired number of Pod replicas
- scalable deployment
- self-healing in case of failures

```bash
kubectl apply -f 05-replicaset.yaml
kubectl delete replicaset demo-replicaset
```
