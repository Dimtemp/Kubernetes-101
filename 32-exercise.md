# Exercise
```bash
kubectl create deployment nginx –image=nginx
kubectl expose deployment nginx –port=80 –type=NodePort
# determine the port:
kubectl get service nginx
# access http://<NodeIP>:<NodePort>
minikube service nginx –url
# http://192.168.49.2:30205
# trouble: 
kubectl describe pod <nginx-pod-name>
```
