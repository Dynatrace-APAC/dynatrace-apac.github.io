<!-- Code for k8s Process Group & Service Naming Rules -->
Duration: 10



**Settings -> Processes and containers -> Process group naming**へ移動し、**Add a new rule**をクリックします。

ルールに名前を付けてください（例: **Kubernetes Project.Namespace.Container**）

`k8s-{ProcessGroup:Kubernetes:pipeline.project}.{ProcessGroup:KubernetesNamespace}.{ProcessGroup:KubernetesContainerName}`
という形式で入力してください。

Conditionsドロップダウンリストでは, **Kubernetes namespace**と**exists**を選択します。

![JSON](../assets/k8s/Picture15.png)

**Preview**をクリックし、マッチしているエントリーを確認します。

![JSON](../assets/k8s/Picture16.png)

**Create Rule**と**Save Changes**をクリックします。

### 確認

作業が完了したら、Dynatraceで変更を確認することができます。

![JSON](../assets/k8s/Picture17.png)

### サービス名の命名規則

**Settings -> Server-side service monitoring -> Service naming rules**へ移動し、**Add a new rule**をクリックします。

ルールに名前を付けてください（例: **Kubernetes Project.Namespace.Container**）

`{Service:DetectedName}.{ProcessGroup:KubernetesNamespace}`
という形式で入力してください。

Conditionsドロップダウンリストでは, **Kubernetes namespace**と**exists**を選択します。

![JSON](../assets/k8s/Picture18.png)

**Preview**をクリックし、マッチしているエントリーを確認します。

![JSON](../assets/k8s/Picture19.png)

**Create Rule**と**Save Changes**をクリックします。

### 確認

作業が完了したら、Dynatraceで変更を確認することができます。

![JSON](../assets/k8s/Picture20.png)