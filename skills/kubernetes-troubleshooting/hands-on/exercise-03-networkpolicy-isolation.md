# Exercise 03: NetworkPolicy による通信遮断の診断と修復

## シナリオ

`frontend` Pod から `backend` Pod への HTTP 通信が突然遮断されました。最近 NetworkPolicy が導入されたとのことです。NetworkPolicy の設定ミスを特定し、通信を復旧させてください。

## 準備

以下のマニフェストを `exercise-03.yaml` として保存し、apply してください。

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  namespace: k8s-handson
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backend
      tier: api
  template:
    metadata:
      labels:
        app: backend
        tier: api
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: backend
  namespace: k8s-handson
spec:
  selector:
    app: backend
  ports:
  - port: 80
    targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: k8s-handson
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
      tier: web
  template:
    metadata:
      labels:
        app: frontend
        tier: web
    spec:
      containers:
      - name: curl
        image: curlimages/curl
        command: ["sleep", "3600"]
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
  namespace: k8s-handson
spec:
  podSelector:
    matchLabels:
      app: backend
      tier: api
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
          tier: backend
    ports:
    - protocol: TCP
      port: 80
```

```bash
kubectl apply -f exercise-03.yaml
```

## 演習

1. frontend Pod から backend Service への通信をテストし、遮断されていることを確認してください
2. NetworkPolicy の ingress ルールを確認し、何が問題か特定してください
3. frontend Pod と backend Pod それぞれのラベルを確認してください
4. NetworkPolicy を修正して通信を復旧させてください

## ヒント

<details>
<summary>ヒント 1: 通信テスト</summary>

```bash
# frontend Pod からの通信テスト
kubectl exec -n k8s-handson deploy/frontend -- \
  curl -s --connect-timeout 5 http://backend
```

タイムアウトする場合、NetworkPolicy がトラフィックをブロックしています。

</details>

<details>
<summary>ヒント 2: ラベルの比較</summary>

```bash
# frontend のラベルを確認
kubectl get pods -n k8s-handson -l app=frontend --show-labels

# NetworkPolicy の from セレクタを確認
kubectl get networkpolicy allow-frontend-to-backend -n k8s-handson -o yaml
```

`from.podSelector.matchLabels` と frontend Pod の実際のラベルを比較してください。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### 診断手順

```bash
# 通信テスト — タイムアウトする
kubectl exec -n k8s-handson deploy/frontend -- \
  curl -s --connect-timeout 5 http://backend
# curl: (28) Connection timed out

# frontend Pod のラベルを確認
kubectl get pods -n k8s-handson -l app=frontend --show-labels
# LABELS: app=frontend,tier=web

# NetworkPolicy を確認
kubectl get networkpolicy allow-frontend-to-backend -n k8s-handson -o yaml
```

### 根本原因

NetworkPolicy の ingress ルールで `tier: backend` を許可していますが、frontend Pod の実際のラベルは `tier: web` です。ラベルが一致しないため、全ての ingress トラフィックが暗黙的に拒否されています。

### 修復

NetworkPolicy の podSelector を frontend の実際のラベルに合わせます。

```bash
kubectl patch networkpolicy allow-frontend-to-backend -n k8s-handson \
  --type='json' \
  -p='[{"op": "replace", "path": "/spec/ingress/0/from/0/podSelector/matchLabels/tier", "value": "web"}]'
```

通信を再テスト：

```bash
kubectl exec -n k8s-handson deploy/frontend -- \
  curl -s --connect-timeout 5 http://backend
# nginx のデフォルトページが表示される
```

</details>

## まとめ

- NetworkPolicy が Pod を選択すると、明示的に許可されていないトラフィックは全て拒否される
- ラベルの微妙な不一致（`tier: backend` vs `tier: web`）はエラーメッセージなく通信を遮断する
- `--show-labels` と NetworkPolicy の YAML 出力を突き合わせて診断する
- 変更前に一時的にポリシーを緩和して NetworkPolicy が原因であることを確認するのも有効
