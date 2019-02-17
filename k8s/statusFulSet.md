
 ##statusFulSet
 
 - Kubernetes Engine が、スケジュールされた場所に関係なく保持する、一意で永続的な ID と固有のホスト名を持つポッドのセットを表します。
 - 指定された StatefulSet ポッドの状態情報とその他の復元性のあるデータが、StatefulSet に関連付けられた永続ストレージ オブジェクトで保持されます。
 - StatefulSet は、ポッド テンプレートを使用します。これには、そのポッドの仕様が含まれています。ポッドの仕様によって、各ポッドの外観、つまり、コンテナ内で実行するアプリケーション、マウントするボリューム、ラベルとセレクタなどが決定されます。
 - StatefulSets are valuable for applications that require one or more of the following.
   - Stable, unique network identifiers.
   - Stable, persistent storage.
   - Ordered, graceful deployment and scaling.
   - Ordered, automated rolling updates.
 
 
 ##statusFulSetの更新
 
 - OnDelete
 - RollingUpdate
 ```
kubectl rollout status statefulset [STATEFULSET_NAME]
```  

