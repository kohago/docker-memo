##trouble shooting 
```
kubectl exec -it my-pod --container main-app -- /bin/bash
#busybox is /bin/ash

kubectl delete deployments/deploymentName
kubectl create -f xxx.ymal
kubectl get pods
kubectl logs podName containerName
```

###cluster->node->pod
```
kubectl get nodes
kubectl get pods -o wide
gcloud computer ssh node-name
  curl http://pod-ip/
```

##port-forward(curl localhost)
```
kubectl port-forward <pod_name> <local_port>:<pod_port>
```
##Does the Service have any Endpoints
- check that the Pods you ran are actually being selected by the Service.


##hot configMap(patch pod)
```
 kubectl apply -f configMap.ymal
 kubectl patch deployment my-deployment --patch '{"spec": {"template": {"metadata": {"annotations": {"version/config": "20180411" }}}}}'
```

## top 
```
kubectl top pod
NAME   CPU(cores)   MEMORY(bytes)
```
