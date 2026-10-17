# Secrets

- similar to ConfigMaps
- storing sensitive data such as passwords, OAuth tokens, and SSH keys
- stored as Base64-encoded strings
- data encryption at the etcd level is possible

```bash
kubectl apply -f 10-secret.yaml
```
