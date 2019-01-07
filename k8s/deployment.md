##Deployment
- Deployment is a part of cluster master( in cluster master)
- Deployment configuration(instructs Kubernetes how to create and update instances of your application)
- Once the application instances are created, a Kubernetes Deployment Controller 
  - continuously monitors those instances
  - kubectl deployment provide a self-healing　mechanism to address machine failure or maintenance.
  - very better than deployment,installation scripts
 
 ###create a deployment to deploy your app
 
 - need to specify the container image for your application
 - need to specify the number of replicas that you want to run. (default is 3?)

```
#searched for a suitable node where an instance of the application could be run
#scheduled the application to run on that Node
#let the cluster to reschedule the instance on a new Node when needed

kubectl run deploymentName --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080
  deployment.apps "k8sbootcamp" created

#1 deployment running a single instance of your app. 
#The instance is running inside a Docker container on your node (in a pod)
kubectl get deployments

#  
```

# see deoloyed app
- within a private, isolated network. 
  - cluster-wide:By default they are visible from other pods and services within the same kubernetes cluster, 
  - private: but not outside that network. 
  - kubectl is endpoint: we're interacting through an API endpoint to communicate with our application
  - kubectl can create proxy
  
  ```
  #http://127.0.0.1:8001/version
  #APIs ↑　hosted through the proxy endpoint, now available ↑
  kubectl proxy
  
  #http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
  # ↑　↑　The API server（Cluster Master） will automatically create an endpoint for each pod, based on the pod name
  kubectl get pods
  

  ```
 
 # create,update by file
```
 #create
 kubectl create -f xx.ymal
 
 #update
 kubectl apply -f yy.ymal

```