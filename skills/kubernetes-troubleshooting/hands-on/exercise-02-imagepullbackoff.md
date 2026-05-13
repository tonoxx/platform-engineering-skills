# Exercise 02: ImagePullBackOff の診断と修復

## シナリオ

新しいマイクロサービス `auth-service` をデプロイしたところ、Pod が ImagePullBackOff 状態になりました。イメージはプライベート ECR レジストリにプッシュ済みと報告されています。原因を特定し、Pod がイメージを正常にプルできるようにしてください。

## 準備

以下のマニフェストを `exercise-02.yaml` として保存し、apply してください。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: auth-service
  namespace: k8s-handson
spec:
  containers:
  - name: auth
    image: 999999999999.dkr.ecr.us-east-1.amazonaws.com/auth-service:v1.0.0
    ports:
    - containerPort: 8080
  imagePullSecrets:
  - name: ecr-credentials
```

```bash
kubectl apply -f exercise-02.yaml
```

## 演習

以下の問いに答えながら診断を進めてください。

1. Pod のステータスと Events に表示されるエラーメッセージは何ですか？
2. このエラーが発生する原因として考えられるものは何ですか？（3つ挙げてください）
3. `imagePullSecrets` で参照されている Secret は存在しますか？
4. この問題を修復する手順を実行してください

## ヒント

<details>
<summary>ヒント 1: Events の確認</summary>

```bash
kubectl describe pod auth-service -n k8s-handson
```

Events セクションの `Failed to pull image` メッセージを確認してください。

</details>

<details>
<summary>ヒント 2: Secret の確認</summary>

```bash
kubectl get secrets -n k8s-handson
```

`ecr-credentials` という Secret が存在するか確認してください。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### 診断手順

```bash
# Pod ステータス確認
kubectl get pod auth-service -n k8s-handson
# NAME           READY   STATUS             RESTARTS   AGE
# auth-service   0/1     ImagePullBackOff   0          1m

# Events 確認
kubectl describe pod auth-service -n k8s-handson
```

Events に以下のようなメッセージが表示されます：
- `Failed to pull image "999999999999.dkr.ecr...": pull access denied`
- `Error: ImagePullBackOff`

### 根本原因

この演習では複数の原因が重なっています：

1. **イメージ名が無効**: `999999999999` は存在しない AWS アカウント ID です。実際のレジストリ URI を使用する必要があります。
2. **Secret が存在しない**: `ecr-credentials` という imagePullSecret が Namespace 内に作成されていません。

### 修復（実環境での対応手順）

**Step 1**: ECR 認証トークンを取得して Secret を作成

```bash
# ECR ログイントークンを取得（実際のアカウント ID に置換）
aws ecr get-login-password --region us-east-1 | \
  kubectl create secret docker-registry ecr-credentials \
  -n k8s-handson \
  --docker-server=<ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com \
  --docker-username=AWS \
  --docker-password-stdin
```

**Step 2**: Pod のイメージ参照を正しい URI に修正

```bash
kubectl delete pod auth-service -n k8s-handson
# exercise-02.yaml のイメージを正しい ECR URI に修正して再 apply
kubectl apply -f exercise-02.yaml
```

**EKS の場合の代替手法**: IAM Roles for Service Accounts (IRSA) を使えば imagePullSecrets なしで ECR からプル可能です。

</details>

## まとめ

- ImagePullBackOff は `kubectl describe pod` の Events で原因を特定する
- 主な原因: イメージ名/タグの誤り、レジストリ認証の不備、レジストリ到達不能
- EKS では IRSA を使うと ECR の認証を自動化でき、Secret 管理が不要になる
- OpenShift では内部レジストリ (ImageStream) を使うことで認証を簡素化できる
