```  
apiVersion: v1
kind: List
items:
  - kind: Deployment
    apiVersion: apps/v1
    metadata:
      name: demo
      labels:
        app: demo
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: demo
      strategy:
        type: Recreate
        rollingUpdate: null
      template:
        metadata:
          labels:
            app: demo
        spec:
          hostname: demo
          initContainers:
            - name: test-akv-access
              image: mcr.microsoft.com/azure-cli
              env:
                - name: SP_USER_NAME
                  value: "sp app id"
                - name: SP_PASSWORD
                  value: "sp passsword"
                - name: TENANT_ID
                  value: "tenant id"
                - name: AKV_NAME
                  value: "test akv" 
                - name: RESOURCE_GROUP
                  value: "resource group" 
                - name: AKS_CLUSTER
                  value: "test aks cluster" 
                - name: LOGIN_ID_AKV_SECRET
                  value: "test aks cluster" 
                - name: LOGIN_PASSWORD_AKV_SECRET
                  value: "test aks cluster" 
                - name: AKV_KEY
                  value: "test aks cluster" 

              command: ["/bin/bash","-c"]
              args:
                - |
                  az login --service-principal --username $SP_USER_NAME --password $SP_PASSWORD --tenant $TENANT_ID;
                  az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER;
                  export HSM_TEST_LOGINID=$(az keyvault secret show --vault-name $AKV_NAME --name $LOGIN_ID_AKV_SECRET | jq -r .value);
                  export HSM_TEST_LOGIN_PASSWORD=$(az keyvault secret show --vault-name $AKV_NAME --name $LOGIN_PASSWORD_AKV_SECRET | jq -r .value);
                  export DECRYPT_LOGINID=$(az keyvault key decrypt --vault-name=$AKV_NAME --name $AKV_KEY --algorithm RSA-OAEP --value $HSM_MIND_LOGINID 2> $0 | jq -r .result | base64 -d)
                  export DECRYPT_LOGIN_PASSWORD=$(az keyvault key decrypt --vault-name=$AKV_NAME --name $AKV_KEY --algorithm RSA-OAEP --value $HSM_MIND_LOGIN_PASSWORD 2> $0 | jq -r .result | base64 -d)
                 
                  curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl;
                  chmod +x ./kubectl;
                  mv ./kubectl /usr/local/bin/kubectl;
                  kubectl delete secrets test-secret;
                  kubectl create secret generic test-secret --from-literal=loginId=$DECRYPT_LOGINID --from-literal=loginPassword=$DECRYPT_LOGIN_PASSWORD;
          containers:
            - name: demo-test
              image:some 
              env:
                - name: LOGIN_ID
                  valueFrom:
                    secretKeyRef:
                      name: test-secret
                      key: loginId
                - name: LOGIN_PASSWORD
                  valueFrom:
                    secretKeyRef:
                      name: test-secret
                      key: loginPassword
                - name: OTHER_PATH
                  value: "/xx/xxx/xx.sh"
              ports:
                - containerPort: 10002
                  name: port1
              volumeMounts:
                - name: testVolume
                  mountPath: /opt/path1
          volumes:
            - name: testVolume
              secret:
                secretName: demo-config
```


###memo
- base64 password ,userName, encrypt. save into akv using portal UI

```
echo "1" | base64

echo "2" | base64

az keyvault key encrypt --algorithm RSA-OAEP --value xx --id  https://xxx.vault.azure.net/keys/xxx-key

az keyvault key encrypt --algorithm RSA-OAEP --value xx --id  https://xxx.vault.azure.net/keys/xxx-key
```

- create sp

```
az ad sp create-for-rbac -n "akv-access" --role contributor --scopes /subscriptions/xxx/resourceGroups/xxx
    Changing "akv-access" to a valid URI of "http://akv-access", which is the required format used for service principal names
    Creating a role assignment under the scope of "/subscriptions/xxx/resourceGroups/xxx"
      Retrying role assignment creation: 1/36
      Retrying role assignment creation: 2/36
      Retrying role assignment creation: 3/36
    {
      "appId": "xx",
      "displayName": "test-akv-access",
      "name": "xxx",
      "password": "xxx",
      "tenant": "xxx"
    }

```

- assgin akv access policy 
```
    Object ID: xx
```

- debug. watch 
```

kubectl get  secrets | grep test-secret

kubectl apply -f test.yaml | watch -n  2 kubectl get pod

kubectl get pods | grep test | awk '{print $1}' | xargs kubectl describe pod

kubectl get pods | grep test | awk '{print $1}' | xargs kubectl logs -c akv-access 

kubectl get pods | grep test | awk '{print $1}' | xargs kubectl exec -it  -- /bin/bash

```

- tips
```
watch -n 1 xxxx
jq .name
# exclude ""
jq -r name 
export xxx=$()
echo 'YmFzZTY0IGVuY29kZQ==' | base64 -D
```

- az login. aks login ,get secret from akv. set k8s secret
```
az login --service-principal --username xxx --password xxx --tenant xxx;
az aks get-credentials --resource-group xxx --name xxx;
export LOGINID=$(az keyvault secret show --vault-name xxx --name xx | jq -r .value)
export LOGIN_PASSWORD=$(az keyvault secret show --vault-name xxx --name xxx | jq -r .value)
kubectl create secret generic mind-secret --from-literal=loginId=$LOGINID --from-literal=loginPassword=$LOGIN_PASSWORD

kubectl get  secrets | grep test-secret
```