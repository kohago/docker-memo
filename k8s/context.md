#context(環境の切り替え)
```
kubectl config current-context
kubectl config set current-context
kubectl config get-contexts
kubectl config set-context xxxxxx

kubectl config view

#delete kube config
kubectl config unset clusters.someclusters

```

#gke
```
gcloud config list
gcloud container clusters create xxx

#generate entry for cluster
gcloud container clusters get-credentials clusterName --region==xxxx


```
