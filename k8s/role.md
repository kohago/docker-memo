- role based access control
```
role
clusterRole
    Role / ClusterRoleは権限を表すルールを指定し、ロールを作成する
    RoleはNamespaceで分離されている。ClusterRoleはNamespaceで分離されておらず、クラスター全体から参照できる
    
        apiVersion: rbac.authorization.k8s.io/v1
        kind: Role
        metadata:
          name: test-role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]

roleBinding
clusterRoleBinding
    Roleとユーザーやグループを紐付けるリソース
        
        apiVersion: rbac.authorization.k8s.io/v1
        kind: RoleBinding
        metadata:
          name: test-rolebinding
        #ロールを指定する
        roleRef:
          apiGroup: rbac.authorization.k8s.io
          kind: Role
          name: test-role
        #ロールを紐付けるユーザやグループ、ServiceAccountを指定する
        subjects:
        - apiGroup: ""
          kind: ServiceAccount
          name: test-sa
          namespace: default
```

- how to use
```
    #service account 作成
    kubectl create sa test-sa
    kubectl get sa/test-sa

    #saのSecret
    kubectl get sa/test-sa -o yaml
        apiVersion: v1
        kind: ServiceAccount
        metadata:
          creationTimestamp: "2018-12-24T14:30:48Z"
          name: test-sa
          namespace: default
          resourceVersion: "86877"
          selfLink: /api/v1/namespaces/default/serviceaccounts/test-sa
          uid: 8200b4a5-0788-11e9-94ed-9e6340a852c2
        secrets:
        - name: test-sa-token-hfv65
    
    #Secretをyaml形式で取得し、Tokenを確認する
    kubectl get secret/test-sa-token-hfv65 -o yaml
        apiVersion: v1
        data:
          ca.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMrekNDQWVPZ0F3SUJBZ0lKQVBQTnpIZkdaNy9DTUEwR0NTcUdTSWIzRFFFQkN3VUFNQlF4RWpBUUJnTlYKQkFNTUNURXlOeTR3TGpBdU1UQWVGdzB4T0RFeU1UUXhPRE0wTVRGYUZ3MDBOakExTURFeE9ETTBNVEZhTUJReApFakFRQmdOVkJBTU1DVEV5Tnk0d0xqQXVNVENDQVNJd0RRWUpLb1pJaHZjTkFRRUJCUUFEZ2dFUEFEQ0NBUW9DCmdnRUJBTU1pbEFPSTFUd01zc0lDQ2xzVnNFVkNEU2NZcHo0ajBoZE1Ubzg5WU5tclNDZHBIL01pcjFQK2NwbzEKZHpXUlRjaDNudnZwc1VOR1lyRXJMTmpIUVlDbGh3WFdSYmZ5a0pCV0NoR3p6T0RPbEtkSkc3RDFsc0NoNzNxdQpPeEYzKzEwVlJNRUY4R05XeWdDSEJINzlHS0V0VnMyWEhPSHkwSmRxTG92a00wbjkxeWIwZ25nUThMeEhwYVJCCkloNGdkL2pLeHk4UDd5ME0xcm9BenIvb2lwbGhzMTFjQkI2cFVzVXA2WUU2VjVuQ2htOXVOQTRDaWdlQTRTSjUKNjkzcFI0NThyNUFOeXVFdzZrSzFiNzdkL000aG1PUDByVHdvRFFQa0dJN2FTZHhMZzR6T2hzRU9LSURLT21VWQppYi9rbHFEdWZwNVlETENpUEhmN1RHV3BlS2NDQXdFQUFhTlFNRTR3SFFZRFZSME9CQllFRlBNd2M3R0YxKzhCCk94N1gvR2wwaG5XL0FaUERNQjhHQTFVZEl3UVlNQmFBRlBNd2M3R0YxKzhCT3g3WC9HbDBoblcvQVpQRE1Bd0cKQTFVZEV3UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFBZjF6THR5ZkhVWkZYWE13Skc0OHJFcwpqQTUrYkZLSFNmUmRtOXdkKzBPd3ljeGVLK1NuY3Z2N3NJd0g5cU5GbVVZdUxvMUpVMmZ1L3NtS1ZGc0RNNkNjClpFQTFmK0xCdDVkMEI3NTNoTWJRVDZzOUdPR3BSbWhnK0NLdnZEV1oyT1ZwcG40YWRka0lMNHEwVi94cWNxa20KdGVMNkZOd015YnpBWWZqTlNuZXc4VnMyQ2ZHa0ZGTmlwYXBuaWg4M21SRFpLelV3YVJ2bjYzc2pFOHc4b1doegpNUUZQY2tnQlkvZGt1N0pqTnN4b0Y1dE1tWUFPYkRDQnhVUFc2SVUvc0k4UElVMG1qREdYeVZ0NVN4TFNzbjJMCjA4QVpXQ2YxL2pNbWYzM25JT2xqLzFITHMycEplTGhKY1ZWR0k0MUNZVnY2bTlubk1XcUVYSlZBSXAyNlRwST0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
          namespace: ZGVmYXVsdA==
          token: ZXlKaGJHY2lPaUpTVXpJMU5pSXNJbXRwWkNJNklpSjkuZXlKcGMzTWlPaUpyZFdKbGNtNWxkR1Z6TDNObGNuWnBZMlZoWTJOdmRXNTBJaXdpYTNWaVpYSnVaWFJsY3k1cGJ5OXpaWEoyYVdObFlXTmpiM1Z1ZEM5dVlXMWxjM0JoWTJVaU9pSmtaV1poZFd4MElpd2lhM1ZpWlhKdVpYUmxjeTVwYnk5elpYSjJhV05sWVdOamIzVnVkQzl6WldOeVpYUXVibUZ0WlNJNkluUmxjM1F0YzJFdGRHOXJaVzR0YUdaMk5qVWlMQ0pyZFdKbGNtNWxkR1Z6TG1sdkwzTmxjblpwWTJWaFkyTnZkVzUwTDNObGNuWnBZMlV0WVdOamIzVnVkQzV1WVcxbElqb2lkR1Z6ZEMxellTSXNJbXQxWW1WeWJtVjBaWE11YVc4dmMyVnlkbWxqWldGalkyOTFiblF2YzJWeWRtbGpaUzFoWTJOdmRXNTBMblZwWkNJNklqZ3lNREJpTkdFMUxUQTNPRGd0TVRGbE9TMDVOR1ZrTFRsbE5qTTBNR0U0TlRKak1pSXNJbk4xWWlJNkluTjVjM1JsYlRwelpYSjJhV05sWVdOamIzVnVkRHBrWldaaGRXeDBPblJsYzNRdGMyRWlmUS5Sd1NzRFp5bVlyVTZPcjl3NWtyZXR1eUJURHV3a2hvTXBVODVDM2M1TzVCekt6VklDTWg3TFhhUzVlQWxyb1N1dXY0UHV0NUtBRExkWWdQX2NqVmFlN3kxSzV4ZXU4STRJWkp1MHBMd2czUVJoNENOcDZfWDFqZ3RqdmQzVXd2QVloODZWaG5PcnJ6Y0N3SW9oTzEyVjBhT3FzS1JuNEM1RlE4aXQwVHhMekhNQ0NMc3VFSHBHV2NKS3cxRGZyelZwUnMtS0pmeWRVem1YdlBieDRPTmpVaWo3d3RmbmZnR3JBNUtINkota1c3akZwT1RFM0UzaTBnckVCZGZpaGY5ZTFwd3ItcEx6S2J0MEN4UlAxSnlvYjF5b2N6YjN5djFBVmVuY1lacGlWWnFGQTE0a01pdVBjNUZGekktU0QzVFgya2IybFJDeGJGZTliV3VHeFVDUFE=
        kind: Secret
        metadata:
          annotations:
            kubernetes.io/service-account.name: test-sa
            kubernetes.io/service-account.uid: 8200b4a5-0788-11e9-94ed-9e6340a852c2
          creationTimestamp: "2018-12-24T14:30:49Z"
          name: test-sa-token-hfv65
          namespace: default
          resourceVersion: "86876"
          selfLink: /api/v1/namespaces/default/secrets/test-sa-token-hfv65
          uid: 8207f1df-0788-11e9-94ed-9e6340a852c2
        type: kubernetes.io/service-account-token

    #tokenはbase64であり、decode
     echo "" | base64 -d

    #service accountを使うために、k8s のcredentialに設定する
    kubectl config set-credentials test-sa --token　''

    #Credentialを使用するContextを作成する
    kubectl config set-context test-sa-context --user test-sa --cluster minikube
    
    #そのContextを使う
    kubectl config use-context test-sa-context
    
```