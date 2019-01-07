# How to access
- In Kubernetes, nodes, pods and services all have their own IPs
- In many cases, the node IPs, pod IPs, and some service IPs on a cluster will not be routable, so they will not be reachable from a machine outside the cluster, such as your desktop machine.
- Use a service with type NodePort or LoadBalancer to make the service reachable outside the cluster. See the services and kubectl expose documentation.

  
