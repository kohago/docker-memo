## network
- Kubernetes assumes that pods can communicate with other pods, regardless of which host they land on
- Every pod gets its own IP address ,can be treated much like VMs or physical hosts
- There are requirements imposed on how you set up your cluster networking to achieve this

### Docker model
- Docker uses host-private networking
- creates a virtual bridge, called docker0 by default
- allocates a subnet from one of the private address blocks defined in RFC1918 for that bridge
- container that Docker creates, it allocates a virtual Ethernet device (called veth) which is attached to the bridge. 
  The veth is mapped to appear as eth0 in the container, using Linux namespaces. 
  The in-container eth0 interface is given an IP address from the bridge’s address range.
- The result is that Docker containers can talk to other containers only if they are on the same machine (and thus the same virtual bridge). 
   Containers on different machines can not reach each other
- In order for Docker containers to communicate across nodes, there must be allocated ports on the machine’s own IP address  (port mapping)

### Kubernetes model
- all containers can communicate with all other containers without NAT
- all nodes can communicate with all containers (and vice-versa) without NAT
- the IP that a container sees itself as is the same IP that others see it as

### pod's ip
- In reality, Kubernetes applies IP addresses at the Pod scope.This is called the “IP-per-pod” model.
- containers within a Pod can all reach each other’s ports on localhost
- 