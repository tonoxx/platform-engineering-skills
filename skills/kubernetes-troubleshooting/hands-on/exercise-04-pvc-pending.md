# Exercise 04: PVC Pending の診断と修復

## シナリオ

データベース用の PersistentVolumeClaim が Pending のままで、Pod が起動できません。インフラチームは「StorageClass は設定済み」と言っていますが、PVC がバインドされません。原因を特定して Pod を起動させてください。

## 準備

以下のマニフェストを `exercise-04.yaml` として保存し、apply してください。

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-data
  namespace: k8s-handson
spec:
  accessModes:
  - ReadWriteMany
  storageClassName: gp2
  resources:
    requests:
      storage: 10Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: database
  namespace: k8s-handson
spec:
  replicas: 1
  selector:
    matchLabels:
      app: database
  template:
    metadata:
      labels:
        app: database
    spec:
      containers:
      - name: postgres
        image: postgres:15-alpine
        env:
        - name: POSTGRES_PASSWORD
          value: "handson-password"
        ports:
        - containerPort: 5432
        volumeMounts:
        - name: data
          mountPath: /var/lib/postgresql/data
      volumes:
      - name: data
        persistentVolumeClaim:
          claimName: db-data
```

```bash
kubectl apply -f exercise-04.yaml
```

## 演習

1. PVC のステータスと Events を確認してください
2. 使用されている StorageClass の設定を確認してください
3. PVC の `accessModes` と StorageClass がサポートする AccessMode の互換性を確認してください
4. PVC を修正して正常にバインドさせ、Pod を起動してください

## ヒント

<details>
<summary>ヒント 1: PVC と StorageClass の確認</summary>

```bash
kubectl get pvc db-data -n k8s-handson
kubectl describe pvc db-data -n k8s-handson
kubectl get storageclass gp2 -o yaml
```

</details>

<details>
<summary>ヒント 2: EBS の制約</summary>

AWS EBS (gp2/gp3) は `ReadWriteOnce` (RWO) のみサポートします。`ReadWriteMany` (RWX) が必要な場合は EFS (NFS) ベースの StorageClass が必要です。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### 診断手順

```bash
# PVC ステータス確認
kubectl get pvc db-data -n k8s-handson
# NAME      STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
# db-data   Pending                                       gp2            1m

# PVC の Events 確認
kubectl describe pvc db-data -n k8s-handson
# Events: "waiting for first consumer to be created before binding"
# または "no persistent volumes available for this claim"

# StorageClass 確認
kubectl get storageclass gp2 -o yaml
# provisioner: kubernetes.io/aws-ebs (or ebs.csi.aws.com)
```

### 根本原因

PVC が `ReadWriteMany` (RWX) を要求していますが、gp2 StorageClass の provisioner（AWS EBS）は `ReadWriteOnce` (RWO) のみサポートしています。EBS ボリュームは単一ノードにしかアタッチできないため、RWX はサポート外です。

### 修復

データベースは単一レプリカなので、RWO で問題ありません。PVC を削除して AccessMode を修正します。

```bash
# Deployment を一旦削除（PVC を解放するため）
kubectl delete deployment database -n k8s-handson

# PVC を削除
kubectl delete pvc db-data -n k8s-handson
```

修正した PVC（`ReadWriteOnce` に変更）を含むマニフェストを再 apply：

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-data
  namespace: k8s-handson
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: gp2
  resources:
    requests:
      storage: 10Gi
```

```bash
kubectl apply -f exercise-04-fixed.yaml
kubectl apply -f exercise-04.yaml  # Deployment 部分を再 apply
```

PVC がバインドされ、Pod が Running になることを確認：

```bash
kubectl get pvc db-data -n k8s-handson
kubectl get pods -n k8s-handson -l app=database
```

</details>

## まとめ

- EBS (gp2/gp3) は ReadWriteOnce のみ、EFS は ReadWriteMany をサポート
- PVC の Events メッセージがバインド失敗の理由を示す
- `volumeBindingMode: WaitForFirstConsumer` の場合、Pod がスケジュールされるまでバインドされない
- OpenShift では `oc get storageclass` で利用可能な StorageClass と対応 AccessMode を一覧できる
