### replicaSet COPYのセット

- maintain a stable set of replica Pods running at any given time.
- A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number.
- When a ReplicaSet needs to create new Pods, it uses its Pod template.
```aidl
replicas: 1
  template:
```
- a Deployment is a higher-level concept that manages ReplicaSets 
  and provides declarative updates to Pods along with a lot of other useful features. 
  Therefore, we recommend using Deployments instead of directly using ReplicaSets,
   unless you require custom update orchestration or don’t require updates at all
