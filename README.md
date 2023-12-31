# Deployments' Architecture

![alt text](https://github.com/rossenbergvillanuevaramboanga/aws-eks-EBS/blob/main/images/mysql-architecture-quotas-limits.jpg?raw=true)

## What is the difference between a ClusterIP and a NodePort?
### ClusterIP
Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default that is used if you don't explicitly specify a type for a Service. You can expose the Service to the public internet using an Ingress or a Gateway.
### NodePort
Exposes the Service on each Node's IP at a static port (the NodePort). To make the node port available, Kubernetes sets up a cluster IP address, the same as if you had requested a Service of type: ClusterIP.

## Lifecycle of a Volume and Claim
### Provisioning
#### Dynamic Provisioning
When none of the static PVs the administrator created match a user's PersistentVolumeClaim, the cluster may try to dynamically provision a volume specially for the PVC. This provisioning is based on StorageClasses: the PVC must request a storage class and the administrator must have created and configured that class for dynamic provisioning to occur. Claims that request the class "" effectively disable dynamic provisioning for themselves.

To enable dynamic storage provisioning based on storage class, the cluster administrator needs to enable the DefaultStorageClass admission controller on the API server. This can be done, for example, by ensuring that DefaultStorageClass is among the comma-delimited, ordered list of values for the --enable-admission-plugins flag of the API server component. For more information on API server command-line flags, check kube-apiserver documentation.

#### Static Provisioning
A cluster administrator creates a number of PVs. They carry the details of the real storage, which is available for use by cluster users. They exist in the Kubernetes API and are available for consumption.

## Init Containers
A Pod can have multiple containers running apps within it, but it can also have one or more init containers, which are run before the app containers are started.

Init containers are exactly like regular containers, except:

Init containers always run to completion.
Each init container must complete successfully before the next one starts.
If a Pod's init container fails, the kubelet repeatedly restarts that init container until it succeeds. However, if the Pod has a restartPolicy of Never, and an init container fails during startup of that Pod, Kubernetes treats the overall Pod as failed.

To specify an init container for a Pod, add the initContainers field into the Pod specification, as an array of container items (similar to the app containers field and its contents). See Container in the API reference for more details.

The status of the init containers is returned in .status.initContainerStatuses field as an array of the container statuses (similar to the .status.containerStatuses field).

## Probes
### Liveness Probe
Kubernetes uses liveness probes to know when to restart a container.
Liveness probes could catch a deadlock, where an application is running, but unable to make progress and restarting container helps in such state.
### Readiness Probe
Kubelet uses readiness probes to know when a container is ready to accept traffic.
When a Pod is not ready, it is removed from Service Load Balancers based on this readiness probe signal. 
### Startup Probe
Kubelet uses startup probes to know when a container application has started.
Firstly this probe disables liveness & readiness checks until it succeeds ensuring those pods don't interfere with app startup.
This can be used to adopt liveness checks on slow starting containers, avoiding them getting killed by the kubelet before they are up and running.

#### Options to define Probes
- Check using Commands
- Check using HTTP GET Request
- Check using TCP


## Policies
### Limit Ranges
A LimitRange is a policy to constrain the resource allocations (limits and requests) that you can specify for each applicable object kind (such as Pod or PersistentVolumeClaim) in a namespace.
### Resource Quotas
A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per namespace. It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that namespace.
