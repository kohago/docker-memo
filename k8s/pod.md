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
 kubectl exec POD [-c CONTAINER] -- COMMAND [args...] [options]
 
 kubectl get pods

#起動logを確認できる
 kubectl describe pods　

```

- pod lifecycle、lifecycle events 
```
liftcycle 
    pending(image pull,Init containers)->Running->Successded-
    Failed
    Unknown (podとの通信が失敗、状態を取得できない）
lifecycle event
    PostStart: containerが作成された後、すぐに実行、コンテナのENTRYPOINTの実行前にpostStartが実行される保証はない
    preStop: containerが終了前に実行

example:
    apiVersion: v1
    kind: Pod
    metadata:
      name: lifecycle-check
    spec:
      initContainers:
      - name: init
        image: alpine
        command: ["sh", "-c"]
        args:
        - |
          echo Start at `date`; sleep 3; echo End at `date`. ; exit 0
      containers:
      - name: app
        image: alpine
        command: ["sh", "-c"]
        args:
        - |
          trap 'echo "Trapped SIGTERM"; sleep 3; echo "See you!!"; exit 0' TERM
    
          echo Start.
    
          while true
          do
              date
              sleep 1
          done
        lifecycle:
          postStart:
            exec:
              command:
                - sh
                - -c
                - |
                  echo hook postStart.
                  date
                  exit 0
          preStop:
            exec:
              command:
                - sh
                - -c
                - |
                  echo hook preStop.
                  date
                  exit 0
      restartPolicy: Never
      terminationGracePeriodSeconds: 30
```
  
- init containers

```
Podの初期化処理を行うために、一度だけ起動されるPod初期化専用のコンテナ
Podには複数のコンテナを動かすことができるが、コンテナが起動する順番を制御することはできない
init Containersはアプリケーションのコンテナが起動する前に起動するので、Podの初期化処理を行わせることができる
アプリケーションコンテナとは異なり、起動順番が制御されていて、1つのinit Containerの完了をまってから次のinit Containerが起動

```

- pod phase and pod condition
```
#lifecycle phase
pending,running,successed,unknown
```

```
PodにはPodStatusがある、それはPodが成功したかどうかの情報を持つPodConditionsである。
PodConditionの要素は
　lastProbeTime 最後調査の時間　　probe 調査
```

- probe
```
Probe は kubelet により定期的に実行されるコンテナの診断である、
診断を行うために、kubeletはコンテナに実装された ハンドラーを呼び、結果でコンテナのHealthを確認する
ExecAction->特定のCMDを実行する、機能できるか。startupProbeである。コンテナ中のAppは起動できたか？できていない場合、kuebletよりコンテナを回収する
TcpSocketAction->pingテスト、生きて返事できるか。生存確認のlivenessProbeである。生きてない場合は、kubeletはそのコンテナを回収
HttpGetAction->HttpGetできるか。readinessProbe。ServiceのRequestを受信できるか？できない場合、endpoittControllerよりServiceからそのpodのIPを削除

```