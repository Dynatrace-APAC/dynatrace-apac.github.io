<!-- Step to setup labels and annotations -->
Duration: 5

Sockshopアプリを再起動すると、Dynatraceにサービスが表示されるようになります。

`~/sockshop/manifests/sockshop-app/production/front-end.yml`を参考にして、Dynatraceが自動的にアノテーションとラベルを拾うように設定したいと思います。

```bash
---
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: front-end.stable
    product: sockshop
    release: stable
    stage: prod
    tier: frontend
    version: "1.4"
  name: front-end.stable
  namespace: production
spec:
  replicas: 1
  selector:
    matchLabels:
      app: front-end.stable
      product: sockshop
      release: stable
      stage: prod
      tier: frontend
      version: "1.4"
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      annotations:
        pipeline.build: 1.4.0.7424
        pipeline.project: sockshop
        pipeline.stage: prod-stable
        sidecar.istio.io/inject: "false"
        support.channel: '#support-sockshop-frontend'
        support.contact: jane.smith@sockshop.com
      labels:
        app.kubernetes.io/name: front-end
        app.kubernetes.io/version: "1.4"
        app.kubernetes.io/part-of: sockshop
        app: front-end.stable
        product: sockshop
        release: stable
        stage: prod
        tier: frontend
        version: "1.4"
```
### サービスアカウントへのview role の割り当て

OneAgentはサービスアカウントを使用して、Kubernetes REST APIを介してメタデータを照会します。
そのためにはサービスアカウントに、**viewer role**が付与されている必要があります。
CLIで、**production project**に対して以下のコマンドを実行します。

```bash
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:production --namespace=production
```

**dev project**に対しても同様に実行します。

```bash
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:dev --namespace=dev
```

Dynatraceが変更を認識するまで待ちます。

Negative
: ⚠️ アプリを手動でリサイクルするには、以下のコマンドを実行してください。
`wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash`

### 確認

作業が完了したら、Dynatraceで変更を検証することができます。

![JSON](../assets/k8s/Picture12.png)

Positive
: 詳細は[公式サイト](https://www.dynatrace.com/support/help/technology-support/container-platforms/kubernetes/other-deployments-and-configurations/leverage-tags-defined-in-kubernetes-deployments/)をご確認ください。
