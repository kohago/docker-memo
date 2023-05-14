- top
```
kubectl top node 
kubectl top pod

```

- describe
```
kube describe node xxx

Non-terminated Pods:         (5 in total)
 Namespace                  Name                       CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                       ------------  ----------  ---------------  -------------  ---
  default                    datadog-agent-zqql2        0 (0%)        0 (0%)      0 (0%)           0 (0%)         4d11h
  kube-system                coredns-8bbb65c89-sd5pl    100m (5%)     0 (0%)      70Mi (1%)        170Mi (3%)     4d11h
  kube-system                kube-proxy-h6tvh           100m (5%)     0 (0%)      0 (0%)           0 (0%)         4d11h
  kube-system                XXXXXXXX-vs2xm             75m (3%)      150m (7%)   225Mi (4%)       600Mi (13%)    8h
  XXXXXX                     XXXXXX-67df5fc4fd-wcjjp    0 (0%)        0 (0%)      1G (20%)         3G (62%)       16h

Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Mon, 08 Jun 2020 09:38:50 +0900   Mon, 08 Jun 2020 09:38:50 +0900   RouteCreated                 RouteController created a route
  MemoryPressure       False   Tue, 08 Sep 2020 08:31:13 +0900   Wed, 10 Jun 2020 16:53:42 +0900   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Tue, 08 Sep 2020 08:31:13 +0900   Wed, 10 Jun 2020 16:53:42 +0900   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Tue, 08 Sep 2020 08:31:13 +0900   Wed, 10 Jun 2020 16:53:42 +0900   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Tue, 08 Sep 2020 08:31:13 +0900   Wed, 10 Jun 2020 16:53:44 +0900   KubeletReady                 kubelet is posting ready status. AppArmor enabled

```

```
kube describe pod xxx --namespace xxx
    Events:
      Reason                        Message
      ------                        -------
      FailedScheduling      No nodes are available that match all of the following predicates:: Insufficient cpu (3).
```


- request and limit
```
apiVersion: v1
kind: Pod
metadata:
  name: cpu-demo
  namespace: cpu-example
spec:
  containers:
  - name: cpu-demo-ctr
    image: vish/stress
    resources:
      limits:
        cpu: "1"
      requests:
        cpu: "0.5"
    args:
    - -cpus
    - "2"
```