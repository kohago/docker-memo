## pod autoscaling
- podのHorizontal pod autoscaling
- scaled resource object:  
  deployment,replicaSet,replicationController,statusfullset

-
```
apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: php-hpa
  namespace: default
spec:
  scaleTargetRef: # ここでautoscale対象となる`scaled resource object`を指定
    apiVersion: apps/v1
    kind: Deployment
    name: php-deploy
  minReplicas: 1 # 最小レプリカ数
  maxReplicas: 5 # 最大レプリカ数
  metrics:
  - type: Resource
    resource:
      name: cpu
      targetAverageUtilization: 50 # CPU使用率が常に50%になるように指定

# メモリベースのオートスケーリングはKubernetes1.8から導入された。
CPUベースオートスケーリングと同じように使用できる。autoscaling/v2beta1
```

- time span
  scale up(pod数を増やす)イベントは3分おきに発生する。
  scale down(pod数を減らす)イベントは5分おきに発生する。

- algorithm:
  https://github.com/kubernetes/community/blob/master/contributors/design-proposals/autoscaling/horizontal-pod-autoscaler.md#autoscaling-algorithm

- command
```
kubectl get HorizontalPodAutoscaler xxx
kubectl describe HorizontalPodAutoscaler xxx
ubectl get pod

```
## node autoscaling
- node pool
```
#オートスケーラーを有効にしたノードプールが含まれるクラスターを作る
gcloud container cluster create xxx-cluster --enable-autoscaling --min-nodes=1 --max-nodes=5

#あとnode poolを追加
gcloud container node-pools create xxx-pool --cluster=xxx-cluster --enable-autoscaling --min-nodes=1 --max-nodes=5
```
- preemptible pool + node pool
```
#固定したインスタンス数で作って、そして、別のPreemptibleノードプールでオートスケーリングをする
gcloud container create cluster xxx-cluster --num-nodes 3
gcloud beta container node-pools create xxx-preemptible-pool --cluster=xxx-cluster --preemptible --num-nodes=0
```
