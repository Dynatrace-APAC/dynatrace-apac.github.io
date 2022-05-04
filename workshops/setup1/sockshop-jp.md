<!-- Code for restarting sockshop application -->

### Sockshopアプリケーションの再起動

様々なプロセスが自動的に検出されているのがわかりますが、Dynatraceはそれらを再起動するよう促します。これは、コードを変更せずに自動的に監視を行うために必要です。

以下のコマンドを実行して、**devとproduction**の2つのNamespacesに含まれるPodsを作り直します。

```bash
kubectl delete pods --all -n dev
kubectl delete pods --all -n production
```
