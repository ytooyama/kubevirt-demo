# KubeVirt ストレージの準備

## 詳細

- [kubevirt.io/hostpath-provisioner](https://github.com/kubevirt/hostpath-provisioner)を使いました。

## セットアップの流れ

`kubevirt.io/hostpath-provisioner`は`/var/hpvolumes`をストレージパスとして使うので、事前に作っておく。
ストレージを食いつぶすとシステムが止まるので、システムとストレージを別のデバイスで分けた方が良い。

```bash
sudo mkdir -p /var/hpvolumes
sudo chmod 777 /var/hpvolumes
```

SELinux有効のシステムでは以下を実施。

```bash
sudo chcon -t container_file_t -R /var/hpvolumes
```

`kubevirt.io/hostpath-provisioner`をデプロイ

```bash
git clone https://github.com/kubevirt/hostpath-provisioner.git
cd hostpath-provisioner/deploy
kubectl apply -f kubevirt-hostpath-provisioner.yaml
```

Podができたことを確認する。

```bash
kubectl get po -n kubevirt-hostpath-provisioner
NAME                                  READY   STATUS    RESTARTS   AGE
kubevirt-hostpath-provisioner-hwrfh   1/1     Running   1          24h
```
