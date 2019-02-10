## service(defines a logical set of Pods and a policy by which to access them.)
-  each Pod in a Kubernetes cluster has a unique IP address,to be a way of automatically reconciling changes among Pods so that your applications continue to function.
-  Services enable a loose coupling between dependent Pods.
- A Kubernetes Service is an abstraction which defines a logical set of Pods and a policy by which to access them - sometimes called a micro-service. 

- The set of Pods targeted by a Service is usually determined by a LabelSelector
- Pods IPs are not exposed outside the cluster without a Service. Services allow your applications to receive traffic. 
- Services can be exposed in different ways 

  -  ClusterIP:on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
  - NodePort: Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.
  - LoadBalancer: a fixed, external IP to the Service (LoadBlancer's)
  - ExternalName:Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name. No proxy is used. 
 
- selector in the spec:Endpoints object
- A Service routes traffic across a set of Pods. Services are the abstraction that allow pods to die and replicate in Kubernetes without impacting your application.
- Discovery and routing among dependent Pods (such as the frontend and backend components in an application) is handled by Kubernetes Services.
- can create a Service at the same time you create a Deployment by using
  --expose in kubectl.
- Services match a set of Pods using labels and selectors, a grouping primitive that allows logical operation on objects in Kubernetes. Labels are key/value pairs attached to objects and can be used in any number of ways:
   
    - dev,stg,production
    - version
    - classfiy用、分類
    
````
#create a new service
kubectl get pods
#a Service called kubernetes that is created by default when minikube starts the cluster
kubectl get services

#create new service
kubectl get deployments
kubectl expose deployment/deploymentName --type="NodePort" --port=8080
kubectl get services
#deployment name to be service name
kubectl describe services/deploymentName(serviceName) 
minikube ip
kubectl cluster-info
curl minikubeIp:serviceNodePort



#service label
#The Deployment created automatically a label for our Pod. 
kubectl describe deployment
kubectl get pods -l deploymentLabelName
kubectl get services -l deploymentLabelName
kubectl label pod podName app=v1
kubectl describe pods  podName

#delete a service
kubectl delete service -l app=v1
````

#serviece detail
```
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```
- a new Service object named “my-service” 
- which targets TCP port 9376 on any Pod
- with the "app=MyApp" label
- This Service will also be assigned an IP address (sometimes called the “cluster IP”), which is used by the service proxies (see below).
- service has no selector, the corresponding Endpoints object will not be created. You can manually map the service to your own specific endpoints

#Virtual IPs and service proxies
- Proxy-mode: userspace: kube-proxy watches the Kubernetes master for the addition and removal of Service and Endpoints objects. For each Service it opens a port (randomly chosen) on the local node. Any connections to this “proxy port” will be proxied to one of the Service’s backend Pods (as reported in Endpoints). Which backend Pod to use is decided based on the SessionAffinity of the Service. Lastly, it installs iptables rules which capture traffic to the Service’s clusterIP (which is virtual) and Port and redirects that traffic to the proxy port which proxies the backend Pod. By default, the choice of backend is round robin.
- Proxy-mode: iptables:  kube-proxy watches the Kubernetes master for the addition and removal of Service and Endpoints objects. For each Service, it installs iptables rules which capture traffic to the Service’s clusterIP (which is virtual) and Port and redirects that traffic to one of the Service’s backend sets. For each Endpoints object, it installs iptables rules which select a backend Pod. B


#commands
- kubectl describe svc my-nginx
- kubectl get ep my-nginx
- kubectl get svc my-nginx -o yaml | grep nodePort -C 5
# get nodes' external ip
- kubectl get nodes -o yaml | grep ExternalIP -C 1
- kubectl edit svc kimakuri-1
