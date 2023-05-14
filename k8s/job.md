- what is job
```
Jobは1つ以上のPodを作成し、指定された数のPodが正常に終了することを保証する
kubectl describe jobs/pod-name

Jobが完了すると、Podは作成されなくなるが、Podを削除しない
Jobオブジェクトは完了後も残るため、ステータスを表示できる
ステータスを確認した後、古いJobを削除するのはユーザーの責任である

終了したリソースのためのTTLコントローラにまかせて、CleanUpしてくれる
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: pi-with-ttl
    spec:
      ttlSecondsAfterFinished: 100　#が終了してから100秒後に自動的に削除される
      template:
        spec:
          containers:
          - name: pi
            image: perl
            command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
          restartPolicy: Never
```

- how to use
```
init containerの仕事をjobに外出し。
jobを使ってk8s secretを事前に作る
単一のJobでPodを作成し、そのPodが他のPodを作成し、それらのPodに対するカスタムコントローラーのように動作するというもの
Jobはレプリケーションコントローラーを補完するも
Cron job

```