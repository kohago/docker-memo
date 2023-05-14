- List
  list-items(Service,Deployment)
```
  apiVersion: v1
  kind: List
  items:
    - kind: Service
    - kind: Deployment
 ``` 

- spec 
    Objectの仕様である。Objectを作成する際Specを設定必要。
- status
 
- deployment 
```
.spec.strategyは古いPodから新しいPodに置き換える際の更新戦略を指定します。
.spec.strategy.typeは"Recreate"もしくは"RollingUpdate"を指定できます。
デフォルトは"RollingUpdate"です。

```

- container args
```
If you supply only args for a Container,
 the default Entrypoint defined in the Docker image is run with the args that you supplied.
 ENTRYPOINTは「必ず実行
 #see the entryPoint of an image:
    docker pull  someImage
    docker image inspect someImange 
```