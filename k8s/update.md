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
##notified the Deployment to use a different image for your app and initiated a rolling update.
kubectl set image deployments/depolymentName deploymentName=v2 
```

```
#example: set nginx to ver1.9.1

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80

kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1

#notified the Deployment to use a different image for your app and initiated a rolling update.

the replica controller will rolling-update pods,can confirm history 

kubectl rollout history deployment/nginx-deployment
    deployments "nginx-deployment"
    REVISION        CHANGE-CAUSE
    1               kubectl create -f nginx.yaml --record
    2               kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1

kubectl rollout history deployment/nginx-deployment --revision=2
```

```
#confirm
kubectl get pods

# roll 回転 rollout 拡張する、増強する
#resume : return to do something(resume playing piano after the dinner)
#restart: have a new start
kubectl rollout status deployments/depolymentName
kubectl describe pods

```  

- undo
```
kubectl rollout undo deployment/deploymentName
kubectl get pods
kubectl describe pods　
```