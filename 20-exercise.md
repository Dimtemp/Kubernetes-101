# Exercise
```bash
kubectl apply -f 20-deployment.yaml
kubectl apply -f 21-service.yaml
kubectl get pods
kubectl get service demo-service
```

Use the StatefulSet manifest from 3.1 StatefulSets. Adjust the manifest as follows:

Steps:
    Create a Secret with the following key-value pair:
    password: secretpassword
    Include the Secret in the StatefulSet:
    Include the Secret as an environment variable POSTGRES_PASSWORD in the StatefulSet.
    Change the number of replicas:
    Change the number of replicas from 3 to 2.


22-error.yaml
four errors in the manifest. Find out what is wrong and correct it.
