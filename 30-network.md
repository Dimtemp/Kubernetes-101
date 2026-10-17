# Services
A Kubernetes Service functions as a stable abstraction layer above individual Pods and ensures that an application remains accessible under a constant IP address or a consistent DNS name. This is of particular importance because Pods can “die” at any time and be automatically replaced by Kubernetes, which generally results in changing IP addresses. The Service thus provides the application with a consistent identity and simultaneously performs load balancing by automatically distributing incoming traffic across all Pods selected via labels. In this way, external clients or other Pods in the cluster never need to know the changing Pod IPs directly, but always access the application via the Service IP or Service DNS name.

Kubernetes offers different service types for various use cases. The most common type is the so-called ClusterIP Service, where the Service is only visible internally in the cluster and therefore cannot be reached from outside. This is particularly suitable for microservices that only need to communicate with each other without receiving external traffic. If, on the other hand, you want to enable direct access from outside, you can use the NodePort Service, which opens a port in the range 30000–32767 on each cluster node. This approach is well-suited for development or test environments where you need external access to your application without much effort. In productive environments, particularly in the cloud, the LoadBalancer Service is advantageous. This ensures that an external load balancer is automatically set up, which distributes the data traffic evenly to all involved Pods. This not only allows for smooth high-availability operation but also offers options for automatic failover and scaling scenarios.

Overall, Services greatly facilitate the integration of applications by providing an abstract and reliable interface. Whether internal service traffic or external access: with the right configuration and the right service type, you can provide both secure and highly available access to the application in the Kubernetes cluster.

# Ingress
An Ingress in Kubernetes defines detailed HTTP and HTTPS routing rules that control access to different services in the cluster. Unlike a LoadBalancer Service, which only publishes a single service, an Ingress allows different hosts and paths to be forwarded to different services. For example, requests to api.example.com or to the path /api can be routed to a backend service, while calls to /frontend go to a different service.

This flexibility is achieved through host- and path-based routing that is stored in the Ingress manifest. Additionally, the Ingress Controller, which must be properly installed in the cluster, often handles SSL/TLS termination in many cases. This means it decrypts incoming HTTPS connections and then distributes the traffic unencrypted within the cluster to the appropriate services. Through this division, complex routing scenarios can be realized: multiple domain names can refer to different applications, paths can be rewritten, and additional configurations such as header manipulation or access rules are often also possible. In this way, an Ingress not only provides a central entry point for all web and API requests but also facilitates the securing and management of all data traffic.

Important Note Traditionally, an Ingress Controller (such as the Ingress NGINX Controller) must be installed in the cluster to transform the Ingress rules into actual proxy or load balancer configurations. Note, however, that Ingress NGINX is being retired. Its recommended successor is the Gateway API, which the examples below use; it likewise requires a corresponding controller (a Gateway API implementation) to be installed in the cluster.

# Network Policies
Network Policies allow fine-grained control over which Pods can communicate with each other or with external resources.

Ingress Traffic controls incoming connections to Pods, e.g., only selected Pods from a specific namespace are allowed to access a database Pod. Egress Traffic, on the other hand, regulates outgoing connections from Pods to other Pods, to the Internet, or to services outside the cluster. For example, you can prevent certain Pods from unintentionally establishing external connections.

Important: Network Policies apply in addition to Services and Ingress. Thus, it's possible to make a Service externally available while simultaneously protecting it from unauthorized internal access through Network Policies.

```bash
kubectl apply -f 30-network.yaml
```

Explanation:
- selector: app=demo links the Service with all corresponding Pods. 
- port: 80 is the port where the Service is accessible; targetPort: 80 is the container port. 
- type: ClusterIP means that the Service is only accessible internally in the cluster.

To test connectivity to the demo application, the following command can be used:

```bash
kubectl get svc demo-service
```

Shows information like the Cluster IP and port.

Port forwarding:

```bash
kubectl port-forward svc/demo-service 8080:80
```

If you then access http://localhost:8080 in the browser, you'll see (for example) the Nginx welcome page.
Ingress

Example for a Gateway API manifest (a Gateway plus an HTTPRoute) that listens on demo.localhost and forwards traffic to demo-service (Prerequisite: a Gateway API implementation installed in the cluster):

```bash
kubectl apply -f 30-gateway.yaml
```

Explanation: 
- The Gateway replaces the Ingress Controller entry point. gatewayClassName: nginx selects the installed Gateway API implementation (here the NGINX-based one) and the listeners define on which ports and protocols traffic is accepted. 
- The HTTPRoute replaces the routing rules of the former Ingress. parentRefs attaches it to the Gateway, hostnames and matches take over host- and path-based routing, and backendRefs forwards traffic to the Service. 
- The URLRewrite filter (ReplaceFullPath: /) reproduces the behavior of the former nginx.ingress.kubernetes.io/rewrite-target: / annotation, which is no longer expressed through vendor-specific annotations but as a first-class part of the Gateway API.

Important In local development environments, you may need to adjust /etc/hosts (Linux/Mac) or C:\Windows\System32\drivers\etc\hosts (Windows) so that demo.localhost points to the cluster IP or node IP.

Then access http://demo.localhost in the browser to see the application.

# trouble
```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

Install (if necessary) tools like curl or ping in your containers to check the network connection from Pod A to Pod B.

Service & Ingress Check
1. Make sure your Pod actually has the expected label that matches the Service selector.
1. Check if your Ingress Controller or Gateway API implementation is correctly installed and configured.

Note: When dealing with network problems always start with the innermost circle Pod IP and Pod logs and work your way step by step to Services and Ingress.
