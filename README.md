# Deployments' Architecture

![alt text](https://github.com/rossenbergvillanuevaramboanga/aws-eks-EBS/blob/main/images/mysql-architecture.jpg?raw=true)

### What is the difference between a ClusterIP and a NodePort?
#### ClusterIP
Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default that is used if you don't explicitly specify a type for a Service. You can expose the Service to the public internet using an Ingress or a Gateway.
#### NodePort
Exposes the Service on each Node's IP at a static port (the NodePort). To make the node port available, Kubernetes sets up a cluster IP address, the same as if you had requested a Service of type: ClusterIP.



