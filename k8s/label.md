- The set of pods that a service targets is defined with a label selector
- You need to add selector in spec of Deployment.And also, these selector should match with labels in PodTemplate.
  template's label must be the same as selector's label
  
  ```
  apiVersion: apps/v1beta1
  kind: Deployment
  metadata:
    name: moverick-mule-pre
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: moverick-mule-pre
        commit: $CI_COMMIT_SHA
    strategy:
      type: RollingUpdate
      rollingUpdate:
        maxSurge: 1
        maxUnavailable: 1
    template:
      metadata:
        labels:
          app: moverick-mule-pre
          commit: $CI_COMMIT_SHA
 ```
