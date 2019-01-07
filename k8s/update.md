#update(Rolling update)
- Rolling updates allow Deployments' update to take place with zero downtime by incrementally updating Pods instances with new ones. 
- By default,one pod will be updated at once
- updates are verioned and can be reverted to previous versions
- the Service will load-balance the traffic only to available Pods during the update. 
- update
```
#show the current status
kubectl get deployments
kubectl get pods
kubectl describe pods

#update the image of the application to version 2
kubectl set image deployments/depolymentName deploymentName=v2 
#↑　notified the Deployment to use a different image for your app and initiated a rolling update.

#confirm
kubectl get pods
kubectl rollout status deployments/depolymentName
kubectl describe pods

```  

- updte
```
kubectl rollout undo deployment/deploymentName
kubectl get pods
kubectl describe pods　
```