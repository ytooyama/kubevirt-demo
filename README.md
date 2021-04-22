# KubeVirtでクラウドイメージを使ってVMを作成して使う

## 手順概要

KubeVirtで一つのVMを作るためには、次の二つをVMごとに実施。

- OSイメージを[Experiment with the Containerized Data Importer (CDI)](https://kubevirt.io/labs/kubernetes/lab2.html)に従ってPV+PVCペアに書き込む。
- VMを作るためのYAMLを書いて作成する。

## VMを作るためのYAMLについて

こんな感じのYAMLを書く。同じようなファイルを複数書くようになると思うので、`kustomize`を使った方が効率が良いと思う。

```yaml
apiVersion: kubevirt.io/v1alpha3
kind: VirtualMachine
metadata:
  creationTimestamp: 2018-07-04T15:03:08Z
  generation: 1
  labels:
    kubevirt.io/os: linux
  name: fedora1
spec:
  running: true
  template:
    metadata:
      creationTimestamp: null
      labels:
        kubevirt.io/domain: fedora1
    spec:
      domain:
        cpu:
          cores: 2
        devices:
          disks:
          - disk:
              bus: virtio
            name: disk0
          - cdrom:
              bus: sata
              readonly: true
            name: cloudinitdisk
        machine:
          type: q35
        resources:
          requests:
            memory: 1024M
      volumes:
      - name: disk0
        persistentVolumeClaim:
          claimName: fedora-local
      - cloudInitNoCloud:
          userData: |
            #cloud-config
            hostname: fedora1
            disable_root: false
            password: myfedora
            ssh_pwauth: True
            chpasswd: { expire: False }
        name: cloudinitdisk
```

公開鍵認証をしたい場合は`ssh_authorized_keys`を設定して、公開鍵を貼り付ける。

```yaml
      - cloudInitNoCloud:
          userData: |
            #cloud-config
            hostname: vm1
            ssh_pwauth: True
            disable_root: false
            ssh_authorized_keys:
            - ssh-rsa YOUR_SSH_PUB_KEY_HERE
```

## VMへのアクセス方法について

SSHでアクセス

```bash
kubectl get vmi
NAME      AGE   PHASE     IP               NODENAME
fedora1   8s    Running   10.244.241.168   kubevirt-demo2

ssh fedora@10.244.241.168
```

VNC-Viewerを使って（ただし、パスワード認証+デスクトップ環境が要件）

```bash
virtctl vnc vmname
```

コンソール接続でアクセス（ただし、パスワード認証が要件）

```bash
virtctl console vmname
```
