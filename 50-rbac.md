# Role-based access control (RBAC)

permissions for all namespaces across the entire cluster, you use ClusterRoles
bind these roles to specific users, groups, or Service Accounts, RoleBindings and ClusterRoleBindings are available. 



## best practices
- Follow the principle of least privilege (PoLP).
- Perform regular audits.
- encryption at rest and in transit.
- Secrets should never be stored in source code or container images

```bash
kubectl apply -f 51-role.yaml
kubectl apply -f 52-rolebinding.yaml
```

The role pod-reader defines that demo-user may read, watch, and list Pods in the default namespace. 
Subsequently, the roleBinding binds the role pod-reader to the user demo-user.

```bash
kubectl apply -f 53-secret.yaml
kubectl apply -f 54-podmanifest.yaml
```

## Troubleshooting
```bash
kubectl auth can-i <verb> <resource>
kubectl describe cm <configmap-name>
kubectl describe secret <secret-name>
```
