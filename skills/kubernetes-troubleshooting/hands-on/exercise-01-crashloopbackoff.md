# Exercise 01: CrashLoopBackOff の診断と修復

## シナリオ

あなたのチームがデプロイした `memory-hog` アプリケーションが CrashLoopBackOff に陥っています。アプリケーションチームは「ローカルでは動いていた」と報告しています。原因を特定し、Pod を正常に稼働させてください。

## 準備

演習用 Namespace を作成し、問題のある Deployment を apply します。

```bash
kubectl create namespace k8s-handson --dry-run=client -o yaml | kubectl apply -f -
```

以下のマニフェストを `exercise-01.yaml` として保存し、apply してください。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: memory-hog
  namespace: k8s-handson
spec:
  replicas: 1
  selector:
    matchLabels:
      app: memory-hog
  template:
    metadata:
      labels:
        app: memory-hog
    spec:
      containers:
      - name: stress
        image: polinux/stress
        command: ["stress"]
        args: ["--vm", "1", "--vm-bytes", "256M", "--vm-hang", "1"]
        resources:
          requests:
            memory: "64Mi"
            cpu: "50m"
          limits:
            memory: "128Mi"
            cpu: "100m"
```

```bash
kubectl apply -f exercise-01.yaml
```

## 演習

以下の問いに答えながら診断を進めてください。

1. Pod の現在のステータスは何ですか？何回再起動していますか？
2. Pod が終了した理由（Termination Reason）は何ですか？
3. コンテナの Exit Code は何を意味していますか？
4. なぜこのコンテナはメモリ制限を超過したのですか？
5. Pod を正常稼働させるにはどう修正すれば良いですか？

## ヒント

<details>
<summary>ヒント 1: Pod のステータス確認</summary>

```bash
kubectl get pods -n k8s-handson
kubectl describe pod -n k8s-handson -l app=memory-hog
```

`Last State` セクションの `Reason` と `Exit Code` を確認してください。

</details>

<details>
<summary>ヒント 2: OOMKilled の意味</summary>

Exit Code 137 は SIGKILL (128 + 9) を意味し、OOMKilled はカーネルの OOM Killer がプロセスを強制終了したことを示します。コンテナのメモリ使用量が `limits.memory` を超過しました。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### 診断手順

```bash
# Pod ステータスを確認
kubectl get pods -n k8s-handson
# NAME                          READY   STATUS             RESTARTS   AGE
# memory-hog-xxx                0/1     CrashLoopBackOff   3          2m

# 詳細を確認
kubectl describe pod -n k8s-handson -l app=memory-hog
```

`describe` の出力で以下を確認：
- **Last State**: Terminated, Reason: **OOMKilled**, Exit Code: **137**
- **Limits**: memory: 128Mi

### 根本原因

アプリケーションは `stress --vm-bytes 256M` で 256MB のメモリを確保しようとしていますが、コンテナのメモリ制限は 128Mi に設定されています。カーネルの OOM Killer がコンテナプロセスを強制終了し、kubelet が再起動を繰り返すことで CrashLoopBackOff に陥っています。

### 修復

メモリ制限をアプリケーションの実際の使用量に合わせて増加させます。

```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "50m"
  limits:
    memory: "512Mi"
    cpu: "100m"
```

```bash
kubectl set resources deployment/memory-hog -n k8s-handson \
  --limits=memory=512Mi --requests=memory=256Mi
```

Pod が再作成され、Running 状態になることを確認します。

```bash
kubectl get pods -n k8s-handson -w
```

</details>

## まとめ

- OOMKilled は Exit Code 137 と `Reason: OOMKilled` で識別できる
- `kubectl describe pod` の Last State セクションが最も有用な情報源
- メモリ制限はアプリケーションの実際のメモリ使用量 + マージンで設定する
- 本番環境では VPA (Vertical Pod Autoscaler) の推奨値を参考にする
