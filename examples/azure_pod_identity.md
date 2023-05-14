- azure kubernetes provision status
  provision state: failed  支給失敗なので、作成失敗と理解して良い

- get aks cluster info
```
az aks list -g xxx --query '[].{name:name}'

az aks show -g xxx -n test-cluster --query '{nodeResourceGroup:nodeResourceGroup,servicePrincipalProfile:servicePrincipalProfile,id:id,enableRbac:enableRbac,kubernetesVersion:kubernetesVersion,identityProfile:identityProfile}'

        {
        "enableRbac": true,
        "id": "/subscriptions/xxx/resourcegroups/xxx/providers/Microsoft.ContainerService/managedClusters/test-cluster",
        "identityProfile": null,
        "kubernetesVersion": "1.18.2",
        "nodeResourceGroup": "MC_xxx_test-cluster_japaneast",
        "servicePrincipalProfile": {
            "clientId": "clientid"
        }
        }
```

- create managed identity
```
 az identity create -g MC_xxx_test-cluster_japaneast -n test-pod-identity --subscription xxx

    {
    "clientId": "c4752caf-9404-43bb-b831-ce7248f2cbce",
    "clientSecretUrl": "https://control-japaneast.identity.azure.net/subscriptions/xxx/resourcegroups/MC_xxx_test-cluster_japaneast/providers/Microsoft.ManagedIdentity/userAssignedIdentities/test-pod-identity/credentials?tid=tid&oid=pID&aid=c4752caf-9404-43bb-b831-ce7248f2cbce",
    "id": "/subscriptions/xxx/resourcegroups/MC_xxx_test-cluster_japaneast/providers/Microsoft.ManagedIdentity/userAssignedIdentities/test-pod-identity",
    "location": "japaneast",
    "name": "test-pod-identity",
    "principalId": "pID",
    "resourceGroup": "MC_xxx_test-cluster_japaneast",
    "tags": {},
    "tenantId": "tid",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }

az identity show -g MC_xxx_test-cluster_japaneast -n test-pod-identity --subscription xxx --query '{clientId:clientId,id:id}'

    {
  "clientId": "cid",
  "id": "/subscriptions/xxx/resourcegroups/MC_xxx_test-cluster_japaneast/providers/Microsoft.ManagedIdentity/userAssignedIdentities/test-pod-identity"
}
```

- assign reader role to the managed identity,scope is resource group
```
az role assignment create --role Reader --assignee c4752caf-9404-43bb-b831-ce7248f2cbce --scope /subscriptions/xxx/resourceGroups/MC_xxx_test-cluster_japaneast
   {
     "canDelegate": null,
     "id": "/subscriptions/xxx/resourceGroups/MC_xxx_test-cluster_japaneast/providers/Microsoft.Authorization/roleAssignments/xx",
     "name": "xx",
     "principalId": "xx",
     "principalType": "ServicePrincipal",
     "resourceGroup": "MC_xxx_test-cluster_japaneast",
     "roleDefinitionId": "/subscriptions/xxx/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
     "scope": "/subscriptions/xxx/resourceGroups/MC_xxx_test-cluster_japaneast",
     "type": "Microsoft.Authorization/roleAssignments"
   }
```


- Deploy AzureIdentity and binding
```
kubectl apply -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
kubectl apply -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/mic-exception.yaml

cat <<EOF | kubectl apply -f -
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzureIdentity
metadata:
  name: test-pod-identity
spec:
  type: 0
  resourceID: /subscriptions/xxx/resourcegroups/MC_xxx_test-cluster_japaneast/providers/Microsoft.ManagedIdentity/userAssignedIdentities/test-pod-identity
  clientID: xx
EOF


cat <<EOF | kubectl apply -f -
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzureIdentityBinding
metadata:
  name: test-pod-identity-binding
spec:
  azureIdentity: test-pod-identity
  selector: test-pod-identity
EOF

```

- deploy validation
```
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: demo
spec:
  containers:
  - name: demo
    image: mcr.microsoft.com/k8s/aad-pod-identity/demo:1.2
EOF
     
kubectl exec -it demo -- /bin/bash

#install curl
apt update
apt install curl

#test with curl
curl 'http://169.254.169.254/metadata/identity/oauth2/token?resource=https://vault.azure.net' -H Metadata:true -s
  {"access_token":"token","refresh_token":"","expires_in":"86399","expires_on":"1598400718","not_before":"1598314018","resource":"https://vault.azure.net","token_type":"Bearer"}

curl -X POST -H "Content-Type: application/json" -H "Authorization:Bearer token"}' https://tts-hsm-terraform-test.vault.azure.net/keys/hsm-key-test/8bc9d25842a44db994b2e28e7ccddfe2/encrypt?api-version=7.1
    {"error":{"code":"Forbidden","message":"The user, group or application 'appid=c4752caf-9404-43bb-b831-ce7248f2cbce;oid=4255bbf4-f3c5-422e-9bc5-5676944785e1;iss=https://sts.windows.net/tid/' does not have keys encrypt permission on key vault 'tts-hsm-terraform-test;location=japaneast'. For help resolving this issue, please see https://go.microsoft.com/fwlink/?linkid=2125287","innererror":{"code":"AccessDenied"}}}

#give permission to managed identity
   
```

- add label to oracle pod 
```
kubectl label pods podname aadpodidbinding=test-pod-identity

#from circleCI?
 k8s: oracle.yamlにlabelを使い、定数で良い
```

- delete from cluster
```
kubectl get deployment
kubectl get DaemonSet 

# delete using file is good
kubectl delete -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
kubectl delete -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/mic-exception.yaml
```