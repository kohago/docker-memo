#context(環境の切り替え)
```
kubectl config current-context
kubectl config set current-context
kubectl config get-contexts
kubectl config set-context xxxxxx

kubectl config view

#delete kube config
kubectl config delete-cluster someclusters
kubectl config delete-context somecontext

#get azure kube credentials 
az aks get-credentials --resource-group xx --name=aks-stage xx
```

#gke
```
gcloud config list
gcloud container clusters create xxx

#generate entry for cluster
gcloud container clusters get-credentials clusterName --region==xxxx


```
