# Volumes
- a directory that is made available to a Pod
- can exist beyond the restart of a Pod

# emptyDir
- created when a Pod starts and deleted when the Pod is removed
- temporary data / short-term caches

# hostPath
- Mounts a directory on the host node into the Pod
- Not highly available and dependent on the node where the Pod runs
- special use cases (e.g., access to logs of the host system)

# configMap and secret
- provide configurations or sensitive information (passwords, API keys)
- can be mounted as files or environment variables into the Pod

# persistentVolumeClaim
- The central concept for application-independent, persistent storage
- dynamically binds a Persistent Volume (PV) to the Pod

# Persistent Volumes (PV)
- specially reserved storage areas that exist independently of the lifecycle of individual Pods and are provided by the cluster
- details of their physical implementation (NFS share, iSCSI LUN, cloud storage, ...) are abstracted by the Kubernetes cluster
- can define important properties
    - available storage capacity
    - access mode, e.g.:
         - ReadWriteOnce
         - ReadOnlyMany

# Persistent Volume Claims (PVC)
- interface between applications and provisioned volumes
- Instead of dealing with the details of a specific storage system or configuration steps, the application or Pod simply formulates the desired storage requirements through a PVC
- example: minimum size and the appropriate access mode
    - Kubernetes searches for an existing PV that meets these requirements or creates a new volume using a Storage Class if needed
- aka dynamic provisioning, enables flexible and automated management of storage without administrators having to manually perform each step

# Storage Classes
- basis for dynamic provisioning
- define what type of storage is provided and thus establish relevant performance and security features
- SSD or HDD, encrypted or unencrypted, local or cloud – various Storage Classes can exist in parallel to serve diverse requirements
- when a PVC requests a specific Storage Class, Kubernetes automatically creates a suitable Persistent Volume with the desired properties via this class
- this approach enables teams to quickly and easily consume new storage while ensuring that each application receives exactly the storage resources it needs

```bash
kubectl apply -f 40-pv.yaml
kubectl apply -f 40-pvc.yaml
kubectl apply -f 40-pod.yaml
kubectl get pods
kubectl describe pod demo-pod
```

Make sure the volume is correctly mounted and the Pod is running.
