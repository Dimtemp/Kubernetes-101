# ConfigMaps

- enable the separation of configuration data from container images
- store non-sensitive configuration data as key-value pairs
- can be included in a Pod as environment variables, command arguments, or configuration files

```bash
kubectl apply -f 09-configmaps.yaml
```