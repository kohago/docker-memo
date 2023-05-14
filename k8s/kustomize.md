- aws cli
    - aws --version
        - aws-cli/2.0.52 Python/3.7.4 Darwin/21.5.0 exe/x86_64
    - brew ? brew upgrade awscli; not found
    - so, download the latest version, install it
        - https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html
    - aws --version
        - aws-cli/2.7.5 Python/3.9.11 Darwin/21.5.0 exe/x86_64 prompt/of
- aws cli 現状確認

- aws cli set password to login to aws console
    - aws configure import csv
    - aws iam list-users
    - aws iam get-login-profile --user-name hoge
    - aws iam create-login-profile --user-name hoge --password hoge
    - aws iam update-login-profile --user-name hoge --password hoge
    - aws configure list-profiles

- kubectl
    - kubectl version --client
        - GitVersion:"v1.22.5"
    - version up
        - curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"
        - chomd +x kubectl
        - mv kubectl /usr/local/bin/kubectl
        - kubectl version --client
            - GitVersion:"v1.24.1"

- kubectl config view -o=json
    - clusters
    - contexts
        - clusterとuserの組み合わせ、アクセス制御用
    - users
    - unset
        - kubectl config unset contexts
        - kubectl config unset clusters
        - kubectl config unset users

- kubectl connect to eks
    - aws eks list-clusters
    - rm ~/.kube/config
    - aws eks update-kubeconfig --region ap-northeast-1 --name product-search-prod-01
    - kubectl config view
    - kubectl config current-context
    - kubectl get pods

- kubectl operations
    - kustomize
        - https://kubectl.docs.kubernetes.io/installation/kustomize/
    - patch方式
        - create kustomization.yaml in base,overlays/prod,dev
        - kustomize build base,kustomize build overlays/prod
        - diff -u <(kustomize build base) <(kustomize build overlays/prod)
        - kustomize build overlays/prod | kubectl apply -f -
        - kustomize build overlays/prod | kubectl diff -f -

- how to write patch
```
patches:
  - target:
      kind: Deployment
      labelSelector: io.kompose.service=hoge
    patch: |
      - op: replace
        path: /spec/template/spec/containers/0/ports/0/containerPort
        value: 8090
  - source:
      kind: ConfigMap
      name: env
      fieldPath: data.[api.admin.profile]
    targets:
      - select:
          kind: Deployment
          name: api-hoge
        fieldPaths:
          - spec.template.spec.containers.[name=api-hoge].env.[name=PROFILE].value
  - source:
      kind: ConfigMap
      name: env
      fieldPath: data.[job.site]
    targets:
      - select:
          kind: Job
          name: hoge-job
        fieldPaths:
          - spec.template.spec.containers.[name=hoge].args.2
```

- kustomize のデフォルトの設定は kustomize config save コマンドで取得できます。
- そこで、代わりに --server-dry-run オプションや kubectl diff コマンドの使用をおススメします。