# イメージの作成

## 詳細

- [Experiment with the Containerized Data Importer (CDI)](https://kubevirt.io/labs/kubernetes/lab2.html)

## セットアップの流れ

CDIを使ったイメージのインポートは次のようなファイルを使って行う。

```yaml
$ vi pvc_fedora.yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: "fedora"
  labels:
    app: containerized-data-importer
  annotations:
    cdi.kubevirt.io/storage.import.endpoint: "https://download.fedoraproject.org/pub/fedora/linux/releases/33/Cloud/x86_64/images/Fedora-Cloud-Base-33-1.2.x86_64.raw.xz"
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

以下を実行してイメージを書き込む。

```bash
kubectl create -f pvc_fedora.yml
```

以下のようなコマンドで、イメージ書き込みのステータスを確認。たまに止まったりする。失敗っぽいときは`kubectl delete -f pvc_fedora.yml --wait`し他あと、`kubectl create -f pvc_fedora.yml`して再試行。

```bash
kubectl logs -f importer-fedora
```

ただ、毎回外部のソースからイメージのインポートをするのはインターネット接続必須になるしネットワークの無駄遣いになるので、例えばローカルにNGINX Webサーバーを用意してそこにクラウドイメージを置いて、そこからダウンロードする。

以下の設定を追加したNGINX Webサーバーを用意する。

```bash
http{
    # ファイル一覧を有効化
    autoindex on;
    # 正確なサイズで表示しない（ファイルサイズに単位をつける）
    autoindex_exact_size off;
    ...
}
```

インポートに使うYAMLの`storage.import.endpoint`を書き換える。

example:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: "fedora33"
  labels:
    app: containerized-data-importer
  annotations:
    kubevirt.io/provisionOnNode: kubevirt-demo2
    cdi.kubevirt.io/storage.import.endpoint: "http://192.168.0.9/Fedora-Cloud-Base-33-1.2.x86_64.raw.xz"
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: kubevirt-hostpath-provisioner
```
