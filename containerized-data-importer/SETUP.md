# KubeVirt CDIのセットアップ

## 詳細

- [Experiment with the Containerized Data Importer (CDI)](https://kubevirt.io/labs/kubernetes/lab2.html)

## セットアップの流れ

最新バージョンを確認

```bash
export VERSION=$(curl -s https://github.com/kubevirt/containerized-data-importer/releases/latest | grep -o "v[0-9]\.[0-9]*\.[0-9]*")

echo $VERSION
v1.33.0
```

デプロイする。動作確認したのは`v1.33.0`

```bash
kubectl create -f https://github.com/kubevirt/containerized-data-importer/releases/download/v1.33.0/cdi-operator.yaml
kubectl create -f https://github.com/kubevirt/containerized-data-importer/releases/download/v1.33.0/cdi-cr.yaml
```
