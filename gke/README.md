- cli
  - install 
    - https://cloud.google.com/sdk/docs/install?hl=ja
    - python3.7 
  - login
    - gcloud auth login
    - gcloud config list
    - gcloud config set ...
    
``````
gcloud components list
gcloud components install kubectl

gcloud config set project xxx
gcloud config set account xxxx
gcloud config set compute/zone asia-northeast1-a

//create cluster master machine(GCE) and worker machines
gcloud container clusters create test-cluster

//get cluster credentials
//Fetching cluster endpoint and auth data.
//kubeconfig entry generated for test-cluster.
gcloud container clusters get-credentials test-cluster

//and then deploy containered apps to the cluster
//kubernetes engine manage cluser use k8s objects(deployment,Service[accessRules,LoadBalancerRules])
//create deployment using image ,port
kubectl run hello-deployment --image gcr.io/google-samples/hello-app:v1.0 --port 8080
//create a service to expose the deployment to public
kubectl expose deployment hello-deployment --type "LoadBalancer"

//check the created Service,get the global ip
//access http://xxx.xx.xx:port/
kubectl get service hello-deployment

//can delete the service to save money
kubectl delete service hello-service
gcloud container clusters delete test-cluster

Deployment=Pod(コンテナ）
```
