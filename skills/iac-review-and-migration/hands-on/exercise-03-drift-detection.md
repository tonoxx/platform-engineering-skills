# Exercise 03: Drift 検出と修復

## シナリオ

Terraform で管理している Security Group に対して、運用チームが「緊急対応」として AWS CLI で直接ルールを追加しました。この手動変更（ドリフト）を検出し、適切に修復してください。

## 準備

### Step 1: Terraform でリソースを作成

作業ディレクトリに以下の `main.tf` を配置してください。

```hcl
terraform {
  required_version = ">= 1.5"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "handson" {
  cidr_block = "10.99.0.0/16"
  tags = { Name = "handson-drift" }
}

resource "aws_security_group" "web" {
  name_prefix = "handson-web-"
  vpc_id      = aws_vpc.handson.id
  description = "Web server security group"

  ingress {
    description = "HTTPS"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = { Name = "handson-web-sg" }
}
```

```bash
terraform init
terraform apply -auto-approve
```

### Step 2: 手動でドリフトを発生させる

```bash
# Security Group ID を取得
SG_ID=$(terraform output -raw 2>/dev/null || terraform show -json | jq -r '.values.root_module.resources[] | select(.type=="aws_security_group") | .values.id')
echo "Security Group ID: $SG_ID"

# 手動で SSH ルールを追加（ドリフト発生）
aws ec2 authorize-security-group-ingress \
  --group-id "$SG_ID" \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0
```

## 演習

1. `terraform plan` を実行してドリフトを検出してください
2. plan 出力からどのような変更が検出されたか記述してください
3. この手動変更に対して、以下のどちらの対応が適切か判断してください：
   - **Option A**: `terraform apply` で IaC の状態に戻す（手動変更を破棄）
   - **Option B**: `main.tf` を更新して手動変更を IaC に取り込む
4. 選択した方法で修復を実行し、`terraform plan` で差分なしを確認してください

## ヒント

<details>
<summary>ヒント 1: plan の読み方</summary>

```bash
terraform plan
```

`~ update in-place` の出力で、Terraform が実リソースと config の差分を表示します。`-` は削除される設定、`+` は追加される設定です。

</details>

<details>
<summary>ヒント 2: 判断基準</summary>

SSH (port 22) を `0.0.0.0/0` から許可するのはセキュリティリスクです。これが「緊急対応で一時的に必要だった」なら Option A で除去。「恒久的に必要」なら Option B で取り込みつつ CIDR を絞るべきです。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### ドリフト検出

```bash
terraform plan
```

出力例：

```
aws_security_group.web will be updated in-place
  ~ resource "aws_security_group" "web" {
      ~ ingress = [
          + {
              + cidr_blocks = ["0.0.0.0/0"]
              + from_port   = 22
              + protocol    = "tcp"
              + to_port     = 22
            },
            # (existing HTTPS rule unchanged)
        ]
    }
```

Terraform は手動追加された SSH ルールを検出しています。

### 推奨対応: Option A（手動変更を破棄）

SSH を `0.0.0.0/0` に公開するのはセキュリティリスクが高いため、除去が適切です。

```bash
terraform apply -auto-approve
```

これにより手動追加された SSH ルールが削除され、IaC の定義通りの状態に戻ります。

### もし恒久的に必要な場合: Option B

`main.tf` に SSH ルールを追加し、CIDR を制限します。

```hcl
  ingress {
    description = "SSH from bastion"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["10.99.0.0/16"]  # VPC 内部のみに制限
  }
```

```bash
terraform apply -auto-approve
terraform plan
# No changes.
```

</details>

## クリーンアップ

```bash
terraform destroy -auto-approve
```

## まとめ

- `terraform plan` はドリフト検出の最も基本的な手段
- ドリフト修復は「IaC に戻す」か「IaC を更新する」の二択 — セキュリティ影響で判断する
- 手動変更の根本原因を調査し、再発防止策（コンソールアクセス制限、IaC カバレッジ拡大）を検討する
- 定期的な `terraform plan` の自動実行でドリフトを早期検出する仕組みを作る
