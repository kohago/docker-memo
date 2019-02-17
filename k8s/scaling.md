#Scaling
- Scaling is accomplished by changing the number of replicas in a Deployment
- Kubernetes also supports autoscaling of Pods
- Scaling to zero is also possible, and it will terminate all Pods of the specified Deployment
-  Services have an integrated load-balancer that will distribute network traffic to all Pods of an exposed Deployment
- Services will monitor continuously the running Pods using endpoints, to ensure the traffic is sent only to available Pods.
- Once you have multiple instances of an Application running, you would be able to do Rolling updates without downtime

- scaling up
```

kubectl get deployments
kubectl get nodes -o wide

kubectl scale deployments/deploymentName --replicas=4

kubectl get deployments

#There are 4 Pods now, with different IP addresses. 
kubectl get nodes -o wide

#The change was registered in the Deployment events log.(See Event Tag)
kubectl describe deployments/deploymentname
```

- see loadBalanceing between 4 replicas
```
kubectl get services
kubectl describe services/serviceName #see sevice's endpoint!!
minikube ip

#↓Do many times
curl minikubeIp:ServiceNodePorts

```

- scaling down
```
kubectl scale deployments/deploymentName --replicas=2
kubectl get deployments
kubectl get nodes -o wide

```

- hpa(pod Horizental pod scaling)
```
kubectl get hpa
kubectl del hpa
```

- cluster scaling
```
gcloud container clusters describe xxx
gcloud container clusters update xxx --enable-autoscaling --min-node=2 --max-node=20 --node-pool xxx

gcloud container clusters update [CLUSTER_NAME] --enable-autoscaling \
    --min-nodes 1 --max-nodes 10 --zone [COMPUTE_ZONE] --node-pool default-pool
```
- node-pool
 ```
 gcloud container node-pools create [POOL_NAME] --cluster [CLUSTER_NAME] \
     --enable-autoscaling --min-nodes 1 --max-nodes 5 [--zone [COMPUTE_ZONE]
 ```
 
 


