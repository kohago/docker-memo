##Cluster=Master+Node
  - The Master is responsible for managing the cluster
    - scheduling applications,
    - maintaining applications' desired state,
    - scaling applications,
    - rolling out new updates.

  - A node is
    - a VM or a physical computer serves as a worker machine in a Kubernetes cluster.
    - have tools for handling container operations
    - has a Kubelet an agent for managing the node and communicating with the Kubernetes master
  
  - production traffic should have a minimum of three nodes
  - deploy applications(containerized applications) on Kubernetes is
    -  tell the master to start the application containers
    -  the master schedules the containers to run on the cluster's nodes (using k8s api|kubectl)
    
  #minikube(local cluster tool)
  - install
    - brew update && brew install kubectl && brew cask install docker minikube virtualbox
    - https://github.com/kubernetes/minikube/releases
  
  - create a local k8s cluster
    - create a vm
    - create a cluster(kubelet(node?),kubeadm(master)) in that vm
   ```
   minikube start
       Starting local Kubernetes v1.12.4 cluster...
       Starting VM...
       Downloading Minikube ISO
        178.88 MB / 178.88 MB [============================================] 100.00% 0s
       Getting VM IP address...
       Moving files into cluster...
       Downloading kubelet v1.12.4
       Downloading kubeadm v1.12.4
       Finished Downloading kubeadm v1.12.4
       Finished Downloading kubelet v1.12.4
       Setting up certs...
       Connecting to cluster...
       Setting up kubeconfig...
       Stopping extra container runtimes...
       Starting cluster components...
       Verifying kubelet health ...
       Verifying apiserver health ...Kubectl is now configured to use the cluster.
       Loading cached images from config file.

  ```
  
   - kubectl
   
    ```
    #The client version is the kubectl version; 
    #the server version is the Kubernetes version installed on the master
    
    kubectl version
      kubectl version
      Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.3", GitCommit:"2bba0127d85d5a46ab4b778548be28623b32d0b0", GitTreeState:"clean", BuildDate:"2018-05-21T09:17:39Z", GoVersion:"go1.9.3", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"12", GitVersion:"v1.12.4", GitCommit:"f49fa022dbe63faafd0da106ef7e05a29721d3f1", GitTreeState:"clean", BuildDate:"2018-12-14T06:59:37Z", GoVersion:"go1.10.4", Compiler:"gc", Platform:"linux/amd64"}
    ```
   - see cluster details 
   
    ```
    kubectl cluster-info
      Kubernetes master is running at https://192.168.99.100:8443
      KubeDNS is running at https://192.168.99.100:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    kubectl get nodes
      NAME       STATUS    ROLES     AGE       VERSION
      minikube   Ready     master    39m       v1.12.4
     
    ```
  - In Kubernetes, nodes, pods and services all have their own IPs
  
