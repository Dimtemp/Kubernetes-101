# Deployment

- central control object
- declaratively defines the desired state of an application and automatically enforces it
- which container images should be used
- how many Pod replicas should be operated
- which specific configurations (such as environment variables, resource limits, etc.) apply
- Rolling Update: When there is a change – for example, through a new container image – the entire application is not immediately updated, but the 
- old Pods are gradually replaced by new ones
- remains continuously available during update
- error occurs: rollback

```bash
kubectl apply -f 06-deployment.yaml
# clean up:
kubectl delete deployment demo-deployment
```
