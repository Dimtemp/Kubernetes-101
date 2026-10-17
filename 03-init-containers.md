# Init containers
- run before the regular containers of a Pod
- fulfill dependencies, load configurations, or cover waiting times for external resources
- when all Init Containers have successfully completed, the main container starts
- executed sequentially and cannot run in parallel: second Init Container only starts after the first has successfully terminated

```bash
kubectl apply -f 03-init-containers.yaml
# inspect:
kubectl get pods -w
```

combine with sidecar
```bash
kubectl apply -f 04-sidecar-and-init.yaml
```
