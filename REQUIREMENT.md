# 動作要件

KubeVirtを使うにはK8sクラスターが必要。

## kubeadmを使う場合

### K8sクラスターのセットアップ

公式ドキュメントに従って、kubeadmをインストールし、クラスターを作成する。
CRIはDockerかCRI-Oを選択する。

- [Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [KubeVirt Installation Requirements](https://kubevirt.io/user-guide/operations/installation/)

### KubeVirtのセットアップ

kubeadmでK8sクラスターをセットアップした後にKubeVirtを導入する。KubeVirtのセットアップは
[KubeVirt quickstart with cloud providers](https://kubevirt.io/quickstart_cloud/) を参考にする。

## その他の方法

[KuberVirt.io](https://kubevirt.io/)サイトを開くと、Minikube、Kind、クラウドでKubeVirtを利用する方法が公開されている。初めて試すならKindを使った方法がおすすめ(kind CLIとDockerがあればすぐ動くので)。

- [kind Quickstart](https://kind.sigs.k8s.io/docs/user/quick-start)
- [KubeVirt quickstart with kind](https://kubevirt.io/quickstart_kind/)
