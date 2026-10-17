# Sidecar
	- additional container extends the functionality of the main container
	- logging, monitoring, proxy services, networking (Envoy in Istio)...

```bash
kubectl apply -f 02-sidecar.yaml
```

In the example, a Pod with two containers is deployed in the Namespace demo-namespace: main-app with the Nginx image and log-collector with the Busybox image. Both containers share an empty directory (emptyDir) to store Nginx logs. The log-collector container reads the Nginx logs and outputs them. The main-app container additionally has a port defined to make the Nginx web server available externally. The log-collector container uses the command tail -n+1 -f /var/log/nginx/access.log to continuously output the logs.

inspect logs from sidecar container (assumes pod has deployed)
```bash
kubectl logs sidecar-example -c log-collector -f
# forward nginx port 80 to host port 8080
kubectl port-forward pods/sidecar-example 8080:80
# open web browser: http://localhost:8080
# stop port fwd using ctrl-c and clean up
kubectl delete -f 02-sidecar.yaml
```
