## pod and node

### pod
- When you created a Deployment , Kubernetes created a Pod to host your application instance. 
- A Pod is a Kubernetes abstraction that represents a group of one or more application containers
- and some shared resources for those containers. 
  - Shared storage, as Volumes
  - Networking, as a unique cluster IP address
  - Information about how to run each container, such as the container image version or specific ports to use
- The containers in a Pod share an IP Address and port space, are always co-located and co-scheduled, and run in a shared context on the same Node.
- Pods are the atomic unit on the Kubernetes platform. 
- Each Pod is tied to the Node where it is scheduled, 
  - remains there until termination (according to restart policy) or deletion. 
  - In case of a Node failure, identical Pods are scheduled on other available Nodes in the cluster.
  
 ###node (Kubelet + A container runtime)
 - A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine,depending on the cluster. 
 - Each Node is managed by the Master
 ```
 kubectl logs - print the logs from a container in a pod
 
 #We can execute commands directly on the container once the Pod is up and running.
 kubectl exec - execute a command on a container in a pod
 
 kubectl get pods
 kubectl describe pods

```

  
 