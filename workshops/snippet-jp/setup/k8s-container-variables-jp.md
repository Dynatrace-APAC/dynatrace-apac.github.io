<!-- Code for k8s Container Environment Variables-->
Duration: 5

### 環境変数の追加

シェルターミナルで、以下のコマンドで環境変数を追加します。

`nano ~/sockshop/manifests/sockshop-app/production/front-end.yml`

**インデントが正しく行われているか、エラープロンプトが表示されていないかを確認してください。**

```bash
        env:
        - name: DT_TAGS
          value: "product=sockshop"
        - name: DT_CUSTOM_PROP
          value: "SERVICE_TYPE=FRONTEND"
```
![JSON](../assets/k8s/Picture13.png)

修正したファイルを**Ctrl-X**、**Y**、**Enter**で保存し、以下のコマンドを実行して変更を再適用します。

```bash
kubectl apply -f ~/sockshop/manifests/sockshop-app/production/front-end.yml
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash
```

### 確認

作業が完了したら、Dynatraceで変更を検証することができます。

![JSON](../assets/k8s//Picture14.png)

