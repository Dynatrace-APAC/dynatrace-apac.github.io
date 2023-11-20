今回のハンズオンにおけるKubernetes環境の構築手順を掲載します。Kubernetesの環境としてはMicrok8Sを利用しており、Ubuntuホスト1台から試すことが可能です。
ハンズオン終了後に試してみたい方は以下の手順に従って、環境の構築を行うことが可能です。

### Ubuntu環境の準備

AWS EC2などにUbuntu環境を用意します。OSバージョンは以下を参考にインスタンスサイズはt3.xlarge相当以上（ストレージ容量30GB）が推奨となります。

```bash
$ cat /etc/os-release
NAME="Ubuntu"
VERSION="20.04.6 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.6 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```

必要なパッケージのインストールやアップデートを実施します。

### Microk8s環境の構築

以下のコマンドを実行し、microk8s環境を構築します。

```bash
sudo snap install microk8s --classic --channel=1.27/stable
sudo snap alias microk8s.kubectl kubectl
mkdir -p ~/.kube
sudo microk8s.config > ~/.kube/config
sudo usermod -a -G microk8s $USER
sudo chown -R $USER ~/.kube
```

このタイミングで一度、シェルの再接続を実施します。

```bash
microk8s.start
microk8s.enable dns
microk8s.enable hostpath-storage
microk8s.enable ingress
CLUSTER_SERVER=$(microk8s config | grep "server:" | sed 's/^.*server: //')
kubectl config set-cluster microk8s-cluster --insecure-skip-tls-verify=true --server="$CLUSTER_SERVER"
```

### サンプルアプリのインストール

サンプルアプリケーション（easyTravel）を展開していきます。スクリプトは[Github](https://github.com/sukatsu1222/easytravel-k8s)上にありますので
そちらを利用します。

```bash
git clone https://github.com/sukatsu1222/easytravel-k8s.git
easytravel-k8s/deploy-easytravel.sh
easytravel-k8s//deploy-ingress.sh
```

しばらくするとPodsが起動し、ingress経由でアクセスができるようになります。

![angular](../assets/cloud-observe/jp/angular.png)
