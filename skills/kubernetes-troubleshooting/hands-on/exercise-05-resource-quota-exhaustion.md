# Exercise 05: ResourceQuota 枯渇の診断と修復

## シナリオ

チームの Namespace に新しい Pod をデプロイしようとしたところ、`Forbidden: exceeded quota` エラーで拒否されました。既存のワークロードは正常に動いていますが、新しい Pod を追加できません。Quota の使用状況を調査し、新しい Pod をデプロイできるようにしてください。

## 準備

以下のマニフェストを `exercise-05.yaml` として保存し、apply してください。

```yaml
---
apiVersion: v1
kind: ResourceQuota
metadata:
  name: team-quota
  namespace: k8s-handson
spec:
  hard:
    requests.cpu: "500m"
    requests.memory: "512Mi"
    limits.cpu: "1"
    limits.memory: "1Gi"
    pods: "4"
---
apiVersion: v1
kind: LimitRange
metadata:
  name: default-limits
  namespace: k8s-handson
spec:
  limits:
  - default:
      cpu: "250m"
      memory: "256Mi"
    defaultRequest:
      cpu: "125m"
      memory: "128Mi"
    type: Container
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: existing-app
  namespace: k8s-handson
spec:
  replicas: 3
  selector:
    matchLabels:
      app: existing-app
  template:
    metadata:
      labels:
        app: existing-app
    spec:
      containers:
      - name: app
        image: nginx:alpine
        resources:
          requests:
            cpu: "150m"
            memory: "160Mi"
          limits:
            cpu: "300m"
            memory: "320Mi"
```

```bash
kubectl apply -f exercise-05.yaml
```

しばらく待ってから、新しい Pod をデプロイしてみてください。

```bash
kubectl run new-service -n k8s-handson \
  --image=nginx:alpine \
  --restart=Never \
  --requests='cpu=100m,memory=128Mi' \
  --limits='cpu=200m,memory=256Mi'
```

## 演習

1. `new-service` Pod の作成が拒否された場合、エラーメッセージの内容は何ですか？
2. 現在の ResourceQuota の使用状況を確認してください（used vs hard）
3. どのリソース（CPU / メモリ / Pod 数）が枯渇していますか？
4. 既存の Deployment をリソース効率化（right-sizing）して、新しい Pod 分の枠を確保してください

## ヒント

<details>
<summary>ヒント 1: Quota 使用状況の確認</summary>

```bash
kubectl describe resourcequota team-quota -n k8s-handson
```

`Used` と `Hard` の値を比較してください。

</details>

<details>
<summary>ヒント 2: 計算</summary>

既存 Deployment: 3 replicas × requests.cpu=150m = 450m / 500m (90%)
新 Pod: requests.cpu=100m → 合計 550m > 500m (超過)

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### 診断手順

```bash
# エラーメッセージの確認
kubectl run new-service -n k8s-handson \
  --image=nginx:alpine \
  --restart=Never \
  --requests='cpu=100m,memory=128Mi' \
  --limits='cpu=200m,memory=256Mi'
# Error: exceeded quota: team-quota,
# requested: requests.cpu=100m, used: requests.cpu=450m, limited: requests.cpu=500m

# ResourceQuota の使用状況
kubectl describe resourcequota team-quota -n k8s-handson
```

出力例：

```
Name:            team-quota
Resource         Used    Hard
--------         ----    ----
limits.cpu       900m    1
limits.memory    960Mi   1Gi
pods             3       4
requests.cpu     450m    500m
requests.memory  480Mi   512Mi
```

### 根本原因

既存の `existing-app` Deployment（3 replicas × 150m CPU）で requests.cpu を 450m 使用しています。新しい Pod の 100m を加えると 550m となり、quota の 500m を超過します。

### 修復

既存 Deployment のリソースを right-sizing して枠を確保します。

```bash
# 実際の使用量を確認（metrics-server が必要）
kubectl top pods -n k8s-handson

# requests を減らす（例: 150m → 100m に right-sizing）
kubectl set resources deployment/existing-app -n k8s-handson \
  --requests=cpu=100m,memory=128Mi \
  --limits=cpu=200m,memory=256Mi
```

Quota の使用状況を再確認：

```bash
kubectl describe resourcequota team-quota -n k8s-handson
# requests.cpu: 300m / 500m（余裕ができた）
```

新しい Pod を再作成：

```bash
kubectl run new-service -n k8s-handson \
  --image=nginx:alpine \
  --restart=Never \
  --requests='cpu=100m,memory=128Mi' \
  --limits='cpu=200m,memory=256Mi'
# pod/new-service created
```

</details>

## まとめ

- `kubectl describe resourcequota` で Used / Hard を比較してボトルネックを特定する
- ResourceQuota は requests と limits を独立して制限する — どちらが制約になっているか確認する
- LimitRange はリソース未指定の Pod にデフォルト値を適用する — 想定外の消費に注意
- VPA の推奨値を使った right-sizing は Quota 枯渇の根本対策として有効
- Quota を増やすだけでなく、既存ワークロードの効率化を常に検討する
