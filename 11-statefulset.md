# StatefulSets
- suitable for stateful applications where the order of deployment {0..N-1} and unique identities are important
- names of the Pods in a StatefulSet have the following form: <statefulset-name>-<ordinal-index>
- ideal for applications like databases that need a stable identity and ordered deployment

```bash
kubectl apply -f 11-statefulset.yaml
```

- StatefulSet postgres is created
- defines three Pods with the label app: postgres and a PostgreSQL container
- Each Pod has its own PersistentVolumeClaim (PVC) postgres-data that requests 1 GB of storage
- PersistentVolumeClaim can be defined in the StatefulSet template to provide persistent storage for each Pod
- If the cluster is configured accordingly, a PersistentVolume (PV) is created for each Pod and bound to the Pod. The POSTGRES_PASSWORD is hardcoded, but in practice, it should be stored as a Secret and included via environment variables.

```bash
kubectl get statefulset postgres
kubectl get pods -l app=postgres
kubectl get pods -l app=postgres -w
kubectl delete statefulset postgres
```
