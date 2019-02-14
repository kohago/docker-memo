## What is DNS Service in k8s?

- k8s's dns service
- schedules a DNS Pod and Service on the cluster, and configures the kubelets to tell individual containers to use the DNS Service’s IP to resolve DNS names
- Every Service defined in the cluster (including the DNS server itself) is assigned a DNS name

```
a Service named foo in the Kubernetes namespace bar
   A Pod running in namespace bar can look up this service by simply doing a DNS query for foo.
   A Pod running in namespace quux can look up this service by doing a DNS query for foo.bar.
```

## Service DNS

- Normal Service

```
cluster IP of the Service = my-svc.my-namespace.svc.cluster.local
```

- Headless Service = without a cluster IP

```
my-svc.my-namespace.svc.cluster.local = set of IPs of the pods selected by the Service(round-robin the pod)
```

## Port DNS

```
_my-port-name._my-port-protocol.my-svc.my-namespace.svc.cluster.local

```

## Pod DNS

- pod-ip-address.my-namespace.pod.cluster.local
- hostname
  - when a pod is created, its hostname is the Pod’s metadata.name value.
  - The Pod spec has an optional hostname field, which can be used to specify the Pod’s hostname
- The Pod spec also has an optional subdomain field which can be used to specify its subdomain

```
 hostname.subdomain.namespace.pod.cluster.local 
```
- pod dns policy
 - specified in the dnsPolicy field of a Pod Spec
 - Default: inherits the name resolution configuration from the node that the pods run on
 - ClusterFirst: Cluster administrators may have extra stub-domain and upstream DNS servers configured
 - “Default” is not the default DNS policy. If dnsPolicy is not explicitly specified, then “ClusterFirst” is used.
 
 
